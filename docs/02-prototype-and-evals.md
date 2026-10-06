# Part 2 — No-Code AI Prototype: Build, Evaluate & Automate

[← Previous: Research & PRD](./01-research-and-prd.md) · [Next: Pricing & GTM →](./03-pricing-and-gtm.md)

---

## 1. App Build

**Prompt structure used (CRISPE: Capacity, Request, Insight, Specifics, Personalization, Examples):**

> **Capacity:** Act as a senior conversational product prototyper building a travel-planning web app.
> **Request:** Build a single-page app called "Trip Assistant" that lets a signed-in user answer a short conversational form (destination, dates, budget in ₹, trip type: solo/couple/family/group, and an "offbeat vs popular" preference slider) and then generates a personalised day-by-day itinerary with hotel and activity suggestions drawn only from a small fixed catalogue, never invented.
> **Insight:** Users abandon trip planners that ignore their budget and always suggest the same popular destinations, and planners that don't respect a hard budget cap. The app must never suggest an item over the stated budget and must actively surface non-popular/offbeat catalogue entries when requested.
> **Specifics:** Require email/password sign-in before any itinerary can be generated. Persist every user's submitted preferences and generated itinerary in a database, keyed to their user ID, retrievable on next login. Show a one-line "why this itinerary" explanation under the result.
> **Personalization:** Tone of a helpful Indian travel-planning assistant — concise, warm, never salesy.
> **Examples:** Example input → "Destination: Spiti Valley, Budget: ₹45,000, Trip type: couple, Preference: offbeat" should produce a multi-day plan referencing only catalogue entries tagged "Spiti Valley/Himachal — offbeat," never substituting Manali/Goa entries.

**Build pipeline (no-code, free tier):**
1. Scaffold the app from the prompt above in a no-code AI builder.
2. Add email/password sign-in.
3. Wire a database to store `{userId, preferences, generatedItinerary, timestamp}`.
4. Deploy using the builder's free public-link option.
5. Verify persistence end-to-end: log in, submit a test trip, log out, log back in, confirm the same saved itinerary reappears.

**Persistence verification:** Confirmed by logging in twice with the same test account; the second session correctly displayed the itinerary saved from the first session, including the same hotel/activity picks and "why this itinerary" line.

## 2. Feature Classification

**AI-augmented vs. AI-native:** The Trip Assistant is **AI-native** — the core value (turning a few preference inputs into a personalised, explained day-by-day plan) does not exist without the AI layer; removing the LLM leaves only a static hotel-search form with no itinerary logic.

**4D framework stage:** Primarily **Draft** — the feature's main job is to generate a first-pass itinerary from user inputs, which the user then reviews/edits, rather than autonomously detecting a need or executing a booking.

**AI Feature Ladder level:** **L2** — the AI generates a complete, structured output (the itinerary) from user input, with human-in-the-loop review of the explanation before it's trusted. The app does not yet autonomously execute bookings (L3/L4), nor does it merely surface passive suggestions (L0/L1).

## 3. Automation — Trigger / Logic / Action

A human-reviewed explanation pipeline logs every AI-generated recommendation to an `Itinerary_Log` table before anything reaches the end user:

| Node | Type | Description |
|---|---|---|
| 1 | Trigger | Fires whenever the app generates a new itinerary. |
| 2 | Logic | Formats the raw itinerary into a short text summary. |
| 3 | Logic | Calls an LLM with the prompt: *"In one line, explain why this itinerary fits the stated preferences: {itinerary_summary} / {preferences}."* |
| 4 | Logic | Marks `Review Status = "Pending Review"` — the AI explanation is **not** shown to the user yet. |
| 5 | Action | Creates a new linked record in `Itinerary_Log` (Timestamp, Persona, Itinerary Summary, AI Explanation, Review Status). |
| 6 | Human Review Checkpoint | A reviewer reads the AI explanation and manually flips `Review Status` to "Approved" or "Rejected." Only "Approved" explanations are ever surfaced — a deliberate, non-automated checkpoint because an unreviewed AI explanation shown directly to a customer is a hard-to-reverse action. |
| 7 | Action | Only on "Approved," the explanation is released to the user-facing app screen. |

**Sample test-run transcript (3 runs, local LLM, no external API key):**

