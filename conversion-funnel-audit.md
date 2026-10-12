---
name: conversion-funnel-audit
description: "Audits the conversion funnel from the site's own analytics (GA4, PostHog) next to ad spend: where visitors drop off, whether a weak result is the ad, the landing page or a miscounted conversion event, what to scale, what to stop targeting and whom to retarget."
metadata:
  version: 1.0.0
---

## 1. Purpose

Find out where paid and organic visitors drop off before converting, and
why. The output is a short list of changes, each tied to the numbers that
justify it.

Ad platforms grade their own homework: each one counts the conversions it
can claim. The site's analytics (GA4, PostHog) count what actually happened
on the site. This skill puts the two side by side, checks that the
conversion is counted correctly, then follows the visitors down the funnel.

Every finding names its evidence: the source, the date range and the counts.
"Meta looks weak" is not a finding. "Meta: 3,578 sessions with an fbclid, 1
sign-up, 60 days" is.

## 2. Data Needed

| Need | From | In Adside |
| :--- | :--- | :--- |
| Connected channels and sync health | Each integration | `list_channel_connections` |
| Site events, key events, purchases by traffic source and page | GA4 | `query_conversion_capture` with `source: "ga4"` (reports `event_by_traffic`, `key_event_by_page`, `purchase_by_transaction`) |
| Sessions by entry source and click ID; conversions by sender, host and page; a sample of conversion rows | PostHog | `query_conversion_capture` with `source: "posthog"` (reports `event_by_traffic`, `sessions_by_entry`, `conversions`, `conversion_events`) |
| Spend, clicks, CTR, CPC by channel, campaign and ad | Ad platforms | `query_ad_performance` |
| Platform-reported conversions, and the event each channel optimises toward | Ad platforms | `reporting_conversions`, `get_channel_conversion_mappings` |
| Audience breakdowns (age, gender, placement, region, device) | Ad platforms | `reporting_query` type `demographics`, or the platform's insights breakdowns |

GA4 or PostHog is required. With neither connected, stop: say what to
connect, and offer the ad-platform-only parts of §6–§8, labelled as
platform-reported.

## 3. Ground Rules

- **Check the counting before the funnel.** A broken or double-counted
  conversion event looks exactly like a bad ad or a bad page. Run §4 in full
  before you blame anything.
- **Site counts and platform counts never match.** They use different
  attribution windows, view-through credit, time zones and dedup rules. A gap
  is normal. A gap that **changed**, or a ratio far outside the rest of the
  account, is the finding.
- **Minimum volume before any verdict.** Do not call a segment good or bad
  until it has at least **100 sessions**, or spend of at least **3× the
  account's cost per conversion**, or **20 conversions**. Below that, say
  "too little data" and give the count.
- **Leave out the last 2 days.** GA4 needs 24–48 h to settle traffic sources,
  and both imports refetch the last 3 days. End the range 2 days ago unless
  the user asks about today.
- **Default range:** the last 30 days (60 if the account spends under $3,000
  a month), compared with the period just before it.
- **Find the conversion first.** List the custom events (PostHog) and key
  events (GA4), order them into a funnel (landing → intermediate steps →
  the money event: purchase, paid, booked call, qualified lead). If two
  events could be the final conversion, ask the user which one counts. Note
  any ad channel optimising toward a different event than that one.
- **Judge each campaign by the conversion it is built for.** A lead or demo
  campaign landing on a booking page is judged on bookings, not sign-ups. A
  sign-up campaign is judged on sign-ups. When a campaign's own conversion
  can't be attributed (see §4), its verdict is **Can't tell**. Never fill
  the gap with a different event: a demo page will always look bad on
  sign-ups.
- **Every number in the answer is computed, not estimated.** Recompute
  ratios and "N× better" from the table before writing them, and quote each
  pair separately rather than as a range that doesn't hold. A statement the
  data didn't show (an audience's retention window, what a pixel fires on)
  is labelled as an assumption to check.
- **No personal data in the answer.** Counts and click-ID presence only:
  never a person, an email or a full URL.
- **Read-only.** This skill recommends. Pausing, excluding an audience or
  building one is a separate request: state the change and get a go-ahead
  first.

## 4. Counting Health

