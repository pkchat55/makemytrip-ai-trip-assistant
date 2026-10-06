# Part 1 — Research, Personas & Feature PRD

[← Back to README](../README.md) · [Next: Prototype, Build & Evals →](./02-prototype-and-evals.md)

---

## 1. Proto-Personas

### Persona 1 (Primary) — Anjali Mehta, "The Offbeat Explorer"

**Background:** 27-year-old marketing executive based in Bengaluru. Travels 4x/year, usually with her partner. Budget-conscious (~₹40,000–₹60,000 per trip) but values unique, less-touristy experiences over luxury or brand-name resorts.

**Goals:** Discover authentic, offbeat destinations that fit her budget; minimize time spent planning; trust that recommendations reflect what she actually asked for.

**Behaviors:** Cross-checks 4–5 apps and travel blogs before booking anything. Uses Instagram and Reddit for inspiration. Screenshots blog itineraries instead of saving in-app suggestions.

**Pain Points:** MakeMyTrip's recommendation logic keeps surfacing the same popular, expensive, heavily-reviewed cities (Goa, Manali) even when she explicitly types an offbeat destination like Spiti Valley.

**Needs:** A personalized itinerary that actually reflects her stated destination/budget, with visible reasoning for why something was suggested.

**Quote:** *"I typed 'Spiti Valley' and it gave me Manali five-star resorts again."*

**Empathy Map**
- **Think:** "Why does it keep pushing Goa/Manali no matter what I type?"
- **Feel:** Frustrated, distrustful of the app's "personalization" claim.
- **Say:** "This app doesn't listen to me."
- **Do:** Closes the app and manually searches travel blogs/Reddit for offbeat itinerary ideas instead of using in-app suggestions.

**AEIOU**
- **Activities:** Browsing itinerary options, comparing prices across many tabs.
- **Environment:** Mobile app, often used during commute with patchy network.
- **Interactions:** Chat-style search box, budget/date filters.
- **Objects:** Phone, a personal wishlist note, screenshots of blog itineraries.
- **Users:** Herself and her partner, who co-decides remotely over WhatsApp.

**Contradiction / Workaround:** Anjali says she wants "personalized recommendations" (expressed need), but her actual behavior is to abandon the app's personalization feature entirely and take screenshots of external blog itineraries instead. This reveals she doesn't trust the app's reasoning — the workaround exposes a need the app currently fails to meet.

**Segmentation tags:** Psychographic (experience-seeker, low trust in mainstream recommendations), Behavioral (comparison shopper, low brand loyalty).

**Latent need (distinct from expressed need):** Her expressed need is "cheaper, offbeat destination options." Her **latent need** is *transparency/explainability* — she needs to see *why* a place was recommended, not just get a different recommendation.

---

### Persona 2 — Rajeev Nair, "The Time-Starved Family Planner"

**Background:** 39-year-old IT manager in Pune, married with two kids (ages 6 and 9). Plans 1–2 family trips a year with roughly two hours a week to spare on planning.

**Goals:** Get a ready-made, kid-friendly itinerary fast, without juggling ten browser tabs, and stay within a hard budget cap.

**Behaviors:** Plans late at night after the kids are asleep; sets a strict budget filter; frequently abandons the flow midway and texts a travel-agent friend instead.

**Pain Points:** Existing recommendation engine keeps surfacing resort packages above his stated budget cap; too many steps before an itinerary even appears.

**Needs:** A quick, hard-budget-capped, family-safe itinerary in one pass.

**Quote:** *"I don't have time to fight with filters — just tell me a plan for ₹60,000, kids included."*

**Empathy Map**
- **Think:** "Why do I need to re-enter the budget three times and it still shows me options above it?"
- **Feel:** Anxious about overspending, guilty about limited planning time with family.
- **Say:** "Just give me the plan already."
- **Do:** Abandons the in-app flow mid-way and texts a travel-agent friend on WhatsApp for a manual itinerary instead.

**AEIOU**
- **Activities:** Short, interrupted late-night planning bursts.
- **Environment:** Home, mobile, low patience window between kid interruptions.
- **Interactions:** Multi-step form, budget slider.
- **Objects:** Shared family calendar, a saved-trips folder.
- **Users:** His wife co-approves the plan via a shared link.

**Contradiction / Workaround:** Expressed need = "a faster itinerary tool." Actual behavior = he abandons the *faster* in-app tool and reverts to a *slower* manual channel (a friend) — because he doesn't trust the tool to actually respect his budget. This is the same workaround pattern surfacing a different need.

**Segmentation tags:** Demographic (parent, 35–44), Need-based (convenience/time-saving).

**Latent need (distinct from expressed need):** Expressed need is "faster planning." Latent need is *guaranteed hard-budget compliance* — speed alone won't stop him from abandoning the tool if it still shows him over-budget options.

---

## 2. Competitive Teardown

**Direct competitor:** Goibibo's in-app AI trip planner (same product form — an OTA app with an AI itinerary feature and integrated booking).

**Indirect competitor:** A general-purpose LLM chatbot (e.g., ChatGPT used ad hoc for trip planning) — it solves the same underlying job ("help me plan and book a trip I'll be happy with") through a completely different product form (no dedicated catalogue, no booking rails, no persistent trip database).

**Feature Comparison Table**

| # | Feature | Classification |
|---|---------|----------------|
| 1 | Date/budget input form | Table stakes |
| 2 | Hotel search & booking | Table stakes |
| 3 | AI-generated day-by-day itinerary | Differentiator |
| 4 | Conversational follow-up refinement | Differentiator |
| 5 | Live price comparison across providers | Differentiator |
| 6 | Offbeat/personalized destination discovery beyond top-10 cities | Delighter |
| 7 | Group-trip cost splitting | Delighter |
| 8 | Explainable "why this suggestion" reasoning shown to the user | Delighter |

