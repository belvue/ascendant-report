# LS-030: Unauthenticated loginserver healthcheck endpoint

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (reproduction) |
| **Component** | C++ loginserver HTTP API |
| **CWE** | CWE-200 (limited exposure) |

## Summary

`GET /probes/healthcheck` on the loginserver HTTP port returns liveness status **without authentication**. Standard for orchestration; low sensitivity if port is internal-only.

## Example

```http
GET /probes/healthcheck HTTP/1.1
```

**Response:** `HTTP 200` - `{ "status": 105 }` (reproduction)

## Attack vector

**Prerequisites:** Network access to loginserver HTTP port (:6000 mapped).

**Steps:** Probe healthcheck to confirm service presence and uptime.

**Attacker role:** Anonymous (if port exposed).

## Implications

- **Reconnaissance only** - confirms loginserver API is running.
- Risk rises if `:6000` is internet-exposed alongside unauthenticated healthcheck.

## Remediation

1. Keep `:6000` on **internal network only**.
2. Do not expose healthcheck publicly; use internal load balancer health checks.
3. Optional: require token even for healthcheck if must be public.

## Verification

External port scan shows :6000 closed; healthcheck reachable only from internal monitoring.
