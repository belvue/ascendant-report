# PF-006: Public funding model - allocation priorities not defined

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (materials review) |
| **Component** | Governance - funding model |
| **CWE** | N/A (disclosure) |

## Summary

A reasonable contributor cannot determine from official sources:

- **What** community funds are allowed to support,
- **How** priorities are set when inflows are limited,
- **Whether** surplus accumulates for reserves, hardware, or future features, or
- **Who** decides allocation (operator-only vs published policy).

The project presents **runway months** and **voluntary support** language without a defined **funding model** document.

## Example

**What exists today:**

| Element | Present? | Defines allocation? |
|---------|----------|---------------------|
| Ko-fi + Server Support appeals | Yes | Partial - blended categories |
| 6-month runway **target** | Yes | Target only - not priority rules |
| `/api/server-status/funding` | Yes | Backend numbers - not public formula (**DT-022**) |
| Published **priority order** (e.g. uptime before features) | **No** | - |
| Quarterly/annual funding summary | **No** | - |

**Ambiguity example:** If donations exceed cloud bills, may surplus fund **hardware upgrade**, **reserve**, **event prizes**, or **operator reimbursement**? Public docs do not say.

## Player scenario

**Context:** Large one-time gift month (observed Jun 2026 Ko-fi activity in brief **03**).

**What happens:**

1. Contributor cannot see how surplus affects **reserve** vs **spend**.
2. Runway bar may rise without explaining **what changed** (inflow vs burn assumption edit).
3. Published model would set expectations without revealing private accounting.

**Not claimed:** Misappropriation - **governance clarity gap**.

## Implications

- **PF-001 - 005** roll up here: categories, labor, reserves, capital, compensation need a **single funding model** anchor.
- **Pairs with DT-011** (inflows), **PF-007** (optional template).

## Remediation

1. Publish a **Funding model** section on Server Support: purpose, allowed uses, surplus rule, decision owner.
2. State whether the project is **break-even**, **reserve-building**, or **growth** oriented.
3. Add **annual or quarterly** summary paragraph (bands OK).
4. Cross-link **no in-game perks** (**DT-031**) and **hosting topology** (**RH-011**).

## Verification

- Contributor can answer: “What happens to donations above monthly cloud bills?” from official text.
- Funding model page has **last updated** date.
