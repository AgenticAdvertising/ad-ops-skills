---
name: ad-qa-checklist
description: "Pre‑launch preflight for new ads and campaigns — verifies tracking (UTMs, pixel, events), naming conventions, placement coverage, creative specs, and landing‑page message match. Also produces a required UTM inventory showing each ad's current vs. proposed destination URL so the reviewer can copy‑paste fixes. Catches preventable mistakes before they cost money."
metadata:
  version: 1.1.0
---

## 1. Purpose

Runs **before** a campaign or ad goes live. A checklist pass returns **green to launch**, **yellow (launch but fix within 24h)**, or **red (block launch)**. Every item is machine‑checkable — no human judgment required. The goal is to catch the silent failures that waste the first 48h of budget: a broken pixel, a 404 landing page, a Reels ad uploaded at 1.78:1.

## 2. Checklist Categories

### 🎯 Tracking & Attribution

| Check                           | Severity       |
| :------------------------------ | :------------- |
| Pixel fires on landing page     | 🔴 Block       |
| Conversion API server event OK  | 🔴 Block       |
| Conversion event mapped correctly (Purchase / Lead / etc.) | 🔴 Block |
| All UTM params present (`source`, `medium`, `campaign`)    | 🟡 Warning |
| `utm_content` unique per ad (no cross‑ad collision)        | 🟡 Warning |
| Pixel test event received in last 24h                      | 🟡 Warning |

### 🏷 Naming & Org

| Check                                                                              | Severity   |
| :--------------------------------------------------------------------------------- | :--------- |
| Campaign name follows convention (e.g. `[Brand]_[Objective]_[Audience]_[Date]`)    | 🟡 Warning |
| Ad set name includes audience identifier                                           | 🟡 Warning |
| Ad name is unique within the ad set                                                | 🟡 Warning |
| No `test`, `tmp`, `copy` in production names                                       | ℹ️ Info    |

### 📱 Placements

| Check                                                                | Severity   |
| :------------------------------------------------------------------- | :--------- |
| Selected placements match campaign objective                         | 🔴 Block   |
| Creative asset exists for every selected placement aspect ratio      | 🔴 Block   |
| No placement‑blocklist violations (e.g. Audience Network off for B2B)| 🟡 Warning |

### 🎨 Creative Specs

| Check                                                   | Severity   |
| :------------------------------------------------------ | :--------- |
| Image/video dimensions match placement spec             | 🔴 Block   |
| Video duration within platform limits                   | 🔴 Block   |
| File size under platform cap                            | 🔴 Block   |
| Safe‑zone respected (no critical text in crop zone)     | 🟡 Warning |
| Logo/brand visible in first frame or top‑third          | ℹ️ Info    |

### 🔗 Landing‑Page Match

| Check                                                               | Severity   |
| :------------------------------------------------------------------ | :--------- |
| LP URL reachable (no 404/5xx)                                       | 🔴 Block   |
| LP loads in < 3s on 4G                                              | 🟡 Warning |
| LP headline reinforces ad promise (offer, price, audience)          | 🟡 Warning |
| LP has a visible CTA matching the ad's conversion event             | 🟡 Warning |
| LP is mobile‑responsive                                             | 🟡 Warning |

## 3. Severity Rules

| Severity    | Meaning                                                           |
| :---------- | :---------------------------------------------------------------- |
| 🔴 Block    | Launch must NOT proceed until resolved                            |
| 🟡 Warning  | Launch allowed; item must be fixed within 24h of going live       |
| ℹ️ Info     | Advisory — worth reviewing, not required                          |

Overall status is the highest severity present: any 🔴 = BLOCK, any 🟡 = PROCEED WITH FIXES, otherwise GREEN.

## 4. Operational Output: Preflight Report

