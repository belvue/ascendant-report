# PC-030: Patcher binary - no credential-handling code found

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (static analysis + IL decompile) |
| **Component** | `EQAscendant.exe` |
| **CWE** | N/A |

## Summary

Static analysis and IL decompilation of **`EQAscendant.exe`** v1.0.6.9 found **no username/password handling**, no keylogging APIs, and no login-protocol code. Native imports are **`mscoree.dll` only** (CLR bootstrap). Behavior is manifest download, MD5 verify, file write, optional self-update, launch game.

## Example

**Absent from patcher:** eqhost parsing, credential fields, `login.*` compile-time strings.

**Present:** `UtilityLibrary.GetMD5`, HTTP download, `MainForm.PlayGame`.

## Attack vector

**N/A** - positive control for malware claims.

## Implications

- **Password trust boundary** is **`login.eqemulator.dev`** (or whatever `eqhost.txt` specifies) - not the patcher exe.
- Does not negate PC-010 login routing concern.

## Remediation

None required for this finding - document for player FAQ.

## Verification

Independent IL review reproduces same conclusion.
