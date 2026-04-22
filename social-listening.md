---
name: social-listening
description: "Brand sentiment analysis across Reddit, X, and G2 — aggregates mentions, classifies sentiment, clusters themes, extracts verbatim language, and translates findings into paid‑ads moves (hook material, objections to preempt, competitor comparison angles, messaging pivots)."
metadata:
  version: 1.0.0
---

## 1. Purpose

Every other skill in the kit optimizes *what the account does with its own data*. This one listens to **what the market is saying about the brand** — on Reddit, X, and G2 — and translates that into concrete moves for paid ads: hook material, objections to preempt, competitor angles, messaging pivots, and creatives to pause.

Run **weekly** (Mon is typical), or **on‑demand** before a new creative batch or in response to a sentiment spike flagged by `anomaly-watch`.

Every run answers two questions:

1. **"How does the market feel about us right now?"** — sentiment distribution + delta vs. last period.
2. **"What should we change in our ads because of it?"** — specific, quoted, actionable.

## 2. Sources & What to Pull

| Source     | Pull                                                                                                                    | Why it matters for ads                                                                                        |
| :--------- | :---------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------ |
| **Reddit** | Brand mentions across relevant subreddits (last 30d) · threads where brand is compared or recommended · top‑voted comments | Unfiltered language. "Hot take" threads surface hooks no brand would write themselves.                        |
| **X**      | Brand handle mentions, replies, quote tweets · hashtags adjacent to the brand                                           | Speed of sentiment shift. Early detection of viral framings.                                                  |
| **G2**     | All reviews last 90d · star‑rating distribution · sorted by recency and helpfulness                                     | Structured feedback. Competitor comparisons are native to the format.                                         |

**Minimum viable run:** ≥ **30 mentions** across all sources combined. If under threshold, output a single *"Insufficient data — re‑run in 14 days"* notice.

## 3. Sentiment Classification

Every mention gets one label:

| Label            | Definition                                                                   |
| :--------------- | :--------------------------------------------------------------------------- |
| 🟢 **Positive**  | Praise, recommendation, success story                                        |
| 🟡 **Neutral**   | Informational, feature‑asking, non‑judgmental                                |
| 🔴 **Negative**  | Complaint, warning others away, criticism                                    |
| 🆚 **Comparison**| Brand mentioned alongside a competitor (sentiment may tilt either way)       |

Track the **distribution this period vs. the prior period** — a **≥ 10pp shift** in any direction is the signal, not the absolute number.

## 4. Theme Clustering

Group mentions into themes. Do **not** report a theme with fewer than **3 mentions** — that's noise.

Per theme, capture:

- **Cluster label** — short phrase (*"onboarding is slow"*, *"love the reporting UI"*, *"switched from Competitor X"*)
- **Sentiment mix** — % positive / neutral / negative within the cluster
- **Representative quotes** — 3 verbatim, one per source when possible
- **Volume trend** — this period vs. prior (↑ · ↓ · flat)

## 5. Paid‑Ads Insight Logic

Raw sentiment isn't the deliverable — the ad‑strategy move is. Translate each theme pattern:

| Theme pattern                                                         | Paid‑ads move                                                                                                                                             |
| :-------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **High‑volume positive praise on a specific feature or outcome**      | Lift the verbatim phrase into **hook material** via [creative-brief](creative-brief.md) and [copy-rotation](copy-rotation.md).                            |
| **Recurring objection or complaint**                                   | Add a **preempt line** to body copy (*"No spreadsheets required — it just works."*). Check ad/LP match via [ad-qa-checklist](ad-qa-checklist.md).        |
| **Competitor comparisons where you win**                               | Brief a **versus ad** — *"X vs. Y"* angle, quoting the market's actual comparison language. Target the competitor's audience.                             |
| **Competitor comparisons where you lose**                              | Pause any competitor‑targeting ads; rework positioning before re‑engaging. Escalate to brand/positioning.                                                 |
| **Feature‑request clusters (neutral)**                                 | Unmet desires in the audience. Test a *"coming soon"* angle only if legitimate; otherwise don't lead with the gap.                                        |
| **Negative sentiment spike (≥ −10pp WoW)**                             | 🚨 Pause any ads reinforcing the complained‑about theme. Alert via `anomaly-watch`. Consider a crisis‑response creative.                                  |

## 6. Operational Output: Weekly Sentiment Report

> **Social Listening — Week of Apr 20 · `Acme Ads` · 187 mentions (↑22% vs. prior week)**
>
> **Sentiment distribution**
>
> - 🟢 **Positive 61%** (↑4pp) · 🟡 Neutral 22% · 🔴 **Negative 11%** (↓2pp) · 🆚 Comparison 6%
>
> **Per‑source breakdown**
>
> | Source | Mentions | Pos  | Neu  | Neg  | Comp |
> | :----- | :------- | :--- | :--- | :--- | :--- |
> | Reddit | 82       | 57%  | 26%  | 12%  | 5%   |
> | X      | 74       | 68%  | 18%  | 9%   | 5%   |
> | G2     | 31       | 58%  | 19%  | 13%  | 10%  |
>
> **Top themes**
>
> **🟢 "Reporting speed" — 28 mentions (↑)** · 93% positive
>
> - *"Finally a tool that doesn't make me wait 40 seconds for a campaign report."* (Reddit, 147 upvotes)
> - *"The reporting is honestly 3× faster than [Competitor]."* (G2 review, 5 stars)
> - *"Acme reports so fast it feels illegal."* (X, 112 likes)
>
> → **Ads move:** lift *"3× faster than [Competitor]"* into a 👑 Status hook via [creative-brief](creative-brief.md) → [copy-rotation](copy-rotation.md).
>
> **🔴 "Onboarding friction" — 11 mentions (↑ from 4 last week)** · 82% negative
>
> - *"Took me 2 hours to connect all my ad accounts."* (G2, 3 stars)
> - *"Onboarding felt like homework."* (Reddit)
>
> → **Ads move:** add preempt line (*"5‑minute setup, no engineer required"*) to body copy. **Pause** the *"Launch in 60 seconds"* hook — it over‑promises against current reality. Escalate to product/onboarding.
>
> **🆚 "Acme vs. [Competitor]" — 9 mentions** · 78% tilt positive
>
> - *"Switched from [Competitor] to Acme last month, no regrets — reporting alone justifies it."* (G2)
> - *"Acme gives you what [Competitor] charges 3× for."* (Reddit, 342 upvotes)
>
> → **Ads move:** brief a **Versus** creative set — *"Switched from [Competitor]? See what you've been missing."* Target the `Interest: [Competitor]` audience.
>
> **🟡 "Dashboard customization" — 6 mentions** · all feature requests
>
> → **Ads move:** don't lead with dashboard imagery until the feature ships; the gap is being noticed.
>
> **This week's ad‑strategy summary**
>
> 1. **New hook** — lift *"3× faster reporting"* into the creative pipeline this sprint.
> 2. **New versus creative set** — target [Competitor] audiences with switcher‑framed copy.
> 3. **One pause** — the *"Launch in 60 seconds"* hook until onboarding friction is resolved.
