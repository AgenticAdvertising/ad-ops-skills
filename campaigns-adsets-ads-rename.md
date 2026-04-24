---
name: campaigns-adsets-ads-rename
description: "Audits campaign, ad‑set, and ad names against a three‑level naming convention (Foundr / How To Run Facebook Ads style) and proposes standardized renames for non‑compliant assets, with confidence scores per suggestion."
metadata:
  version: 1.0.0
---

## 1. Purpose

Most ad accounts inherit years of naming drift — `Test 1`, `Copy of Summer Launch`, `Retargeting v3 FINAL`. Inconsistent names break every downstream skill: `past-7days` groups them wrong, `whats-running` can't spot stale tests, `bleeders-winners` surfaces them as mystery IDs. This skill scans active (and optionally archived) assets, flags non‑compliant names, and proposes a standardized rename for each — with a confidence score so you can batch‑approve high‑confidence renames and review the rest.

Default convention: the **Foundr / "How To Run Facebook Ads"** three‑level scheme (Campaign → Ad Set → Ad). Override the tokens in §2 if your team uses a different scheme.

## 2. Naming Convention

### Campaign

`{Marketer} - {Funnel} - {Highlight} - {Type} - [{BidType}] - [{KPI}] - {Date}`

| Token         | Meaning                          | Values                                                                     |
| :------------ | :------------------------------- | :------------------------------------------------------------------------- |
| **Marketer**  | Initials of the marketer or team | e.g. `NS`, `JS`, `SS`                                                      |
| **Funnel**    | Where in the funnel              | `Prospecting` · `Remarketing` · `Reengagement` · `Prospecting/Remarketing` |
| **Highlight** | Unique campaign identifier       | `Evergreen` · `Back to School` · `Leopard Print`                           |
| **Type**      | Budget structure                 | `CBO` · `ABO`                                                              |
| **BidType**   | _(optional)_ bid strategy        | `CC` (Cost Cap) · `BC` (Bid Cap) · `LC` (Lowest Cost) · `MB` (Manual Bid)  |
| **KPI**       | _(optional)_ target metric       | `$32` (target CPA) · `2.0x` (target ROAS)                                  |
| **Date**      | Launch date                      | `M/YY` or `MM/YY` (e.g. `8/25`)                                            |

**Examples**

- `NS - Prospecting - Evergreen - CBO - 8/25`
- `JS - Prospecting - Evergreen - Leopard Print - CBO - CC - 8/25`
- `SS - Remarketing - Back to School - ABO - 8/25`
- `JS - Prospecting/Remarketing - Evergreen - Leopard Print - CBO - CC - 8/25 - $32`

### Ad Set

`{FunnelCode} - [{BidCode}] - {AudienceCode} - {AudienceDescription} - [{Note}] - {Date}`

| Token                   | Meaning                               | Values                                                        |
| :---------------------- | :------------------------------------ | :------------------------------------------------------------ |
| **FunnelCode**          | Short funnel code                     | `P` (Prospecting) · `RE` (Reengagement) · `RM` (Remarketing)  |
| **BidCode**             | _(optional)_ `MB` + bid type + amount | `MBCC50` (Manual Bid, Cost Cap $50)                           |
| **AudienceCode**        | Audience type                         | `I` (Interest) · `L` (Lookalike) · `B` (Broad) · `C` (Custom) |
| **AudienceDescription** | Freeform audience name                | `Kardashian` · `IG Engagers 60` · `Purch1%`                   |
| **Note**                | _(optional)_ differentiating detail   | `Content Refresh` · `UGC Only` · `POW`                        |
| **Date**                | Launch date                           | `M/YY`                                                        |

**Examples**

- `P - I - Kardashian - Content Refresh - 8/25`
- `RE - C - IG Engagers 60 - UGC Only - 8/19`
- `RM - C - Rainbow PDP 30 - POW - 8/15`
- `P - MBCC50 - L - Purch1% - Rainbow PDP all ads - 08/30`

### Ad

`{FileID} - {CopyID} - {HeadlineID} - {AdUnit} - {Date}`

