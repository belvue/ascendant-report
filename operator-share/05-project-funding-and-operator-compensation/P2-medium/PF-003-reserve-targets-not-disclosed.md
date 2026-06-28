# PF-003: Reserve targets and rationale not explicitly disclosed

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (runway UI + API shape) |
| **Component** | Server Support - reserves / runway |
| **CWE** | N/A (disclosure) |

## Summary

The Server Runway widget targets **6 months** of coverage and displays **months remaining**, but public pages do **not** explain:

- What **reserve balance** the bar represents,
- How **monthly burn** is estimated,
- Why **6 months** is the goal, or
- Whether surplus donations **must** stay in reserve vs may be spent.

**Reserve accumulation is legitimate.** The finding is whether contributors understand **reserve policy**.

## Example

**UI copy (observed):**

> “Our target is **6 months** of runway. Costs are estimated - actual expenses may vary.”

**API fields (not shown on page - DT-022):**

| Field | Role |
|-------|------|
| `amount` | Implied reserve balance |
| `monthlyRate` | Divisor for months |
| `updatedAt` | Last reconciliation |
| `months` | Displayed headline |

**Missing:** Published rule for when reserves are drawn down, refilled, or re-targeted.

## Player scenario

**Context:** Runway shows “Healthy” at ≥4 months.

**What happens:**

1. Donor does not know if **4 months** means cash on hand or a modeled figure.
2. They cannot tell if the operator is **below target** (seeking funds) or **holding surplus** by choice.
3. Labeled reserve policy would clarify without opening books.

**Not claimed:** Hoarding donations - **disclosure gap**.

## Implications

- Pairs with **DT-010**, **DT-011**, **DT-022**, **PF-001**.

## Remediation

1. Publish **reserve policy**: target months, what counts toward reserve, spend triggers.
2. Show `amount` and `monthlyRate` (or rounded bands) on Server Support.
3. Label API **fallback** months as offline estimate (**DT-022**).
4. Note when operator **manually adjusts** burn assumptions - dated changelog line.

## Verification

- Visitor sees **target**, **current months**, and **as-of date** without staff contact.
- Reserve policy states whether **6 months** is minimum, goal, or cap.
