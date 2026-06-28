# KF-001: Public Ko-fi feed is not a complete donation record

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (observed Jun 12 - 16, 2026) |
| **Component** | Ko-fi public activity feed - `ko-fi.com/ascendanteq` |
| **CWE** | N/A (disclosure / record-keeping) |

## Summary

The Ko-fi **public activity feed** does not reliably show all supporter activity. Between Jun 12 and Jun 16, entries disappeared selectively and then the feed went **fully empty** while donation solicitation remained active. Players and donors viewing the page cannot reconstruct who supported, when, or for how much from the feed alone. This pairs with **DT-011** (no published receipt totals) - the feed must not be treated as the operator’s financial ledger.

## Example

**Jun 15 transition (EDT):**

| Time | Public feed state |
|------|-------------------|
| 10:41 AM | **7** visible supporter entries |
| 10:56 AM | **0** visible supporters - first fully empty state |
| Afterward | Feed remained empty through at least Jun 16 |

**Earlier window:**

- Jun 12 - 13: Fuller feed including repeat donors and character-name notes
- Jun 14 evening: Feed reduced to 7 rows; weekend repeat-donor cluster no longer on the public page

**Ko-fi platform behavior (documented):** Creator or logged-in supporter can hide individual messages via **Make Message Private** on the public feed. That is consistent with disappearance; it does not establish who acted.

**What donors reasonably expect:**

- A public feed that reflects recent support, **or**
- A clear statement that the feed is optional/social only, with **published totals elsewhere** (**DT-011**)

## Player scenario

**Context:** Player checks Ko-fi before donating to see community support level.

**What happens:**

1. Feed shows **no supporters** after Jun 15 morning despite ongoing asks for server funding.
2. They cannot verify recent large gifts (e.g. Jun 14 dashboard-reported charges) from the public page.
3. Runway months on Server Support (**DT-010**) cannot be reconciled to visible Ko-fi activity.

**Not claimed:** Embezzlement or deliberate concealment - **record and disclosure gap**.

## Implications

- **Informed giving:** Transparency promise is undermined if Ko-fi is the primary public channel but the feed is empty or incomplete.
- **Community trust:** Empty feed during active fundraising invites speculation; published totals from creator records resolve this.
- **Operator accounting:** Internal books (Ko-fi creator dashboard, exports) are the authoritative source - not scraped public HTML.

Pairs with **DT-011**, **DT-022**, **KF-003**.

## Remediation

1. **Publish community support summary** on Server Support - rolling 30/90-day Ko-fi totals, last updated date (**DT-011**).
2. **State explicitly** that the Ko-fi activity feed is not the donation ledger.
3. **Explain** Jun 15 feed-empty state if intentional (privacy cleanup, settings change) or document that Ko-fi platform controls caused it.
4. Maintain an **internal donation log** (dashboard export or webhook) independent of public feed visibility.
5. If exact dollars are sensitive, publish **rounded bands** - better than an empty public feed.

## Verification

- Visitor sees **received totals + as-of date** without Ko-fi creator login.
- Public Ko-fi page either shows current support **or** Server Support explains why it does not and where totals live.
- Operator can produce creator-dashboard export for Jun 12 - 16 window on request.
