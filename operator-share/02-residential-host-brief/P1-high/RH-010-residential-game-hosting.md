# RH-010: Live game world hosted on consumer residential broadband

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (probe) |
| **Component** | Infrastructure - DNS / hosting |
| **CWE** | N/A (continuity / transparency) |

## Summary

The live Ascendant **game world** (`eqemu.ascendanteq.com`) resolves to **`99.42.xxx.xxx`**, an **AT&T Lightspeed consumer broadband** connection in Illinois Metro East - not a datacenter. EQEmu ports **5998** (binary login) and **9000** (world) accept connections on this IP.

## Example

**DNS (2026-06-12):**

```
eqemu.ascendanteq.com → 99.42.xxx.xxx
```

**IP intelligence:**

| Field | Value |
|-------|-------|
| rDNS | `99-42-204-12.lightspeed.stlsmo.sbcglobal.net` |
| ISP | AT&T Enterprises (AS7018) |
| Location | Illinois Metro East area |
| Allocation | Consumer/residential AT&T netblock |

**Contrast - Ascendant website:**

```
ascendanteq.com → 147.135.10.69 (OVH US, Warrenton VA)
```


## Attack vector

**Prerequisites:** N/A - not an exploit; environmental risk.

**Impact scenarios:**

- ISP outage or power loss → server down
- Dynamic IP change (if applicable) → DNS break unless DDNS managed
- Residential ToS or abuse complaints → service termination
- Physical location tied to consumer connection

## Implications

- **Availability:** Higher downtime risk vs OVH/datacenter peers (e.g. ProFusion game still on OVH).
- **Trust:** Players may assume datacenter hosting when donating for "hosting costs."
- **Security:** Residential IPs are common scan targets; combines with RH-001 - 003.

## Remediation

1. **Disclose** residential game hosting to players (see RH-011, player notice).
2. Document uptime expectations and recovery plan (power, ISP, backups).
3. Consider migrating world server back to datacenter if SLA matters.
4. If staying residential: UPS, monitored DDNS, off-site backups, separate management network.

## Verification

- Publish hosting topology page listing game = residential, web = OVH.
- `dig +short eqemu.ascendanteq.com` documented in operator runbook with expected ISP.
