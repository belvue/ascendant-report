# RH-024: Ascendant game residential; ProFusion game on OVH

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (probe) |
| **Component** | Operator cluster transparency |
| **CWE** | N/A |

## Summary

Operator runs **multiple EQEmu servers** with **different hosting models**:

| Server | Game hostname | Game IP | Hosting |
|--------|---------------|---------|---------|
| **Ascendant** | `eqemu.ascendanteq.com` | 99.42.xxx.xxx | AT&T residential |
| **ProFusion** | `eqemu.eqprofusion.org` | 147.135.9.147 | OVH datacenter |

Shared **web portal** IP `147.135.10.69` (OVH) serves Ascendant/ProFusion marketing sites - creates visual association with datacenter hosting while Ascendant game is home-hosted.

## Example

DNS and port probes (2026-06-12) documented in hosting validation artifact.

ProFusion `:5998`/`:9000` on OVH; Ascendant game ports on residential.

## Attack vector

**N/A** - transparency for players choosing between servers.

## Implications

- Players comparing Ascendant vs ProFusion may assume **equivalent infrastructure** - false for game layer.
- Fairness / marketing clarity issue.

## Remediation

1. Disclose on each server's page which hosting model applies.
2. Avoid implying Ascendant game is OVH-hosted because website shares ProFusion portal stack.

## Verification

Ascendant website states game world = residential; ProFusion states OVH (if accurate).
