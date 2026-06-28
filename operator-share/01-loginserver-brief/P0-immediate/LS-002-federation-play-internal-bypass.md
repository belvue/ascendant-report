# LS-002: Internal federation-play route accepts empty client IP

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (reproduction) |
| **Component** | Web - internal API |
| **CWE** | CWE-287 (Improper Authentication) |

## Summary

The endpoint `POST /api/internal/federation-play` is intended for **internal** use only (C++ loginserver → web app). The guard treats an **empty** `X-Forwarded-For` / `X-Real-IP` as trusted internal traffic. External callers without forwarding headers bypass the IP check.

## Example

**Reproduction:**

```http
POST /api/internal/federation-play HTTP/1.1
Content-Type: application/json

{
  "server_short_name": "ascendant",
  "account_id": 1,
  "account_name": "test",
  "login_key": "abcdefghij"
}
```

**Response:** `HTTP 404` - `{ "error": "Server not found as federated" }`  
**Not** `HTTP 403 Forbidden`.

The 404 proves the **authentication gate was passed**; the handler proceeded to lookup federated world servers.

**Source:** `web/src/app/api/internal/federation-play/route.ts` (lines ~30 - 33):

```typescript
const isInternal = ip.startsWith("172.") || ip.startsWith("10.") || ... || ip === "";
if (!isInternal) {
  return NextResponse.json({ error: "Forbidden" }, { status: 403 });
}
```

## Attack vector

**Prerequisites:** Network access to the web app; federated world server rows present in DB.

**Steps:**

1. Attacker sends POST to `/api/internal/federation-play` **without** `X-Forwarded-For`.
2. Request is treated as internal.
3. If a matching federated server exists, the app forwards play authentication to the federation master (signed outbound request).

**Attacker role:** Anonymous (network path to web tier).

## Implications

- **Integrity:** Potential forged federated play-auth forwarding if world rows exist.
- **Defense in depth failure:** IP-only internal trust is fragile behind reverse proxies and direct port exposure.
- Combined with misconfigured nginx, could expose cross-node auth plumbing.

## Remediation

1. **Remove `ip === ""` from trusted condition** - empty IP must be rejected.
2. Prefer **shared secret header** (e.g. `X-Internal-Token` from env) or mTLS between loginserver container and web container.
3. Bind internal routes to **private network only** at reverse-proxy layer (do not expose `/api/internal/*` publicly).
4. Log and alert on any external-source hits to `/api/internal/*`.

## Verification

From outside the internal network (no `X-Forwarded-For`):

```bash
curl -s -o /dev/null -w "%{http_code}" -X POST https://eqemulator.dev/api/internal/federation-play \
  -H "Content-Type: application/json" \
  -d '{"server_short_name":"x","account_id":1,"account_name":"x","login_key":"1234567890"}'
```

Expected after fix: **403** (not 404/200).
