---
name: bulk-asset-rename
description: "Bulk-renames creative asset files (from Google Drive or similar) following a configurable naming convention. Inspects visual content + metadata of each file and generates a consistent filename per the template below."
metadata:
  version: 1.0.0
  tags: [batch]
---

# Bulk Asset Rename — Default

You are renaming creative assets in bulk. For each asset, you receive the file's metadata (original name, mime type, dimensions, duration) and — for images and video thumbnails — the visual content. Produce a single new filename that follows the convention configured below.

---

## ✏️ Configurable: Naming Convention

> **Edit this block to change how files are named.** The workflow reads this template verbatim — update the token order, add/remove tokens, or change the separator, and the rest of the skill still applies. Every token you list here MUST have a matching section under "Schema Fields" below (or be added by you).

```template
Style_AssetType_Length_CreatorAge_Hook_Dimensions.ext
```

### Configurable: Separator

The character joining tokens. Default: `_`. Supported alternatives: `-`, `.` (avoid any character that conflicts with your filesystem).

```separator
_
```

### Configurable: Extension policy

How to handle the file extension. Options: `preserve` (keep original), `lowercase` (preserve but lowercase), or a fixed literal like `.jpg`. Default: `preserve`.

```extension
preserve
```

---

## Universal Rules

- Join tokens with the configured separator exactly — no spaces, no slashes, no punctuation other than the separator and the extension dot.
- Emit tokens in the exact order listed in the template. Never drop a token, even when its value is `n/a`.
- Output only the filename as a plain string in the `proposedName` field. Put your reasoning in `reason`.

## Schema Fields

### 1. Style — required (All)

Dominant content style. Pick ONE using this decision hierarchy (first match wins):

1. **Founder** — primary on-screen person is the founder/owner of the brand speaking on behalf of the company.
2. **Influencer** — a recognizable creator or influencer endorsing the product.
3. **Testimonial** — customer-facing quote/review spoken to camera or overlaid as text.
4. **UGC** — user-generated-content look (phone-shot, casual, informal), not clearly an influencer or testimonial.
5. **Comparison** — explicit before/after or side-by-side vs. competitor/alternative.
6. **Reviews** — aggregated star ratings, review screenshots, or press logos.
7. **SocialProof** — numeric proof ("10,000+ customers", "#1 on Product Hunt"), awards, trust badges.
8. **Question** — hook frames a question to the viewer.
9. **Product** — clean studio/product-focused shot with minimal human subject.
10. **Informative** — explainer-style, data/stats/how-it-works content.
11. **Other** — none of the above.

### 2. AssetType — required (All)

- `Static` for images (jpg, jpeg, png, webp, gif).
- `Video` for videos (mp4, mov, webm) and animated content.

### 3. Length — required (All)

Duration bucket for videos. For images always emit `n/a`.

Allowed values: `n/a`, `Under10s`, `10-20s`, `20-30s`, `30-45s`, `45s-1m`, `1m-1m30s`, `1m30s+`.

### 4. CreatorAge — required (All)

Estimated age range of the primary on-screen person. If there is no person on screen (pure product, text-only, animation), emit `n/a`.

Allowed values: `n/a`, `20-30`, `30-40`, `40-50`, `50-60`, `60-70`.

### 5. Hook — required (All)

Short descriptive label for the creative concept. Free text in PascalCase, 1–3 words joined without spaces, letters only.

Examples: `WomanUnboxing`, `ProductSpin`, `FounderMonologue`, `BeforeAfter`, `CustomerQuote`, `PriceCompare`.

### 6. Dimensions — required (All)

Aspect ratio detected from pixel dimensions. Compute from width × height:

- `9x16` — portrait / stories / reels (ratio ≈ 0.56)
- `4x5` — portrait feed (ratio ≈ 0.80)
- `1x1` — square (ratio ≈ 1.00)
- `16x9` — landscape (ratio ≈ 1.78)
- `4x3` — legacy landscape (ratio ≈ 1.33)

Snap to the nearest ratio within ±5%. If dimensions are missing, emit `unknown`.

## Examples

- Founder UGC image, 1080×1350: `Founder_Static_n/a_30-40_FounderMonologue_4x5.jpg`
- Influencer unboxing reel, 23s, 1080×1920: `Influencer_Video_20-30s_20-30_WomanUnboxing_9x16.mp4`
- Product-only square photo, 1080×1080, no person: `Product_Static_n/a_n/a_ProductSpin_1x1.png`
- Testimonial video, 45s, 1920×1080: `Testimonial_Video_30-45s_30-40_CustomerQuote_16x9.mp4`

## Rules

- Always emit every token listed in the template in the exact order configured — even if a token is `n/a`.
- Never invent enum values. If a field truly doesn't fit any enum, use the defined fallback for that field (`Other` for Style, `n/a` for Length and CreatorAge).
- `Hook` is the only field that accepts free-text PascalCase — keep it terse (≤24 chars).
- If you cannot confidently determine `Style` or `Hook` from the visual + metadata, fall back to `Other` / `Generic` and explain in `reason`.
- Follow the configured extension policy exactly.
- Never output anything other than the filename in `proposedName`.
