# 10. Audits & Compliance

## 10.1 Smart Contract Audit

The *AVV* SPL token contract and any associated treasury programs will be audited by **CertiK** prior to mainnet deployment.

Final audit reports will be published at `aivive.ai/docs/audits` and referenced in this whitepaper post-completion.

## 10.2 Multi-Signature Governance

Both treasuries operate under multi-signature control:

| Chain | Multisig Type | Threshold | Purpose |
|---|---|---|---|
| **Base mainnet** | Safe (Gnosis Safe) | 2-of-3 | USDC inflow + CCTP burn authorization |
| **Solana mainnet** | Squads Protocol | 2-of-3 | Burn execution + Jupiter swap authorization |

Signatories are aligned across both chains. **One signatory seat in each multisig is reserved for an automated service signer** (which can co-sign threshold-bound, pre-authorized burn transactions); the remaining seats are held by humans.

Any non-routine treasury action — including discretionary allocation, migration, or emergency intervention — **requires human consensus**.

## 10.3 Content Moderation Compliance

A two-layer content moderation pipeline (OpenAI Moderation API and fal NSFW classification) operates on every generated image.

A prompt blocklist enforces restrictions on:

- Politicians
- Named celebrities (real-face deepfake protection)
- Minors
- Violence
- Hate-related content

A **3-strike enforcement model** leads to account suspension on confirmed policy violation.

## 10.4 Withdrawal Integrity

The platform's primary operational commitment to its users and to its exchange listing partners is **zero unresolved withdrawal complaints**.

Top-up flows include:

- Automatic refund handling for failed or excess payments
- A **24-hour customer support SLA**
- A transparent reconciliation flow at `/me/wallet/history`

---

[← Team](09-team.md) · [Risk Disclosures →](11-risks.md)
