# DT-001: “Hosting costs” messaging does not match split infrastructure

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (public page + hosting probe) |
| **Component** | Governance / Server Support · Ko-fi |
| **CWE** | N/A (transparency) |

## Summary

Ascendant’s **Server Support** page and Ko-fi link ask donors to help with **“hosting, operating, maintenance, tooling, and development.”** Probe-confirmed infrastructure is **split**: **OVH US** hosts website, CDN, and patch downloads; the **live game world** runs on **residential AT&T** with near-zero marginal compute cost. Average donors reasonably infer **datacenter game hosting** and **paid uptime/maintenance** - not a blended appeal that includes home broadband and operator time.

**Player-facing copy** lives in [player-donation-notice-v1.md](../player-donation-notice-v1.md) and [player-donation-acknowledgement-block.md](../player-donation-acknowledgement-block.md).

## Example

**Public copy (Server Support page, observed):**

> “…help cover the ongoing costs of running Ascendant EQ - things like **hosting**, operating, maintenance, tooling, and development.”

**Infrastructure map (cross-ref [RH-011](../../02-residential-host-brief/P1-high/RH-011-hosting-topology-disclosure-gap.md)):**

| Layer | Provider | Donor-facing clarity today |
|-------|----------|----------------------------|
| Website / CDN / downloads | OVH US | Implied by “hosting” |
| **Live game world** | AT&T residential | **Not stated** |
| Federation login | OVH Canada | Separate brief (01) |

**Gap:** Donation materials do not separate **cloud bills donors plausibly fund** from **home-hosted game compute** donors may believe they fund.

## Player scenario

**Context:** Player reads Server Support or Ko-fi before donating to “keep the server online.”

**What happens:**

1. Copy references **hosting** and **operating** costs without topology.
2. Player assumes donations maintain **datacenter uptime** for gameplay.
3. Game world actually runs on **residential ISP** - different reliability model.

**Not an exploit:** Informed-consent gap for donors.

## Implications

- **Donor expectations:** Community support framed as **server maintenance and uptime** may not match where marginal hosting spend occurs.
- **Trust:** Material optics gap when residential hosting is discovered elsewhere.
- **OVH spend is real** for web/CDN - partially consistent with appeals; the gap is **unsplit messaging**, not “zero costs exist.”

Pairs with **DT-010**, **DT-011**, **RH-011**.

## Remediation

1. Publish a **cost-category table** on Server Support: OVH web/CDN (monthly range), login stack (if billed separately), residential game (marginal ISP/compute note), optional dev/tooling line.
2. Replace generic **“hosting costs”** with **“infrastructure & operations”** and link to [02 hosting topology diagram](../../02-residential-host-brief/P1-high/RH-011-hosting-topology-disclosure-gap.md).
3. Ko-fi description mirrors the same split - no separate story on Ko-fi vs website.
4. Embed [player-donation-acknowledgement-block.md](../player-donation-acknowledgement-block.md) before external donate links.

## Verification

- New donor can state **what category their payment supports** without reading DNS or forum threads.
- Server Support and Ko-fi updated on the same date with matching language.
