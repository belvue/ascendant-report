# DT-010: Server Runway bar uses undifferentiated burn - not tied to real cost buckets

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (public page + funding API shape) |
| **Component** | Server Support UI - `RunwayMeter` |
| **CWE** | N/A (transparency) |

## Summary

The **Server Runway** widget shows **“X.XX months of funding remaining”** against a **6-month target**, implying donations cover **“hosting, operating, and development costs.”** The displayed value is a **single blended monthly burn rate** - it does **not** break out OVH invoices, residential game marginal cost, or tooling/dev time. Donors treating the bar as **“server maintenance runway”** may over-read how much of each dollar funds **datacenter uptime vs home-hosted gameplay**.

## Example

**UI copy (observed on `/server-support`):**

> “This reflects how many months of **hosting, operating, and development costs** are currently covered by community donations. Our target is **6 months** of runway. Costs are estimated - actual expenses may vary.”

**Implementation (public frontend, observed):**

- Fetches `GET /api/server-status/funding` → `{ months, amount, monthlyRate, updatedAt }`.
- Progress bar scales `months / 6` with health labels (Critical / Low / Moderate / Healthy).
- If API fails, UI falls back to a **hardcoded default** (~2.68 months) - see **DT-022**.

**What's missing from the meter:**

| Donor question | Shown today? |
|----------------|--------------|
| What monthly burn includes | No - single `monthlyRate` |
| OVH vs residential game split | No |
| Which costs are **fixed** vs **optional** (dev time) | No |
| Whether “runway” means **game uptime** or **whole project** | Ambiguous |

**Cross-ref:** Game world on residential AT&T ([RH-010](../../02-residential-host-brief/P1-high/RH-010-residential-game-hosting.md)); OVH for web/CDN ([RH-011](../../02-residential-host-brief/P1-high/RH-011-hosting-topology-disclosure-gap.md)).

## Player scenario

**Context:** Recurring Ko-fi supporter checks Server Support to see if the server is “funded.”

**What happens:**

1. Runway reads **“Healthy”** at ≥4 months on a unified burn number.
2. Player interprets this as **paid hosting runway for the live world**.
3. A large share of **game compute** may not be a recurring datacenter line item - bar may **overstate** infrastructure urgency or **under-explain** where reserves go.

## Implications

- **Expectation mismatch:** Donors giving for **maintenance and uptime** deserve a runway tied to **categories they fund**, not one opaque divisor.
- **Good-faith path:** Operator can keep a summary bar **if** sub-lines explain the math.

Pairs with **DT-001**, **DT-011**, **DT-022**.

## Remediation

1. **Replace or supplement** the single bar with **stacked or labeled segments**, e.g.:
   - **Cloud (OVH web/CDN)** - $X/mo, Y months covered
   - **Login / shared portal** - $X/mo (if applicable)
   - **Game world (residential)** - marginal cost note (power/ISP uplift, not datacenter rent)
   - **Development / tooling** - optional line, clearly labeled non-uptime
2. Rename headline if needed: **“Operating reserve”** vs **“Server hosting runway”** - pick language that matches the formula.
3. Document **formula** inline: `reserve_balance ÷ monthly_burn = months` with **what is in monthly_burn**.
4. Tie **“Healthy”** thresholds to **documented targets per category**, not only a 6-month aesthetic goal.

## Verification

- Donor can explain **one line item** in the monthly burn without asking staff.
- Runway text distinguishes **cloud bills** from **home-hosted game** costs.
- Updating OVH pricing or topology triggers a **dated changelog** on Server Support.
