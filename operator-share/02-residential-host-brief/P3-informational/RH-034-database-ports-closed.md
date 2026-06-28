# RH-034: Database ports not exposed (3306/6379 closed)

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (probe) - positive control |
| **Component** | Data layer network exposure |
| **CWE** | N/A |

## Summary

Common database ports **MySQL (3306)**, **PostgreSQL (5432)**, and **Redis (6379)** were **closed** to external probes on the residential game IP. No direct internet access to data stores was observed.

Note: ATLAS status API reports DB/redis **unreachable from the ATLAS app** (RH-012) - internal connectivity issue, not public DB exposure.

## Example

**Port probe (2026-06-12):**

| Port | Service | State |
|------|---------|-------|
| 3306 | MySQL/MariaDB | closed/timeout |
| 5432 | PostgreSQL | closed/timeout |
| 6379 | Redis | closed/timeout |

Also scanned closed: 25, 5999, 8443, 9001, 9100, 9999, 10000, 20000, 27015.

## Attack vector

**N/A** - positive finding.

Direct database brute force or Redis injection from internet is **not** available via these ports on external scan.

## Implications

- **Good practice:** Data stores not bound to `0.0.0.0` on game host public interface.
- ATLAS degraded backend (RH-012) is a **separate** internal/docker networking issue.

## Remediation

Keep DB/cache on localhost or private network only; firewall deny inbound on data ports.

## Verification

Repeat external port scan - 3306/5432/6379 remain closed.
