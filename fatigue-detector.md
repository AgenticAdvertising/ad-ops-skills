---
name: fatigue-detector
description: "Detects 'Creative Decay' early using consecutive signal tracking. Always outputs the full active‑ad inventory with a per‑ad fatigue label (✅ Healthy / 🟡 Warning / 🔴 Critical) — not just flagged ads — so drift is visible before it becomes a crisis."
metadata:
  version: 1.2.0
---

## 1. Detection Signals (The Fatigue Matrix)
| Signal | Indicator | Logic / Action |
| :--- | :--- | :--- |
| **Frequency Creep** | Freq > 2.2 (7-day) | Analyze if CTR is dropping. If yes, the audience is "blind" to the ad. |
| **CPM Inflation** | +20% Cost/M | High frequency + rising CPM = Meta's algorithm is penalizing "stale" content. |
| **CTR Erosion** | 7-day CTR < 30-day CTR | The creative is exhausted. Even if ROAS is stable, a crash is imminent. |
| **Sentiment Shift** | Negative Comments | Scans for "Seen this 100 times" or "Stop showing me this." |

## 2. Severity Matrix
| Level | Criteria | AI Action |
| :--- | :--- | :--- |
| **🟡 Warning** | Freq > 3.0 OR 10% CTR Drop | Flag for creative refresh in next 48 hours. |
| **🔴 Critical** | 20%+ CTR Decay (3 days) OR Impression Drop | **Pause Ad** or reduce budget by 50% immediately. |

## 3. Required Output: Full Ad Inventory + Critical Alerts

Every run **must** produce two things together — the pinned alerts surface urgency, the full inventory makes drift visible before it becomes a crisis.

### 3.1 Pinned Critical Alerts (at top of report)

Every 🔴 **Critical** ad gets a highlighted alert in the Daily Briefing format — CTR trend across 3 points, frequency, triggered signals, and recommended action (pause, reduce budget, deploy replacement from [copy-rotation](copy-rotation.md)).

### 3.2 Active Ad Inventory — **required table**

List **every active ad** with ≥ 1,000 impressions in the last 7 days. Do not omit Healthy ads — the full list is the point.

| Column           | Meaning                                                                |
| :--------------- | :--------------------------------------------------------------------- |
| **Ad**           | Ad ID · Name                                                           |
| **Freq (7d)**    | 7‑day frequency                                                        |
| **CTR 7d / 30d** | 7‑day CTR vs. 30‑day CTR, with % delta                                 |
| **CPM Δ**        | % change vs. 14‑day baseline                                           |
| **Signals**      | Which of the 4 Fatigue Matrix signals are triggered (or `—` if none)   |
| **Status**       | ✅ Healthy · 🟡 Warning · 🔴 Critical (per §2)                          |

**Label rules** (first match wins):

- 🔴 **Critical** — any §2 Critical criterion met
- 🟡 **Warning** — any §2 Warning criterion met
- ✅ **Healthy** — no signals triggered

End the table with a one‑line summary: *"X Critical · Y Warning · Z Healthy."*

## 4. Operational Output: Example Report

> **Fatigue Report — Wed, Apr 23 · 12 active ads**
>
> **🔴 Critical (2)**
>
> - **`AD_238` (Notes App Hero)** — CTR **3.2% → 2.8% → 2.1%** (🔴 34% drop) · Freq **3.8** · signals: Frequency Creep + CTR Erosion + CPM Inflation. **Action:** pause and deploy replacement from [copy-rotation](copy-rotation.md).
> - **`AD_733` (Spring Sale Carousel)** — CTR **1.4% → 1.1% → 0.8%** (🔴 43% drop) · Freq **4.2** · signals: all 3 quantitative signals. **Action:** pause immediately — freq past saturation.
>
> **Active Ad Inventory**
>
> | Ad                              | Freq (7d) | CTR 7d / 30d       | CPM Δ | Signals          | Status      |
> | :------------------------------ | :-------- | :----------------- | :---- | :--------------- | :---------- |
> | `AD_238` Notes App Hero         | 3.8       | 2.1% / 3.2% (−34%) | +28%  | Freq + CTR + CPM | 🔴 Critical |
> | `AD_733` Spring Sale Carousel   | 4.2       | 0.8% / 1.4% (−43%) | +35%  | Freq + CTR + CPM | 🔴 Critical |
> | `AD_388` Social Proof Carousel  | 3.1       | 2.4% / 2.6% (−8%)  | +12%  | Freq + CPM       | 🟡 Warning  |
> | `AD_554` Pattern Interrupt      | 2.8       | 1.9% / 2.1% (−10%) | +5%   | CTR              | 🟡 Warning  |
> | `AD_412` Founder UGC Hook       | 1.8       | 2.9% / 2.7% (+7%)  | −3%   | —                | ✅ Healthy  |
> | `AD_501` Testimonial Sarah      | 2.4       | 3.1% / 3.0% (+3%)  | +1%   | —                | ✅ Healthy  |
> | `AD_110` Kitchen Selfie v2      | 2.0       | 2.6% / 2.5% (+4%)  | −2%   | —                | ✅ Healthy  |
> | `AD_155` Notes App v3           | 2.2       | 3.0% / 3.1% (−3%)  | +4%   | —                | ✅ Healthy  |
> | `AD_201` Founder Talking Head   | 1.6       | 2.8% / 2.8% (flat) | 0%    | —                | ✅ Healthy  |
> | `AD_445` Stat Hook Chart        | 1.9       | 2.3% / 2.4% (−4%)  | +6%   | —                | ✅ Healthy  |
> | `AD_612` Benchmark Carousel     | 2.1       | 2.2% / 2.3% (−4%)  | +3%   | —                | ✅ Healthy  |
> | `AD_888` UGC Review Clip        | 1.5       | 3.4% / 3.3% (+3%)  | −5%   | —                | ✅ Healthy  |
>
> **Summary:** 2 Critical · 2 Warning · 8 Healthy.
