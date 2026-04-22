---
name: audience-health
description: "Ad‑set‑level audience diagnostic: frequency across the ad set, reach saturation, and audience overlap between active ad sets. Tells you when to refresh, expand, split, or retire an audience — the layer above creative fatigue."
metadata:
  version: 1.0.0
---

## 1. Purpose

`fatigue-detector` asks *"is this creative exhausted?"* — this skill asks the adjacent question: *"is this **audience** exhausted?"* Even fresh creative can stall when the audience has been fully reached, is bidding against itself (overlap), or has saturated to the point where new delivery costs more than it returns. Run weekly, or whenever `bleeders-winners` flags ads that fail *despite* recent creative refreshes.

## 2. Signals

| Signal                  | Definition                                                                                                | Source                       |
| :---------------------- | :-------------------------------------------------------------------------------------------------------- | :--------------------------- |
| **Frequency (ad set)**  | Impressions ÷ reach across all ads in the ad set over the trailing 7 days                                 | Platform reporting           |
| **Reach saturation**    | `unique reach (30d) / estimated audience size` — how much of the audience we've already hit               | Platform estimate + reach    |
| **Audience overlap**    | % of users served by two+ active ad sets in the same account                                              | Meta Audience Overlap tool   |
| **CPM creep**           | Ad‑set CPM trend over 14 days (rising CPM on stable creative = audience pressure)                         | Platform reporting           |
| **Delivery friction**   | CPP (cost per purchase) rising > 20% WoW despite no creative change, or dropping out of learning          | Platform + internal metrics  |

## 3. Health Matrix

| Status         | Frequency | Reach Saturation | Overlap (peak pair) |
| :------------- | :-------- | :--------------- | :------------------ |
| 🟢 Healthy     | < 2.5     | < 40%            | < 15%               |
| 🟡 Refreshing  | 2.5 – 3.5 | 40 – 60%         | 15 – 30%            |
| 🔴 Exhausted   | > 3.5     | > 60%            | > 30%               |

An ad set is **Exhausted** if any single signal hits 🔴, or if two or more signals hit 🟡.

## 4. Refresh Playbook

| Condition                                                | Recommended Action                                                                   |
| :------------------------------------------------------- | :----------------------------------------------------------------------------------- |
| **High frequency, low saturation**                       | Creative is over‑served to a narrow slice — *widen delivery*, remove interest caps.  |
| **High saturation, moderate frequency**                  | Audience is used up — *build a fresh lookalike* from recent converters (last 60–90d).|
| **High overlap between two ad sets**                     | Consolidate or exclude — stop bidding against yourself.                              |
| **Rising CPM + stable creative**                         | Auction pressure — *expand* targeting (broaden interests, raise LAL % tier).         |
| **Exhausted AND unprofitable**                           | Pause the ad set. Relaunch with fresh seed + fresh creative.                         |
| **Exhausted BUT still profitable**                       | Scale cautiously via [budget-shifter](budget-shifter.md); queue a refresh in 7 days. |

## 5. Operational Output: Audience Health Report

> **Audience Health — Mon, Apr 22**
>
> **🔴 Exhausted (1)**
>
> - **`LAL 1% Purchase Q4`** — campaign: *Summer Launch*
>   Frequency **4.2** · Reach saturation **68%** · Overlap with `LAL 3% Purchase` = **42%**
>   **Action:** pause, launch `LAL 1% Purchase Last 60d` (fresh seed from recent converters).
>
> **🟡 Refreshing (2)**
>
> - **`Interest: Project Management`** — campaign: *Retargeting*
>   Frequency 3.1 · Saturation 48% · Overlap 22%
>   **Action:** enable interest expansion OR build `LAL 3% Page Engagers` to dilute.
> - **`Website Visitors 30d`** — campaign: *Retargeting*
>   Frequency 2.9 · Saturation 52% · Overlap 18%
>   **Action:** layer in 60d and 90d windows to extend the pool.
>
> **🟢 Healthy (3)**
>
> - `LAL 2% Purchase Last 90d` · `Broad US 18–45 women` · `Page Engagers 14d`
>   *(No action — monitoring continues.)*
