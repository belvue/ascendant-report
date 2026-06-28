# PC-012: Modified `eqgame.exe` deployed without player-visible manifest

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (static analysis) |
| **Component** | CDN-deployed client binary |
| **CWE** | N/A (supply-chain transparency) |

## Summary

The patcher deploys a **custom `eqgame.exe`** (MD5 `73b218a6…`, differs from stock `testeqgame.exe` in the same install directory). The patcher UI shows **patch progress only** - not a **file list** of executables and DLLs being added or replaced.

## Example

**Manifest:**

```yaml
- name: eqgame.exe
  md5: 73b218a6c9cabdd33e97e1bb03f8efaa
  date: "20260304"
```

**Comparison:** Deployed SHA-256 ≠ local stock `testeqgame.exe` - modified client build.

**Login behavior:** `eqgame.exe` reads **`eqhost.txt` at runtime** - no embedded `login.eqemulator.dev` string.

## Attack vector

**N/A** - transparency finding.

## Implications

- Players cannot see **which binaries** changed without reading manifest files manually.
- Custom client + hook DLL (PC-002) compound opacity.
- Integrity-conscious players cannot review changes before launch.

## Remediation

1. Pre-patch screen: list files to add/update/delete with hashes.
2. Require explicit **Continue** before write.
3. Post-patch summary before `PlayGame()`.
4. Publish hash list on operator website.

## Verification

- UI lists `eqgame.exe`, `dinput8.dll`, `eqhost.txt` on every patch that touches them.
- Player can cancel before write.
