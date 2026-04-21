---
name: fatigue-detector
description: "This skills detects 'Creative Decay' early using consecutive signal tracking, preventing budget waste before ROAS actually crashes"
metadata: version: 1.1.0
---

## 1. Detection Signals (The Fatigue Matrix)
| Signal | Indicator | Logic / Action |
| :--- | :--- | :--- |
| **Frequency Creep** | Freq > 2.2 (7-day) | Analyze if CTR is dropping. If yes, the audience is "blind" to the ad. |
| **CPM Inflation** | +20% Cost/M | High frequency + rising CPM = Meta's algorithm is penalizing "stale" content. |
| **CTR Erosion** | 7-day CTR < 30-day CTR | The creative is exhausted. Even if ROAS is stable, a crash is imminent. |
| **Sentiment Shift** | Negative Comments | Scans for "Seen this 100 times" or "Stop showing me this." |

## 2. Severity Matrix
| Level | Criteria | AI Action |
| :--- | :--- | :--- |
| **🟡 Warning** | Freq > 3.0 OR 10% CTR Drop | Flag for creative refresh in next 48 hours. |
| **🔴 Critical** | 20%+ CTR Decay (3 days) OR Impression Drop | **Pause Ad** or reduce budget by 50% immediately. |

## 3. Operational Output: The "Daily Briefing"
> **Fatigue Alert:** Ad `AD_ID_238` (**Notes App Hero**) is dying. 
> - **CTR Trend:** 3.2% → 2.8% → 2.1% (🔴 34% drop). 
> - **Frequency:** 3.8. 
> **Action:** Pause `AD_ID_238`. Deploy replacement from 'The Hook Architect' queue (Style: 'Testimonial Variant') to maintain delivery volume.
