# 04 - Infrastructure security

## Layered hosting (Jun 12 probe)

Independent DNS and port probes split Ascendant into two worlds:

| Layer | Host | Provider | Game ports |
|-------|------|----------|------------|
| Web, downloads, portal | `ascendanteq.com` / `147.135.10.69` | OVH US | 80/443 |
| Live EQEmu world | `eqemu.ascendanteq.com` / `99.42.xxx.xxx` | AT&T residential (Lightspeed rDNS) | 5998, 9000 open |

`login.ascendanteq.com` was NXDOMAIN at probe time. Players hitting the world use the residential host when resolving `eqemu.ascendanteq.com`. **Residential IPs in this report use partial masking (`99.42.xxx.xxx`); the game hostname is already public in the patcher.**

ProFusion's game host on OVH in the same operator cluster shows the operator **can** run worlds in a datacenter. Ascendant's live world is not on that OVH game IP.

## Login stack

Login for patched clients targets `login.eqemulator.dev`, separate from the world IP above. See [03-login-trust-boundary](03-login-trust-boundary.md).

## Residential attack surface (Jun 12 probe)

Independent port scans on `99.42.xxx.xxx` found more than EQEmu:

| Port | Open | Notes |
|------|------|-------|
| 22 | Yes | OpenSSH on Debian, reachable from the internet |
| 80, 443, 8080 | Yes | nginx; HTTPS serves Next.js |
| 5998, 9000 | Yes | EQEmu login / world |
| 5999, 3306, 7300-7329 | No | Not exposed at probe time |

ProFusion's game host in the same operator cluster runs on OVH with a sane split. Ascendant stacks **SSH, game traffic, and a web admin product** on a consumer AT&T line. That is not how you run something that bills itself like a mature hosted service.

## ATLAS admin on the game IP

The critical finding is not EQEmu itself. On `https://eqemu.ascendanteq.com/admin/settings` the residential game host serves **ATLAS - Capability & Context Control Plane**, a separate Next.js control-plane app (not the eqemulator.dev federation console; that path 404s there).

At probe time (2026-06-12):

- `/admin/settings`, `/admin/federation`, and related admin nav returned **HTTP 200 without a login redirect**.
- `GET /api/admin/settings` returned **JSON with no authentication**, leaking internal Docker service names, Kong gateway version, and degraded DB/Redis health.
- TLS on the raw IP did not match the cert. OG meta tags referenced `localhost:3000`, which reads like dev config shipped to a public residential address.

This is misconfiguration and excessive exposure. We did not authenticate, modify settings, or create keys. Document only.

The operator can run polished portals on OVH. The live world stack still reads as **vibe-deployed**: enterprise admin software on the same box as the game, bound to a home ISP IP, with management ports wide open.

## Misconfiguration notice (optional detail)

Unauthenticated admin UI on a **consumer residential IP** is a standard misconfiguration report, not a player-facing story. Investigators used AT&T's published abuse contact on **2026-06-12** to flag the exposed ATLAS routes, open SSH, and game ports on the host behind `eqemu.ascendanteq.com`. Framing: **security misconfiguration disclosure** for the ISP and subscriber. Not a takedown request, not an exploit attempt. No attachments, no authentication bypass, no write tests.

AT&T auto-acknowledged. Re-probes after that window showed **no visible hardening** on the residential stack at collection close. The point for donors is simpler: **management software on a home line stayed exposed through the probe window**, not who emailed whom.

## Dependency hygiene (partial, not a program)

Operator self-audit (Mar 2026) fixed critical SQLi and open sync endpoints. That is reactive patch work, not a security posture. Residual npm advisories remained on the pinned web lockfile at review time.

Straps joked in `#patch-notes` (2026-02-16, message `1472844679274958879`) that progress tracking ran on **"vibes and wishful thinking"** until he started "writing things down." The bit is funny. The pattern behind it is not: ship fast, patch later, run production donations on a home lab that still exposes admin UI and SSH to the internet.

"Self-hosted" here means **someone's house on AT&T**, not "you control the metal." Uptime, DDoS, legal exposure, and breach blast radius look different on residential broadband.

## Diagram (logical)

```
Player client
  | patcher CDN -> eqhost.txt -> login.eqemulator.dev (operator login DB)
  | game connect -> eqemu.ascendanteq.com -> 99.42.xxx.xxx (residential world + SSH + ATLAS admin :443)
  | website -> ascendanteq.com -> OVH (portal, patch zip)
```

Next: [04b-operational-security](04b-operational-security.md)
