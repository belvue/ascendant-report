# PC-021: Operator can change deployed files from CDN without patcher release

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (architecture) |
| **Component** | CDN manifest model |
| **CWE** | N/A (supply-chain governance) |

## Summary

Patch content is controlled by **`filelist.yml`** on the operator CDN. Updating manifest entries (add, replace, or `deletes[]` files) changes what every auto-patching client receives **without** shipping a new `EQAscendant.exe` binary.

## Example

**Patch algorithm:** GET remote `filelist_{ClientVersion}.yml` → download changed entries → apply `deletes[]`.

**Login routing change:** Update CDN `eqhost.txt` + MD5 in manifest - clients pick up on next patch (manifest date `20260331` for eqhost).

**Hook DLL:** `dinput8.dll` added via manifest entry dated `20260411`.

## Attack vector

**N/A for players** - operator capability by design.

**Supply-chain:** Compromised CDN credentials or HTTP MITM (PC-001) could abuse same mechanism.

## Implications

- **Remote control:** Full client-directory policy from CDN.
- **Audit:** Players cannot correlate changes to patcher version number.
- **Transparency:** Requires manifest disclosure (PC-012) and HTTPS (PC-001).

## Remediation

1. Signed manifests tied to published patcher release notes.
2. Changelog on website for every manifest version bump.
3. HTTPS + access control on CDN upload path.

## Verification

- Manifest version logged in patcher UI after each patch.
- Operator publishes changelog entry for each CDN manifest update.
