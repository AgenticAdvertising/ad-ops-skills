---
name: budget-shifter
description: "Reallocates budget across campaigns and ad sets based on ROAS and CPA — finds donors (underperformers with budget to spare) and receivers (scalable winners), with guardrails against learning‑phase resets. Also forecasts what‑if scenarios (spend ±X%, campaign scale/pause) grounded in the account's actual history, never generic projections."
metadata:
  version: 1.1.0
---

## 1. Purpose

When a campaign wins, raising its budget in isolation is half the job — the other half is *finding the money* without blowing total account spend. This skill identifies **donors** (underperformers that can shed budget without hurting delivery) and **receivers** (winners with room to scale), then proposes a set of moves with amounts and reasoning.

Two modes:

- **Redistribute** (default): total account spend unchanged; money moves between campaigns.
- **Scale up**: additive — suggest incremental budget for receivers without donor cuts.

## 2. Donor Signals (pull budget from)

A campaign is a **donor** if it hits **any** of these:

| Signal                    | Threshold                                                          |
| :------------------------ | :----------------------------------------------------------------- |
| **Underperforming ROAS**  | ROAS < 0.8× account median (last 14 days)                          |
| **Overshooting CPA**      | CPA > 1.3× target CPA for 7+ days                                  |
| **Fatigued at scale**     | Frequency > 3.5 AND spending > 10% of account daily total          |
| **Underdelivering**       | Spending < 70% of its budget daily for 5+ days — budget is unused  |

## 3. Receiver Signals (send budget to)

A campaign is a **receiver** only if it meets **all** of these:

| Signal                  | Threshold                                                                          |
| :---------------------- | :--------------------------------------------------------------------------------- |
| **ROAS above target**   | ROAS > 1.2× target for 7+ days                                                     |
| **Delivery headroom**   | Daily spend at ≥ 85% of current budget — the campaign is actually using its money  |
| **Audience freshness**  | Frequency < 2.5                                                                    |
| **Past learning phase** | ≥ 50 conversions in trailing 7 days OR ad set out of platform "Learning" status    |

## 4. Move Sizing & Guardrails

| Rule                       | Value                                                                                    |
| :------------------------- | :--------------------------------------------------------------------------------------- |
| **Max single move**        | 20% of the receiver's current daily budget per shift                                     |
| **Donor floor**            | Never cut a donor below the learning threshold (~50 conversions/week/ad set)             |
| **Cooldown**               | Don't shift the same (source, destination) pair twice in < 48 hours                      |
| **Aggregate cap per run**  | Total reallocation ≤ 10% of account daily spend                                          |
| **New‑campaign shield**    | Campaigns < 7 days old are neither donors nor receivers                                  |
| **One‑way donor rule**     | A donor in one run can't become a receiver in the next 48h (avoid churn)                 |

## 5. Operational Output: Budget Shift Proposal

> **Budget Shifts — Mon, Apr 22** · Mode: **Redistribute** · Total moved: **$180/day** (4.5% of $4K account daily spend)
>
> | From → To                                               | Amount   | Why                                                                                                                                   |
> | :------------------------------------------------------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------ |
> | **Q1 Test Campaign** → **Retargeting — Conversions**    | $40/day  | Donor: CPA $38 vs. $25 target (7 days). Receiver: ROAS 3.4× (target 2.0×), freq 1.8, past learning.                                  |
> | **Evergreen Prospecting** → **Founder UGC Hook**        | $60/day  | Donor: underdelivering (52% of $120 budget used last 5 days). Receiver: 17 conversions in 7 days, CPA $15.                           |
> | **Spring Sale Carousel** → **Summer Launch — Traffic**  | $80/day  | Donor: fatigued (freq 4.1, CTR 0.8%, spending 14% of account). Receiver: CPC 42% of account avg, freq 2.1, spending 93% of budget.   |
>
> **Action:** apply all 3 in one batch, then re‑check in 48h before the next shift.

## 6. Scenario Forecasts

