---
name: copy-rotation
description: "Generates Meta ad copy matched to the visual and to the account's proven winners — each variant hits a different psychological angle, reinforces the image instead of repeating it, and respects proven length/opener/closer specs."
metadata:
  version: 1.3.0
---

## 0. Source Ad(s) — anchor before generating

Copy rotation always generates variants for ONE OR MORE specific source ads (the ones being rotated), not for an abstract "ad idea". Before §1 and §2:

1. **Identify the source ad(s)** the user wants rotated. Common sources:
   - An ad the user named (e.g. `/copy-rotation @AdX`)
   - The fatigued ads surfaced by a recent `/fatigue-detector` run
   - A specific winner from §2's audit
2. **Look up each source ad's actual creative image** — the visual the ad is currently running — and stash it with that ad before writing any copy.
3. **Bind variants to source ad**: every variant in §6 inherits the SAME source ad's image — the whole point of rotation is to swap copy on a known visual. When you render the variants for the user, every preview MUST display the source ad's real image. Empty image placeholders read as a broken feature.

If the user is rotating multiple source ads at once, treat each ad's variants as its own pack — variants for ad A use ad A's image, variants for ad B use ad B's image.

## 1. Core Principle

**Copy reinforces the image. It never repeats it.** Each visual gets its own psychology, opener, and voice — driven by what the image shows (notes app, receipt, chart, tweet, product shot) and where it sits in the funnel. If the copy and image would read the same out loud, the copy is wrong.

## 2. Audit Account Winners First

Before writing a single word, pull the account's top‑converting ads and extract the **pattern signature** they share. Match the signature — don't invent a new one.

**Extract from winners:**
| Dimension | What to capture |
| :--- | :--- |
| **Headline shape** | Avg char length · questions vs. statements vs. numbers vs. commands |
| **Body opener** | Pain · data · question · story |
| **Social proof** | Customer counts, % lifts, revenue figures that actually appear |
| **Reading level** | Short‑punchy vs. narrative |
| **Winning CTA** | "Learn More" vs. "Get Started" vs. "Book Demo" |

**Report the signature back before generating copy:**

> _Your top 3 ads all open with a specific number and close with social proof. Average headline: 32 chars. Winning CTA: "Book Demo". I'll match that pattern._

Then every variant generated in §6 must be cross‑checked against this signature.

## 3. Psychology Framework — The Four Horsemen

Every variant must hit exactly one Horseman. Across a variant pack, cover at least three different ones.

| Horseman      | Trigger                          | Opening Pattern                      | Best‑Fit Visuals                      |
| :------------ | :------------------------------- | :----------------------------------- | :------------------------------------ |
| **💰 Money**  | Revenue or cost savings          | "Every [thing] you miss is worth $X" | Receipts, pricing cards, calculators  |
| **⏱ Time**    | Wasted hours, efficiency         | "Your [person] isn't a [wrong role]" | Slack / notes app, calendars, timers  |
| **👑 Status** | Peer comparison                  | "X+ [peers] already [did thing]"     | Charts, leaderboards, logo walls      |
| **⚠️ Fear**   | Loss aversion, being left behind | "The [competitor] already has this"  | Tweets, news‑style, red/alert visuals |

## 4. Copy Specifications

| Element        | Spec                                          | Why                                           |
| :------------- | :-------------------------------------------- | :-------------------------------------------- |
| **Headline**   | 25–40 chars (max 50)                          | Mobile truncation caps CTR above 50.          |
| **Body**       | 50–120 words                                  | Enough to persuade, short enough to scan.     |
| **Paragraphs** | 2–3, each 1–2 sentences                       | Mobile scannability.                          |
| **Numbers**    | ≥ 1 specific number per variant ($, %, count) | Specificity beats adjectives for credibility. |
| **Opener**     | Pain, stat, or claim                          | Never the brand name or "Introducing".        |
| **Closer**     | Social proof + soft CTA                       | "X+ made the switch" beats "Buy now".         |

## 5. Funnel Stage Adjustments

| Stage    | Objective             | Copy Focus                    | CTA                          | Proof Level                  | Body Length  |
| :------- | :-------------------- | :---------------------------- | :--------------------------- | :--------------------------- | :----------- |
| **ToFu** | Awareness / traffic   | Problem framing, curiosity    | Learn More                   | Light — "X+ use this"        | 50–80 words  |
| **MoFu** | Consideration / leads | Solution proof, features      | Get Free Audit / See Demo    | Medium — case‑study snippets | 80–120 words |
| **BoFu** | Conversion / sales    | Urgency, risk reversal, price | Start Free Trial / Book Demo | Heavy — ROI, testimonials    | 60–100 words |

## 6. Variant Generation Rules

- Produce **3–5 headlines** and **3–5 body variants** per creative.
- Each variant hits a **different Horseman** — no two variants share the same psychology.
- **Mix headline structures**: at least one question, one statement, one number‑led, one command.
- **At least one variant must feel like organic content** (a friend's text, a tweet, a review) — not an ad.
- Match the chosen Horseman to the image format (see §3).
- Cross‑check every variant against the account signature from §2: headline length, opener type, social‑proof style, CTA.

## 7. Forbidden Words & Anti‑Patterns

**Banned vocabulary (default):** revolutionary, game‑changer, cutting‑edge, innovative solution, transform your business, unlock potential, unleash, harness power, leverage, next‑level, best‑in‑class, world‑class, seamless, robust, synergy.

**Anti‑patterns:**

- Reusing the same body across creatives.
- Two variants hitting the same Horseman.
- Starting with the brand name or "Introducing…".
- Emoji lists in place of a real hook.
- Copy that describes what the image already shows.
- Identical headline structures across a pack.
- Hard‑sell CTAs on ToFu ads.

## 8. Operational Output: Variant Pack

> **Account signature (§2):** top 3 ads open with a specific number, close with social proof. Avg headline: 32 chars. Winning CTA: **Book Demo**.
>
> **Creative:** `IMG_554` — _Notes‑app screenshot: "Things my agency does that my CFO hates"_ · **Stage:** MoFu · **Assigned Horsemen:** Money · Time · Status
>
> **Headlines**
>
> 1. `Your CFO is not a spreadsheet.` — 28 chars · ⏱ Time
> 2. `$40K in ad spend, 0 reports.` — 28 chars · 💰 Money
> 3. `217 agencies switched last quarter.` — 35 chars · 👑 Status
>
> **Body variants**
>
> 1. _💰 Money_ (86 words) — Opens on wasted spend ($40K / 0 reports), closes on "217 agencies made the switch → See a 2‑min demo".
> 2. _⏱ Time_ (74 words) — Opens on CFOs pasting CSVs at 11pm, closes on "Free yourself from Monday exports → Get a free audit".
> 3. _👑 Status_ (92 words) — Opens on peer benchmark ("The top 10% of agencies report in minutes, not days"), closes on case‑study snippet + "Book a demo".
