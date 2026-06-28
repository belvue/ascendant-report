# Player Advisory: Where Ascendant EQ Actually Runs (Hosting & Security)

**Version:** 1.0  
**Date:** 2026-06-13  
**Scope:** Ascendant live game world (`eqemu.ascendanteq.com`)  
**Companion notice:** Login routing via `login.eqemulator.dev` is covered in a **separate advisory** (01-loginserver-brief).

---

## Summary (read this first)

**Your game world may run on a home internet connection - not a datacenter.**

After you log in, Ascendant world traffic goes to **`eqemu.ascendanteq.com`**, which resolves to **`99.42.xxx.xxx`** - an **AT&T residential broadband** line in the Illinois Metro East area, **not** OVH or another datacenter.

The **public website**, patch downloads, and donation page sit on **OVH US** (`147.135.10.69`) - a **different network** from the live game.

The same residential IP also exposes **EQEmu world ports**, **internet-facing SSH**, and a **public ATLAS enterprise admin interface** without a visible login gate. This was re-checked on **2026-06-13** and was **unchanged** since first observation.

Running a game server at home is **not automatically malicious**, but it is a **different reliability and security model** than cloud or datacenter hosting - and that difference is **not clearly disclosed** in patcher or donation messaging today.

**One-sentence takeaway:** When you play Ascendant, your client connects to a consumer ISP line that also carries operator admin tools.

---

## Who operates what (hosting map)

**Three layers, three networks.**

| Layer | Hostname / IP | Provider | What it does for you |
|-------|---------------|----------|----------------------|
| **Website / patch CDN** | `ascendanteq.com`, downloads → `147.135.10.69` | OVH US | Patcher, website, server info |
| **Federation login** | `login.eqemulator.dev:5999` | OVH Canada | Password validation *(see login advisory)* |
| **Live game world** | `eqemu.ascendanteq.com` → `99.42.xxx.xxx` | AT&T residential | Zones, characters, inventory |

```
EQ Client
  ├─ login.eqemulator.dev:5999 (OVH CA)     ← credential path [separate notice]
  └─ eqemu.ascendanteq.com:9000 (AT&T home) ← world / zone traffic
         │
         ├── :9000  EQEmu world server
         ├── :5998  EQEmu binary login (local)
         ├── :22    SSH (OpenSSH, internet-facing)
         └── :443   ATLAS admin UI + status API
```

**Context:** Sister server **ProFusion** runs its game stack on **OVH** (`eqemu.eqprofusion.org`). Ascendant’s **website** is on OVH; Ascendant’s **game world** specifically is on residential broadband. OVH hosting the website does **not** mean the game world is in a datacenter.

Residential DNS proves **consumer ISP hosting**, not a verified street address.

---

## What happens when you play

1. The patcher updates client files and sets login routing *(login advisory covers `eqhost.txt`)*.
2. Your client authenticates via the federation login gateway (OVH Canada).
3. World connection targets **`eqemu.ascendanteq.com`** → residential **`99.42.xxx.xxx`**.
4. The world server listens on **`:9000`**; binary login on **`:5998`**. Text login **`:5999` is not listening** on the game host.

**What is proven:** A **disclosure gap** - players are not clearly told the world runs on home broadband.

**What is not proven:** That operators **deliberately hid** this from everyone; that donations are **legally fraudulent**; or that attackers have **already compromised** ATLAS or SSH. Investigators found **misconfiguration**, not confirmed exploitation.

---

## Why hosting location matters

| Factor | Datacenter (e.g. ProFusion game on OVH) | Residential (Ascendant game) |
|--------|----------------------------------------|------------------------------|
| **Uptime** | Provider SLAs, redundant power/network | Home power, ISP outages, IP changes |
| **DDoS / abuse** | Provider mitigation | Consumer line - limited options |
| **Latency** | Stable routing | Depends on operator uplink |
| **Maintenance** | Scheduled windows | Router reboots, home network issues |

Community staff have discussed Ascendant game hosting moving from cloud to **local/residential** - DNS and port probes **confirm** the game path is residential. Long sessions, raids, and economy play carry **higher disconnect risk** than a datacenter-hosted peer server.

---

## Security exposure on the game host (June 2026)

Investigators used **read-only** checks only - no SSH attempts, no admin form submissions, no changes to operator systems.

