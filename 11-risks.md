# 11. Risk Disclosures

The following risks are material and should be considered by any token holder, exchange listing reviewer, or potential partner.

## 11.1 Smart Contract Risk

Despite CertiK audit, smart contract code carries residual risk. A vulnerability could result in token supply manipulation or treasury loss. Multi-sig and limited mint authority mitigate but do not eliminate this risk.

## 11.2 Cross-Chain Bridge Risk

While CCTP is widely regarded as the safest USDC cross-chain primitive, it is a relatively new system and depends on Circle's attestation infrastructure remaining operational. **A prolonged Circle outage would interrupt the burn cycle** (though not user payments).

## 11.3 AI Provider Dependency

The product depends on third-party AI generation APIs (fal.ai, OpenAI). A pricing change, content policy shift, or service interruption from a major provider could affect product economics or user experience. **Routing across multiple providers mitigates but does not eliminate this risk.**

## 11.4 Liquidity Risk

*AVV* will launch with bootstrap liquidity on Solana DEXs. Insufficient liquidity depth could cause swap execution to suffer high slippage during burn cycles. **The Liquidity allocation (18%) is structured to mitigate this risk at launch.**

## 11.5 Regulatory Risk

Token-based products operate in evolving regulatory environments. A jurisdictional ruling that classifies *AVV* as a security in any major market could impact access. The project intends to monitor regulatory developments and adapt where required.

## 11.6 Roadmap Execution Risk

The 60-day production timeline is ambitious. While progress is on track, circumstances could delay specific milestones. **The project commits to transparent communication of any timeline adjustments.**

## 11.7 Forward-Looking Statements

This document contains statements about future plans, capabilities, and market conditions. Such statements reflect current intentions but are **not guarantees**. Token holders should evaluate based on shipped product, audited contracts, and observable on-chain activity rather than forward statements alone.

---

[← Audits & Compliance](10-audits.md) · [Resources →](12-resources.md)
