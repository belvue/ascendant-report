# PC-032: Login redirect is CDN `eqhost.txt`, not hardcoded in patcher exe

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (static analysis) |
| **Component** | Architecture |
| **CWE** | N/A |

## Summary

`EQAscendant.exe` contains **no hardcoded** `login.eqemulator.dev`, `5999`, or `eqhost` strings. Login routing is entirely determined by **`eqhost.txt`** written from CDN. **`eqgame.exe`** also has no embedded login host - reads file at runtime.

## Example

**Mechanism chain:** CDN `eqhost.txt` → patcher write → `eqgame.exe` reads on launch.

**Prior hypothesis “exe forces .dev login”:** **Not supported** - CDN manifest model instead.

## Attack vector

**N/A**

## Implications

- Remediation can target **CDN content + patcher disclosure** without recompiling exe for host changes.
- Operator can change routing remotely (PC-021).

## Remediation

Document architecture in operator `/about/trust` page.

## Verification

Strings search on patcher + eqgame confirms no embedded login host.

Cross-reference: PC-010, PC-030.
