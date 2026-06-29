# Residential game host - security findings archive

**Product:** Ascendant EQ game infrastructure (`eqemu.ascendanteq.com`)  
**Primary IP:** `99.42.xxx.xxx` (AT&T residential, Illinois Metro East)  
**Assessment date:** 2026-06-12 through 2026-06-13 (re-probe)  
**Scope:** DNS, TCP port map, read-only HTTPS GET to public URLs - no writes, no SSH attempts, no admin form submission

---

## Priority index

### P0 - Immediate (remediate before continued public exposure)

| ID | Title | File |
|----|-------|------|
| RH-001 | ATLAS admin UI reachable without authentication gate | [P0-immediate/RH-001-atlas-admin-ui-exposed.md](/operator-share/02-residential-host-brief/P0-immediate/RH-001-atlas-admin-ui-exposed.md) |
| RH-002 | Unauthenticated `/api/admin/settings` information disclosure | [P0-immediate/RH-002-atlas-admin-api-unauthenticated.md](/operator-share/02-residential-host-brief/P0-immediate/RH-002-atlas-admin-api-unauthenticated.md) |
| RH-003 | SSH (OpenSSH) exposed to the internet on game host | [P0-immediate/RH-003-ssh-internet-exposed.md](/operator-share/02-residential-host-brief/P0-immediate/RH-003-ssh-internet-exposed.md) |

### P1 - High (continuity, trust, co-location risk)

| ID | Title | File |
|----|-------|------|
| RH-010 | Live game world hosted on consumer residential broadband | [P1-high/RH-010-residential-game-hosting.md](/operator-share/02-residential-host-brief/P1-high/RH-010-residential-game-hosting.md) |
| RH-011 | Split infrastructure (OVH web vs home game) not disclosed to players | [P1-high/RH-011-hosting-topology-disclosure-gap.md](/operator-share/02-residential-host-brief/P1-high/RH-011-hosting-topology-disclosure-gap.md) |
| RH-012 | ATLAS control plane degraded while admin UI remains public | [P1-high/RH-012-atlas-backend-degraded.md](/operator-share/02-residential-host-brief/P1-high/RH-012-atlas-backend-degraded.md) |
| RH-013 | Game, SSH, and enterprise admin stack on one residential IP | [P1-high/RH-013-colocated-attack-surface.md](/operator-share/02-residential-host-brief/P1-high/RH-013-colocated-attack-surface.md) |
| RH-014 | Player world path terminates on residential IP after federation login | [P1-high/RH-014-login-vs-world-network-split.md](/operator-share/02-residential-host-brief/P1-high/RH-014-login-vs-world-network-split.md) |

### P2 - Medium (hardening, optics, configuration hygiene)

| ID | Title | File |
|----|-------|------|
| RH-020 | Production deploy leaks dev configuration (localhost OG tags) | [P2-medium/RH-020-dev-config-in-production.md](/operator-share/02-residential-host-brief/P2-medium/RH-020-dev-config-in-production.md) |
| RH-021 | TLS certificate mismatch when accessing host by raw IP | [P2-medium/RH-021-tls-cert-ip-mismatch.md](/operator-share/02-residential-host-brief/P2-medium/RH-021-tls-cert-ip-mismatch.md) |
| RH-022 | Alternate HTTP port 8080 open on game host | [P2-medium/RH-022-port-8080-exposed.md](/operator-share/02-residential-host-brief/P2-medium/RH-022-port-8080-exposed.md) |
| RH-024 | Ascendant game residential while sister server ProFusion on OVH | [P2-medium/RH-024-profusion-ascendant-hosting-split.md](/operator-share/02-residential-host-brief/P2-medium/RH-024-profusion-ascendant-hosting-split.md) |

_Donation / Ko-fi / runway findings moved to [03-transparency-and-accountability](../03-transparency-and-accountability/README.md) (formerly RH-023 → DT-001)._

### P3 - Informational (context, policy, positive notes)

| ID | Title | File |
|----|-------|------|
| RH-030 | ISP abuse report sent to AT&T (2026-06-12) | [P3-informational/RH-030-att-abuse-report-sent.md](/operator-share/02-residential-host-brief/P3-informational/RH-030-att-abuse-report-sent.md) |
| RH-031 | Re-probe 2026-06-13 - no remediation observed | [P3-informational/RH-031-reprobe-unchanged.md](/operator-share/02-residential-host-brief/P3-informational/RH-031-reprobe-unchanged.md) |
| RH-032 | Anonymous ATLAS write access not demonstrated | [P3-informational/RH-032-atlas-write-access-unverified.md](/operator-share/02-residential-host-brief/P3-informational/RH-032-atlas-write-access-unverified.md) |
| RH-033 | Zone ports not exposed to internet (730x closed) | [P3-informational/RH-033-zone-ports-not-exposed.md](/operator-share/02-residential-host-brief/P3-informational/RH-033-zone-ports-not-exposed.md) |
| RH-034 | Database ports not exposed (3306/6379 closed) | [P3-informational/RH-034-database-ports-closed.md](/operator-share/02-residential-host-brief/P3-informational/RH-034-database-ports-closed.md) |

---

## Player-facing materials

| Document | Purpose |
|----------|---------|
| [player-hosting-notice-v1.md](player-hosting-notice-v1.md) | Plain-language summary for players |
| [player-acknowledgement-block.md](player-acknowledgement-block.md) | Checkbox consent text for operator to embed |

---

## Controls verified (probe)

- **eqemulator.dev** does not host ATLAS admin (`/admin/settings` → 404) - exposure is on game IP only
- No POST/PUT/DELETE to operator admin APIs during assessment
- No SSH login attempts or brute force
- MySQL/Redis/Postgres ports closed on game IP from external probe

---

## Response

[operator-response.md](../operator-response.md) (hosting section) · [private-operator-summary.md](private-operator-summary.md)

---

## Out of scope

This archive covers **the residential game host** (`eqemu.ascendanteq.com` / `99.42.xxx.xxx`) - ports, ATLAS/SSH exposure, hosting topology. It does **not** assess:

- **Login credential path** - validation, LSPX, federation hash sync on `login.eqemulator.dev` ([01-loginserver-brief](../01-loginserver-brief/README.md); RH-014 notes the network split only)
- **OVH web portal** - `147.135.10.69` website/CDN except where compared for disclosure (RH-011, RH-024)
- **EQEmu game protocol and in-game data** - world/zone logic, character DB contents, economy (external port map only)
- **ATLAS or SSH write access** - no form POST, API key creation, RBAC changes, or login attempts (RH-032)
- **Internal zone networking** - 730x closed from internet; no audit of processes behind `:9000`
- **ProFusion or other servers** - except public hosting comparison (RH-024)
- **Donations, trust hub, patcher disclosure** - [03-transparency-and-accountability](../03-transparency-and-accountability/README.md)
- **Governance disputes** - ban narrative, fraud allegations, operator intent (misconfiguration and disclosure gaps only)
