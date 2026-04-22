---
name: competitor-ads-watch
description: "Weekly external intelligence — ingests Meta Ad Library for tracked competitors, surfaces new ads launched since last run, clusters them by angle/format, and flags shifts in what the market is testing. Closes the 'am I missing fresh ideas?' gap the inward‑facing skills don't cover."
metadata:
  version: 1.0.0
---

## 1. Purpose

Every other skill looks **inward**. This one looks **outward**: *what are competitors testing this week?* The goal isn't to copy — it's to catch when an angle is hot in the market, when a competitor is doubling down on a format, or when a pattern your account hasn't tried is suddenly everywhere.

Run **weekly** (Mon is typical) for most accounts; **daily** for tight competitive races.

## 2. Tracked Competitors

Expects a list of competitor brands/domains maintained by the user. For each, pull from the Meta Ad Library:

- **Active ads** currently running, with estimated run duration.
- **New ads this period** — launched since the previous run.
- **Paused ads since last run** — creative‑attrition signal.

**Minimum viable list:** 5–10 competitors. Tracking more dilutes signal — dedup to the closest functional competitors.

## 3. Per‑Ad Tagging

Apply the same axes as [creative-brief](creative-brief.md) so patterns map 1:1 across inward and outward skills:

| Axis                     | Source                                                                  |
| :----------------------- | :---------------------------------------------------------------------- |
| **Hook type**            | Inferred from primary text (question, stat, story, testimonial, problem, command) |
| **Visual format**        | Inferred from first frame (notes‑app, UGC, chart, product shot, text‑on‑photo, before/after) |
| **Mood**                 | Inferred from visual treatment                                          |
| **Angle / Horseman**     | Inferred from message (💰 · ⏱ · 👑 · ⚠️)                                  |
| **Funnel stage guess**   | CTA + landing page type (Learn More → ToFu; Free Trial → BoFu)          |
| **Duration (days live)** | From Ad Library metadata                                                |
| **Placement mix**        | Feed · Stories · Reels (as reported by Meta)                            |

## 4. Change Detection

Compare this week's scan to last week's. Surface **signal only**, not the full inventory:

| Signal                      | Trigger                                                                           |
| :-------------------------- | :-------------------------------------------------------------------------------- |
| **New angle**               | Competitor launched ≥ 3 ads in a Horseman they weren't using 30 days ago          |
| **Format doubling‑down**    | ≥ 5 new ads in the same visual format from one competitor this week               |
| **Long‑running winner**     | A competitor ad has been live > 30 days — likely profitable, worth studying       |
| **Mass pause**              | Competitor paused > 5 ads at once — campaign end or failed batch                  |
| **Market pattern**          | ≥ 3 different competitors running the same (format, angle) pair this week         |

## 5. Gap Analysis (vs. your account)

Compare the competitor mix against the account's own creative pattern library:

- **Angle gaps** — Horsemen competitors use that your account doesn't.
- **Format gaps** — visual formats common in the set but absent from your account.
- **Velocity gap** — competitors shipping ≥ 10 new creatives/week while your account ships 2 = a creative‑pipeline bottleneck.

## 6. Operational Output: Weekly Competitor Report

> **Competitor Ads Watch — Week of Apr 20** · 7 competitors tracked · **42 new ads detected**
>
> **🔔 Market Pattern — emerging**
>
> - **Format: "AI calculator / ROI widget"** — 4 of 7 competitors launched this format this week (3 of them hadn't used it before). Angle: 💰 Money. **Action:** brief an equivalent via [creative-brief](creative-brief.md).
>
> **🚀 Long‑running winners — study these**
>
> - **`AcmeCo — AD_abc123`** · live **47 days** · Notes‑app + ⏱ Time + Professional · CTA: Book Demo
> - **`Rival Inc — AD_xyz456`** · live **38 days** · UGC + 💰 Money + Casual · CTA: Learn More
>
> **📈 Doubling down**
>
> - **`AcmeCo`** — launched **7 new ads** this week, all in the "Split‑screen before/after" format. Angle mix: 5× 💰 Money, 2× 👑 Status. Signal: they've found something worth testing.
>
> **🆕 New angles by competitor**
>
> - **`BetaBrand`** — first time using ⚠️ Fear (3 new ads). Shift from their usual 💰 Money mix.
> - **`GammaCorp`** — pivoted to UGC‑only this week; paused all polished studio ads.
>
> **⚠️ Mass pause**
>
> - **`DeltaLabs`** paused 11 ads on Apr 18 — likely end of a seasonal push or a failed batch.
>
> **🕳 Gap vs. your account**
>
> - Competitors running ⚠️ Fear: **6 of 7**. Your account: **0**. → Brief a fear‑framed variant via [creative-brief](creative-brief.md).
> - "AI calculator" format (see Market Pattern): **4 competitors**. Your account: **0**.
> - **Velocity gap:** tracked set shipped **42 new ads** this week · your account shipped **3**. Expand the creative pipeline or accept less test‑variance than the market.
