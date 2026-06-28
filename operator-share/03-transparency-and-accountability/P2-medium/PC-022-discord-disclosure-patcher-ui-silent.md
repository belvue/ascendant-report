# PC-022: Discord discusses `dinput8`; patcher UI does not

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (Discord + static analysis) |
| **Component** | Disclosure channel split |
| **CWE** | N/A (transparency) |

## Summary

Operator Discord channels (`#server-files`, `#patch-notes`, `#helpful-info`) have discussed **`dinput8.dll`**, eqhost changes, and MacroQuest-related fixes. The **patcher executable UI** does **not** carry equivalent disclosure - players who only run the patcher never see it.

## Example

**Discord context (paraphrased):** `dinput8` linked as AA / MacroQuest helper; eqhost re-patch instructions in `#helpful-info`.

**Patcher UI:** No strings referencing `dinput8`, MacroQuest, eqhost, or login.eqemulator.dev.

**Gap:** Disclosure exists for **Discord-active** players only - not at point of install.

## Attack vector

**N/A**

## Implications

- **Unequal information:** Discord members ≠ entire player base.
- **Reframes PC-002:** Hook DLL is not wholly secret - but **not player-facing at patch time**.
- Does not resolve PC-011 eqhost consent gap.

## Remediation

1. Mirror Discord patch notes in patcher UI and website.
2. Link `#helpful-info` content from first-run modal.
3. Treat Discord as supplement, not substitute, for PC-012 file list.

## Verification

- Patcher first-run text matches published Discord patch-note summary.
- New player without Discord access sees same DLL/login facts in patcher.
