---
name: whats-running
description: "Inventory of every active campaign with its daily budget, flagging anything that should have been paused or ended — expired tests, past-end-date launches, or zero-spend zombies quietly burning budget."
metadata:
  version: 1.0.0
---

## 1. Report Scope

List every **active campaign** in the account right now — name, status, daily budget. The purpose is to catch **leaks**: campaigns that should be off but aren't. Old tests, expired promos, seasonal launches past their end date. These quietly burn budget until someone notices.

## 2. What to Show per Campaign

| Field             | Source                                                                |
| :---------------- | :-------------------------------------------------------------------- |
| **Campaign name** | As set in the ad platform                                             |
| **Daily budget**  | Campaign‑level daily budget (or sum of ad‑set budgets if CBO is off)  |
| **Status flag**   | See §3                                                                |
| **Note**          | Reason when flagged — e.g. *"scheduled to end 3/1, still running"*    |

## 3. Status Flags

| Flag | Meaning                 | Trigger                                                                                                                                           |
| :--- | :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| ✅   | Running as expected     | Active · no scheduled end date reached · no leak heuristic triggered                                                                              |
| ⚠️   | Should probably be off  | Scheduled end date has passed · name contains "test" and campaign is > 30 days old · no spend in the last 7 days · budget exceeds account norms   |
| ⏸   | Paused (context only)   | Include only when the user explicitly asks for paused campaigns too                                                                               |

## 4. Operational Output: Active Campaigns List

> **Active Campaigns (4):**
>
> - ✅ **Summer Launch — Traffic** ($150/day)
> - ✅ **Retargeting — Conversions** ($200/day)
> - ⚠️ **Q1 Test Campaign** ($50/day) — *scheduled to end 3/1, still running*
> - ✅ **Brand Awareness** ($75/day)
>
> **Leaks flagged: 1** — unintended spend today ≈ **$50**.
