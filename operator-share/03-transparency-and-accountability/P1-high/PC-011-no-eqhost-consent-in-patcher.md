# PC-011: No patcher acknowledgement or opt-in before `eqhost.txt` overwrite

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (source + observed behavior) |
| **Component** | Patcher UX - consent |
| **CWE** | N/A (transparency) |

## Summary

The patcher writes **`eqhost.txt`** during auto-patch **without** a first-run modal, checkbox, or opt-in/out choice. Players are not prompted before login routing changes. If a player manually restores **`login.eqemulator.net`**, the next auto-patch can **overwrite** their choice when the CDN manifest updates.

## Example

**Patcher flow (decompiled IL):** Fetch manifest → MD5 compare → write files including `eqhost.txt` → launch `eqgame.exe`. No consent gate.

**Observed player behavior:** Manual eqhost edit reverted to `login.eqemulator.dev` on subsequent patch when CDN `eqhost.txt` differs.

**UI strings present:** `"Checking for updates..."`, `"Complete! Patched "` - none describe login routing.

**Not implemented:** First-run acknowledgement ([player-patcher-acknowledgement-block.md](../player-patcher-acknowledgement-block.md)).

## Attack vector

**N/A** - consent gap.

## Implications

- **Informed consent:** Login path change is silent at the moment it matters (file write).
- **Player agency:** Opt-out requires editing `eqemupatcher.yml` (`autoPatch: false`) or re-editing eqhost after every patch - not documented in UI.
- **Pairs with LS-015** and PC-010.

## Remediation

1. **Before first `eqhost.txt` write:** show acknowledgement + link to trust notice.
2. **Opt-in default:** “Use Ascendant login path (`login.eqemulator.dev`)” vs “Keep my current eqhost unchanged.”
3. **On CDN eqhost change:** prompt “Login routing updated - review before apply.”
4. Document `autoPatch: false` in patcher help UI.

## Verification

- Fresh install shows modal before eqhost write; decline path documented.
- Re-patch after manual `.net` eqhost shows prompt (or respects saved preference).

Cross-reference: [01-loginserver-brief/player-acknowledgement-block.md](../../01-loginserver-brief/player-acknowledgement-block.md) (login trust text).
