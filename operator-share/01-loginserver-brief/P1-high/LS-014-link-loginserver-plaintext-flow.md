# LS-014: Web link-loginserver accepts plaintext legacy passwords

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (source); full web flow not automated in reproduction |
| **Component** | Web - account linking |
| **CWE** | CWE-319 (Cleartext Transmission of Sensitive Information) - mitigated by HTTPS; server-side handling risk |

## Summary

Authenticated platform users can POST plaintext eqemu login passwords to `/api/account/link-loginserver`. The server validates against local DB, loginserver API, and upstream `.net` (LSPX), then stores a **scrypt hash** in `login_accounts`.

## Example

**Source flow** (`web/src/app/api/account/link-loginserver/route.ts`):

1. Require platform session (401 if absent - verified in reproduction).
2. Receive JSON `{ username, password }` in HTTPS body.
3. Validate via local → validate/local → validate/external.
4. `createScryptHash(password)` → INSERT into `login_accounts`.

**Unauthenticated request (reproduction):**

```http
POST /api/account/link-loginserver
{"username":"x","password":"y"}
→ HTTP 401
```

## Attack vector

**Prerequisites:** Valid platform session (attacker must compromise user session or phish login to web UI).

**Steps:**

1. User (or attacker with session) submits legacy eqemu password to eqemulator.dev website.
2. Password exists in Node handler memory during request.
3. Hash persisted locally even if user believed they only used community login.

**Attacker role:** Session holder; phishing site mimicking link flow is a separate governance risk.

## Implications

- **Confidentiality:** Plaintext password on server during request; logging/misconfigured reverse proxy could capture bodies.
- **Trust:** Users may not distinguish web link from game-only `.net` login.
- Complements LS-010 (game path) - two migration surfaces.

## Remediation

1. **Mandatory disclosure** on link page - what happens to password, where hash is stored.
2. Checkbox acknowledgement before submit ([player-acknowledgement-block.md](../player-acknowledgement-block.md)).
3. Ensure nginx/access logs do **not** log request bodies.
4. Prefer OAuth-style migration where feasible; if not, clear copy that upstream `.net` password is verified on `.dev` infrastructure.
5. Rate-limit link attempts per session/IP.

## Verification

- Link page shows disclosure + acknowledgement.
- Access log format confirmed body-free.
- Session required (401 without cookie).
