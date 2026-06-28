# RH-031: Re-probe 2026-06-13 - no remediation observed

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (probe) |
| **Component** | Assessment timeline |
| **CWE** | N/A |

## Summary

A **read-only re-probe on 2026-06-13** (~24 hours after initial observation and AT&T abuse report) found **no change** to ATLAS admin exposure, unauthenticated API, or overall port map on `99.42.xxx.xxx`.

## Example

**2026-06-13 vs 2026-06-12 comparison:**

| Check | 2026-06-12 | 2026-06-13 |
|-------|------------|------------|
| `/admin/settings` | HTTP 200, full nav | HTTP 200, full nav |
| `/api/admin/settings` | HTTP 200, 343 B JSON | HTTP 200, identical JSON |
| Login wall visible | No | No |
| Backend status | DB/redis unreachable | Unchanged |

Both `99.42.xxx.xxx` and `eqemu.ascendanteq.com` return identical responses.

## Attack vector

**N/A** - timeline documentation.

## Implications

- P0 findings remain **actively exposed** as of package date.
- Embargo / operator remediation window should assume **unchanged state** until operator confirms fixes.

## Remediation

Operator completes RH-001 through RH-003 and requests optional coordinated re-probe after fixes.

## Verification

External GET to `/admin/settings` returns **401/403/302** - not **200** with admin shell.

**Next scheduled check:** Before any public player advisory (T+7 or operator publication).
