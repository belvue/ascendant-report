# PC-010: CDN `eqhost.txt` routes login to `login.eqemulator.dev`

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (CDN + local install match) |
| **Component** | CDN-deployed `eqhost.txt` |
| **CWE** | N/A (trust / routing) |

## Summary

The patcher does **not** hardcode a login server. Instead, CDN **`eqhost.txt`** is MD5-checked and written into the EverQuest folder. Current content sets **`login.eqemulator.dev:5999`** active and comments out **`login.eqemulator.net:5999`**.

This is the **patcher-side mechanism** for the disclosure gap documented in **[LS-015](../../01-loginserver-brief/P1-high/LS-015-login-path-disclosure-gap.md)**.

## Example

**CDN / local `eqhost.txt` (2026-06-13):**

```ini
[LoginServer]
Host=login.eqemulator.dev:5999
#Host=eqemu.ascendanteq.com:5999
#Host=login.eqemulator.net:5999
```

**Manifest:**

```yaml
- name: eqhost.txt
  md5: 2a3af4b7cd3130c01dcec7772c52c663
  date: "20260331"
```

**Patcher executable:** No `login.*` or `5999` strings compiled in - redirect is **CDN-controlled**.

## Attack vector

**N/A** - documented product behavior.

Operator can change login routing by updating CDN `eqhost.txt` + manifest; clients receive on next patch.

## Implications

- Players auto-patching are **persistently routed** to eqemulator.dev federation login.
- Credential trust boundary shifts - see **01-loginserver-brief**.
- Manual edit to `.net` is **overwritten** when CDN eqhost changes and autoPatch runs (PC-020).

## Remediation

1. Patcher modal before first `eqhost.txt` write ([03 PC-011](../../03-transparency-and-accountability/P1-high/PC-011-no-eqhost-consent-in-patcher.md)).
2. Publish login-path notice ([01 player materials](../../01-loginserver-brief/player-login-notice-v1.md)).
3. Optional: offer `.net` eqhost variant with documented compatibility tradeoffs.

## Verification

- CDN and local `eqhost.txt` hashes match published operator documentation.
- After remediation, patcher shows intended login host before write.