| Token          | Meaning                    | Values                               |
| :------------- | :------------------------- | :----------------------------------- |
| **FileID**     | Asset or file identifier   | `A10` · `FM-V2` · `Lifestyle Summer` |
| **CopyID**     | Body‑copy reference        | `013` · `Testimonial` · `UGC`        |
| **HeadlineID** | Headline or hook reference | `Just In` · `Love It` · `Hate It`    |
| **AdUnit**     | Creative format            | `Image` · `Video` · `Carousel`       |
| **Date**       | Launch date                | `M/YY`                               |

**Examples**

- `A10 - 013 - Just In - 10,000 Customer - Image - 8/15`
- `FM-V2 - Testimonial - Love It - Video - 8/19`
- `Lifestyle Summer - Testimonial - Hate It - Carousel - 8/31`

## 3. Validation Rules

For each existing asset name:

| Rule                                                                  | Severity               |
| :-------------------------------------------------------------------- | :--------------------- | -------- |
| Missing one or more required tokens                                   | 🔴 Non‑compliant       |
| Contains banned patterns: `test`, `tmp`, `copy`, `FINAL`, bare `v\d+` | 🔴 Non‑compliant       |
| Inconsistent delimiter (`_`, `                                        | `, `/`instead of` - `) | 🟡 Drift |
| Date format mismatch (e.g. `Aug 25` instead of `8/25`)                | 🟡 Drift               |
| Unknown token that doesn't match any convention slot                  | 🟡 Ambiguous           |
| Matches convention exactly                                            | ✅ OK                  |

## 4. Rename Logic

1. **Infer from context** — if the ad set targets a 1% Purchase LAL, use `L` + `Purch1%`. If the ad uses a video asset, AdUnit = `Video`.
2. **Never guess identity** — Marketer initials and exact launch dates must be confirmed, not inferred. Mark as `*(ask)*` when unknown.
3. **Preserve history** — when renaming, write the original name to the asset's description/note field so historical reports can still trace back.
4. **Rename parent → child** — campaign first, then ad sets under it, then ads. Avoid reordering.
5. **Confidence score** per suggestion:
   - **95–100%**: all tokens inferable from asset metadata
   - **80–94%**: 1 token ambiguous or missing; suggested value is defensible
   - **< 80%**: multiple tokens missing — needs human input before rename

## 5. Operational Output: Rename Proposal

> **Rename Proposal — Account `Acme Ads` · 12 assets flagged**
>
> **Campaigns (3)**
>
> | Current                    | Proposed                                                                   | Confidence |
> | :------------------------- | :------------------------------------------------------------------------- | :--------- |
> | `Summer Launch - Traffic`  | `JS - Prospecting - Summer Launch - CBO - 8/25` _(Marketer: ask)_          | 70%        |
> | `Retargeting v3 FINAL`     | `RM - Retargeting - CBO - 8/25` — banned token `FINAL` removed             | 85%        |
> | `Copy of Q1 Test Campaign` | 🔴 **Archive or relabel** — `test`/`copy` in name, past scheduled end date | —          |
>
> **Ad Sets (5)**
>
> | Current           | Proposed                                           | Confidence |
> | :---------------- | :------------------------------------------------- | :--------- |
> | `LAL 1% Purchase` | `P - L - Purch1% - 8/25`                           | 95%        |
> | `Interest PMGT`   | `P - I - Project Management - 8/25`                | 90%        |
> | `Engagers_60`     | `RE - C - IG Engagers 60 - 8/19` _(delimiter fix)_ | 90%        |
> | `Broad US`        | `P - B - Broad US - 8/25`                          | 85%        |
> | `test audience 2` | 🔴 **Archive** — banned pattern `test`             | —          |
>
> **Ads (4)**
>
> | Current                   | Proposed                                                  | Confidence |
> | :------------------------ | :-------------------------------------------------------- | :--------- |
> | `Testimonial_Sarah_FINAL` | `Sarah - Testimonial - Love It - Video - 8/25`            | 80%        |
> | `IMG_554`                 | `A554 - UGC - Notes App Hero - Image - 8/20`              | 75%        |
> | `hook-v2`                 | `A555 - UGC - Hook V2 - Video - 8/22` _(HeadlineID: ask)_ | 65%        |
> | `Carousel Spring Sale`    | `Spring Sale - Offer - 40% Off - Carousel - 3/15`         | 85%        |
>
> **Action:** approve the **7 renames at ≥ 85% confidence** in one batch; review the remaining 4 manually.
