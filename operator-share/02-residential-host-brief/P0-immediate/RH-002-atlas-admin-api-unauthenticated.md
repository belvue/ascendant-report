# RH-002: Unauthenticated `/api/admin/settings` information disclosure

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (probe) - unchanged 2026-06-13 |
| **Component** | ATLAS - REST API |
| **CWE** | CWE-200 (Exposure of Sensitive Information) |

## Summary

`GET /api/admin/settings` returns **HTTP 200** with JSON **without any authentication headers**. The response leaks internal Docker service names, Kong API gateway version, backend health state, and ATLAS Nexus version.

## Example

**Request:**

```http
GET /api/admin/settings HTTP/1.1
Host: eqemu.ascendanteq.com
```

**Response (2026-06-12 and 2026-06-13 - identical):**

```json
{
  "nexus": {
    "status": "degraded",
    "version": "0.1.0",
    "checks": { "db": "unreachable", "redis": "unavailable" },
    "api_url": "http://atlas-api:8080"
  },
  "provisioning": {
    "kong_configured": true,
    "kong_reachable": true,
    "kong_version": "3.9.1",
    "agentgateway_configured": false
  },
  "kong": {
    "status": "connected",
    "version": "3.9.1",
    "admin_url": "Kong reachable via ATLAS API"
  }
}
```

No `Authorization` header was sent. Same result on `99.42.xxx.xxx` and hostname.

## Attack vector

**Prerequisites:** None.

**Steps:**

1. `curl https://eqemu.ascendanteq.com/api/admin/settings`
2. Parse JSON for internal topology (Docker service names, Kong version, degraded state).
3. Use in targeted attacks against Kong or atlas-api if other vulnerabilities exist.

**Attacker role:** Anonymous.

## Implications

- **Confidentiality:** Internal architecture fingerprinting without credentials.
- **Reconnaissance:** Attackers learn Kong 3.9.1, docker-compose-style hostnames, degraded DB/redis.
- **Operational security:** Degraded backend visible publicly - signals immature deployment (see RH-012).

## Remediation

1. Require authentication on **all** `/api/admin/*` routes (session, API key, mTLS).
2. Block `/api/admin/*` at nginx for non-VPN source IPs.
3. Remove sensitive fields from unauthenticated health endpoints if a public health URL is needed.
4. Audit other `/api/admin/*` routes for same pattern.

## Verification

```bash
curl -s https://eqemu.ascendanteq.com/api/admin/settings | head -c 200
curl -s -o /dev/null -w "%{http_code}" https://eqemu.ascendanteq.com/api/admin/settings
```

Expected after fix: **401/403** without valid credentials; no Kong/docker internals in anonymous responses.
