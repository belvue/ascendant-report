# RH-003: SSH (OpenSSH) exposed to the internet on game host

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (probe) - 2026-06-12 |
| **Component** | Host administration - TCP :22 |
| **CWE** | CWE-284 (Improper Access Control) |

## Summary

TCP port **22** is **open to the internet** on the residential game server IP (`99.42.xxx.xxx`). Banner identifies **OpenSSH 10.0p2** on **Debian 13**. This is full administrative access surface on the same machine running the live EQEmu world server and ATLAS admin UI.

## Example

**Port probe (2026-06-12):**

```
99.42.xxx.xxx:22 OPEN
Banner: SSH-2.0-OpenSSH_10.0p2 Debian-7+deb13u2
```

**Co-located services on same IP:**

| Port | Service |
|------|---------|
| 22 | SSH |
| 443 | ATLAS / nginx / Next.js |
| 5998 | EQEmu binary login |
| 9000 | EQEmu world server |

**Assessment:** No SSH login attempts, brute force, or credential testing were performed.

## Attack vector

**Prerequisites:** Network reachability to :22 (global internet).

**Steps:**

1. Attacker scans residential IP ranges or targets known game host.
2. Password spray, key guessing, or exploit against OpenSSH (if CVE applicable).
3. On success - full host compromise including game DB, ATLAS, player data at rest.

**Attacker role:** Anonymous internet attacker.

## Implications

- **Confidentiality / integrity / availability:** Root on game box = total compromise.
- **Player impact:** Character data, server integrity, downtime.
- **Residential ISP:** Home network pivot risk beyond EQEmu.

## Remediation

1. **Close SSH to internet** - allowlist management IPs only, or VPN (WireGuard/Tailscale) required.
2. Disable password authentication; **key-only** if SSH must exist.
3. Use `fail2ban` or equivalent; non-default port is weak alone - prefer firewall allowlist.
4. Move admin access off the game host entirely where possible.
5. Monitor auth logs for scanning activity.

## Verification

External scan from unrelated network:

```bash
nc -zv eqemu.ascendanteq.com 22
```

Expected after fix: **filtered/closed** for general internet, or reachable only from VPN endpoint.

Document management access method in operator runbook.
