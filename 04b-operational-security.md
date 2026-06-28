# 04b - Operational security

This chapter is the surface map of **how the operator runs the stack**, not just one bad port scan. The investigation logged **75 structured findings** across login, hosting, patcher, transparency, and funding briefs. What follows is the public-safe summary players and donors were never given in product.

## The gap in one sentence

Ascendant presents like a hosted MMO service: donation runway, polished portal, federation login, leaderboards, and staff enforcement rhetoric. Under that skin the game world sits on a **home ISP box** with **management ports and admin UI exposed**, the **website talks to live characters through unauthenticated JSON routes**, and **trust-critical facts live in Discord pins** instead of the patcher or support site.

That is operational immaturity, not a single misconfigured firewall rule.

## Three networks, one brand

Players experience "Ascendant." Probes split it into **three different networks** with no in-client explanation:

| Layer                              | Where it runs           | Provider (Jun 2026)                | Player told?                                                                                   |
| ---------------------------------- | ----------------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------- |
| Website, patch CDN, character APIs | `ascendanteq.com`       | OVH US (`147.135.10.69`)           | Ko-fi **~$411/mo** runway vs **~$55/mo**  est. ([11-hosting-cost-gap](11-hosting-cost-gap.md)) |
| Login / federation gateway         | `login.eqemulator.dev`  | OVH Canada (`144.217.76.180`)      | Buried Discord / forum and GitHub only                                                         |
| Live world + SSH + ATLAS admin     | `eqemu.ascendanteq.com` | AT&T residential (`99.42.xxx.xxx`) | Joke photo in `#general`                                                                       |

Credentials, character data APIs, and the game process **do not share one security boundary**. A player who reads the donation page reasonably thinks they fund datacenter hosting. The game process runs on a consumer line with internet SSH and an admin control plane on `:443`. Detail on probes and ATLAS: [04-infrastructure-security](04-infrastructure-security.md). Login path: [03-login-trust-boundary](03-login-trust-boundary.md).

## Ascendant and ProFusion: shared stack, split risk

Cross-reference work (2026-06-08) treats **Ascendant and ProFusion as one operator cluster** with high confidence:

| Signal | Ascendant | ProFusion |
|--------|-----------|-----------|
| Marketing / portal web IP | `147.135.10.69` | **Same IP** |
| Web stack | nginx + Vite/React portal | **Same stack** |
| Public character search API | `/api/characters/search` → **200 JSON** | **Same schema, 200 JSON** |
| Game world IP | `99.42.xxx.xxx` (residential) | `147.135.9.147` (OVH) |
| Federation on eqemulator.dev | trusted + claimed | trusted + claimed |

ProFusion's **game** stayed in OVH. Ascendant's **game** moved home. The **website layer stayed shared**, which makes both servers **look** datacenter-hosted even when only one world is.

Operational continuity shows up elsewhere:

- Straps told rule-breakers to go to **ProFusion** if they want warp/AFK servers (Discord `#server-files`, captured in [data/discord-quotes-public.md](data/discord-quotes-public.md)).
- ProFusion `#server-support` published the same **PayPal Friends and Family** pattern and MoA-adjacent support rails before Ascendant's Ko-fi page (chapter [05-donations-moa-kofi](05-donations-moa-kofi.md)).
- Both list as **trusted** entries on `login.eqemulator.dev`'s server directory.

**Why it matters for security:** Shared portal code, shared APIs, and shared federation mean a vulnerability or misconfiguration on the **web tier** or **login gateway** can affect **both communities**. Players choosing "the strict server" vs "the relaxed server" are not choosing equivalent infrastructure on the game layer.

## Web APIs that reach into live game state

Investigators used **read-only GET probes** only. No write tests, no auth bypass attempts. Even so, the **public surface** is larger than a game server should expose casually.

### Character and roster APIs (Ascendant + ProFusion portal)

| Route (on `ascendanteq.com` / `eqprofusion.org`) | Auth observed | What it enables |
|--------------------------------------------------|---------------|-----------------|
| `GET /api/characters/search` | None | Paginated roster JSON; same response shape on both domains from the shared portal IP |
| `GET /api/characters/{name}` | None | Per-character profile payload; BotWatch's watch worker pulls inventory/stats this way (`inventory_scraper.pull_watch_character`) |

