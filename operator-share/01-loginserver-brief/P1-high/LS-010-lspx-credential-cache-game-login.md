# LS-010: LSPX caches legacy account hashes on game login

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (reproduction) - architectural product behavior |
| **Component** | C++ loginserver - LSPX |
| **CWE** | N/A (trust boundary / disclosure) |

## Summary

When `LSPX=1`, the loginserver validates unknown accounts against **login.eqemulator.net**, then **inserts a local row** in `login_accounts` with a scrypt password hash and `source_loginserver='eqemu'`. This is documented product behavior, not a covert channel - but players routed through eqemulator.dev may not understand their legacy credentials are cached on operator infrastructure.

## Example

**Documented flow:** `LOGINSERVER.md` §2 - INSERT on successful upstream validate.

**Reproduction - full EQ client login (2026-06-13):**

- Client → loginserver game port with legacy account
- After successful login, database row observed:

| Column | Value |
|--------|-------|
| `id` | matches upstream account id |
| `account_name` | (legacy username) |
| `source_loginserver` | `eqemu` |
| `account_password` | scrypt hash (`$7$C6...`) |

**Contrast:** `POST /v1/account/credentials/validate/external` confirms upstream password but **does not insert** a row - only full game login does.

## Attack vector

**Prerequisites:** Player uses loginserver with LSPX enabled (default on eqemulator.dev); player authenticates with legacy eqemulator.net credentials.

**Steps:**

1. Player logs in via EQ client to `login.eqemulator.dev:5999`.
2. Loginserver proxies credential check to `.net`.
3. On success, hash stored locally in operator MariaDB.
4. Operator (or DB compromise) can read hashes; offline cracking depends on password strength.

**Attacker role:** Not an external exploit - **operator/insider** or **DB breach** after voluntary player login.

## Implications

- **Confidentiality:** Password hashes for all LSPX-migrated players reside in operator DB.
- **Trust:** Different model than authenticating only against community `.net` login.
- **Player expectation:** Ascendant patcher routes to `.dev` without in-client disclosure.

This finding does **not** allege covert theft - no hidden exfil endpoint was found in web source.

## Remediation

1. **Publish clear player disclosure** - login path diagram, LSPX behavior, hash storage (see LS-015 and [player-login-notice-v1.md](../player-login-notice-v1.md)).
2. Add **in-product acknowledgement** before first login or account link ([player-acknowledgement-block.md](../player-acknowledgement-block.md)).
3. Document data retention and who can access `login_accounts`.
4. Optional: offer documented path to use `.net` login directly (compatibility caveats).
5. Ensure MariaDB backups and API tokens are access-controlled (see LS-011, LS-033).

## Verification

1. Fresh DB snapshot; login with legacy account via client only.
2. Confirm new `login_accounts` row with `source_loginserver='eqemu'`.
3. Confirm player-facing docs mention LSPX caching.
