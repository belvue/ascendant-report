# RH-012: ATLAS control plane degraded while admin UI remains public

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (probe) |
| **Component** | ATLAS - backend health |
| **CWE** | CWE-1104 (Use of Unmaintained Third Party Components) / operational |

## Summary

The unauthenticated `/api/admin/settings` response reports ATLAS Nexus status **`degraded`**, with **`db: unreachable`** and **`redis: unavailable`**, while Kong reports **connected** and the full admin UI continues to serve publicly on :443.

## Example

From RH-002 JSON (2026-06-12 and 2026-06-13):

```json
"nexus": {
  "status": "degraded",
  "checks": { "db": "unreachable", "redis": "unavailable" },
  "api_url": "http://atlas-api:8080"
},
"kong": { "status": "connected", "version": "3.9.1" }
```

Admin HTML pages still return 200 with full navigation; settings page shows loading spinner (client waiting on broken backend).

## Attack vector

**Prerequisites:** None for observation.

**Risk:** Operating a **partially broken control plane** on a public IP suggests rushed deployment, incomplete firewall rules, and unpredictable behavior if admin write paths are attempted.

## Implications

- **Operational maturity:** Control plane without functioning DB/redis is not production-ready.
- **Security:** Broken backends sometimes fail open or expose debug endpoints.
- **Availability:** Operator may not receive accurate telemetry from ATLAS.

## Remediation

1. **Do not expose** ATLAS publicly until backend is healthy (`db` + `redis` reachable).
2. Fix docker-compose/networking for atlas-api, DB, redis on game host - or remove ATLAS from game box.
3. If ATLAS is experimental, run on internal network only.
4. Add external monitoring that alerts when `nexus.status != healthy`.

## Verification

```bash
curl -s https://eqemu.ascendanteq.com/api/admin/settings | jq '.nexus.status, .nexus.checks'
```

Expected: `"healthy"` with db/redis reachable - **and** endpoint requires auth (RH-002).
