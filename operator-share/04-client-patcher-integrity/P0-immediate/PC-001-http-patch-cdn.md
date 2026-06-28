# PC-001: Patch CDN serves files over HTTP (no TLS)

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (source + CDN fetch) |
| **Component** | Patcher supply chain - downloads |
| **CWE** | CWE-319 (Cleartext Transmission) / CWE-494 (Download of Code Without Integrity Check beyond MD5) |

## Summary

`EQAscendant.exe` downloads patch manifests and files from **`http://downloads.ascendanteq.com/ascendantpatch/`** using plain HTTP. MD5 checksums in `filelist.yml` detect accidental corruption but do **not** authenticate the CDN against a network attacker.

## Example

**Patcher compile-time constant:** `FileListUrl` / `PatcherUrl` base → `http://downloads.ascendanteq.com/ascendantpatch/`

**Deployed via HTTP:** `filelist_Rain_Of_Fear_2.yml`, `eqhost.txt`, `dinput8.dll`, `eqgame.exe`

**Self-update URL:** GitHub releases (HTTPS) - patch **payload** is not.

## Attack vector

**Prerequisites:** MITM on path between player and CDN (same network, compromised router, ISP injection).

**Steps:** Substitute manifest or file bytes; if MD5 in attacker-controlled manifest matches malicious file, client accepts it.

**Attacker role:** Network position - not the patcher author by default.

## Implications

- **Integrity:** Attacker could swap `eqhost.txt`, `dinput8.dll`, or `eqgame.exe`.
- **Confidentiality:** Low for binary payloads; high for trust in operator CDN.
- **Operator reputation:** Below common practice for game client delivery.

## Remediation

1. Serve patch CDN over **HTTPS** with valid TLS certificate.
2. Consider **code signing** or manifest signatures beyond MD5.
3. Document CDN host and certificate pin in operator security page.

## Verification

```bash
curl -I http://downloads.ascendanteq.com/ascendantpatch/filelist_Rain_Of_Fear_2.yml
curl -I https://downloads.ascendanteq.com/ascendantpatch/filelist_Rain_Of_Fear_2.yml
```

Expected: HTTPS URL returns 200 with valid cert; HTTP redirects or deprecated.
