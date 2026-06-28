# LS-013: Public /api/status and /api/servers without authentication

| Field         | Value                                       |
| ------------- | ------------------------------------------- |
| **Priority**  | P1 - High                                   |
| **Status**    | Confirmed (reproduction)                    |
| **Component** | Web - public API                            |
| **CWE**       | CWE-200 (Exposure of Sensitive Information) |

## Summary

Two read-only API routes return operational and directory data **without authentication** in the test deployment. Author self-audit (SA-06) attempted to tier-gate sensitive metrics; `/api/status` and `/api/servers` remain publicly readable.

## Example

**Reproduction:**

```http
GET /api/status HTTP/1.1
```

**Response:** `HTTP 200` - JSON keys include `overall`, `loginserver`, `database`, `services`, `summary`, `checkedAt` (host CPU/memory/disk class metrics).

```http
GET /api/servers HTTP/1.1
```

**Response:** `HTTP 200` - array of server objects with trust flags, player counts, names.

**Contrast:**

```http
GET /api/metrics HTTP/1.1
```

**Response:** `HTTP 403` (reproduction) - Prometheus metrics correctly blocked for external callers.

**Production note:** `/api/servers` on eqemulator.dev has returned both 200 and 403 at different probe times - treat as **configuration-dependent**.

## Attack vector

**Prerequisites:** None.

**Steps:**

1. Attacker requests `/api/status` and `/api/servers`.
2. Reconstructs infrastructure health, player counts, server roster, trust/claimed flags.
3. Uses for reconnaissance, competitive intelligence, or targeted attacks.

**Attacker role:** Anonymous.

## Implications

- **Confidentiality:** Operational telemetry and live player distribution exposed.
- **Trust manipulation visibility:** Curated directory and `is_server_trusted` flags visible (see LS-025).
- Lower severity than credential endpoints - but aids attackers mapping the platform.

## Remediation

1. Decide **intentional public** vs **authenticated** for each route; document the choice.
2. If public by design, strip sensitive fields (internal service names, DB error detail, raw IPs).
3. If not public, require session or API key; align with SA-06 intent.
4. Add rate limiting and CDN/WAF rules on `/api/status` and `/api/servers`.
5. Re-probe production after changes.

## Verification

```bash
curl -s -o /dev/null -w "%{http_code}" https://eqemulator.dev/api/status
curl -s -o /dev/null -w "%{http_code}" https://eqemulator.dev/api/servers
```

Document expected status codes and field allowlist in operator runbook.
