---
name: hook-architect
description: "Isolates the first 3 seconds of every video ad to optimize thumb‑stop efficiency. Scores each hook against the account's rolling Hook Rate (±1σ), cross‑checks Hold Rate to separate 'misleading' from 'weak' hooks, tags hook archetypes, and produces iteration briefs that recombine the best hook with the best body."
metadata:
  version: 1.0.0
---

## 1. Purpose

Treats the **hook** (first 3 seconds of a video ad) as a **standalone variable**. Filters out down‑funnel noise — offer, landing page, targeting, body script — to answer one question: *is the creative relevant enough to stop the scroll?* Every other creative skill optimizes *what comes after* the stop; this one optimizes the stop itself.

Run when:

- Auditing a batch of new creatives to see which hooks are actually stopping scroll.
- `fatigue-detector` flags declining CTR — a hook audit disambiguates *"hook fatigued"* from *"body fatigued"*.
- Briefing a new creative via [creative-brief](creative-brief.md) — hook‑architect contributes the recommended archetype.

## 2. Primary Metric — Hook Rate

**Hook Rate = `3‑second video views / impressions`**

Benchmarked against the **rolling account average** over the trailing 30 days.

| Tier            | Threshold                                | Meaning                                                             |
| :-------------- | :--------------------------------------- | :------------------------------------------------------------------ |
| 🏆 **Elite**    | Hook Rate **> account avg + 1σ**         | Repeat and clone this hook archetype.                               |
| 🟢 **On‑par**   | Within **±1σ** of account avg            | Keep running; not a priority for iteration.                         |
| 🔴 **Below**    | Hook Rate **< account avg − 1σ**         | Flag for hook‑swap — the body may still be fine (see §3).           |

Account average and σ are recomputed each run from the **last 30 days of video ads with ≥ 1,000 impressions each**. Ads below the impression threshold are excluded from both the baseline and the scoring.

## 3. The Hand‑off — Hook × Hold Quadrants

A high Hook Rate alone can be misleading. Cross‑reference with **Hold Rate** to diagnose *why* a creative is or isn't working.

**Hold Rate = `15‑second views / 3‑second views`** (retained attention among those the hook stopped).

| Hook Rate | Hold Rate | Diagnosis                                                  | Recommended Action                                                            |
| :-------- | :-------- | :--------------------------------------------------------- | :---------------------------------------------------------------------------- |
| 🟢 High   | 🟢 High   | **Complete winner** — hook and body both working           | Scale via [budget-shifter](budget-shifter.md); clone the hook archetype.     |
| 🟢 High   | 🔴 Low    | **Clickbait** — hook stops scroll but body disappoints     | **Keep the hook, replace the body.**                                          |
| 🔴 Low    | 🟢 High   | **Weak hook, strong content** — those who stay love it     | **Keep the body, swap the hook.** Lowest‑risk, highest‑leverage iteration.    |
| 🔴 Low    | 🔴 Low    | **Full failure** — hook and body both unengaging           | Pause; brief a fully new concept via [creative-brief](creative-brief.md).    |

## 4. Hook Archetype Tagging

Every scored hook is tagged with an **archetype** + **variable set** so patterns can be compared across the account.

### 4.1 Archetypes

| Archetype              | Example opener                                           |
| :--------------------- | :------------------------------------------------------- |
| **Direct Question**    | *"Are you still losing $40K a year to manual reporting?"* |
| **Negative Constraint**| *"Don't launch another ad until you've seen this."*       |
| **Social Proof**       | *"2,400 agencies switched to this last quarter."*         |
| **Direct Problem**     | *"Your CFO is drowning in CSV exports."*                  |
| **Visual ASMR**        | Satisfying visual (pour, reveal, cut) with no voiceover   |
| **Pattern Interrupt**  | Unexpected cut, color flash, or contradiction in frame 1  |
| **Character Intro**    | Founder / user on camera, direct address                  |
| **Curiosity Gap**      | *"Nobody told me this about Meta ads…"*                   |