**SWOT (MakeMyTrip's current position in AI-assisted trip planning):** MakeMyTrip's **strength** is its large first-party catalogue and existing booking/payment rails, meaning any AI feature it ships can go straight to transaction rather than just inspiration. Its core **weakness** is a recommendation engine that systematically favours popular, expensive, heavily-reviewed, and geographically concentrated listings, which directly undermines trust for users like Anjali. The **opportunity** is to differentiate on trustworthy, explainable, budget-honouring personalization — a gap neither Goibibo nor generic LLM chatbots currently close well. The **threat** is that if MakeMyTrip does not fix the underlying bias, users will increasingly default to free general-purpose LLM chatbots for the planning step and only return to MakeMyTrip for the booking step, eroding the platform's role as the trusted starting point of the journey.

## 3. Candidate Feature Ideas — RICE Scoring

RICE Score = (Reach × Impact × Confidence) ÷ Effort

| Idea | Reach (users/qtr) | Impact (0.25–3) | Confidence | Effort (person-months) | RICE Arithmetic | RICE Score | Target Persona |
|---|---|---|---|---|---|---|---|
| A. Text-only itinerary generator | 7,000 | 1 | 0.8 | 2 | (7000×1×0.8)/2 | **2,800** | Rajeev |
| B. Conversational assistant with follow-up questions | 6,000 | 2 | 0.8 | 3 | (6000×2×0.8)/3 | **3,200** | Anjali |
| C. Itinerary generator + live price comparison | 5,000 | 1 | 0.5 | 4 | (5000×1×0.5)/4 | **625** | Rajeev |
| D. Itinerary generator + group-trip splitting | 2,000 | 0.5 | 0.5 | 3 | (2000×0.5×0.5)/3 | **166.7** | Anjali |

**Selected feature (highest-scoring, most build-appropriate): Idea B — "Conversational Trip Assistant with Follow-Up Clarifying Questions and Explainable Itinerary Recommendations."** This is the feature every remaining part of this project is built around.

## 4. Backlog Tracking (Airtable)

Personas and the opportunity backlog above are tracked in a linked two-table Airtable base (`Personas`, `Opportunity_Backlog`), with each backlog idea linked to its target persona record and a `Decision` column marking the selected idea. A third table, `Itinerary_Log`, is added in Part 2 to support the human-review automation pipeline.

## 5. PRD-Lite — Conversational Trip Assistant

**(a) Problem statement:** Travellers like Anjali Mehta ask MakeMyTrip's planning tools for offbeat, budget-fitting destinations but are repeatedly shown the same popular, expensive, heavily-reviewed cities, so they abandon the tool and plan manually via blogs and forums — eroding trust in MakeMyTrip as a planning starting point.

**(b) Solution + rejected alternative:** Chosen solution: a conversational AI Trip Assistant that asks clarifying follow-up questions (destination type, budget, offbeat preference, travel companions) and shows a one-line explanation for each recommendation. Rejected alternative: Idea A, a text-only one-shot itinerary generator — rejected because it cannot ask clarifying questions or explain its reasoning, so it does not address the latent transparency need uncovered in Anjali's empathy map.

**(c) User stories (checked against INVEST):**
1. *As Anjali, I want to ask follow-up questions and see why a place was suggested, so that I can trust the itinerary matches my preferences.* — Independent, Negotiable, Valuable (fixes her trust gap), Estimable, Small, Testable (explanation text presence is checkable).
2. *As Rajeev, I want to set a hard budget cap upfront, so that every suggested item stays within it.* — Independent, Negotiable, Valuable (removes his #1 pain point), Estimable, Small, Testable (assert no item exceeds cap).
3. *As Anjali, I want the assistant to prioritize offbeat destinations when I say so, so that I'm not shown the same popular cities repeatedly.* — Independent, Negotiable, Valuable, Estimable, Small, Testable (diversity-of-destination check).
4. *As Rajeev, I want to mark a trip "kid-friendly," so that suggested activities are age-appropriate.* — Independent, Negotiable, Valuable, Estimable, Small, Testable (assert flagged activities are age-tagged appropriately).
5. *As either persona, I want to save/export the generated itinerary, so that I can share it with a travel companion for approval.* — Independent, Negotiable, Valuable, Estimable, Small, Testable (assert export produces a retrievable record).

**(d) Metrics:**
- **North Star (primary):** % of Trip Assistant users who save or start booking at least one recommended itinerary item within 24 hours of receiving it = (users who save/book ≥1 item within 24h ÷ total users who received a completed itinerary) × 100.
- **Guardrail:** Budget-violation rate = (sessions where any recommended item's price exceeds the stated budget by more than 10% ÷ total sessions) × 100 — must stay below 5%.

**(e) Edge cases:**
- A user requests a destination with fewer than 5 catalogued hotels → the assistant explicitly states the catalogue is limited for that destination, offers the nearest well-catalogued alternative in the same region, and never invents a hotel name.
- A user abandons the conversation mid-way through providing preferences → the assistant saves the partial preferences as a draft, sends one gentle reminder, and auto-expires the draft after 7 days with no further prompts.

**(f) Out of scope:** Real-time flight booking and live payment processing are out of scope for this version of the feature.

---

[← Back to README](../README.md) · [Next: Prototype, Build & Evals →](./02-prototype-and-evals.md)
