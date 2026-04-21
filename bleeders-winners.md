---
name: bleeders-winners
description: "Automates the Stop/Scale cycle by identifying ads burning budget (Bleeders) and ads ready for aggressive scaling (Winners), then triggering the corresponding operational workflow."
metadata:
  version: 1.0.0
---

## 1. Detection Logic

### 🩸 Bleeder Signal (Stop Logic)
An ad is flagged as a **Bleeder** if it hits **any** of these red flags:
| Signal | Threshold | Meaning |
| :--- | :--- | :--- |
| **Low Engagement** | CTR < 1.0% | The audience is ignoring the ad. |
| **Ad Fatigue** | Frequency > 3.5 | The same people are seeing it too often. |
| **Wasted Spend** | > $10 spent with 0 conversions (or 1× Target CPA with 0 conversions) | Budget burning with no return. |
| **Inefficiency** | CPA > 2× Target CPA | Conversions exist but are unprofitable. |

### 🏆 Winner Signal (Scale Logic)
An ad is flagged as a **Winner** only if it meets **all** of these green flags:
| Signal | Threshold | Meaning |
| :--- | :--- | :--- |
| **High Engagement** | CTR > 2.0% | The audience is responding. |
| **Efficient Cost** | CPC < 50% of account average | Delivery is cheap relative to peers. |
| **Proven Results** | > 10 conversions | Enough volume for statistical significance. |
| **Scaling Room** | Frequency < 2.5 | Audience is still fresh — room to scale. |

## 2. Automated Action Workflow
| Status | AI Action | Strategic Intervention |
| :--- | :--- | :--- |
| **Bleeder** | Flag for "Immediate Pause" | Verify if the ad is < 3 days old (new ads need time to stabilize). |
| **Winner** | Suggest 20% Budget Increase | Confirm inventory/stock levels can handle increased sales. |
| **Winner** | Flag for "Creative Extraction" | Analyze the visual/hook to replicate it in new variants. |

## 3. Operational Output: The "Daily Briefing"
> **Bleeder Alert:** Ad `AD_ID_412` (**Spring Sale Carousel**) is burning budget.
> - **CTR:** 0.7% (🔴 below 1.0% floor).
> - **Spend:** $24 with 0 conversions.
> - **Action:** Pause `AD_ID_412` immediately (ad is 6 days old — past stabilization window).
>
> **Winner Alert:** Ad `AD_ID_388` (**Founder UGC Hook**) is ready to scale.
> - **CTR:** 2.9% · **CPC:** 42% of account avg · **Conversions:** 17 · **Frequency:** 1.8.
> - **Action:** Increase budget by 20% (confirm stock), and queue for Creative Extraction to replicate the hook in new variants.
