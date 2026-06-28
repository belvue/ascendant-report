# LS-031: Platform users lack MFA (admins and server owners have MFA)

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (source) |
| **Component** | Web - authentication |
| **CWE** | CWE-308 (Weak Authentication) |

## Summary

Email MFA is enforced for **platform admins** and **server owners** (when Resend configured). Regular platform users logging in via username/password receive a session **without MFA**.

## Example

**Source:** `web/src/app/api/account/login/route.ts` - MFA branch only when `isAdmin || isServerOwner`.

## Attack vector

**Prerequisites:** User password compromise (phishing, reuse, breach).

**Steps:** Attacker uses password on web UI; no second factor for regular accounts.

**Attacker role:** Credential thief.

## Implications

- **Account linking risk:** Compromised web account could link legacy loginserver credentials (LS-014).
- Acceptable for many communities - document as known posture.

## Remediation

1. Optional MFA for all users (TOTP or email).
2. Encourage strong unique passwords.
3. Monitor for credential stuffing (rate limits already present).

## Verification

Regular user login does not prompt MFA; admin login does (when Resend configured).
