# Part 4 — Growth Analytics, Funnel Diagnostic & Responsible-AI Rollout

[← Previous: Pricing & GTM](./03-pricing-and-gtm.md) · [Back to README →](../README.md)

---

## 1. Funnel Conversion & Drop-off

| Transition | Converted | % Converted | % Dropped | Absolute users lost |
|---|---|---|---|---|
| 1→2 (Banner shown → Feature opened) | 20,000 / 50,000 | 40% | 60% | **30,000** |
| 2→3 (Feature opened → Preferences submitted) | 14,000 / 20,000 | 70% | 30% | 6,000 |
| 3→4 (Preferences submitted → Itinerary viewed) | 11,900 / 14,000 | 85% | 15% | 2,100 |
| 4→5 (Itinerary viewed → Booking/save taken) | 3,570 / 11,900 | 30% | **70%** | 8,330 |

**Largest absolute loss:** Stage 1→2 (30,000 users). **Largest percentage drop-off:** Stage 4→5 (70%).

**Why Stage 4→5 is the higher near-term priority:** Stage 1→2's loss is a top-of-funnel awareness problem that, even if fully fixed, only feeds more users into a funnel that is already losing 70% of people at the last, revenue-driving step. Fixing Stage 4→5 directly recovers bookings/saves from users who have already invested effort through preference submission and itinerary viewing — the same acquisition spend converts to far more revenue, whereas fixing top-of-funnel alone guarantees nothing downstream if the itinerary-to-booking trust gap remains unresolved.

## 2. Growth Loop — AI Product Loop

A user generates a personalised itinerary, then exports/shares it as a link with travel companions over WhatsApp to coordinate the trip. Recipients open the shared itinerary, land on a branded Trip Assistant page showing that plan, and are prompted to "customize this trip for yourself" — converting them into new signed-in users who generate and share their own itinerary, repeating the cycle.

## 3. Product Lifecycle Mapping

The Trip Assistant is currently in the **Introduction** stage (a working prototype has just completed a small, month-long pilot; not yet broadly launched).

- **Next — Growth:** Scale acquisition through the growth loop above while fixing the Stage 4→5 funnel drop-off.
- **Following — Maturity:** Shift focus to retention and monetization — improving paid-tier conversion and reducing churn rather than chasing new users.

## 4. North Star & Guardrail Metrics (funnel-connected)

**North Star:** % of users who complete Stage 5 (booking/save) within 24 hours of viewing their Stage 4 itinerary = (3,570 ÷ 11,900) × 100 = **30%** at pilot baseline; target is to raise this above 30% post-launch.

**Guardrail:** Stage 4→5 drop-off rate = ((11,900 − 3,570) ÷ 11,900) × 100 = **70%** at baseline — must not increase as bias-mitigation and pricing changes roll out; any post-launch reading above 70% triggers review.

## 5. Post-Launch Responsible-AI Monitoring Plan

**Govern:** Any change to ranking/prompt logic requires sign-off from the PM and a designated Responsible-AI reviewer; a monthly review board (PM, ML engineer, privacy lead) owns the plan.

**Map:** Risks in scope — the four named biases (popularity, price, geographical, review), hallucinated catalogue entries, prompt injection, and PII exposure.

**Measure:** The 8-input golden dataset is re-run **bi-weekly** against the live app, alongside a weekly diversity-of-recommendations metric on real traffic.

**Manage — walkthrough for popularity bias resurfacing after launch:**
- **Detect:** Weekly diversity-of-recommendations metric (unique destinations ÷ total sessions) drops below a 30% threshold, indicating the rotation cap has stopped working.
- **Contain:** New itinerary generations are auto-flagged in `Itinerary_Log` (`Review Status = "Flagged"`), pausing automatic release of explanations until a human clears them — reusing the same human-review checkpoint from Part 2.
- **Fix:** PM and ML engineer re-weight the ranking logic (strengthen the offbeat-rotation cap) and re-run the golden dataset to confirm the diversity metric recovers above 30% before re-enabling automatic release.
- **Communicate:** The incident, root cause, and fix are logged against the affected `Itinerary_Log` records, with a short internal postmortem sent to leadership.

---

[← Previous: Pricing & GTM](./03-pricing-and-gtm.md) · [Back to README →](../README.md)
