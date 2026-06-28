# TR-001: No unified player trust / transparency hub on official surfaces

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (public surfaces review) |
| **Component** | Governance / UX - program |
| **CWE** | N/A (transparency) |

## Summary

Players must assemble trust facts from **Discord**, **forum threads**, and **investigator notices** because Ascendant lacks a **single official hub** (e.g. `/about/trust` or `/transparency`) linking **login path**, **hosting topology**, **patcher file policy**, and **donation/runway** honesty. Technical gaps are documented in briefs **01 - 04**; this finding is the **missing rollup page and navigation**.

## Example

**Topics requiring disclosure (each has a brief + player notice draft):**

| Topic | Brief | Draft notice |
|-------|-------|--------------|
| Login / eqhost / LSPX | 01 | `player-login-notice-v1.md` |
| Residential vs OVH hosting | 02 | `player-hosting-notice-v1.md` |
| Ko-fi / runway | 03 | `player-donation-notice-v1.md` |
| Patcher files / eqhost consent | 04 | `player-client-notice-v1.md` |

**Official website / patcher today:** No single “start here” page linking all four.

## Player scenario

**Context:** New or returning player asks “what am I agreeing to?”

**What happens:**

1. Website shows server marketing - not trust architecture.
2. Patcher deploys files silently (04).
3. Player discovers `.dev` login or residential host from **third parties**.

**Not an exploit:** Publication gap - not proof of hidden malice.

## Implications

- Undermines remediation in **LS-015**, **RH-011**, **DT-001**, **PC-011** until hub exists.
- Operators lose one place to post **changelog** for trust changes (**TR-020**).

## Remediation

1. Publish **`/about/trust`** (or equivalent) with diagram + links to four notices.
2. Patcher **Help → Trust & transparency** opens same URL.
3. Server Support page links hub above Ko-fi CTA.
4. Maintain [player-transparency-index-v1.md](../player-transparency-index-v1.md) as sitemap.

## Verification

- New player answers login + hosting + patcher + donation questions from **one official URL**.
