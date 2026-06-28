# DT-020: Ko-fi character-name field creates donor-tracking optics

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (Server Support + Ko-fi policy text) |
| **Component** | Ko-fi UX / Server Support |
| **CWE** | N/A (transparency) |

## Summary

Server Support invites donors to **include a character name** in the Ko-fi message. Combined with Ko-fi notes visible to the page owner, this creates **optics of donor - player identity linking** even when **no in-game perk** is offered. EQEmu forum thread **t44563** raised this as a Terms **optics** concern - not pay-to-win.

## Example

**Server Support copy (observed):**

> “You are welcome to include your **character name** in the donation message if you want us to know who you are.”

**Stated elsewhere on same page:**

- No exclusive in-game items for donations (**DT-031**).
- Donations are personal gifts - no goods or services.

**Gap:** Optional identity field without explaining **how names are used, stored, or displayed** (thank-you only vs internal ledger vs public recognition).

## Player scenario

**Context:** Player donates with character name in Ko-fi note.

**What happens:**

1. Operator can associate **payment ↔ character**.
2. Other players cannot see policy for **whether that affects support priority or recognition**.
3. Reasonable donor may wonder if naming themselves creates **informal social pressure** or **staff awareness**.

**Not an exploit:** Governance/optics - see **DT-030** for one documented donor chat (no perks claimed).

## Implications

- **Trust:** Clarify intent - gratitude vs bookkeeping vs leaderboard.
- **Terms optics:** Character-linked donations were flagged for EQEmu review (**DT-021**).

## Remediation

1. Publish **data use** for character names: e.g. “private thank-you only; not shown publicly; not used for in-game advantage.”
2. Consider **removing** the invite to include character names - use optional **anonymous** support only, or generic “Ascendant player” default.
3. If names are kept, add **opt-out** language on Ko-fi and Server Support.

## Verification

- Policy states **whether** character names appear anywhere public (Discord shout-out, web list, etc.).
- New donor understands **no gameplay benefit** from naming themselves - aligns with **DT-031**.