Run every check below and list all eight rows, even when a check could not
run (then it is **Can't tell**, with the reason). Report each as **OK**,
**Problem** or **Can't tell**, with the numbers. The checks are about the
site's own conversion events; the ad platform's pixel is covered only by the
last row.

| Check | How | Problem when |
| :--- | :--- | :--- |
| **Event still fires** | Conversion count by day | A day at 0 between non-zero days, or down > 50% vs the previous period while sessions held |
| **Marked as a key event** | GA4 events by `is_key_event` | The event exists but is `false` or `(not set)`. GA4, and Google Ads imports from it, count nothing |
| **Double counting** | GA4 purchases by transaction id; PostHog conversions by sender (`lib`) | A transaction id counted more than once, or the same conversion sent by both `web` and a server library at similar daily volume |
| **Attributable** | A sample of PostHog conversion rows | > 30% carry no session, no entry UTM and no click ID. Usually a server-side event sent under an id the browser never shared. Those conversions can't be tied to any ad |
| **Click IDs survive** | Sessions by entry UTM source and click-ID type | Under 80% of sessions tagged as a paid channel carry that channel's click ID: a redirect or link shortener strips it |
| **Clicks arrive** | Platform link clicks vs site sessions from that channel | Sessions under 60% of link clicks: slow page, broken link, or no analytics snippet on the landing page |
| **Test traffic** | Events by host | Conversions on localhost, preview or staging hosts. Exclude them from every later step |
| **Platform vs site** | Find which platform event each channel counts (its conversion mapping), match it to the site event that means the same thing, then compare counts from that channel | The platform claims > 2× the site's count for the **same** event. Check that the pixel fires on the final step (not on page view or form start) and how much is view-through. Name the ratio. If the platform event has no site counterpart, or the counterpart can't be attributed, this row is **Can't tell**: comparing a platform "Lead" with site sign-ups when leads are demo bookings is not an overcount |

Later sections must not treat a broken count as real performance. If a lead
pixel overcounts, that channel's cost per lead is wrong too.

## 5. Funnel and Drop-off

Build the funnel for the whole site, then per channel: sessions (with
bounces) at the top, then each funnel event. For each step show the count,
the step-to-step rate and the rate from the first step.

The **biggest drop-off** is the step that loses the most visitors in absolute
terms. The **weakest step** is the one with the lowest step-to-step rate.
Name both when they differ, and compute both from the table.

Split the worst step by **channel** (entry channel, UTM source, click-ID
type), **landing page** (GA4 key events by page; PostHog conversion page)
and **campaign** (UTM campaign, or GA4's Google Ads campaign id). A step that
is bad for every channel points at the page or the product. A step that is
bad for one channel points at that channel's ads or audience.

## 6. Diagnosis: Ad, Landing Page or Tracking?

Decide per channel and per major campaign. Go through the rows in order and
stop at the first that fits. Tracking comes first because it can look like
either of the others.

| Verdict | Pattern in the data |
| :--- | :--- |
| **Tracking** | Any §4 Problem that touches this channel, or conversions stopped on a date while sessions did not |
| **Traffic quality / audience** (ad side) | Platform clicks are healthy, but sessions bounce > 85%, average under 1.2 pages, or come from a device or country the product doesn't serve. Other channels landing on the same page convert |
| **Ad-to-page mismatch** (ad side) | Visitors engage (2+ pages), but the ad promises something the page doesn't lead to: a demo-led ad on a self-serve sign-up page, an offer the page doesn't show |
| **Landing page** | The same page converts badly for every channel, or much worse than another page getting the **same kind of traffic**. Cold prospecting and retargeting are not the same traffic: retargeting converts better on any page. When page and audience change together, the verdict is "page or audience, can't separate", and the next step is to split cold traffic between the two pages |
| **Ad** (creative / targeting) | Low CTR or high CPC against the account's own other campaigns, before anyone reaches the site |
| **Working** | Conversion rate from the channel's sessions at or above the site average, with enough volume |

Show the evidence for each verdict, and say what would confirm it.

## 7. What Is Working: Scale

Rank channels, then campaigns, by **site-counted conversions per dollar**,
with the platform-reported figure beside it. Join spend to site conversions by
click ID and UTM campaign (exact when ads use dynamic UTM macros such as
`{{campaign.id}}` / `{{ad.id}}`); use names only when the user confirms the
match. Include unpaid sources (search, partners, directories, AI assistants):
they show who converts, which decides where to spend next.

To **double down**, a segment needs a site cost per conversion at least
**30% better** than the account's, enough volume, and a conversion that passed
§4. For the audience inside a winning campaign (age, gender, placement,
region, device), use the platform's breakdowns, and say these are
platform-reported, checked against the site total for that campaign.

If the account's ads carry no campaign or ad ids in their UTMs, recommend
dynamic UTM macros. It is the cheapest attribution fix there is.

## 8. Stop Targeting and Retarget

**Stop targeting.** A segment qualifies when it has spent at least 3× the
account's site cost per conversion, or sent at least 300 sessions, with no site
conversion. A bounce rate above 90%, or under 1.2 pages per session, also
qualifies. Candidates: campaigns or ad sets with spend and no site
conversions; placements, devices or regions that take spend but never
convert; audiences that click and bounce. Give the spend each would have saved
over the range. If §6 said **Tracking**, say "fix the measurement first"
instead: pausing on a broken count kills a campaign that may be working.

**Retarget.** Estimate each pool from the counts (aggregates, so present them
as estimates), then recommend the platform audience that reaches it (a
website custom audience by URL or event):

| Pool | Estimated size | Message |
| :--- | :--- | :--- |
| Engaged, did not start | Non-bounced sessions − first-step events | Proof and the offer |
| Started, did not finish | First funnel event − final conversion | Remove the blocker found in §5 |
| Converted, not upgraded | Free conversion − paid conversion | The upgrade |
| Paid-click bouncers | Bounced sessions from paid channels | Usually exclude, not retarget |

Exclude people who already converted from acquisition campaigns, and say how
many there are.

## 9. Operational Output: Funnel Audit

Sections in this order, under one screen where possible:

1. **Headline**: three sentences. The biggest drop-off, its most likely cause,
   and the change with the most money behind it.
2. **Counting health**: the §4 table.
3. **Funnel**: step counts and rates, whole site, worst channel beside best.
4. **Diagnosis**: per channel, the §6 verdict and its evidence.
5. **Scale**: the §7 ranking, top rows only.
6. **Stop**: each candidate with the spend it would have saved.
7. **Retarget**: each pool with its size and the audience to build.
8. **What to fix next**: up to five actions, ordered by money at stake.
   Tracking fixes come first when any check failed.
9. **Limits**: what the data could not show, and what would close the gap.

Exact counts; currency in the account currency; rates as percentages with one
decimal. Never round a small count up to a trend.

## 10. Example Output

Illustrative numbers.

> **Funnel audit — Sep 10 to Oct 9 (30 days, vs the 30 before)**
>
> **Headline:** Meta's demo campaigns ($9,400, 47% of spend) can't be judged
> yet: `book_call` reaches PostHog with no session, so no booking ties to an
> ad, and Meta's 164 "Lead" count can't be checked. On sign-ups, Google Search
> works ($41 per sign-up) and Google Display doesn't (0 sign-ups from 1,380
> sessions, $1,960). The change with the most money behind it is making
> bookings attributable.
>
> **Counting health**
>
> | Check | Result |
> | :--- | :--- |
> | Event still fires | OK: `signup` every day, 18–41 a day |
> | Marked as a key event | Problem: `book_call` is not a GA4 key event |
> | Double counting | OK: one sender per event, no repeated transaction ids |
> | Attributable | Problem: 100% of 63 `book_call` events are server-only with no session |
> | Click IDs survive | OK: 93% of Meta-tagged sessions carry an fbclid |
> | Clicks arrive | OK: 71% of Meta link clicks became sessions |
> | Test traffic | Problem: 37 sign-ups on a staging host, excluded below |
> | Platform vs site | Can't tell: Meta's "Lead" is the booking, and bookings can't be attributed |
>
> **Funnel (whole site)**: 9,812 sessions → 1,204 `signup` (12.3%) →
> 611 `onboarding_done` (50.7%) → 88 `paid` (14.4%). Biggest loss and weakest
> step: session → sign-up (8,608 lost, 12.3%). After sign-up, onboarding loses
> the most people (593).
>
> **Diagnosis**
>
> - Meta (demo campaigns): **Can't tell.** Their conversion is the booking,
>   which can't be attributed. Clicks do reach the page (71%). Its 86% bounce
>   rate may be normal for a booking page; judge it after the fix.
> - Google Search: **Working.** 9.8% session → sign-up, 212 sign-ups, $41 per
>   sign-up.
> - Google Display: **Traffic quality.** 1,380 sessions, 94% bounce,
>   0 sign-ups, while Search traffic to the same page converts.
>
> **Stop**: Google Display, $1,960 over 30 days with 0 sign-ups. Meta is held
> until bookings are measurable.
>
> **Retarget**: about 593 people signed up but did not finish onboarding.
> Build a website custom audience on `/onboarding` visitors, excluding `/paid`.
>
> **What to fix next**
>
> 1. Send `book_call` with the visitor's analytics id so bookings can be
>    attributed, and mark it as a GA4 key event.
> 2. Check that Meta's "Lead" fires on the booking confirmation, not on form
>    start (an assumption until checked in Events Manager).
> 3. Exclude the Google Display network ($1,960 over 30 days, no sign-ups).
> 4. Keep the staging host out of the production analytics project.
>
> **Limits**: device split comes from Meta's breakdown; the site data has no
> device field.
