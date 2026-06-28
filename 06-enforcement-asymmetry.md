# 06 - Enforcement asymmetry

## Published rules

**Straps, `#rules-and-guidelines`:**

- MQ allowed; **unattended / AFK play prohibited** with ladder: warning → level 1 reset → AA wipe → ban.
- MQ2 fine; unattended not (later clarification message).

**Ali, `#general`, Jun 2 2026:** Mark reimbursement requires Straps log review: "31 Marks? Nope, you've gotta go to Straps on that one."

## Measured behavior (BotWatch, Jun 27 2026)

| Metric | Value |
|--------|------:|
| Active roster | 8,116 |
| Watch targets | 259 |
| Signal rows AFK score ≥ 75 | 2,679 |
| Watched chars max score ≥ 75 | 151 |
| Open investigations | 219 |

High AFK scores persisted on watched characters while ban activity in May removed a **small linked account cluster** (~8) instead of that queue. UI reconstructions (mostly masked): [09b-botwatch-capabilities](09b-botwatch-capabilities.md).

Policy text says unattended play is the serious sin. Telemetry shows a long tail of high-AFK targets still on roster. Ban activity and that queue did not move together.

## Chardok camp case (Jun 2026)

**Actor_1** and **Actor_2** (same watched guild cluster) both show multi-day **chardok** hard-camp telemetry in BotWatch. I do not know whether they shared a DZ instance. On a **one-day window lined up side by side** (Jun 8 - 9 crops in [09b](09b-botwatch-capabilities.md#chardok-pair-deep-dive-actor-1-actor-2)), both zone lanes show the same hop: **chardok → guildlobby → chardok** at the same timestamps, with **Actor_2** taking a vendor dump on the lobby hop and **Actor_1** doing the same trip without loot movement. That is plausibly **one operator** running two boxes on the same script schedule, not two strangers who happen to camp alike. **Note:** those crops include an **undocumented BotWatch sampling gap** (~06/09 01:00 - 11:00 UTC); see the disclosure in [09b](09b-botwatch-capabilities.md#chardok-pair-deep-dive-actor-1-actor-2) before reading empty lane time as offline time.

| Target | BotWatch read |
|--------|----------------|
| **Actor_1** | Bags fill, **loot flatlines**, character **stays in chardok for days** - script looks like it stopped picking up loot, box never logged off |
| **Actor_2** | Same zone habit; **loot flat for long stretches while AA bars stay steady** - kills continue, inventory does not (full bags, no bag-full stop); **synced lobby hops** with Actor_1 on the Jun 8 - 9 window |

That pattern is what players report as obvious unattended camp. BotWatch had it on watch targets while enforcement attention went elsewhere. These camps stayed on roster.

On **2026-06-05** **Ali/Kalioptra** posted **Actor_1** in `#general` with an in-game screenshot (nameplate redacted in public crop). His reply was **sarcastic**, not a good-faith bot review: mock "botting really hard," then *if the program is set to sit AFK for hours it's doing its job*, then **"Unfortunately being afk isn't actually against the rules"** and **"I'm pretty okay with any of y'all just sitting afk in your zone, full bags or not."** A follow-up message said the character was **"in fact not botting."** That public staff line **contradicts** Straps' published AFK ladder in `#rules-and-guidelines`. Full context: [12-operator-presence](12-operator-presence.md#afk-dismissal-in-public-discord). BotWatch crops: [09b - Chardok pair deep dive](09b-botwatch-capabilities.md#chardok-pair-deep-dive-actor-1-actor-2).

## Player reports to staff

Multiple players (including this report's author) describe bringing **documented** rule violations to staff: timestamps, screenshots, video, camp timelines built from public telemetry. The responses captured in Discord and forum threads often skew **sarcastic or demeaning** rather than "show me the logs and we will investigate."

That is not a substitute for a full staff ticket audit. It is consistent with why private reports dried up while public complaints grew. When staff meet evidence with sarcasm instead of log review, players stop reporting in good faith.

## Inner circle, donors, and selective read (author interpretation)

BotWatch telemetry pairs a **long high-AFK tail** on roster with **selective ban activity**, not the AFK queue. Ko-fi and MoA tie **USD and character names** to operator-visible support ([05-donations-moa-kofi](05-donations-moa-kofi.md)).

**Author read (speculation, not proven):** repeat donors and Discord-regular insiders get **political amnesty** for conduct that would trigger the published AFK ladder for a non-donor. Patrons who only play, without supporting or inner-circle access, read as **low importance** when they ask for enforcement. Staff tone toward the general player base often reads as **disdain** (sarcasm, public reframing, "go to ProFusion" deflection) rather than stewardship.

**What would falsify this read:** published enforcement stats, blind review of donor vs non-donor tickets, and consistent AFK action against the long high-AFK tail visible from public roster telemetry (same class of evidence [09b-botwatch-capabilities](09b-botwatch-capabilities.md) reconstructs without staff access).

## Operator quiet after scrutiny

Multiple independent signals (Discord activity drop, Ko-fi feed curation, operator-presence timeline in [12-operator-presence](12-operator-presence.md)) align with **Straps going quieter** when donation and login questions heated up in mid-June 2026. In-game witness material that named other players is **withheld** ([10-witness-summary](10-witness-summary.md)).

## Related quotes

[data/discord-quotes-public.md](data/discord-quotes-public.md) tags `06-enforcement-asymmetry`.

Next: [07-anti-cheat-and-patcher](07-anti-cheat-and-patcher.md)