Any watcher on the internet can **enumerate and profile characters** without a game client. That is economic surveillance (who holds what, who is online in roster views) and reconnaissance for targeted harassment or RMT tracking. It is not proof the operator sells data. It **is** proof the web tier was built like a convenience API, not a least-privilege game backend. BotWatch used the same public routes; no special access was required ([09-botwatch-methodology](09-botwatch-methodology.md)).

### Character move tooling (ProFusion portal inventory)

ProFusion's public URL inventory (2026-06-08 crawl) references **`/api/characters/move`** and **`/api/characters/move/zones`**. These routes sit in the same portal family as search. Investigators did **not** attempt authenticated moves or POST bodies. Their presence still matters: the operator's **website stack includes game-manipulation tooling** wired toward live characters, not a read-only fansite.

Treat unauthenticated read routes as confirmed exposure. Treat move routes as **architectural coupling** between portal and game ops until the operator publishes auth and audit documentation.

### Federation gateway APIs (`eqemulator.dev`)

| Route | Auth observed | Notes |
|-------|---------------|-------|
| `GET /api/servers` | Public (200 at collection; 403 seen at other times) | Live player counts, trust flags, curated directory |
| `GET /api/status` | Public (200 in lab reproduction) | Host health, DB/service summary JSON |
| `GET /api/metrics` | Blocked (403) | Prometheus correctly gated in lab |
| `GET /api/federation/sync_data` | Signed federation peer only | Exports **all login password hashes** to approved peers ([LS-012](operator-share/01-loginserver-brief/P1-high/LS-012-federation-sync-data-hash-export.md)) |