Beyond individual shift proposals, the skill answers **what‑if** questions grounded in the **account's actual history** — never generic projections. Every number in a forecast must be derivable from this account's data.

### 6.1 Supported Scenarios

| Scenario                       | Typical Question                                                          |
| :----------------------------- | :------------------------------------------------------------------------ |
| **Total spend ±X%**            | *"What happens if we spend 30% more next week?"*                          |
| **Single‑campaign scale**      | *"What if we doubled Retargeting's budget?"*                              |
| **New campaign launch**        | *"If we add a $200/day campaign, where does it bite into existing performance?"* |
| **Campaign pause**             | *"If we pause Summer Launch, where does its spend need to go?"*           |
| **Target‑driven**              | *"What spend gets us to $50K revenue/week at ≥ 2.0× ROAS?"*               |

### 6.2 Data Required Before Forecasting

Pull these from the account — if any are missing for a given campaign, skip or mark that campaign as 🔴 Unreliable in the output:

- **Per‑campaign spend → outcome curve** (14‑day rolling): observed ROAS and CPA at each realized daily spend tier.
- **Past scaling response**: actual % change in ROAS/CPM when that campaign's daily budget moved ±20% in the trailing 90 days.
- **Saturation state** from [audience-health](audience-health.md): does the campaign have headroom, or is it already Exhausted?
- **Account auction context**: current CPM vs. 30‑day avg (rising CPM ⇒ less elasticity at higher spend).

### 6.3 Forecast Rules

- **Per‑campaign elasticity** — never blend across campaigns. A Winner with headroom and a saturated brand campaign respond very differently to the same % increase.
- **Bound to history** — cap any projection at the historical extreme observed. Don't extrapolate past what this account has actually done.
- **Confidence tiers**:

| Tier                    | Criteria                                                                                   |
| :---------------------- | :----------------------------------------------------------------------------------------- |
| 🟢 **High**             | ≥ 3 past budget changes to sample, similar CPM regime, current saturation Healthy/Refreshing |
| 🟡 **Low**              | 1–2 past samples, or CPM regime differs > 20% from sample window                            |
| 🔴 **Unreliable**       | No relevant history (new campaign, no prior budget changes) — show range only, no point estimate |

### 6.4 Operational Output: "What if we spend 30% more?"

> **Scenario:** account daily spend **$4,000 → $5,200** (+30%) · horizon: next 7 days
>
> | Campaign                          | Current  | Proposed   | Projected ROAS       | Projected CPA         | Conf. | Notes                                                                 |
> | :-------------------------------- | :------- | :--------- | :------------------- | :-------------------- | :---- | :-------------------------------------------------------------------- |
> | **Retargeting — Conversions**     | $1,400   | **$1,820** | 3.1× (was 3.4×, −0.3)| $19 (was $18.50, +$0.50) | 🟢    | Last 2 +20% moves held ROAS within −5%. Freq 1.8 — still headroom.    |
> | **Summer Launch — Traffic**       | $1,050   | **$1,260** | — (traffic objective)| CTR flat              | 🟢    | CPM only +6% on last +25% move. Freq 2.1.                             |
> | **Evergreen Prospecting**         | $320     | **$700**   | 1.4× (was 2.0×, −30%)| $42 (was $30, +$12)   | 🟡    | Only 2 prior budget changes. Likely hits saturation at this tier.     |
> | **Founder UGC Hook**              | $280     | **$420**   | 2.8× (was 2.9×)      | $15 (was $15.20)      | 🟢    | Winner: 17 conv / 7d, freq 1.8, past learning phase.                  |
>
> **Aggregate forecast:** account ROAS **2.4× → 2.2×** (−8%) · CPA **$24 → $26** (+8%) · **+21 conversions/day**.
>
> **Caveat:** Evergreen Prospecting's elasticity sample is thin (🟡). Consider redirecting its +$380 to Retargeting + Founder UGC Hook (both 🟢) — projected aggregate ROAS with that redirect: **2.3×** (vs. 2.2× baseline).
