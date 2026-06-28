# LS-001: Turnstile registration bypass when captcha unset

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (reproduction) |
| **Component** | Web - account registration |
| **CWE** | CWE-799 (Improper Control of Interaction Frequency) |

## Summary

When `TURNSTILE_SECRET_KEY` is not configured, the registration endpoint accepts an empty captcha token and creates platform accounts. This enables automated bot registration, spam accounts, and resource exhaustion against the login platform.

## Example

**Reproduction (isolated deployment, 2026-06-13):**

```http
POST /api/account/register HTTP/1.1
Content-Type: application/json

{
  "username": "example_user_001",
  "password": "ExamplePass123!",
  "email": "example@example.invalid",
  "turnstileToken": ""
}
```

**Response:** `HTTP 200` with `{ "success": true }`

**Source:** `web/src/lib/turnstile.ts` - verification is skipped when the secret env var is empty.

## Attack vector

**Prerequisites:** Public reachability of the web app; Turnstile not configured (or misconfigured) in production.

**Steps:**

1. Attacker scripts `POST /api/account/register` with unique usernames/emails.
2. No captcha challenge is presented or validated.
3. Attacker obtains valid platform accounts for spam, credential stuffing prep, or federation noise.

**Attacker role:** Anonymous internet client.

## Implications

- **Availability:** Database and email (if Resend configured) load from unbounded registrations.
- **Integrity:** Polluted account namespace; harder to audit real users.
- **Trust:** Weak front door on a platform that also handles loginserver account linking.

Production Turnstile status on eqemulator.dev was **not independently verified** at assessment time; reproduction behavior matches source when unset.

## Remediation

1. **Require Turnstile in production** - set `TURNSTILE_SECRET_KEY` and `NEXT_PUBLIC_TURNSTILE_SITE_KEY`; fail closed if missing in `NODE_ENV=production`.
2. Change `turnstile.ts` to **reject** registration when secret is unset (do not silently skip).
3. Keep existing Redis rate limits on register; consider lowering thresholds.
4. Add monitoring alert on registration velocity spikes.

## Verification

```bash
# Should return 400/403 when Turnstile required and token empty
curl -s -o /dev/null -w "%{http_code}" -X POST https://eqemulator.dev/api/account/register \
  -H "Content-Type: application/json" \
  -d '{"username":"probe","password":"ProbePass123!","email":"probe@invalid.test","turnstileToken":""}'
```

Expected after fix: non-200 unless valid Turnstile token supplied.
