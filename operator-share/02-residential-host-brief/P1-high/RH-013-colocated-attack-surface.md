# RH-013: Game, SSH, and enterprise admin on one residential IP

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (probe) - architectural |
| **Component** | Network segmentation |
| **CWE** | CWE-653 (Insufficient Compartmentalization) |

## Summary

The residential IP **`99.42.xxx.xxx`** simultaneously exposes:

- EQEmu **world server** (:9000) and **binary login** (:5998)
- **SSH** administration (:22)
- **ATLAS** enterprise admin + catalog UI (:443)
- Alternate HTTP (:8080)

There is no evidence of network segmentation isolating player game traffic from admin/control-plane services.

## Example

```text
99.42.xxx.xxx (eqemu.ascendanteq.com) - AT&T residential
├── :22    OPEN   SSH (Debian 13)
├── :443   OPEN   nginx + Next.js (ATLAS admin)
├── :5998  OPEN   EQEmu binary login
├── :8080  OPEN   HTTP (timeout on probe)
└── :9000  OPEN   EQEmu world server
```

Compare: **eqemulator.dev** admin is not on this IP. **Ascendant OVH portal** (`147.135.10.69`) serves web only - game ports closed there.

## Attack vector

**Prerequisites:** Vulnerability or credential compromise on any exposed service.

**Steps:**

1. Attacker exploits ATLAS, SSH, or nginx issue on :443/:22.
2. Lateral movement to world server process, game database, or backups on same host.
3. Player-facing impact from admin-layer breach.

**Attacker role:** Depends on entry point - segmentation failure amplifies all P0 findings.

## Implications

- **Blast radius:** Single IP compromise affects game + admin + home network pivot.
- **Best practice violation:** Production game, remote admin, and experimental control plane should not share a consumer IP.

## Remediation

1. **Segment:** Game on dedicated host or VLAN; admin on VPN-only management network.
2. Remove ATLAS and SSH from internet-facing interfaces on game box.
3. Use OVH or separate VPS for public web; residential only for game if unavoidable - still restrict admin ports.
4. Firewall default-deny; allow only 5998/9000 from players if needed.

## Verification

External port scan shows only required game ports; 22/443 admin not reachable from internet.

Document architecture diagram post-remediation.
