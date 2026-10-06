# Part 3 — Pricing, Positioning & Go-To-Market

[← Previous: Prototype, Build & Evals](./02-prototype-and-evals.md) · [Next: Growth & Responsible AI →](./04-growth-and-responsible-ai.md)

---

## 1. Pricing & Packaging

**Model: Good-Better-Best tiers**, justified against the Part 1 personas: Anjali is price-sensitive but will pay for genuinely differentiated trust/transparency; Rajeev will pay to save time and guarantee budget compliance rather than escalate to a travel agent.

| Tier | Price | Includes |
|---|---|---|
| **Free** | ₹0/month | 3 itineraries/month, no follow-up refinement |
| **Explorer** | ₹149/month | Unlimited itineraries, conversational follow-up refinement, hard budget-lock guarantee |
| **Concierge** | ₹399/month | Everything in Explorer + live price comparison, group-trip splitting, kid-friendly filter, priority human-reviewed explanations |

**Psychological pricing tactics:**
- **Decoy tier** — "Explorer Plus" at ₹349/month (near-identical to Explorer, slightly higher cap) makes Concierge at ₹399 look like the obviously better deal for only ₹50 more.
- **Price anchoring** — the pricing page shows "vs. ₹1,500+ for a human travel agent consultation" next to Concierge.
- **No free trial** on paid tiers — the Free tier already serves as an ungated trial, so a separate time-limited trial would be redundant and could cannibalize Free-to-paid conversion.

## 2. GTM Framework

**User Segment:** Primary launch segment is Anjali-like urban, 25–34, digitally-native travellers who currently abandon MakeMyTrip's planner for blogs/Reddit due to poor personalization trust — a large, high-intent segment already inside the app.

**Value Positioning:** The only trip planner that shows *why* it recommended something and structurally guarantees it will honour the stated budget and offbeat preference — directly answering Anjali's distrust and Rajeev's budget-violation frustration.

**Pricing & Packaging:** Free / Explorer (₹149) / Concierge (₹399) ladder with a decoy tier, anchored against the cost of a human travel agent.

**Channel Strategy:** Launch via an in-app entry banner to existing MakeMyTrip users, supplemented by a growth loop driving shared-itinerary referrals rather than paid external acquisition.

**Scale & Measurement:** Track North Star and guardrail metrics weekly per cohort, re-run the golden-dataset evals bi-weekly, and gate any pricing-tier or prompt-logic change behind the Responsible-AI Govern/Map/Measure/Manage review (see Part 4).

## 3. Value Proposition Canvas — Anjali Mehta (primary persona)

| Customer side | Product side |
|---|---|
| **Job:** Plan an offbeat trip that fits her budget | **Pain Reliever:** Hard pre-filter removes over-budget catalogue items before the model ever ranks them |
| **Job:** Discover authentic, non-touristy spots | **Pain Reliever:** Geographic-cluster diversity requirement in the ranking prompt |
| **Job:** Quickly validate a trip plan with her partner | **Gain Creator:** One-tap save/export of the itinerary for sharing over WhatsApp |
| **Pain:** Recommendations always show the same popular, expensive cities | **Pain Reliever:** Popularity-bias rotation cap on top-5-catalogued destinations |
| **Pain:** No explanation for why a place was suggested | **Gain Creator:** One-line "why this itinerary" explanation shown with every result |
| **Pain:** Distrusts the algorithm, does manual research instead | **Gain Creator:** Human-reviewed explanation checkpoint before anything reaches her |

## 4. Price-Quality Positioning

Plotted against **Goibibo's AI planner** (direct competitor — mid price, mid perceived quality due to the same popularity-skewed logic) and **a generic LLM chatbot** (indirect competitor — free, but low reliability for trip-specific budget/catalogue accuracy): the Trip Assistant is positioned at **mid price, high perceived quality** — priced above the free generic chatbot but justified by catalogue-verified accuracy, hard budget guarantees, and visible reasoning neither competitor offers.

**Positioning statement:** The Trip Assistant is not the cheapest way to get a trip plan — it is the only one that combines a verified hotel/activity catalogue, a guaranteed budget lock, and a visible explanation for every recommendation, so travellers can trust the plan enough to act on it immediately.

## 5. Bottom-Up First-Year Revenue Estimate

| Assumption | Value |
|---|---|
| Monthly active users (MAU) | 100,000 |
| Conversion rate to a paid tier | 4% |
| Paid users/month | 4,000 |
| Blended ARPU (70% Explorer / 30% Concierge) | ₹225/month |
| Monthly revenue | ₹9,00,000 |
| **Year 1 total** | **≈ ₹1.08 crore** |

---

[← Previous: Prototype, Build & Evals](./02-prototype-and-evals.md) · [Next: Growth & Responsible AI →](./04-growth-and-responsible-ai.md)
