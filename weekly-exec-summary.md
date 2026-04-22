---
name: weekly-exec-summary
description: "Monday client‑meeting roll‑up — narrative synthesis of past‑7days performance, budget‑shifter moves, anomaly‑watch incidents, bleeders‑winners pauses, and audience‑health shifts, written for a stakeholder (not the operator). Agency‑facing counterpart to daily‑brief."
metadata:
  version: 1.0.0
---

## 1. Purpose & Audience

`daily-brief` is for the person **running** the account. This skill is for the person the account runs **for** — the client, the CMO, the founder on a Monday call. It trades alert lists for **narrative**, operator shorthand for **business language**, and specific ad IDs for the story of *what changed, why, and what's next*.

Run **once a week** (Mon 8am is typical, before client syncs).

## 2. Chain of Skills

Like `daily-brief`, this orchestrates rather than re‑implements — but the *selection* rules differ: executives care about **direction and delta**, not item‑level alerts.

| Step | Skill                                         | Keep                                                                                            |
| :--- | :-------------------------------------------- | :---------------------------------------------------------------------------------------------- |
| 1    | [past-7days](past-7days.md)                   | Full campaign roll‑up with WoW deltas — the data spine of the summary                           |
| 2    | [budget-shifter](budget-shifter.md)           | Every shift > $50/day applied this week + total reallocated                                     |
| 3    | [bleeders-winners](bleeders-winners.md)       | Count of pauses and scales this week, plus the single biggest winner creative                   |
| 4    | [anomaly-watch](anomaly-watch.md)             | Only 🚨 Page and 🔴 Critical incidents this week — with duration and $ impact                   |
| 5    | [audience-health](audience-health.md)         | Any audience that moved between Healthy / Refreshing / Exhausted status this week               |

## 3. Narrative Structure

Output is **prose, not a table**. Five sections, each tight:

| Section                              | Purpose                                                                              | Length          |
| :----------------------------------- | :----------------------------------------------------------------------------------- | :-------------- |
| **Lede**                             | Headline verdict — good week, rough week, steady                                     | 1 sentence      |
| **The week in numbers**              | Spend, conversions, CPA/ROAS — each with WoW delta + one sentence of context         | 4–6 lines       |
| **What we changed**                  | Budget moves, pauses, launches, audience refreshes — and *why*                       | 1 short paragraph |
| **What happened that shouldn't have**| Incidents from `anomaly-watch`, in business terms (dollars lost, duration, fixed Y/N)| 1 paragraph, or *"nothing to flag"* |
| **Next week**                        | 2–3 concrete moves planned, each tied to a skill output (not vague promises)         | 2–3 bullets     |

## 4. Tone Rules

- **Business language, not operator shorthand.** Write *"we paused the ads that weren't earning their keep"*, not *"3 bleeders, avg CPA $38 vs. $25 target"*.
- **Dollars before rates.** Lead with *"cost us $420"* before *"CPA rose 18%"*.
- **Explain the why.** Every change gets a one‑line rationale — the client should never have to ask *"so why did we do that?"*.
- **No undefined jargon.** CPM, ROAS, ABO/CBO — fine, but define on first use if the client isn't technical.
- **Never paste a raw table from another skill.** Prose or nothing.

## 5. Operational Output: Weekly Exec Summary

> **Weekly Exec Summary — Week of Apr 14–20 · `Acme Ads`**
>
> **Lede:** Strong week — revenue up 14% on 8% higher spend, and our biggest winner (the Founder UGC ad) scaled cleanly without efficiency loss.
>
> **The week in numbers**
>
> - **Spend:** $28,200 (**↑8%** WoW) — driven by a planned +$100/day scale on Retargeting.
> - **Conversions:** 1,172 (**↑14%**) — both volume and efficiency moved in the right direction.
> - **ROAS:** **2.6×** (up from 2.4×) — Retargeting alone pulled 3.4×.
> - **Cost per purchase:** **$24.10** (↓6%) — under the $25 target for the first time in three weeks.
>
> **What we changed**
>
> We paused three fatigued creatives in the Summer Launch campaign — all showed the same drop in click‑through rate and rising repeat‑exposure, signals the audience had seen them too many times. We replaced them with three fresh UGC variants produced this week. On budget, we shifted **$180/day** out of a stale test campaign and the Evergreen Prospecting set into Retargeting and the Founder UGC ad — the two pulling clear profit. We also refreshed the "1% Purchase LAL" audience, which had saturated to the point costs were climbing without delivery improving.
>
> **What happened that shouldn't have**
>
> One incident on Wednesday: the Meta pixel briefly stopped firing for **~90 minutes** in the afternoon (resolved automatically). No budget impact — conversions resumed within the same hour the pixel recovered.
>
> **Next week**
>
> - **Scale Retargeting +10%** if the new UGC batch holds ROAS above 3.0× for five more days.
> - **Launch 3 new creative concepts** for Summer Launch, briefed from the "Notes‑app + Time" winner pattern (via `creative-brief`).
> - **Monitor the refreshed 1% LAL** — if repeat‑exposure climbs past 2.5 within 10 days, build a fresh lookalike seed from last‑60‑day buyers.
