# 02 - Executive summary

This is the one-page version. If you stop here, you still get the shape of the problem.

## Login

The patcher ships an `eqhost.txt` that sends credentials to `login.eqemulator.dev:5999`, not the usual `login.eqemulator.net` path (finding [PC-010](operator-share/04-client-patcher-integrity/P1-high/PC-010-eqhost-login-redirect-via-cdn.md)). That is an operator-controlled login stack. Every patch can rewrite the file from CDN. The same login path showed a **six-server curated directory** (Ascendant and ProFusion in Legends) instead of the **dozens-wide community list** on `.net`. If you use the required launcher, your password is validated on infrastructure Straps runs. We did not find covert exfiltration in reviewed web source.

The trust boundary moved anyway. Straps did write about it. A Mar 14 post in Ascendant `#helpful-info` (message `1482395586249363457`) explains the patcher pointing at a local login server and how to revert `eqhost.txt`. He also pitched `login.eqemulator.dev` at length in the **New Tanaan** EQEmu community Discord (Apr 2026, "New Loginserver" thread). None of that is in the patcher UI, the new-player path, or anywhere a normal player would stumble on it. You have to go looking. Buried disclosure is not the same as informed consent.

## Hosting

The live world at `eqemu.ascendanteq.com` resolves to **AT&T residential** service. The public website and CDN sit on **OVH**. Ali (Kalioptra) stated on Discord that Straps moved from cloud hosting to a **physical machine at his place**. Straps posted a photo in `#general` captioned that the homebuilt PC is "Norrath." The Ko-fi support page presents **~$411/month as infrastructure burn** (runway widget). A generous investigator estimate for **actual** infrastructure (OVH web + home power + ISP marginal) is **~$55/month**; total operation including operator and dev time could fairly be **$411+**, but that is not how the page labels it. Detail: [11-hosting-cost-gap](11-hosting-cost-gap.md).

## Backend security

Probes on the residential IP found **SSH, game ports, and an exposed ATLAS admin control plane** on the same box. `/admin/settings` and `/api/admin/settings` responded without authentication at collection time, leaking internal gateway and health metadata. Re-probes through mid-June showed no visible hardening. This reads as a **home lab running like production**, not a mature hosted operation. Detail: [04-infrastructure-security](04-infrastructure-security.md).

## Operational security (the wider surface)

Beyond the residential box, the **shared Ascendant/ProFusion portal** exposes **`/api/characters/search` and `/api/characters/{name}` without authentication**, enabling roster enumeration and character profiling from the public web. ProFusion's portal inventory also references **character move routes**. The **login gateway** exposes directory and status APIs and can **replicate password hashes to approved federation peers**. Trust-critical migration facts (eqhost rewrite, home hosting, `dinput8`) were **Discord-primary**, not patcher or website disclosures. Full map: [04b-operational-security](04b-operational-security.md).

## Donations and MoA

Ascendant runs Ko-fi, Marks of Ascension, referral contests, and leaderboard buffs on top of a "voluntary gift, no exchange" policy. Straps confirmed in a published staff capture that he **sent MoA to players** tied to support activity. That pattern did not start with Ascendant: ProFusion `#server-support` already published PayPal **Friends and Family** instructions to `straps.eq@gmail.com` with **character name only** in the note and Timekeeper credits on the in-game side (screenshot in chapter 05). USD support and premium currency are connected in practice even when the public copy says otherwise.

## Ko-fi feed

BotWatch polled the public Ko-fi feed through mid-June 2026. Rows disappeared selectively, then the feed went empty on June 15. In a private message (screenshot withheld; recipient-traceable), Straps characterized the gaps as the platform clearing the feed at random. Ko-fi's own Help Center documents **per-message hide only**, by creator or logged-in supporter. Nothing about random cleanup. The telemetry fits **administrative curation by the operator**, not undocumented platform behavior, and it points to **questionable ethics from the provider** running the page, not a Ko-fi glitch.

## Enforcement

Discord policy channels document rules around AFK play and MQ tooling. BotWatch measured high AFK scores on watched characters while a ban wave landed elsewhere. The pattern is asymmetric: documented policy vs selective application.

Players who filed detailed reports (logs, screenshots, even video) on botting, warping, and other rule breaks describe staff responses ranging from dismissive to sarcastic, not a consistent investigation queue. That is anecdotal across multiple players; it still matches the telemetry: a long AFK tail on roster while ban activity did not track that tail. Detail: [06-enforcement-asymmetry](06-enforcement-asymmetry.md).

## Outside observer

Everything above was rebuilt as an **outside observer** with **extremely limited access**: no staff SQL, no zone console, no insider tickets, no permission to run their backend. It was **not hard**. Repeated `/who` checks, periodic inventory headers, public Ko-fi HTML, Discord exports, client eqlogs, patcher and CDN artifacts, and forum posts were enough to reconstruct marathon camps, donation runway, login routing, and who got banned when. BotWatch is those pieces on a schedule.

