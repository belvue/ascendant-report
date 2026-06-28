# RH-022: Alternate HTTP port 8080 open on game host

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (probe) |
| **Component** | Network exposure |
| **CWE** | CWE-668 (Exposure of Resource to Wrong Sphere) |

## Summary

TCP **8080** is **open** on `99.42.xxx.xxx`. HTTP request to the port **timed out** during probe - service listener present but behavior unclear. Increases unexplained attack surface on residential host.

## Example

**Port map (2026-06-12):**

```
8080 OPEN - HTTP probe timed out
```

Closed on comparison OVH web IP for game ports.

## Attack vector

**Prerequisites:** Identify service bound to 8080 (docker proxy, debug app, alternate admin).

**Steps:** Service fingerprinting, vulnerability scan against unknown HTTP stack.

## Implications

- **Reconnaissance:** Extra port on already overloaded residential IP.
- Unknown service may lack hardening.

## Remediation

1. Identify process: `ss -tlnp | grep 8080` on host.
2. Close if unused; firewall deny if internal-only.
3. If required, put behind auth and document purpose.

## Verification

External scan: 8080 closed or returns defined authenticated service.
