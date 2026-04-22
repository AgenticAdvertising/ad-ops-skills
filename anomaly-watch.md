---
name: anomaly-watch
description: "Off‑cadence alerts for things that break between scheduled reports — CPM spikes, conversion outages, delivery freezes, spend runaway. Runs on a continuous baseline, fires only on sustained breaches, and ranks alerts by spend impact."
metadata:
  version: 1.0.0
---

## 1. Purpose

Every other skill in the loop runs on a schedule (intraday, daily, weekly). This one catches what happens **between** those runs — a pixel that stops firing at 2pm, a CPM that doubles overnight, a campaign that enters runaway spend after a bid change. It complements, not replaces, the scheduled monitors: if a signal is already surfaced by [am-i-on-track](am-i-on-track.md) or [fatigue-detector](fatigue-detector.md) within their normal cadence, anomaly‑watch should dedup and stay quiet.

## 2. Signals

| Signal                  | Trigger                                                                                     |
| :---------------------- | :------------------------------------------------------------------------------------------ |
| **CPM spike**           | Hourly CPM > **1.5×** the same‑hour 7‑day baseline, sustained ≥ 1 hour                      |
| **Conversion outage**   | Zero conversions for > 3 hours during a window where 7‑day baseline ≥ 1 conv/hour           |
| **Spend runaway**       | Hourly spend > **2×** average hourly share of daily budget                                   |
| **Spend stall**         | Hourly spend < **25%** of expected for > 2 hours on an active campaign                      |
| **Delivery freeze**     | Ad set transitions to `Not Delivering` unexpectedly (after being active, no schedule change)|
| **CTR crash**           | Hourly CTR drops > 50% vs. 7‑day baseline, sustained ≥ 1 hour                               |
| **Pixel event gap**     | No pixel events received for > 30 min (for pixels with normal 24/7 activity)                |

## 3. Detection Rules

- **Baseline**: same‑hour, 7‑day rolling average (handles weekday/weekend + time‑of‑day seasonality).
- **Sustained breach**: require the condition to hold for the stated duration to avoid single‑spike noise.
- **Dedup**: suppress an alert if the same campaign already has an active flag from `am-i-on-track` (🔴) or `fatigue-detector` (🔴) firing for the same symptom.
- **New‑campaign shield**: campaigns < 48 hours old are excluded (no stable baseline yet).
- **Quiet hours**: low‑volume hours (e.g. 2–6am local) use 14‑day baseline instead of 7‑day to avoid noise on sparse data.

## 4. Alert Severity

| Severity   | Criteria                                                                                              |
| :--------- | :---------------------------------------------------------------------------------------------------- |
| 🚨 **Page**  | Spend runaway · Pixel event gap > 2h · Conversion outage > 6h · Delivery freeze on top‑3 spend campaign |
| 🔴 Critical | CPM spike · Delivery freeze · Conversion outage 3–6h                                                  |
| 🟡 Watch    | CTR crash · Spend stall · Pixel gap 30min–2h                                                          |

Page‑severity alerts fire immediately. Critical and Watch batch to the next 15‑minute tick to avoid chatter.

## 5. Operational Output: Anomaly Alert

> 🚨 **Anomaly — 2:47pm ET · Mon, Apr 22**
>
> **Signal:** Spend runaway
> **Campaign:** `Retargeting — Conversions`
>
> - **Current hourly spend:** $89 (2:00pm hour)
> - **7‑day baseline (same hour):** $32 · **+178%**
> - **Concurrent signals:** CPM up 24% in last 2h · budget raised +$100 at 1:30pm
>
> **Root‑cause candidates (ranked):**
> 1. Manual budget raise at 1:30pm — pace not yet rebalanced
> 2. Audience saturation — see [audience-health](audience-health.md)
> 3. Auction pressure from a competing campaign in same account
>
> **Action:** cap daily budget at current level **now**. Revisit in 60min — if CPM normalizes, restore; if not, roll back bid/audience change.
