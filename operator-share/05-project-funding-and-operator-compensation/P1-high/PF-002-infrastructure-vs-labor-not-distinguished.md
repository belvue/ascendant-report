# PF-002: Infrastructure costs vs labor effort are not distinguished

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (public copy + patch-note context) |
| **Component** | Governance / burn-rate definition |
| **CWE** | N/A (disclosure) |

## Summary

Public materials invite support for **development** and **operating** costs but do **not** define whether **operator, developer, GM, or steward time** is treated as:

- Unpaid volunteer effort,
- Reimbursed from donations,
- Implicitly subsidized by donations via reduced personal out-of-pocket, or
- A separate non-donor expense.

Contributors may reasonably interpret “development costs” as **vendor/tooling bills** or as **paying people** - the site does not resolve that ambiguity.

## Example

**Observed appeals include:**

- “**tooling, and development**”
- “**maintenance**” and “**operating**” costs
- Runway covers “**hosting, operating, and development costs**” (**DT-010**)

**Operator communications (paraphrased, Mar 2026 patch notes):** Donations described as reducing personal **financial burden** and **time commitment** of running the project - suggests labor is a real cost axis, but **no published policy** states whether donations compensate labor.

**Steward program:** Public steward-voting page exists; **no** published line on whether stewards receive funds.

## Player scenario

**Context:** Contributor asks whether donations pay staff.

**What happens:**

1. Official text mentions **development** without a labor policy.
2. Community may assume either **pure volunteer** or **hidden payroll** - both are speculation.
3. A short published policy would collapse ambiguity.

**Not claimed:** Undisclosed wages - **definition gap**.

## Implications

- **Trust:** Labor is often the largest hidden cost in volunteer-run servers; silence invites worst-case assumptions.
- **Pairs with PF-005** (compensation policy), **PF-006**.

## Remediation

1. State explicitly: donations **do / do not** fund operator or volunteer **time**.
2. If “development” means **hosting + code + content** only, list examples (VPS, SaaS, assets) vs **hours**.
3. Include labor line in published **monthly burn breakdown** - even if **$0** compensated.

## Verification

- Server Support answers: “Are any team members paid from Ko-fi?” in **one paragraph**.
- Runway `monthlyRate` tooltip lists whether **labor** is in the divisor.