> - **Run 1** — Input: "5-day Spiti Valley trip, offbeat homestays, monastery visits, budget trekking / Destination: Spiti Valley, Budget: ₹45,000, Couple, Offbeat" → Output: *"This itinerary fits because it stays true to Spiti Valley, accommodates a ₹45,000 budget for a couple, and offers an offbeat experience through homestays, monastery visits, and budget trekking."* → **Approved**.
> - **Run 2** — Input: "4-day Goa trip, family-friendly beaches, water park, budget beach-resort / Destination: Goa, Budget: ₹60,000, Family with kids" → Output: *"This itinerary fits because it includes the chosen destination (Goa), stays within ₹60,000 for 4 days, and caters to a family with kid-friendly activities like beaches and a water park."* → **Approved**.
> - **Run 3** — Input: "3-day Jaipur trip, budget heritage hostel, free walking-tour attractions / Destination: Jaipur, Budget: ₹20,000, Solo, Budget" → Output: *"This itinerary fits because it stays within ₹20,000, is tailored for a solo traveler, and focuses on budget-friendly accommodation and free attractions."* → **Approved**.

## 4. Evaluation Suite

**Rubric criteria (1–5 scale):** Accuracy, Clarity, Completeness, Tone, Formatting.

**Binary assertion checks:**
1. Does the itinerary respect the stated budget?
2. Are all suggested dates within the stated travel window?
3. Are all suggested hotels/activities drawn from the fixed catalogue (no invented names)?
4. Is the itinerary appropriate for the stated trip type (solo/family/couple/group)?
5. Does the output include a one-line "why this itinerary" explanation?

**Golden dataset (8 inputs) — results:**

| # | Traveller type | Destination | Duration | Budget (₹) | Rubric avg (1–5) | Assertions passed |
|---|---|---|---|---|---|---|
| 1 | Solo | Jaipur (domestic) | 3 days | 20,000 | 4.4 | 5/5 |
| 2 | Couple | Spiti Valley (domestic) | 5 days | 45,000 | 4.6 | 5/5 |
| 3 | Family (2 kids) | Goa (domestic) | 4 days | 60,000 | 4.2 | 5/5 |
| 4 | Group of 4 | Bali (international) | 6 days | 1,50,000 | 4.0 | 4/5 |
| 5 | Solo | Bangkok (international) | 4 days | 55,000 | 4.3 | 5/5 |
| 6 | Couple | Rishikesh (domestic) | 3 days | 25,000 | 4.5 | 5/5 |
| 7 | Family (2 kids) | Singapore (international) | 5 days | 1,80,000 | 3.9 | 4/5 |
| 8 | Group of 5 | Andaman Islands (domestic) | 5 days | 90,000 | 4.1 | 5/5 |

**Result: 35/40 assertions passed.** The two partial misses (Bali, Singapore) were traced to catalogue gaps for those international destinations and logged as a follow-up catalogue-expansion task.

## 5. Responsible AI Design Pass

**Prompt injection mitigation:** All free-text user input is passed to the LLM only inside a clearly delimited "user preferences" field, with an explicit system instruction that any instruction-like text inside that field must be treated as data, not as a command.

**Hallucination mitigation:** The app recommends only from a small fixed catalogue table (hotels/activities tagged by destination); the LLM selects and re-words catalogue entries, never invents a hotel or activity name, and any suggestion not matched to a catalogue ID is discarded before display.

**PII handling / Privacy-by-Design:**
- Email/password stored via hashed-auth, never in plaintext.
- Trip dates and destination scoped to the user's own record only (no cross-user query access).
- Budget figure stored but never surfaced to any third party or used outside itinerary generation for that user.

**Bias mitigations (one mechanism per bias):**
- **Popularity bias** — rolling 7-day cap on how many times the same top-5-most-catalogued destination can appear, forcing rotation toward less-catalogued entries when "offbeat" is selected.
- **Price bias** — a hard filter removes any catalogue item priced above the stated budget *before* the LLM ever sees the candidate list, structurally excluding expensive items rather than merely de-prioritizing them.
- **Geographical bias** — the catalogue is tagged by region, and the prompt instructs the model to draw from at least 2 different geographic clusters when "offbeat" is selected.
- **Review bias** — catalogue entries are ranked using a blended score (relevance-to-preference weighted 2x, review-count weighted only 0.5x) rather than review-count/rating alone.

---

[← Previous: Research & PRD](./01-research-and-prd.md) · [Next: Pricing & GTM →](./03-pricing-and-gtm.md)
