# LS-011: Loginserver API bearer token enables account takeover

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (reproduction) - by design; high impact if leaked |
| **Component** | C++ loginserver HTTP API (`:6000`) |
| **CWE** | CWE-798 (Use of Hard-coded Credentials) if token leaked |

## Summary

The environment variable `LOGINSERVER_API_TOKEN` grants **write-level** API access. A bearer holding this token can **create arbitrary local accounts** and **overwrite password hashes** for existing accounts via `POST /v1/account/credentials/update/external`.

## Example

**Reproduction:**

```http
POST /v1/account/create HTTP/1.1
Authorization: Bearer <LOGINSERVER_API_TOKEN>
Content-Type: application/json

{"username": "example_user", "password": "ExamplePass123!"}
```

**Response:** `HTTP 200`

```http
POST /v1/account/credentials/update/external HTTP/1.1
Authorization: Bearer <LOGINSERVER_API_TOKEN>
Content-Type: application/json

{"username": "existing_user", "password": "NewPass123!", "account_id": <id>}
```

**Response:** `HTTP 200` - `{ "message": "Loginserver account credentials updated!" }`

**Anonymous request (no token):** `HTTP 401` on all `/v1/account/*` endpoints.

## Attack vector

**Prerequisites:** Leaked `LOGINSERVER_API_TOKEN` from environment config, deployment secrets, backup, CI log, or insider access.

**Steps:**

1. Attacker obtains token.
2. Creates accounts or overwrites hashes for known `account_id` values.
3. Player login succeeds with attacker-chosen password hash.

**Attacker role:** Token holder / insider - **not** anonymous internet.

## Implications

- **Integrity:** Full account takeover for loginserver accounts.
- **Availability:** Mass account creation.
- Token is equivalent to **root credential** for the loginserver API layer.

## Remediation

1. Store token in **secrets manager** (not plaintext in repos or shared compose files).
2. **Rotate** token periodically; separate read vs write tokens if codebase supports split scopes (currently single token with read/write flags in loginserver config).
3. Restrict loginserver HTTP API port to **internal network only** - never expose publicly.
4. Audit access to secrets, backups, and admin hosts with deployment access.
5. Alert on unusual `/v1/account/create` or `update/external` volume.

## Verification

- Port scan production: `:6000` should not be internet-reachable.
- Confirm token not in git history: `git log -p -- '*.env*'`.
- Test 401 without Authorization header from external network.