### 4.2 Variable Extraction

For each hook, also capture:

- **Text overlay** in seconds 0–3 — presence, size, contrast/color
- **Creator persona** — founder · customer · voiceover · no‑face · animated character
- **Pattern interrupt type** — visual cut · zoom · color flash · text pop · contradictory statement
- **Scene count in 3s** — number of cuts (higher = more pattern interrupt)
- **Audio** — voiceover · music · ASMR · silence

These variables are what make iteration briefs *specific* ("reuse the handheld kitchen‑background UGC persona from `AD_388`"), not abstract ("use a testimonial").

## 5. Iteration Brief — Output Logic

Comparing archetypes by **both** Hook Rate and Hold Rate reveals recombination opportunities. The strategic output is an **iteration brief** that pairs the best stop‑power with the best retention.

**Template:**

> *"The `{winning-hook-archetype}` hook is outperforming `{other-archetype}` by X% on Hook Rate. However, `{other-archetype}` has a Y% higher Hold Rate."*
>
> **Action:** *"Produce N new variations using the `{winning-hook}` opener, transitioning at ~3s into the `{other-archetype}` body script — to maximize both stop‑power and retention."*

Only issue an iteration brief when:

- Both archetypes have ≥ 3 ads in the sample window (statistical floor).
- The Hook Rate gap is ≥ 15% and the Hold Rate gap is ≥ 10% (otherwise the recombination isn't worth the production cost).

## 6. Operational Output: Hook Audit + Iteration Brief

> **Hook Architect Audit — `Acme Ads` · 30‑day window · 42 video ads scored**
>
> **Account baseline:** Hook Rate **28.4%** (σ = 5.1) · Hold Rate **31.2%** (σ = 7.3)
>
> **🏆 Elite hooks (7)** — Hook Rate > 33.5%
>
> - `AD_388` · **Social Proof** · Hook **41.2%** · Hold **38.1%** · *"2,400 founders already made the switch"* → complete winner
> - `AD_412` · **Pattern Interrupt** · Hook **38.9%** · Hold **22.0%** → 🟡 **clickbait risk** (hook high, hold below avg) — keep hook, swap body
> - `AD_554` · **Visual ASMR** · Hook **36.1%** · Hold **44.3%** → clone the archetype
>
> **🔴 Below‑avg hooks — iteration targets (6)**
>
> - `AD_238` · **Direct Problem** · Hook **19.1%** · Hold **41.8%** 🟢 → *weak hook, strong content* — **swap hook, keep body**
> - `AD_501` · **Curiosity Gap** · Hook **17.3%** · Hold **18.9%** → full failure — pause, new concept via [creative-brief](creative-brief.md)
>
> **Archetype performance**
>
> | Archetype            | Avg Hook Rate | Avg Hold Rate | Sample (ads) |
> | :------------------- | :------------ | :------------ | :----------- |
> | Social Proof         | **34.8%** 🏆  | 32.1%         | 9            |
> | Visual ASMR          | 31.2%         | **38.4%** 🏆  | 6            |
> | Direct Problem       | 22.1%         | **39.7%** 🏆  | 8            |
> | Negative Constraint  | 29.4%         | 28.3%         | 5            |
>
> **🎯 Iteration Brief**
>
> The **Social Proof** hook archetype is outperforming **Direct Problem** by **34%** on Hook Rate (34.8% vs. 22.1%). However, **Direct Problem** has a **12% higher** Hold Rate (39.7% vs. 32.1%).
>
> **Action:** produce **3 new variations** opening with a Social Proof hook (*"2,400 agencies already switched"*) that transition at ~3s into a Direct Problem body script (*"…because their CFOs were drowning in CSV exports"*) to maximize both stop‑power and retention.
>
> **Next steps:** pass the pairing `Hook: Social Proof / Body: Direct Problem` into [creative-brief](creative-brief.md) for 3 visual concepts, then [copy-rotation](copy-rotation.md) for 3 headlines + 3 body variants per concept.