The login gateway is not just "where passwords go." It is a **control plane** that can **replicate credential hashes** to every federation peer the operator approves. Anonymous users cannot pull `sync_data`. An **approved** peer (or stolen peer key, or a loose bootstrap approval) can export hashes and, per reviewed federation code, **push password hash updates** by account row id. That is concentrated blast radius, not "any random emu server can hijack you." Detail: [03-login-trust-boundary](03-login-trust-boundary.md#federation-blast-radius-can-another-server-steal-your-account).

### Residential ATLAS (game IP)

On the residential game host (`eqemu.ascendanteq.com`), `/admin/*` and `/api/admin/settings` responded without authentication (see [04-infrastructure-security](04-infrastructure-security.md)). That is a different class of exposure: **infrastructure reconnaissance** (Kong version, Docker service names, RBAC/API-key admin nav) on the same box as SSH and `:9000`.

**Combined risk picture:** Portal APIs leak **who plays and what they hold**. Federation APIs leak **platform health and directory curation**. Residential ATLAS leaks **how the home box is wired**. None of that requires a game account.

## Forced login migration with no product disclosure

The login move to `login.eqemulator.dev` is documented in [03-login-trust-boundary](03-login-trust-boundary.md). Operational security includes **how** the change was deployed:

1. **CDN-driven eqhost rewrite ([PC-010](operator-share/04-client-patcher-integrity/P1-high/PC-010-eqhost-login-redirect-via-cdn.md)):** Patcher ships `eqhost.txt` pointing at `.dev` and comments out `.net`.
2. **Silent re-application ([PC-020](operator-share/04-client-patcher-integrity/P2-medium/PC-020-autopatch-reapplies-cdn-files.md)):** `autoPatch: true` re-fetches CDN files every cycle. A player who manually reverts to `.net` can be overwritten on the next patch without a UI warning.
3. **No first-run consent ([LS-015](operator-share/01-loginserver-brief/P1-high/LS-015-login-path-disclosure-gap.md)):** Patcher UI strings reviewed at investigation time did not disclose the trust-boundary shift.
4. **Discord-primary disclosure ([TR-011](operator-share/03-transparency-and-accountability/P1-high/TR-011-discord-primary-disclosure-pattern.md)):** Trust-affecting facts appear in Discord first or only:

| Topic | Discord | Patcher / website |
|-------|---------|-------------------|
| eqhost / login host change | `#helpful-info`, New Tanaan thread | No first-run modal |
| `dinput8.dll` / MQ compatibility | `#server-files`, `#patch-notes` | No file manifest in patcher UI |
| Cloud → home game host | Staff chat (Ali quote) | Not on donation or trust page |
| Login troubleshooting | Bug / MQ threads | No canonical web recovery doc |

A player **without Discord** hits login failures, misses migration guidance, and only learns about `.dev` or residential hosting from Reddit, witnesses, or reports like this one. That is **operational security through information asymmetry**, not informed consent.

Straps did write about `.dev` in `#helpful-info` (message `1482395586249363457`). Buried staff posts are not equivalent to **decision-point disclosure** at eqhost write time.

## "Vibe coding" vs a security program

Several patterns read as **ship first, harden later, never finish**:

- **Reactive audit (Mar 2026):** Operator fixed critical SQLi and open federation sync endpoints after review. Residual npm/CVE items remained on the login stack ([LS-003](operator-share/01-loginserver-brief/P0-immediate/LS-003-nextjs-vulnerable-dependencies.md), [LS-004](operator-share/01-loginserver-brief/P0-immediate/LS-004-drizzle-orm-sql-injection-cve.md)).
- **Admin UI while backend degraded:** ATLAS settings page loaded on the public internet while API JSON reported DB unreachable and Redis unavailable ([RH-012](operator-share/02-residential-host-brief/P1-high/RH-012-atlas-backend-degraded.md)).
- **Residential exposure, no visible fix on re-probe:** ATLAS admin and SSH on consumer AT&T line; re-probe 2026-06-13 unchanged ([RH-031](operator-share/02-residential-host-brief/P3-informational/RH-031-reprobe-unchanged.md)). Misconfiguration notice to ISP is documented in [04-infrastructure-security](04-infrastructure-security.md#misconfiguration-notice-optional-detail) for readers who need it; it is not the player story.
- **Self-aware ops humor:** Straps posted in `#patch-notes` (2026-02-16, message `1472844679274958879`) that progress tracking ran on **"vibes and wishful thinking"** until he started "writing things down." Funny in character. It matches the artifact pattern: expansion tiers ship with "not fully tested yet" language, Ko-fi feed gets curated, login moves via CDN, and SSH stays open.

A mature operation separates **game**, **web**, **login**, and **admin** networks; publishes a trust page; gates character APIs; and treats federation hash export as a player-visible policy. This cluster did not meet that bar at collection close.

## Findings map (investigator shorthand)

| Prefix | Topic | Public chapter | Operator brief |
|--------|-------|----------------|----------------|
| LS- | Loginserver / federation | [03](03-login-trust-boundary.md), this chapter | [01-loginserver-brief](operator-share/01-loginserver-brief/README.md) |
| RH- | Residential host / ATLAS | [04](04-infrastructure-security.md) | [02-residential-host-brief](operator-share/02-residential-host-brief/README.md) |
| PC- | Patcher / eqhost supply chain | [03](03-login-trust-boundary.md), [07](07-anti-cheat-and-patcher.md) | [04-client-patcher-integrity](operator-share/04-client-patcher-integrity/README.md) |
| TR- / DT- / KF- | Transparency / donations / Ko-fi | this chapter, [05](05-donations-moa-kofi.md) | [03-transparency-and-accountability](operator-share/03-transparency-and-accountability/README.md) |
| PF- | Funding / runway buckets | [11](11-hosting-cost-gap.md), [05](05-donations-moa-kofi.md) | [05-project-funding](operator-share/05-project-funding-and-operator-compensation/README.md) |
| API surface | Portal + federation GET probes | this chapter | - |

Full neutral briefs with remediation checklists: [operator-share/](operator-share/README.md). Response template: [operator-response.md](operator-share/operator-response.md).

## What we do not claim

- No proof that anonymous callers used move APIs or ATLAS write paths.
- No proof of intentional credential harvesting.
- No claim that public character APIs were monetized.

The finding is **negligent exposure and disclosure failure** on a stack that asks for passwords, dollars, and trust.

Next: [05-donations-moa-kofi](05-donations-moa-kofi.md)
