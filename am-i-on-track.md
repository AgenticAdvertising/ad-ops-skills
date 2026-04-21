---
name: am-i-on-track
description: "Intraday pacing check: compares today's spend so far to the expected spend at this hour, projects end‑of‑day spend, and flags over‑ or under‑pacing before budget is blown or opportunity is missed."
metadata:
  version: 1.0.0
---

## 1. Report Scope

Check **intraday pacing** for every active campaign (or at account level when asked). Computes where spend *should* be at the current hour, where it actually is, and where it will land by end of day if the current rate holds. Runs at any time of day — not just EOD.

Catches two failure modes early:

- **Overspend** — campaign blows through budget before EOD
- **Underspend** — campaign leaves budget on the table, missing delivery

## 2. How to Compute Pace

| Signal                          | Definition                                                                                                       |
| :------------------------------ | :--------------------------------------------------------------------------------------------------------------- |
| **Current spend**               | Spend today, from 00:00 account‑time to now                                                                      |
| **Expected spend at this hour** | `(hours elapsed / 24) × daily budget` — use the platform's delivery curve if reported, otherwise assume linear   |
| **Pace %**                      | `(current − expected) / expected × 100`                                                                          |
| **Projected EOD**               | `current × (24 / hours elapsed)`                                                                                 |

## 3. Pace Flags

| Flag | Meaning          | Trigger                                                                                |
| :--- | :--------------- | :------------------------------------------------------------------------------------- |
| 🟢   | On pace          | Within ±10% of expected                                                                |
| 🟡   | Warning          | 10–25% over or under expected                                                          |
| 🔴   | Critical         | > 25% over expected **or** projected EOD will exceed budget by > 15%                   |
| 💤   | Underdelivering  | > 25% under expected for > 3 hours — likely a delivery issue, not just a pacing dip    |

## 4. Operational Output: Pacing Alert

> 🟡 **Spend pacing 18% above target**
>
> - **Current:** $287 (as of 2:30pm)
> - **Expected:** $243 at this time
> - **Projected EOD:** $524 (budget: $450 — **$74 over**)
> - **Action:** cap intraday budget at $450 or pause the lowest‑ROAS ad in the set.
