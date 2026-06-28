# DT-022: Runway widget hides data source, formula, and last update

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (frontend behavior) |
| **Component** | Server Support - funding API consumer |
| **CWE** | N/A (transparency) |

## Summary

The runway widget loads **`/api/server-status/funding`** but displays only **`months`** to visitors. The API also returns **`amount`**, **`monthlyRate`**, and **`updatedAt`** - fields **not shown** on the public page. On API failure, the UI uses a **static fallback** (~2.68 months) without labeling the number as **estimated/offline**.

## Example

**API shape (observed JSON fields):**

| Field | Purpose | Shown on page? |
|-------|---------|----------------|
| `months` | Runway headline | Yes |
| `amount` | Reserve balance (implied) | **No** |
| `monthlyRate` | Divisor for runway | **No** |
| `updatedAt` | Freshness | **No** |

**Fallback behavior (observed in frontend):**

- If fetch fails → display **hardcoded default months** until API succeeds
- No “**data stale**” or “**offline estimate**” banner

## Player scenario

**Context:** Donor checks runway after a site glitch or during API outage.

**What happens:**

1. Page may show **fallback months** indistinguishable from live data.
2. Donor cannot see **when** numbers were last reconciled with Ko-fi.
3. Trust in the bar depends on **invisible backend** - undermines DT-011 goals.

**Not an exploit:** UX/provenance gap.

## Implications

- Undermines the page’s own transparency pledge when **math is hidden**.
- Pairs with **DT-010** (category split) and **DT-011** (inflows).

## Remediation

1. Display **`updatedAt`** under the runway (“Last reconciled: YYYY-MM-DD”).
2. Show **`monthlyRate`** and **`amount`** (or rounded bands) with tooltip explaining formula.
3. On API failure: show **“Runway unavailable - last known: …”** instead of silent fallback.
4. Link to **changelog** when operator manually adjusts burn assumptions.

## Verification

- View-source or UI shows **provenance** for any number displayed.
- Fallback state is **labeled**, not masquerading as live.
