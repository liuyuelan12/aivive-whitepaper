# 4. aivive.ai: The Flagship Application

## 4.1 Form Factor

**aivive.ai is the flagship AI image generation feed running on Aivive.** Civitai meets Pinterest, built for the era when *AI artist* stops being a contradiction.

The choice of a feed — rather than a chatbox, a canvas, or a notebook — is intentional. Of the four interfaces that consumer AI products in 2026 converge to, the feed is the most underbuilt and the most culturally important. Feeds are how taste forms. Search is how memory rots. The feed is the surface where a creative community actually accumulates.

## 4.2 Core Product Surfaces

aivive.ai consists of five primary surfaces:

| Surface | Purpose |
|---|---|
| `/feed` | The discovery surface. Trending / Following / New columns of generated visual content, ranked by community engagement and recency. Infinite scroll, optimized for taste, not search. |
| `/studio` | The generation surface. Prompt input, style preset selection, aspect ratio, and three-tier model selection (Standard / HD / Ultra). |
| `/p/[postId]` | The post detail surface. Full-size image, original prompt, creator card, one-click *Remix*, save, and comment. |
| `/u/[handle]` | The creator surface. A public profile backed by the actual creative footprint: prompts shipped, posts liked, followers earned. |
| `/me/wallet` | The account surface. Credits balance, USDC balance, top-up flow, and a transparent ledger of every credit transaction. |

## 4.3 Two User Journeys

The product is designed around two complementary loops.

### The Creator Loop

A new user signs up by email, receives an embedded wallet automatically, claims 100 free generation credits, generates their first image, publishes it to the feed, and gradually builds a profile through the work they ship. When credits run low, they top up using USDC. The economic loop runs invisibly in the background.

### The Viewer Loop

A user signs up to browse, follows creators whose taste they admire, saves favorite work, and — critically — clicks *Remix* on any image to pre-populate the *Studio* with the originating prompt. In a single click, every viewer is one step away from becoming a creator.

## 4.4 The Three Generation Tiers

Aivive routes generation requests to one of three tiers:

| Tier | Underlying Model | Cost | Credits |
|---|---|---|---|
| **Standard** | FLUX.1 Schnell (via fal.ai) | $0.003 | 1 credit |
| **HD** | FLUX.1 Dev (via fal.ai) | $0.025 | 8 credits |
| **Ultra** | OpenAI gpt-image-2 / Imagen 4 Ultra | $0.04+ | 15 credits |

Standard is the daily driver. Ultra is the surface that produces the work most likely to anchor reputation and stand the test of a curated feed. The HD tier sits in between, providing room for experimentation without committing to Ultra-tier cost.

## 4.5 Why a Feed (Not a Chatbox)

The decision to build a feed rather than a chatbox is the most consequential product decision in the project. Most current AI products converge on the chatbox because chatboxes are easy to build and easy to demo. They are also where taste goes to die.

A feed forces three things that a chatbox cannot:

1. **A persistent record of what you've made.** Your work accumulates. Reputation becomes possible.
2. **Discovery of others' taste.** You cannot discover the work of strangers in a chatbox. You must be in a shared visual space to see what someone else considered worth shipping.
3. **One-click cultural transmission.** Remix only works when there is a feed to remix from. The whole notion of *building on someone else's creative foundation* requires the feed as substrate.

aivive.ai is, in essence, a bet that the next wave of AI consumer products will be measured not by their generation quality (which is a solved problem) but by their cultural infrastructure (which is largely missing).

---

[← The Aivive Network](03-network.md) · [Architecture →](05-architecture.md)
