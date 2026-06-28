# TR-010: Acknowledgement blocks not embedded at decision points

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (product review - not implemented) |
| **Component** | Governance / consent UX |
| **CWE** | N/A |

## Summary

Draft **checkbox acknowledgement blocks** exist for login, hosting, patcher/eqhost, and donations - but **none are embedded** in the patcher, website account link flow, Server Support donate path, or server rules at the **moment of decision**. Informed consent remains **out-of-band**.

## Example

**Draft blocks (in repo - operator may publish):**

| Moment | Block |
|--------|-------|
| Account link / register | [01 player-acknowledgement-block.md](../../01-loginserver-brief/player-acknowledgement-block.md) |
| Long-term play / hosting | [02 player-acknowledgement-block.md](../../02-residential-host-brief/player-acknowledgement-block.md) |
| Before Ko-fi | [03 player-donation-acknowledgement-block.md](../../03-transparency-and-accountability/player-donation-acknowledgement-block.md) |
| Before eqhost write | [player-patcher-acknowledgement-block.md](../player-patcher-acknowledgement-block.md) |

**Product today:** No checkbox modals observed at these points.

Cross-ref: **LS-015**, **PC-011**, **RH-011**, **DT-001**.

## Player scenario

**Context:** Player patches, links account, and donates in one session.

**What happens:**

1. Files and login path change **without** checkbox.
2. Player never saw hosting or donation context **unless** they read Discord.

## Implications

- Remediation copy exists - **implementation** is the gap.
- Pairs with **TR-001** hub and **TR-021** publication.

## Remediation

1. Implement **04 patcher modal** before eqhost write (PC-011).
2. Gate **account link** page with **01** acknowledgement (LS-014).
3. Checkbox before **Ko-fi redirect** on Server Support (DT-001).
4. Optional hosting acknowledgement in patcher first-run (02).

## Verification

- Test install shows **documented** acknowledgement at each decision point.
- Blocks versioned with date in URL or changelog.
