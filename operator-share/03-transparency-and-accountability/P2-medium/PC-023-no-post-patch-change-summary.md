# PC-023: Patcher auto-launches game after patch with no change summary

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source) |
| **Component** | Patcher UX |
| **CWE** | N/A |

## Summary

After patching, the patcher calls **`PlayGame()`** and starts **`eqgame.exe`** without showing **what files changed** in that session (names, hashes, or login-host impact).

## Example

**UI flow:** `"Complete! Patched "` → game launch.

**Missing:** “Updated: eqhost.txt (login host → login.eqemulator.dev), dinput8.dll, eqgame.exe.”

## Attack vector

**N/A**

## Implications

- Players miss the only moment to review changes before game loads hook DLL and reads eqhost.
- Compounds PC-011 and PC-012.

## Remediation

1. Post-patch dialog listing changed files + login host line from eqhost.
2. **Play now** vs **Review files** buttons.
3. Log last patch file list to local `patch-history.yml` player can open.

## Verification

- After patch, UI shows file change list before launch button enables.
