---
name: past-7days
description: "Weekly campaign‑level performance report: last 7 days of spend, CTR, CPC, and CPA with week‑over‑week deltas and the top ad (name + copy) driving each campaign, surfacing momentum shifts and target hits at a glance."
metadata:
  version: 1.1.0
---

## 1. Report Scope

Pull **campaign‑level** metrics for the last 7 days, compared to the 7 days before when prior data exists. Granularity is **per active campaign** — never aggregate across campaigns. Inactive campaigns are skipped. The goal is to show **momentum**: is performance improving or declining, and is the account getting more or less efficient?

Under each campaign, also surface the **top‑performing ad** (or top 1–3 if the campaign has several active creatives) so the reader can see *what's actually driving the numbers*. For each ad, show:

- **Ad name** (as set in the ad platform)
- **Copy snippet** — headline + first line of primary text, truncated to ~120 chars total
- **Ad‑level metrics** — Spend · CTR · CPC (and CPA if conversions > 0)

## 2. Metrics to Report

| Metric          | Always / Conditional | Notes                          |
| :-------------- | :------------------- | :----------------------------- |
| **Spend**       | Always               | Account currency               |
| **Impressions** | Always               |                                |
| **Clicks**      | Always               |                                |
| **CTR**         | Always               | Shown as %                     |
| **CPC**         | Always               |                                |
| **CPA**         | If conversions > 0   | Compare to target CPA when set |

## 3. Week‑over‑Week Trend Rules

For every metric, show the delta vs. the prior 7‑day window:
| Metric type | Delta format | Example |
| :--- | :--- | :--- |
| **Spend / Impressions / Clicks / CPC** | `↑` or `↓` with **% change** | `↑12%` |
| **CTR** | `↑` or `↓` with **percentage‑point** change | `↓0.2pp` |
| **CPA** | `↑` or `↓` with **absolute $** change | `↑$2.40` |

Omit the delta when prior‑week data is unavailable (e.g. brand‑new campaign).

## 4. Performance Flags

| Flag | Trigger                                                                   |
| :--- | :------------------------------------------------------------------------ |
| 🏆   | CPA < target CPA                                                          |
| ⚠️   | CPA > 1.5× target CPA **or** CTR drop > 0.5pp WoW **or** CPC up > 25% WoW |
| 🆕   | Campaign < 7 days old — no prior window to compare                        |

## 5. Operational Output: Weekly Briefing

> **Last 7 Days Performance**
>
> **Summer Launch — Traffic**
>
> - **Spend:** $1,050 (↑12% vs. prior week) · **CTR:** 1.8% (↓0.2pp) · **CPC:** $1.22 (↑$0.15)
> - **Top ad:** `Summer_Launch_UGC_v2` — *"The fastest way to hit $10K/mo — 2,400 founders already did."* · Spend $620 · CTR 2.1% · CPC $1.08
>
> **Retargeting — Conversions**
>
> - **Spend:** $1,400 (↑8%) · **CTR:** 2.4% (↑0.3pp) · **CPA:** $18.50 (target: $25) 🏆
> - **Top ad:** `Retargeting_Testimonial_Sarah` — *"Sarah scaled from $2K to $40K in 90 days. Here's the exact playbook."* · Spend $890 · CTR 3.1% · CPA $15.20 🏆
> - **Ad #2:** `Retargeting_ProductDemo_v3` — *"See how it works in 90 seconds."* · Spend $510 · CTR 1.8% · CPA $24.40
>
> **Evergreen Prospecting — Awareness** 🆕
>
> - **Spend:** $320 · **CTR:** 1.1% · **CPC:** $0.78
> - **Top ad:** `Evergreen_Hook_Notes_v1` — *"Things my agency does that my CFO hates."* · Spend $320 · CTR 1.1% · CPC $0.78
> - _(launched 3 days ago — no prior window to compare)_
