# 07 - Anti-cheat and patcher

## What the patcher ships (without asking)

**Straps, `#server-files`, Feb 3 2026:** Official path is `EQAscendant.zip` patcher or manual `patch.zip`. Includes `dinput8.dll`, spell/db exports from Spire API, and CDN-hosted binaries.

Static analysis of the deployed patcher bundle identified **`dinput8.dll` as MacroQuest / eq-core-dll** (DirectInput proxy with code detours and `WriteProcessMemory`). The same manifest ships a **modified `eqgame.exe`**. Neither is surfaced in the patcher UI or a first-run consent step. Players patch and launch.

CDN **`eqhost.txt`** sets login host to operator infrastructure ([PC-010](operator-share/04-client-patcher-integrity/P1-high/PC-010-eqhost-login-redirect-via-cdn.md)), not the community `.net` path. It is not compiled into the exe; the operator can change routing by updating the CDN manifest.

`autoPatch: true` ([PC-020](operator-share/04-client-patcher-integrity/P2-medium/PC-020-autopatch-reapplies-cdn-files.md)) re-applies CDN files each cycle. Local edits may not persist.

There is no documented first-run consent before writing `eqhost.txt` ([PC-011](operator-share/03-transparency-and-accountability/P1-high/PC-011-no-eqhost-consent-in-patcher.md)).

Detail on binary inventory: investigator `Analysis/04_deployed_binaries_analysis.md` (private workspace; findings summarized here).

## Server list curation

After auth, the patcher shows whatever **`login.eqemulator.dev`** publishes to the EQ client, not the full `eqemulator.net` community roster. A Jun 2026 capture through the Ascendant patcher showed **six servers** total: **Ascendant** and **ProFusion** alone in **Legends**, plus four **Standard** rows (including Clumsy's World and Project Might). The community login list at the same time showed **dozens** of worlds (P99, Quarm, Lazarus, PEQ, and Ascendant listed under Standard with comparable population).

That separation matters for **community fairness**: the ecosystem loginserver many operators built is not what Ascendant players see by default. Detail and screenshot: [03-login-trust-boundary](03-login-trust-boundary.md#curated-server-directory-six-servers-not-the-community-list).

## Client modifications (disclosed in Discord, not in the patcher)

The server requires `dinput8.dll` for AA before 51 and other client behaviors. Straps documented the CDN URL in `#server-files`. That is not the same as telling every player who runs `EQAscendant.zip` that they are installing MacroQuest infrastructure and a custom login route.

Custom DLL plus remote CDN control is normal for emulators. Combined with silent login redirect, it is a **persistent trust install** players should understand before the first patch.

## Anti-cheat posture (server)

Discord rules ban unattended play; MQ permitted. That is policy, not attestation.

**EQEmu core (`cheat_manager`).** The Ascendant fork ships stock EQEmu movement cheat detection: warp/speed math, tunable `[Cheat]` rules in the database (`EnableMQWarpDetector`, etc.), and **`LogCheat` / `player_event_logs` (`POSSIBLE_HACK`)** when trips fire. Default values can catch obvious warp and speed hacks (including the public **1.5×** speed threshold referenced in Discord). If those rules are disabled or loosened in production, the code on GitHub does not matter.

**Straps quest anti-warp (`ascendant_antiwarp.pl`).** Adapted from Profusion. Every second it compares 2D movement against **150 units/second** and **snap-backs** offenders via `MovePCInstance`. The public fork file contains **no logging, no staff alert, no player-facing flag** - only the snap-back. In [`global_player.pl`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/server/quests/global/global_player.pl), **`StartAntiWarp` is commented out**, so the plugin may not even be running unless something else calls it. Layer map: [14d-anti-cheat-layers](14d-anti-cheat-layers.md).

**Independent telemetry.** BotWatch scored AFK patterns on hundreds of characters. The server did not publish those results to players.

## Operator attention (author read)

The pattern that stayed consistent through scrutiny was **Ko-fi feed hygiene** (selective hides, empty feed, paraphrased platform excuses), not transparent patcher disclosure, not clearing the AFK tail, not backend hardening after ISP notice, not wiring enforceable anti-cheat with auditable logs. The visible record shows more public evidence of donation-feed curation than of transparent anti-cheat disclosure or audit-backed enforcement.

That asymmetry fed the decision that a private remediation brief would land on deaf ears while players still lacked an honest record ([01b-before-going-public](01b-before-going-public.md)).

## Expansion item ladder (separate thread)

Release order and gear scaling (LDoN before Luclin, flat PoP band after Luclin's spike) are documented in [data/expansion-item-scaling.md](data/expansion-item-scaling.md). They read as content immaturity, not patcher integrity; kept here only as cross-reference.

Next: [08-referrals-leaderboards](08-referrals-leaderboards.md)
