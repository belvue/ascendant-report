# KF-002: Same-name donor / character Mark of Ascendance jump warrants reconciliation

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (inventory snapshot + Ko-fi display-name match) - 2026-06-14 |
| **Component** | In-game economy - Mark of Ascendance (MOA) |
| **CWE** | N/A (economy review) |

## Summary

On **2026-06-14 04:54:21 UTC**, an inventory snapshot showed **0 → 12** Mark of Ascendance at a single pull for a character whose name **matched a Ko-fi supporter display name** visible in the public feed that week. A Ko-fi tip under that same display name was recorded at **05:00:00 UTC** (~6 minutes later). The public feed often omits amounts; where only a name appears, **≥$5** is the typical Ko-fi minimum floor - **not** a confirmed payment total. Cited server MOA generation is roughly **~2 per day**; a jump of 12 in one snapshot warrants operator reconciliation. **Does not prove** donation-for-item exchange.

## Example

| Event | Timestamp (UTC) | Detail |
|-------|-----------------|--------|
| Inventory snapshot | 2026-06-14 04:54:21 | **0 → 12** MOA at one pull |
| Ko-fi tip | 2026-06-14 05:00:00 | Same display name as character; amount **≥$5 floor** on feed-sourced rows |
| Prior Ko-fi visibility | ~Jun 6 | Earlier feed row under same display name |
| Last public feed appearance | 2026-06-15 10:41 AM EDT | Name among seven visible before feed went empty (**KF-001**) |

**Economy context:** MOA is rate-limited (~2/day cited). Twelve units at once suggests acquisition via bazaar, P2P trade, GM grant, event reward, alt transfer, or another mechanism - not routine generation alone.

**Name match alone** does not prove the Ko-fi payer owns the character.

## Player scenario

**Context:** Community discusses whether donations confer in-game value (**DT-031**).

**What happens:**

1. Same-name correlation between Ko-fi tip timing and inventory change is **observable**.
2. Without published MOA grant policy or reconciliation, players may assume perk linkage.
3. Operator review can confirm or rule out staff grant, trade, or unrelated timing.

**Not claimed:** RMT, quid pro quo, or staff misconduct - **anomaly flag only**.

## Implications

- **Economy fairness:** Unexplained MOA spikes on a donor-correlated name affect trust even if coincidental.
- **Perks policy:** Pairs with **DT-031** - if donations confer nothing, document how MOA is obtained and whether staff can grant it.

## Remediation

1. **Reconcile** the inventory change for Jun 13 - 15 via bazaar, P2P trade logs, GM commands, event rewards, and alt transfers.
2. **Publish** whether Marks of Ascendance can be granted manually and under what policy.
3. **Respond** whether any in-game action was tied to the Ko-fi display-name match (expected if policy is clean: no).
4. Document outcome - cleared as coincidence, trade, event, or corrected if erroneous grant.

## Verification

- Operator can cite **source** of 12 MOA (generation accrual over time, trade, event, grant, or other).
- Published policy states whether donation-linked item grants are permitted (**DT-031**).
- If grant occurred, it appears in staff audit trail.
