# PF-004: Capital hardware vs monthly operating costs not explained

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (hosting topology; no public hardware ledger) |
| **Component** | Infrastructure - capital vs opex |
| **CWE** | N/A (disclosure) |

## Summary

The live game world runs on **residential consumer broadband** (**RH-010**) - not a recurring datacenter line item. Public funding materials do not explain whether donations may fund:

- **Capital** purchases (PC, RAM, storage upgrades),
- **Hardware replacement reserves**, or
- **Only** monthly services (cloud, domains, SaaS, ISP uplift).

Without that split, contributors may fund appeals framed as **“hosting”** while unsure if they are subsidizing **one-time hardware** vs **monthly bills**.

## Example

**Observed infrastructure:**

| Cost type | Example | Disclosed as donation category? |
|-----------|---------|--------------------------------|
| Monthly cloud (OVH web/CDN) | Recurring vendor bill | Implied |
| Residential ISP / power uplift | Marginal home hosting | **No** |
| Game server hardware | Consumer workstation at operator premises | **No public spec or policy** |
| Hardware refresh fund | Future replacement | **No** |

**Note:** This package does **not** cite specific CPU/RAM specs - none are published on Server Support. Finding is **methodology**, not hardware audit.

## Player scenario

**Context:** Donor wants to fund **uptime**, not **someone’s gaming PC**.

**What happens:**

1. Appeals say **hosting** without capital vs opex split.
2. Donor cannot tell if runway includes **amortized hardware** assumptions.
3. A one-line capital policy resolves expectation mismatch.

**Not claimed:** Donations bought operator hardware - **categorization gap**.

## Implications

- Pairs with **PF-001**, **DT-001**, **RH-010**, **RH-011**.

## Remediation

1. State whether donations ever fund **hardware capital** or only **services**.
2. If hardware is operator-owned, say so; if community-funded upgrades occur, publish **policy + band** (not necessarily receipts).
3. Separate **monthly opex runway** from **hardware reserve** in UI or text.

## Verification

- Server Support answers capital vs opex in a **dedicated bullet**.
- Runway formula docs list whether **hardware depreciation** is in `monthlyRate`.