> **Preflight — `Summer Launch — Traffic` → Ad Set `LAL 1% Purchase`**
>
> **Result:** 🔴 **BLOCK** — 2 blockers, 3 warnings, 1 info
>
> **🎯 Tracking & Attribution**
>
> - 🔴 Pixel `Purchase` event not firing on test click to LP
> - 🟡 `utm_content` missing from `AD_554`
>
> **🏷 Naming**
>
> - ℹ️ Ad set name `LAL 1% Purchase` missing date suffix — convention is `[Brand]_[Objective]_[Audience]_[Date]`
>
> **🎨 Creative Specs**
>
> - 🟡 `AD_554` aspect ratio 1.78:1 works for Feed, fails Reels spec (expected 9:16) — upload a 9:16 variant or exclude Reels
>
> **🔗 Landing Page**
>
> - 🔴 `https://example.com/summer-offer` returns **404**
> - 🟡 Message match: ad promises *"50% off"* — LP headline reads *"Welcome"*, no offer reinforced
>
> **Action:** fix the 2 blockers (pixel, LP URL) before launch. Warnings have a 24h SLA after go‑live.

## 5. UTM Parameter Inventory (Required)

Beyond the pass/fail checks in §2, every preflight run **must** produce a UTM inventory listing every ad in the batch with its **current destination URL** and a **proposed URL** with standardized UTMs. This is a required transformation — the reviewer should be able to copy each Proposed URL straight back into the ad set.

### 5.1 Required UTM Parameters

| Parameter       | Value source                                                          | Required |
| :-------------- | :-------------------------------------------------------------------- | :------- |
| `utm_source`    | Ad platform — `meta` · `google` · `linkedin` · `tiktok`               | ✅       |
| `utm_medium`    | Channel type — `paid_social` · `paid_search` · `display`              | ✅       |
| `utm_campaign`  | Campaign name, slugified (lowercase, hyphens)                         | ✅       |
| `utm_content`   | Per‑ad identifier — ad ID or ad‑name slug. **Must be unique per ad.** | ✅       |
| `utm_term`      | Audience or keyword (required for Search, optional for Social)         | ⚪       |

### 5.2 URL Transformation — required table

For **every ad** in the batch:

| Column           | Content                                                                                                 |
| :--------------- | :------------------------------------------------------------------------------------------------------ |
| **Ad**           | Ad ID · Name                                                                                            |
| **Current URL**  | Destination URL currently set on the ad (may be missing UTMs or have wrong values)                      |
| **Proposed URL** | Same destination + all required UTMs appended (existing UTMs replaced if misaligned)                    |
| **Delta**        | What changed — `+all UTMs` · `utm_campaign fixed` · `utm_content collision resolved` · `no change`     |

### 5.3 Transformation Rules

- **Preserve non‑UTM query params** (e.g. `ref=`, `plan=`) — append UTMs after them; don't strip them.
- **Resolve collisions** — if two ads would end up with the same `utm_content`, append a suffix (`-v2`, `-v3`) and note it in Delta.
- **Flag broken bases** — if the current URL returns 404/5xx, still produce the Proposed URL for when the LP is fixed, and cross‑reference the 🔴 Block in the Landing‑Page check.
- **Never strip `fbclid` / `gclid`** templates — platform‑injected at click time.
- **Slug conventions** — lowercase · ASCII only · spaces → `-` · strip punctuation · collapse repeat hyphens.

### 5.4 Example

> **UTM Inventory — `Summer Launch — Traffic` · 3 ads**
>
> | Ad                             | Current URL                                      | Proposed URL                                                                                                                             | Delta                                                   |
> | :----------------------------- | :----------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ |
> | `AD_554` Notes App Hero        | `https://acme.com/summer-offer`                  | `https://acme.com/summer-offer?utm_source=meta&utm_medium=paid_social&utm_campaign=summer-launch&utm_content=ad-554-notes-app-hero`      | +all UTMs (were missing)                                |
> | `AD_388` Founder UGC Hook      | `https://acme.com/summer-offer?utm_content=ugc`  | `https://acme.com/summer-offer?utm_source=meta&utm_medium=paid_social&utm_campaign=summer-launch&utm_content=ad-388-founder-ugc-hook`    | +source / medium / campaign · `utm_content` specialized |
> | `AD_412` Founder UGC Hook v2   | `https://acme.com/summer-offer?utm_content=ugc`  | `https://acme.com/summer-offer?utm_source=meta&utm_medium=paid_social&utm_campaign=summer-launch&utm_content=ad-412-founder-ugc-hook-v2` | collision resolved — was duplicating `AD_388`'s value   |
>
> **Action:** copy each Proposed URL into its ad's destination field before launch.
