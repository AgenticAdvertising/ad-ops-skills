# Skill 1: The Hook Architect
**Objective:** To isolate the first 3 seconds of ad delivery as a standalone variable, optimizing for "Thumb-Stop" efficiency and creative velocity.

---

## 1. The Hook Diagnostic
This skill evaluates if a creative is earning its place in the auction. If the Hook Rate is low, the ad is "invisible" regardless of the offer.
* **Metric:** Hook Rate ($3\text{-second views} \div \text{Impressions}$).
* **The "3-Day Launch" Rule:** Ignore Day 1 fluctuations. Evaluate on Day 3. If Hook Rate is $<15\%$, the visual/hook is the failure point.

## 2. Creative "Hand-off" Logic
Analyzes the bridge between the hook and the conversion narrative.
* **Hold Rate:** ($\text{ThruPlays} \div 3\text{-second views}$). 
* **Diagnosis:**
    * **High Hook + Low Hold:** Clickbait or "Pattern Interrupt" that doesn't align with the product.
    * **Low Hook + High Hold:** Great story, bad "packaging." **Action:** Keep the body, wrap it in a "High-Contrast" hook style.

## 3. Creative Intelligence (The Matt Berman Method)
* **Style Benchmarking:** Compare creative formats (e.g., *Notes App* vs. *UGC* vs. *Aesthetic*).
* **Iteration Briefing:** Automatically suggest "Hook Swaps" for high-hold ads. 
    * *Example:* "Ad_01 has a top-tier 45% Hold Rate but a bottom-tier 12% Hook Rate. Repackage this body copy with the 'Visual ASMR' hook from our current winner."

---
---

# Skill 2: The Fatigue Sentinel
**Objective:** To detect "Creative Decay" early using consecutive signal tracking, preventing budget waste before ROAS actually crashes.

---

## 1. Fatigue Signals (Hierarchy of Urgency)
Based on the Meta Ads Kit methodology, signals are weighted by how early they appear:

1.  **CTR Decay (Most Critical):** Tracking a **20%+ decline** over **3 consecutive days**. This is the first sign of audience "blindness."
2.  **Impression Decline (Auction Priority):** If Impressions drop $>30\%$ while spend stays flat, Meta is deprioritizing the ad because of poor "User Value" (Fatigue).
3.  **Frequency Creep:** Flagging when Frequency exceeds **3.0 - 3.5** (depending on audience size).
4.  **CPC Inflation:** A **15-20%+ increase** in CPC over 3 days (non-seasonal).

## 2. Severity Matrix
| Level | Criteria | AI Action |
| :--- | :--- | :--- |
| **🟡 Warning** | Freq > 3.0 OR 10% CTR Drop | Flag for creative refresh in next 48 hours. |
| **🔴 Critical** | 20%+ CTR Decay (3 days) OR Impression Drop | **Pause Ad** or reduce budget by 50% immediately. |

## 3. The "Reset" Strategy
* **Memory Reset:** If a winner fatigues, log it in `learnings.md` and "rest" it for 2-4 weeks. Re-test once the audience "memory" resets.
* **Lifespan Prediction:** Logs the average days-to-fatigue per creative type (e.g., "Notes App ads last ~10 days; Testimonials last ~21 days").

## 4. Operational Output: The "Daily Briefing"
> **Fatigue Alert:** Ad `AD_ID_238` (**Notes App Hero**) is dying. 
> - **CTR Trend:** 3.2% → 2.8% → 2.1% (🔴 34% drop). 
> - **Frequency:** 3.8. 
> 
> **Action:** Pause `AD_ID_238`. Deploy replacement from 'The Hook Architect' queue (Style: 'Testimonial Variant') to maintain delivery volume.
