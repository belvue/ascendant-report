# RH-033: Zone ports not exposed to internet (730x closed)

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (probe) - positive control |
| **Component** | EQEmu zone networking |
| **CWE** | N/A |

## Summary

EQEmu **zone ports (7300 - 7329)** were **not reachable** from the internet on `99.42.xxx.xxx`. Zones appear to run behind the world server on `:9000` - expected architecture for a properly segmented EQEmu deployment.

## Example

**Port probe (2026-06-12):**

```
7300 - 7329  closed/timeout - No zone ports exposed
9000       OPEN - World server
5998       OPEN - Binary login path
```

Player world traffic enters via `:9000`; individual zone instances are not directly exposed.

## Attack vector

**N/A** - positive finding.

Direct zone port attacks from the internet are **not** available on this host based on external scan.

## Implications

- **Good practice:** Reduces direct zone-level attack surface.
- Does **not** offset RH-001/RH-003 co-location risks on same IP.

## Remediation

Maintain closed zone ports on public interface; document internal zone binding for operator runbooks.

## Verification

External scan of 7300 - 7329 remains closed after infrastructure changes.
