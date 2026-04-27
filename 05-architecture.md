# 5. Architecture & Technology

## 5.1 System Overview

The Aivive stack is intentionally serverless and capital-efficient.

- **Application**: Next.js 16 (App Router, Turbopack), deployed on Vercel.
- **Database**: Supabase Postgres with row-level security.
- **Storage**: Cloudflare R2 with a managed image-resize CDN.
- **Background jobs**: Inngest (AI generation, content moderation, the cross-chain burn pipeline).
- **EVM chain layer**: viem + wagmi for Base.
- **Solana chain layer**: `@solana/web3.js` + `@sqds/multisig`.

## 5.2 Authentication & Wallet Architecture

Authentication is handled by **Privy**. A single Privy account derives **two embedded wallets** simultaneously:

- One **EVM wallet** for USDC payment on Base.
- One **Solana wallet** for receiving future airdrops or interacting with on-chain primitives in V1.5+.

Users perceive a single account; the underlying dual-chain infrastructure is invisible to them.

Email, Google, and Apple sign-in are all supported. **There is no requirement to install a wallet extension, manage seed phrases, or hold any cryptocurrency before entering the product.**

## 5.3 AI Provider Routing

Image generation requests are routed through a unified gateway that selects the appropriate provider based on the requested tier. This abstraction allows Aivive to swap providers without exposing implementation churn to the user, and to take advantage of pricing competition across providers.

- Latency-critical tiers (Standard) → fal.ai for cold-start performance.
- Quality-critical tiers (Ultra) → OpenAI directly.

## 5.4 Content Moderation

A two-layer moderation pipeline runs on every generation:

1. **Text moderation** — OpenAI Moderation API on the input prompt and the generated image caption.
2. **Image moderation** — fal's `nsfw-image-detection` model on the produced image.

Generations flagged as borderline are routed to a human review queue with a **24-hour SLA**. Generations confirmed as policy-violating are auto-hidden and the user receives a strike. **Three strikes result in account suspension.**

A prompt blocklist (politicians, named celebrities, minors, violence, hate) is enforced via both substring and embedding-similarity matching.

## 5.5 The Credit Ledger

The platform's source of truth for usage is an **append-only, double-entry credit ledger** in Postgres. Every credit issuance, deduction, and refund is recorded as an immutable row. Reading a user's current balance is a single indexed query against the latest entry.

The schema (Drizzle ORM, simplified):

```typescript
export const creditLedger = pgTable("credit_ledger", {
  id: bigserial("id", { mode: "bigint" }).primaryKey(),
  userId: uuid("user_id").notNull().references(() => users.id),
  delta: integer("delta").notNull(),            // +/-, immutable
  reason: text("reason").notNull(),             // 'signup_bonus' | 'topup_usdc' | 'gen_image_premium' | 'refund' | ...
  referenceId: text("reference_id"),            // topup -> base_tx_hash, gen -> generation_id
  balanceAfter: integer("balance_after").notNull(),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});

// Read current balance: O(log N) lookup against (user_id, id desc) index.
async function getBalance(userId: string): Promise<number> {
  const row = await db.select({ b: creditLedger.balanceAfter })
    .from(creditLedger)
    .where(eq(creditLedger.userId, userId))
    .orderBy(desc(creditLedger.id))
    .limit(1);
  return row[0]?.b ?? 0;
}
```

The ledger is **anchored to the on-chain economic loop** through the topup intent system: every USDC payment received on Base is reconciled to a ledger entry within seconds via Alchemy Webhooks.

```typescript
// Webhook handler — Alchemy notifies us of USDC.transfer to Treasury
export async function POST(req: Request) {
  const event = await verifyAlchemySignature(req);
  if (event.eventType !== "ADDRESS_ACTIVITY") return ok();

  for (const tx of event.activity) {
    if (tx.toAddress !== TREASURY_BASE) continue;
    if (tx.asset !== "USDC") continue;

    const intent = await matchTopupIntent(tx.fromAddress, tx.value);
    if (!intent) continue;

    await db.transaction(async (trx) => {
      const balance = await getBalance(intent.userId, trx);
      await trx.insert(creditLedger).values({
        userId: intent.userId,
        delta: intent.creditsToBuy,
        reason: "topup_usdc",
        referenceId: tx.hash,
        balanceAfter: balance + intent.creditsToBuy,
      });
      await trx.update(topupIntents)
        .set({ status: "paid", baseTxHash: tx.hash })
        .where(eq(topupIntents.id, intent.id));
    });

    await realtimePublish(`user:${intent.userId}:credits`, "updated");
  }

  return ok();
}
```

## 5.6 Observability

Every layer of the system is instrumented:

- **Sentry** captures application errors.
- **PostHog** captures product analytics.
- **Vercel Analytics** captures performance telemetry.
- The **cross-chain burn pipeline** is monitored independently, with alerting on any cycle that fails to complete within its SLA.

Public-facing metrics — total burns, weekly burn velocity, *AVV* in circulation — are exposed at `aivive.ai/burn` and on the project's Dune dashboard.

---

[← aivive.ai](04-product.md) · [Tokenomics →](06-tokenomics.md)