| Issue | Summary |
|-------|---------|
| **ATLAS admin UI** | `/admin/*` routes return **HTTP 200** without a login wall on `:443` |
| **Status API** | `/api/admin/settings` returns **JSON without authentication** |
| **SSH** | Port **22** open to the internet (OpenSSH on Debian) |
| **Degraded backend** | ATLAS reports database/redis unreachable while admin UI remains public |
| **Dev config leak** | Page metadata references `localhost:3000` in production |

**We are not claiming** anonymous users have verified **write** access to ATLAS. Public UI shell and read API exposure are **confirmed**; destructive access was **not tested**.

Login credential handling via `login.eqemulator.dev` is a **separate trust boundary** - see the companion login advisory.

---

## Disclosure gap (why this notice exists)

| Location | Residential game hosting disclosed? | ATLAS/SSH exposure disclosed? |
|----------|-------------------------------------|-------------------------------|
| Ascendant website / Ko-fi | **Implied datacenter** - “hosting costs” | **No** |
| Patcher UI | **No** hosting topology | **No** |
| Discord `#helpful-info` | **Partial** - staff discussed cloud→home in community context | **No** public advisory |
| Donation page | “Hosting/ops/dev” - **no split** between OVH web and home game | **No** |

This notice fills the gap until operators publish hosting facts and fix ATLAS/SSH exposure.

---

## Donations & hosting costs

Fundraising, **Server Runway**, and Ko-fi transparency are covered in a **separate brief**: [03-transparency-and-accountability](../03-transparency-and-accountability/player-donation-notice-v1.md).

**Hosting fact that matters to donors:** the **live game world** runs on **home broadband** while **website/CDN** use **OVH** - see the topology table above. Donors expecting **datacenter game uptime** should read the donation notice before giving.

---

## What you can do

| Option | Notes |
|--------|-------|
| **Continue playing** | Accept residential uptime/security model |
| **Play but don’t donate** until disclosure improves | See [donation notice](../03-transparency-and-accountability/player-donation-notice-v1.md) |
| **Ask for a written hosting diagram** | Reasonable request of any operator |
| **Use unique passwords** | Limits cross-service reuse |
| **Play a datacenter-hosted EQEmu server** | e.g. ProFusion game on OVH - different tradeoffs |
| **Do not probe `/admin/*` on the game IP** | Avoid legal/ethical issues |

If you are a security researcher: document read-only findings only; do not attempt writes on ATLAS, SSH, or game ports without authorization.

---

## What we’re asking operators to publish

Within a reasonable remediation window:

1. **Hosting topology diagram** on the website or patch notes (same as the table above).
2. **Explicit residential disclosure** for `eqemu.ascendanteq.com` with uptime caveats.
3. **ATLAS remediation** - auth on all `/admin/*` and `/api/admin/*`; confirm fix date.
4. **SSH hardening** - VPN, allowlist, or key-only; confirm fix date.
5. **Donation / runway transparency** - [03-transparency-and-accountability](../03-transparency-and-accountability/README.md) (cost categories, Ko-fi totals, runway formula).
6. **Security contact** - email or Discord role for hosting/security questions.

A new player should be able to answer “where does my character data live?” without reading DNS tools.

---

## FAQ

| Question | Answer |
|----------|--------|
| Is the server hacked? | **No active compromise claimed.** Misconfiguration (public admin UI, SSH exposure) was confirmed. |
| Did the operator steal donation money? | **Fraud not proven.** See [donation notice](../03-transparency-and-accountability/player-donation-notice-v1.md) for informed-giving context. |
| Can I still play safely? | **Your risk judgment.** Misconfiguration increases **potential** impact; no exploitation observed in this package. |
| What about login.eqemulator.dev? | **Separate issue** - see companion login advisory. |
| Did the ISP shut them down? | Abuse report sent **2026-06-12**; **no takedown observed**; exposure **unchanged** on **2026-06-13** re-probe. |
| Why publish this now? | Assessment complete; operators given private remediation window; players deserve informed consent. |

---

## Limitations

- Based on live DNS, TCP port probes, and **read-only HTTPS GET** to public URLs.
- No unauthorized access, no SSH attempts, no admin write actions.
- GeoIP and reverse DNS classify **consumer ISP vs datacenter** - not a home address.
- Point-in-time snapshot - residential IPs can rotate; check publication date.

---

## Acknowledgement

Operators may embed [player-acknowledgement-block.md](player-acknowledgement-block.md) in patcher, donation page, or server rules.

---

*Player notice v1.0 - 2026-06-13. Factual corrections welcome from operator before or after publication.*
