# 01 - Why this exists

Ascendant EQ is a custom EverQuest emulator run by Straps. It launched around March 2026 with fast progression, Marks of Ascension (MoA), Ko-fi support, and a custom patcher. Kali (Kalioptra/Ali) and Beorc show up regularly as senior staff. The server is polished in some places. Many players are having fun and have positive things to say. None of that is why this documentation exists.

This package exists because the **public story and the record do not match**. Players are told to trust a polished launcher, voluntary donations with no exchange, and Discord rules that ban unattended botting. The record shows patcher login on operator infrastructure, a live world on home internet while Ko-fi cites hundreds of dollars per month in hosting burn, Marks of Ascension with real in-game utility after USD support, a Ko-fi feed that thinned while internal records kept more history, and AFK telemetry that does not line up with enforcement rhetoric. That gap is the difference between informed consent and marketing.

In May 2026 a player reported a large platinum-generation bug in private, was permabanned within about twenty minutes, and staff posted mockery and a disputed ledger the next day. That was the moment to stop taking the server's public posture at face value and start looking at login routing, donation optics, hosting claims, and who actually gets enforced against. The full reporter thread lives in [14-reporter-account](14-reporter-account.md) and its appendices.

Most of what followed was done with **no staff access** - and it was **not hard**. That framing matters: see [02-executive-summary - Outside observer](02-executive-summary.md#outside-observer).

This report is a **public, redacted** slice of a larger private investigation. Player names, donor names, and recipient-traceable DMs stay out. Staff on-record Discord quotes stay in. Anything withheld gets a placeholder that explains why it is not shown. Redaction rules (player/donor names out, staff on-record quotes in) apply throughout this package.

## What is in here

If you read straight through, the arc goes like this:

- **Credentials and login** (00 through 03, 03b): what the patcher does to `eqhost.txt`, where credentials validate, and why operator-controlled login matters even when passwords are stored as hashes.
- **Hosting and security** (04, 04b): where the world runs, exposed admin/API surface, Profusion cluster overlap, web-to-game routes, Discord-only disclosure.
- **Donations and Ko-fi** (05, 08): MoA, support page runway, referral gamification, and what happened to the public feed.
- **Enforcement** (06, 09b): policy on Discord vs what telemetry showed on the ground.
- **Patcher and integrity** (07): MQ2, `cheat_manager`, anti-warp, and expansion readiness on record.
- **Methodology** (09): how public/player-visible behavior was measured, how activity-pattern claims were classified, and how confidence levels were assigned.
- **Witness** (10): withheld in public package (redacted placeholder).
- **Hosting cost gap and operator presence** (11, 12): residential game server, cloud website, patch-and-vanish patterns.
- **What you can do** (13): practical next steps.
- **Reporter account and close** (14, 15): timeline, ledger dispute, and short closing.

Supporting data: anonymized Ko-fi events in [data/kofi-events-anonymized.csv](data/kofi-events-anonymized.csv), publish-safe Discord quotes in [data/discord-quotes-public.md](data/discord-quotes-public.md), investigator cost bands in [data/investigator-cost.json](data/investigator-cost.json). Donor re-identification maps do not ship in this package.

## How I read it

Private servers always have drama. This one also routed login through operator infrastructure, ran USD support rails alongside in-game premium currency, and hosts the live world on a residential line while Ko-fi presents **~$411/mo** under **hosting** language against a **~$55/mo** generous infra-only estimate. Total operation including dev time and volunteer labor could fairly reach $411 or more; the page does not split those buckets. I am not claiming wire fraud, tax crimes, or malware without proof. I am saying the gap between marketing and mechanics is wide enough that players deserve a plain-language record of what the artifacts show.

I tried to keep the evidence blocks honest even where I have a grudge. Disagree with my read; check the screenshots, probes, and Discord permalinks.

The original plan was remediation first: present Straps with a fix list in private before any public release. That path went moot after staff harassment, and Straps staying absent from the server and from scrutiny for too long while the residential backend stayed exposed, Ko-fi feed curation continued, and expansion tiers kept shipping on a half-tested stack. Public release is the fallback for players and donors, not a score-settling exercise. [01b-before-going-public](01b-before-going-public.md) walks through that decision in full.

If you only read one chapter after the credential warning, read [02-executive-summary](02-executive-summary.md), then jump to whatever topic you care about.

Previous: [00-credential-warning](00-credential-warning.md) · Next: [01b-before-going-public](01b-before-going-public.md)
