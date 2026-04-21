---
name: past-7days
description: "Weekly campaign‑level performance report: last 7 days of spend, CTR, CPC, and CPA with week‑over‑week deltas, surfacing momentum shifts and target hits at a glance."
metadata:
  version: 1.0.0
---

## 1. Report Scope

Pull **campaign‑level** metrics for the last 7 days, compared to the 7 days before when prior data exists. Granularity is **per active campaign** — never aggregate across campaigns. Inactive campaigns are skipped. The goal is to show **momentum**: is performance improving or declining, and is the account getting more or less efficient?

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
> - **Spend:** $1,050 (↑12% vs. prior week)
> - **CTR:** 1.8% (↓0.2pp)
> - **CPC:** $1.22 (↑$0.15)
>
> **Retargeting — Conversions**
>
> - **Spend:** $1,400 (↑8%)
> - **CTR:** 2.4% (↑0.3pp)
> - **CPA:** $18.50 (target: $25) 🏆
>
> **Evergreen Prospecting — Awareness** 🆕
>
> - **Spend:** $320 · **CTR:** 1.1% · **CPC:** $0.78
> - _(launched 3 days ago — no prior window to compare)_
