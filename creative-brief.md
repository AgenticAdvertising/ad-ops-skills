---
name: creative-brief
description: "Extracts winning angles from the account's top performers and produces creative concepts (visual format, mood, key elements, on‑image text) to hand to a designer or image generator. The missing upstream of copy‑rotation — closes the loop from fatigue detection to new creative."
metadata:
  version: 1.0.0
---

## 1. Purpose

`fatigue-detector` says *which* creative is dying. `copy-rotation` writes *what the words say*. This skill answers the step in between: **"what should the next creative look like?"** — the visual concept, format, and angle, mined from the account's own winners.

Run when:

- `fatigue-detector` or `bleeders-winners` flags a creative for pause — **before** `copy-rotation` generates copy.
- Launching a new ad set and you want 3–5 fresh concepts, not a re‑skin of the last winner.
- `audience-health` recommends refreshing an audience — new audience needs matching fresh angles.

## 2. Inputs

Pull before briefing:

- **Top performers (last 90 days)** — top 10 ads by spend‑weighted ROAS + top 5 by CTR.
- **What's being replaced** — the paused creative(s) from `fatigue-detector`/`bleeders-winners`, so the brief avoids repeating the fatigued angle.
- **Funnel stage** of the destination campaign (ToFu / MoFu / BoFu).
- **Brand voice & forbidden vocabulary** (provided by the platform's BrandKit).

## 3. Winning Angle Extraction

For each top performer, tag four axes:

| Axis                    | What to capture                                     | Examples                                                                                   |
| :---------------------- | :-------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| **Hook type**           | How the ad grabs attention                          | Question · Stat · Story · Testimonial · Problem statement · Command                        |
| **Visual format**       | Structural pattern                                  | Notes‑app · Tweet · Receipt · Chart · UGC selfie · Product shot · Text‑on‑photo · Before/after |
| **Mood**                | Emotional register                                  | Urgent · Organic · Professional · Casual · Flashy · Understated                            |
| **Angle / Horseman**    | Psychological lever (see [copy-rotation](copy-rotation.md)) | 💰 Money · ⏱ Time · 👑 Status · ⚠️ Fear                                                     |

Cluster winners by **(format, angle)** pairs. A pair that appears **3+ times** in the top performers is a **proven pattern** — brief variants of it before experimenting with net‑new formats.

## 4. Brief Structure

Every concept produced is a one‑page brief with these fields:

| Field                    | Purpose                                                                    |
| :----------------------- | :------------------------------------------------------------------------- |
| **Concept name**         | Short label (e.g. `Notes App — CFO Rant v4`)                               |
| **Replaces**             | ID of the paused creative (if applicable)                                  |
| **Funnel stage**         | ToFu / MoFu / BoFu                                                         |
| **Angle / Horseman**     | One of the four Horsemen                                                   |
| **Visual format**        | Specific, imitable pattern                                                 |
| **Mood**                 | Emotional register                                                         |
| **Key visual elements**  | 3–5 concrete things that must appear on screen                             |
| **On‑image text**        | Short on‑creative text (the primary ad copy comes from `copy-rotation`)    |
| **Reference winners**    | 1–3 real account top performers this concept draws from                    |
| **Avoid**                | Elements from the paused creative that likely drove fatigue                |
| **Success criteria**     | CTR and frequency targets taken from the referenced winners                |

## 5. Generation Rules

- **Produce 3 concepts per brief** — not 1 (risk) or 10 (dilutes test signal).
- **Each concept hits a different Horseman** — same rule as `copy-rotation`.
- **At least one must imitate a proven pattern**; at most one can be a net‑new format.
- **Never repeat the exact format of the creative being replaced** — that's the pattern the audience has gone blind to.
- **Cite specific account winners** — every concept references 1–3 real top performers. No generic *"use a testimonial"* briefs.

## 6. Operational Output: Creative Brief Pack

> **Creative Brief Pack — `Summer Launch — Traffic` · 3 concepts · Mon, Apr 22**
>
> **Context:** Replacing `AD_238` (Notes App Hero) — paused by `fatigue-detector` at 34% CTR drop. Funnel: **MoFu**. Top account patterns: `Notes‑app + Time` (6 wins) · `UGC‑Selfie + Status` (4 wins) · `Chart + Money` (3 wins).
>
> **Concept 1 — `CFO Rant v4`** *(imitates proven pattern)*
>
> - **Horseman:** ⏱ Time
> - **Visual format:** Notes‑app screenshot, iPhone frame, handwritten‑style headline
> - **Mood:** Organic, frustrated, relatable
> - **Key elements:** 5 bullets of tasks · 3rd bullet crossed out · today's date in header · battery icon at 12%
> - **On‑image text:** *"Things my agency does that my CFO hates"*
> - **Reference winners:** `AD_112` · `AD_155` · `AD_201` (same format, avg CTR 3.4%)
> - **Avoid:** Red accent from `AD_238`; the verb *"Stop"* (audience is blind to it)
> - **Target:** CTR ≥ 2.8% · freq < 2.0 at day 14
>
> **Concept 2 — `Founder Testimonial — $40K`** *(proven pattern, different Horseman)*
>
> - **Horseman:** 💰 Money
> - **Visual format:** UGC selfie, shoulders‑up, kitchen background
> - **Mood:** Casual, understated
> - **Key elements:** Founder holding phone showing revenue dashboard · no filter · handheld feel
> - **On‑image text:** *"I hit $40K/mo using this"*
> - **Reference winners:** `AD_388` (Founder UGC Hook — 2.9% CTR, 17 conv in 7d)
> - **Avoid:** Polished lighting, corporate backdrop
> - **Target:** CTR ≥ 2.5% · 10+ conversions in first 7 days
>
> **Concept 3 — `Benchmark Chart`** *(net‑new format — test as A/B probe)*
>
> - **Horseman:** 👑 Status
> - **Visual format:** Horizontal bar chart, 3 bars, top 10% highlighted
> - **Mood:** Professional, confident
> - **Key elements:** "Top 10% of agencies" callout · red→green gradient · small logo bottom‑right
> - **On‑image text:** *"The top 10% of agencies report in minutes, not days."*
> - **Reference winners:** `AD_412` (Chart + Money — 2.1% CTR) — adjacent pattern
> - **Avoid:** > 5 data points (clutters on mobile)
> - **Target:** CTR ≥ 2.0% · A/B probe at 20% of ad‑set budget for 7 days
>
> **Next step:** pass each concept's Horseman + funnel stage into [copy-rotation](copy-rotation.md) to generate 3 headlines + 3 bodies per concept.
