# Private operator summary - Ascendant residential game host

**Date:** 2026-06-13  
**Audience:** Ascendant / eqemulator.dev operator (private review)  
**Package:** [02-residential-host-brief/](.) - 17 findings  

---

## Purpose

Neutral **security and transparency review** of the **live Ascendant game stack** at `eqemu.ascendanteq.com` → `99.42.xxx.xxx`. Assessment used **read-only HTTPS GET** and port probes only. This does **not** claim your systems were compromised - it documents **public misconfiguration** and **player disclosure gaps**.

**Login routing** (`login.eqemulator.dev`) is covered in a **separate brief** ([01-loginserver-brief](../01-loginserver-brief/private-operator-summary.md)).

---

## Executive summary

| Priority | Count | Action |
|----------|-------|--------|
| **P0 - Immediate** | 3 | ATLAS auth + SSH this week |
| **P1 - High** | 5 | Player disclosure + hosting honesty |
| **P2 - Medium** | 4 | Config hygiene + hosting comparison |
| **P3 - Informational** | 5 | Context / assessment limits |

**Top actions before public player notice:**

1. **RH-001 / RH-002** - Require authentication on all `/admin/*` and `/api/admin/*`; stop serving ATLAS admin on public residential IP or move behind VPN.
2. **RH-003** - Restrict SSH to VPN or allowlisted IPs; disable password auth if keys only.
3. **RH-011** - Publish hosting topology (OVH web vs residential game).
4. **RH-031** - Re-probe shows **no change** since 2026-06-12 AT&T report - please confirm receipt/remediation plan.

---

## What we verified (critical)

| Finding | Status (2026-06-13) |
|---------|---------------------|
| ATLAS `/admin/settings` | **HTTP 200**, no login redirect |
| `/api/admin/settings` | **HTTP 200 JSON without auth** |
| SSH `:22` | **Open** - OpenSSH 10.0p2 Debian 13 |
| Game world `:9000`, login `:5998` | **Open** on residential AT&T |
| Re-probe after AT&T notice | **Unchanged** |

---

## What we did not do

- POST/PUT/DELETE to ATLAS or game APIs  
- SSH login or credential guessing  
- Claim anonymous **write** access to ATLAS (UI shell + read API only - RH-032)  

---

## ISP notification

Responsible disclosure was sent to **abuse@att.net** on **2026-06-12** regarding public admin interface on `99.42.xxx.xxx` (RH-030). This package is independent of ISP process - it gives you a structured remediation list.

---

## Suggested response

1. Review findings by **RH-xxx** in [README.md](README.md).  
2. Complete [operator-response.md](../operator-response.md).
3. Reply with factual corrections or remediation dates.  
4. If you publish your own player notice, we can align public advisory timing.

---

## Out of scope

See [README.md](README.md).

---

## Contact

_(Operator fills channel in response template.)_
