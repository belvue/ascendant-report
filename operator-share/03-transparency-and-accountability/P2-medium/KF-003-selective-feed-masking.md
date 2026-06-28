# KF-003: Selective supporter messages removed from public feed Jun 12 - 14

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (screenshot comparison) - 2026-06-12 through 2026-06-14 |
| **Component** | Ko-fi public activity feed |
| **CWE** | N/A (transparency optics) |

## Summary

Before the full feed empty state on Jun 15 (**KF-001**), specific supporter entries **disappeared from the public Ko-fi page** while older entries remained. Repeat donations with **in-game character names in notes** were among those removed, alongside entries where the payer name differed from the noted character. Ko-fi documents per-message **Make Message Private** by creator or supporter - consistent with selective removal. **Who** hid entries and **when** is not established.

## Example

**Jun 13 late evening - 8 public feed rows** including repeat donors with character-name notes and at least one entry where the payer name differed from the noted character.

**Jun 14 ~7:12 PM EDT - 7 public feed rows:**

| Pattern | Detail |
|---------|--------|
| Still visible | Seven entries - mostly older single-donor rows and a recent duplicate-display-name pair |
| No longer visible | Weekend repeat-donor cluster with character-name notes |

**Pattern:** Newer repeat donors with character-name notes removed; older single-entry donors remained - may appear curated without explanation.

Pairs with **DT-020** (character-name field optics), **DT-030**.

## Player scenario

**Context:** Player notices weekend donors vanish from Ko-fi while long-ago names stay.

**What happens:**

1. Feed looks selectively edited though Ko-fi allows per-message privacy.
2. Character-name notes (**DT-020**) reinforced donor↔character linkage before removal.
3. Without operator comment, community may infer intentional hiding of high-activity donors.

**Not claimed:** Cover-up or misconduct - **optics and documentation gap**.

## Implications

- **Transparency:** Selective disappearance during active fundraising warrants a one-line public explanation if intentional.
- **Character-name fields:** Removing rows does not remove creator-dashboard records but changes public optics (**DT-020**).
- **Platform vs operator:** Ko-fi permits hide actions; operator may or may not have initiated them.

## Remediation

1. If staff or creator hid messages, **publish brief policy** on public supporter visibility vs private thanks.
2. Align character-name field use with **DT-020** / **DT-031**.
3. Do not rely on public feed as donor recognition channel if messages are routinely hidden.
4. For Jun 12 - 14 window: confirm whether any hide actions were taken and whether community-facing explanation is appropriate.

## Verification

- Server Support or Ko-fi page states how supporter messages are displayed.
- Operator can confirm from Ko-fi creator dashboard whether Jun 12 - 14 entries remain visible to creator after public hide.