The lack of awareness and accountability on staff is **weak relative to the hosted-MMO posture they present**. They sell rules, donations, and enforcement rhetoric, then act surprised when a player documents multi-day AFK boxes, credential paths, or feed edits that anyone with patience and a spreadsheet could see. Methodology detail: [09-botwatch-methodology](09-botwatch-methodology.md#sources-and-scope).

## How I read it

Ascendant is playable and polished in places. The gap is **accountability**: where login goes, where the world actually runs, what donations fund, and who gets enforced against.

On enforcement and culture, the pattern looks worse than inconsistent rules. Discord and forum record show **selective enforcement** next to roster telemetry where high-AFK targets stayed active. When regular players bring **factual reports with evidence** (screenshots, logs, video) about cheating, botting, and other violations, staff often meet them with **sarcasm or demeaning comments** instead of a straight audit trail. The May bridle disclosure and ban wave are the extreme case; detail in [14-reporter-account](14-reporter-account.md).

That leads me to **speculate** (not prove) that **repeat donors and Discord-regular insiders get softer treatment** than players with no donation or social proximity. Ko-fi-linked MoA, referral Marks, and discretionary staff sends make USD and social proximity legible to the operator.

I may be wrong on motive. The **posture toward players in general** still often reads as disdain: mockery of players who raise bugs, forum spin, patch-and-vanish when scrutiny hits, and public copy that treats the community like an audience rather than partners. Players deserve plain-language answers without being the punchline for reporting bugs. Disagree with my read; check [06-enforcement-asymmetry](06-enforcement-asymmetry.md), BotWatch tables in [09-botwatch-methodology](09-botwatch-methodology.md), and staff quotes in [data/discord-quotes-public.md](data/discord-quotes-public.md).

## Charts and source index

- BotWatch Ko-fi dashboard (section crops): [05-donations-moa-kofi](05-donations-moa-kofi.md#botwatch-dashboard-section-crops-jun-27-2026)
- BotWatch capability screenshots (masked roster/timeline/signals): [09b-botwatch-capabilities](09b-botwatch-capabilities.md)
- Anonymized Ko-fi events CSV: [data/kofi-events-anonymized.csv](data/kofi-events-anonymized.csv)
- Staff Discord quotes: [data/discord-quotes-public.md](data/discord-quotes-public.md)

## Where to go next

| If you care about... | Start here |
|---------------------|------------|
| Passwords and patcher | [00-credential-warning](00-credential-warning.md), [03-login-trust-boundary](03-login-trust-boundary.md), [03b-loginserver-lab-methodology](03b-loginserver-lab-methodology.md) |
| Home server vs cloud | [11-hosting-cost-gap](11-hosting-cost-gap.md), [12-operator-presence](12-operator-presence.md) |
| Backend exposure / ISP notice | [04-infrastructure-security](04-infrastructure-security.md) |
| Portal APIs, cluster overlap, disclosure gaps | [04b-operational-security](04b-operational-security.md) |
| Money and MoA | [05-donations-moa-kofi](05-donations-moa-kofi.md), [08-referrals-leaderboards](08-referrals-leaderboards.md) |
| Bans and bots | [06-enforcement-asymmetry](06-enforcement-asymmetry.md), [07-anti-cheat-and-patcher](07-anti-cheat-and-patcher.md) |
| How this was measured | [02 - Outside observer](02-executive-summary.md#outside-observer), [09-botwatch-methodology](09-botwatch-methodology.md), [09b](09b-botwatch-capabilities.md) |
| What to do | [13-what-you-can-do](13-what-you-can-do.md) |

Full infrastructure and security detail: [04-infrastructure-security](04-infrastructure-security.md).

## Why public now

This summary was meant to land in a private remediation brief first. By mid-June the operator was quiet on login and donation questions, the residential backend was still exposed (admin UI, open management ports, no hardening after ISP notice), and PoP shipped with on-record untested language anyway.

Pulling the patterns together made private presentation feel moot for a different reason too: **normalization and attention**. Item tiers show **LDoN before Luclin** (non-live order) and a **PoP band that does not continue the Luclin power jump** while players already wear Luclin-era gear ([07-anti-cheat-and-patcher](07-anti-cheat-and-patcher.md), [data/expansion-item-scaling.md](data/expansion-item-scaling.md)). Effort on one of the game's pillar expansions reads **lackluster at best**. The operator involvement that stayed consistent through scrutiny was **Ko-fi feed curation**, not PoP QA, not AFK enforcement, not infra fixes.

Presenting the same findings privately felt pointless after post-ban harassment, post-mortem silence, and Straps staying absent from the server too long while players and donors still lacked an honest record. Public release is the fallback described in [01b-before-going-public](01b-before-going-public.md), not revenge. I would still prefer Ascendant fix these items and keep the community.

Next: [03-login-trust-boundary](03-login-trust-boundary.md)
