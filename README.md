# Ascendant EQ - Investigation report

> **Formatted reading (sidebar, search):** [belvue.github.io/ascendant-report](https://belvue.github.io/ascendant-report/)  
> This repo is markdown source; use the link above for a documentation-style view.

This page is the whole story in plain language. Every **material factual claim** below points to a chapter, screenshot, probe log, or Discord permalink. Sentences that read between the lines are labeled as interpretation in the chapter they come from.

This is not legal advice. Staff quotes stay in. Player and donor names stay out.

---

### Before you read anything else

If you play on Ascendant EQ, or any server that patches through `downloads.ascendanteq.com`, your login may not go where you think it goes.

The launcher can point EverQuest at **`login.eqemulator.dev`**. That is operator infrastructure, run by the same person who runs the server. It is not the usual community login at `login.eqemulator.net`. The patcher can rewrite that setting on every update. One successful game login through the `.dev` path can **copy your account record into the operator login database** (same account id, password hash stored there). You do not need a separate signup. Detail: [03-login-trust-boundary](03-login-trust-boundary.md#lspx-first-dev-login-copies-your-net-account).

When you log in, your client sends your username and password to whatever host `eqhost.txt` names. EQEmu loginservers store **password hashes**, not plaintext passwords, in normal operation. Because the operator runs that login stack, they **control the login database**: hashes, backups, account resets, and federation copies to approved peers. That is how operator-controlled loginservers are built. It does **not** mean staff routinely see your password in cleartext. It **does** mean you are trusting their infrastructure with credentials you type into the client. Use a **unique password** you do not reuse on `login.eqemulator.net` or anywhere else.

**[Chapter 00 - full credential warning](00-credential-warning.md)**

---

### Why this document exists

Ascendant EQ is a real private server. Custom patcher, fast progression, Marks of Ascension, Ko-fi on the support page, staff in Discord every day. A lot of people are having fun. Nothing here is trying to talk you out of that.

This exists because the **public story and the record do not match**. Nothing here is written hoping Ascendant fails. The first plan was a private fix list because the server can be good and players are clearly having fun.

Players are told to trust a polished launcher, voluntary donations with no exchange, and Discord rules that ban unattended botting. The record shows patcher login on operator infrastructure, a live world on home internet while Ko-fi cites hundreds of dollars per month in hosting burn, Marks of Ascension with real in-game utility after USD support, a Ko-fi feed that thinned while internal records kept more history, and AFK telemetry that does not line up with enforcement rhetoric.

That gap is not drama for drama's sake. It is the difference between informed consent and marketing.

In May 2026 a player reported a serious plat bug in private and was permabanned within about twenty minutes. The next day staff posted mockery and a disputed ledger. That was the moment to stop taking the posture at face value and start writing things down.

What followed did not require staff access or insider tickets. Public probes, Discord exports, Ko-fi pages, patcher files, and repeated `/who` checks were enough. The first plan was a private fix list for the operator. Public release came when that path failed: post-ban harassment from staff, silence on the post-mortem, and Straps absent from the server and from hard questions for too long. Public release is the fallback so players and donors get the record anyway. Not revenge. Not a demand that you quit if you are having fun.

**[Chapter 01 - scope and motivation](01-why-this-exists.md)** · **[Chapter 01b - why it went public](01b-before-going-public.md)**

---

## The story, start to finish

Read straight through, or jump: [Login](#login) · [Where the world runs](#where-the-world-runs) · [Money](#money) · [Rules versus reality](#rules-versus-reality) · [Patcher and integrity](#patcher-and-integrity) · [How we know](#how-we-know) · [The reporter thread](#the-reporter-thread)

### Login

---

You download the patcher. It works. What most players never see is that `eqhost.txt` can authenticate you against **`login.eqemulator.dev:5999`** instead of the wide community login at `.net`. The server list through that path showed six curated servers, not the full EQEmu directory. Straps did write about the migration in Discord. You will not find it in the patcher UI or on the path a new player walks on day one. Buried disclosure is not the same as telling everyone who logs in.

**[Chapters 03 and 03b - login and lab work](03-login-trust-boundary.md)**

### Where the world runs

---

The website and patch CDN sit on OVH. The live realm at `eqemu.ascendanteq.com` resolves to residential AT&T, while the Ko-fi page presents about **$411 per month** as infrastructure burn. A generous estimate for actual infrastructure alone, OVH web plus home power plus ISP, lands around **$55**. Total operation including labor could fairly reach $411 or more. The page does not split those buckets. Donors are left to picture a datacenter bill.

Probes on the residential IP found SSH, game ports, and an ATLAS admin panel responding without authentication. Re-probes through mid-June did not show visible hardening. The public portal also exposes character search APIs with no login required. Trust-critical facts lived in Discord pins, not on the website.

**[Chapters 04, 04b, 11, and 12 - infrastructure and operator presence](04-infrastructure-security.md)**

### Money

---

The official line is familiar: donations are voluntary gifts, no goods or services in exchange. In-game, Marks of Ascension are not cosmetic. They buy buffs, bags, vendor unlocks, referral prizes, and leaderboard perks. A published staff capture shows Straps confirming he sent MoA tied to support. The same operator ran the same shape on ProFusion years earlier: PayPal Friends and Family, character name in the note, in-game credits on the other side.

BotWatch polled the public Ko-fi feed through mid-June. Rows disappeared selectively. On June 15 the feed went empty. Internal records still held history the public page no longer showed. When asked directly, Straps blamed platform behavior. Ko-fi's own docs describe per-message hide by the creator or a logged-in supporter. Nothing about scheduled pruning.

**[Chapters 05 and 08 - donations and referrals](05-donations-moa-kofi.md)**

### Rules versus reality

---

Discord policy is clear: MQ is allowed, unattended AFK play is not, with a ladder up to ban. BotWatch measured **2,679 signal rows at AFK score 75+** and **151 watched characters** in that band on a Jun 27 snapshot. Ban activity did not track that queue.

In June, Ali posted a Chardok camp screenshot after a player reported concern, in `#general`, **sarcastically** brushed off bot claims ("if the bot is set to sit AFK, it's doing its job"), and said **AFK is not against the rules** and full-bag idle is fine, [**Actor_1 crops and staff post**](09b-botwatch-capabilities.md#chardok-pair-deep-dive-actor-1-actor-2). That public staff line **contradicts Straps' published ladder**. Multiple players describe bringing logs and video and getting **sarcasm and dismissal**, not an investigation queue.

**[Chapters 06 and 09b - enforcement and screenshots](06-enforcement-asymmetry.md)**

### Patcher and integrity

---

The required launcher ships a modified client stack with no first-run warning. Static analysis identified the shipped **`dinput8.dll` as MacroQuest** (`eq-core-dll`), alongside a modified `eqgame.exe`. Login routing is covered above; the patcher does not surface either change in the UI. Players patch and launch.

On the server, EQEmu's stock **`cheat_manager`** can trip warp and speed cheats at default rule values and write **`POSSIBLE_HACK` logs**, but rules can be neutered in the database, and Straps' quest **`ascendant_antiwarp.pl`** only snap-backs with **no audit logging**; in the public fork, **`StartAntiWarp` is commented out**. The visible record shows more public evidence of donation-feed curation than of transparent anti-cheat disclosure or audit-backed enforcement.

**[Chapter 07 - patcher and anti-cheat](07-anti-cheat-and-patcher.md)** · **[14d - cheat layers](14d-anti-cheat-layers.md)**

### How we know

---

Findings use the same sources a patient player could reach: public `/who` and character pages, Ko-fi HTML, Discord exports, patcher manifests, and probe dates in each chapter. Independent tooling (BotWatch) scheduled those checks and added AFK scoring. No zone console. No staff SQL. Chapter 11 cites a separate **~$50/year OVH web-edge baseline** for hosting-cost math only, not as a claim about Ascendant's stack.

**[Chapters 09 and 09b - evidence methodology and public screenshots](09-botwatch-methodology.md)**

### The reporter thread

---

On May 8, 2026, a bridle exploit was discovered and disclosed privately. Accounts were banned in minutes. A full post-mortem went to staff and met silence. Public bug reports the next day were dismissed as AI slop; many were fixed later anyway. On May 9, staff posted a Belvue Cluster accounting embed that labeled income **since May 5** and assigned most plat to Harley gambling at 1,500 platinum per Lucky Coin. The embed appears to attribute **May 8** exploit proceeds to **May 5** token purchases.

The reporter disputes that accounting (AA transmutes, not 1,500pp Harley buys; coin activity predates the bridle window) and has published SQL templates staff could use to verify the ledger.

**[Chapters 14, 14b, and 14c - reporter account and appendices](14-reporter-account.md)**

---

### If you only remember one paragraph

Ascendant can be fun. The gap is accountability. Where login goes. Where the world runs. What donation money implies. Who gets enforced against when someone finally writes things down. You do not need to trust this document's tone. You need to decide whether the artifacts are enough for you.

**[Chapter 02 - executive summary](02-executive-summary.md)**

---

### What you can do

If you still play, use a unique password and read `eqhost.txt` after every patch. Ask staff on the record where login goes and where the world runs.

If you donate, treat the public Ko-fi feed as social proof, not an audit log. Ask for a cost breakdown: cloud versus home versus labor. Decide whether MoA utility matches your ethics regardless of forum language about gifts.

If you share this, link chapters. Do not repost withheld exhibits. Send factual corrections with artifacts.

**[Chapter 13 - full action list](13-what-you-can-do.md)**

---

### Close

This is a warning, not an indictment. Nobody wrote this hoping Ascendant fails. The private fix list came first because players are having fun and the project could still do right by them. EQEmu kept Brad McQuaid's world alive in a way the live game never could. The scene still needs a **portable, supportable login path** the community can trust and reuse. Silently rewriting `eqhost.txt` to a curated operator stack is not how you build that. Players deserve plain answers without becoming the punchline for reporting bugs.

> "Just had to get your fill didn't you?" ;)

**[Chapter 15 - closing statements](15-closing-statements.md)**

---

### Read page by page

Start at **[00 - credential warning](00-credential-warning.md)** and follow the **Next** link at the bottom of each chapter, or jump to whatever topic you care about:

00 credential · 01 why · 01b public release · 02 summary · 03 login · 03b lab · 04 infra · 04b opsec · 05 Ko-fi · 06 enforcement · 07 patcher · 08 referrals · 09 methodology · 09b screenshots · 10 witness (withheld) · 11 hosting cost · 12 operator presence · 13 actions · 14 reporter · 15 close · appendices [14b](14b-preliminary-rca.md) [14c](14c-may-5-ledger-audit.md) [14d](14d-anti-cheat-layers.md)

Supporting data: [discord quotes](data/discord-quotes-public.md) · [Ko-fi events](data/kofi-events-anonymized.csv) · [lucky-coin reference](data/lucky-coin-market-reference.md) · [exhibit index](evidence-index.md)
