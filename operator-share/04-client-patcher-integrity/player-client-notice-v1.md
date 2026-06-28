# Player Advisory: What the Ascendant Patcher Installs

**Version:** 1.0  
**Date:** 2026-06-13  
**Companion notices:** Login trust ([01-loginserver-brief](../01-loginserver-brief/player-login-notice-v1.md)) · Hosting ([02-residential-host-brief](../02-residential-host-brief/player-hosting-notice-v1.md))

---

## Summary

**The patcher changes your EverQuest folder - not just “updates.”**

`EQAscendant.exe` downloads files from the operator CDN and writes them into your client directory. Important items include:

| File | What it does |
|------|----------------|
| **`eqhost.txt`** | Tells the game **where to log in** - currently `login.eqemulator.dev:5999`, not the community `login.eqemulator.net` |
| **`dinput8.dll`** | A **MacroQuest-class hook DLL** (game input / automation framework) - not the stock Windows DirectInput library |
| **`eqgame.exe`** | A **modified** game client (different from stock) |

The patcher UI shows **patch progress only** - it does **not** list these files or ask permission before writing them.

**This notice does not claim** the patcher steals passwords. Passwords go to whatever login server is in `eqhost.txt` - see the login advisory.

---

## What happens when you run the patcher

1. Patcher reads `eqemupatcher.yml` (`autoPatch: true` by default).
2. Downloads `filelist.yml` from the operator CDN (**HTTP**, not HTTPS today).
3. Overwrites local files whose checksum differs - including **`eqhost.txt`**.
4. Starts **`eqgame.exe`**, which loads local **`dinput8.dll`** first.

If you manually change `eqhost.txt` back to `login.eqemulator.net`, the **next patch may overwrite it** when the CDN updates.

---

## Opt-out / alternatives (today - manual)

| Option | Effect |
|--------|--------|
| **Accept default** | Keep auto-patch; use `.dev` login path from CDN eqhost |
| **Edit `eqhost.txt`** | Point at `login.eqemulator.net:5999` - Ascendant world may not accept sessions; may be reverted on patch |
| **Set `autoPatch: false`** in `eqemupatcher.yml` | Stops automatic redeploy; you manage files yourself |
| **Review files before play** | Compare manifest hashes on operator website (if published) |

**There is no in-patcher opt-in dialog today** - operators are asked to add one ([PC-011](../03-transparency-and-accountability/P1-high/PC-011-no-eqhost-consent-in-patcher.md)).

---

## Supply-chain notes

- Patch files are delivered over **plain HTTP** - a network attacker in path could theoretically swap files (PC-001).
- The operator can change CDN files **without** shipping a new patcher exe (PC-021).
- Discord may mention `dinput8` or eqhost; **the patcher itself does not** (PC-022).

---

## FAQ

| Question | Answer |
|----------|--------|
| Is the patcher a virus? | **Not claimed.** Static review found no password stealing in the patcher exe. It **does** silently deploy login routing and a hook DLL. |
| Does the patcher read my password? | **No** - credentials go to the login server in `eqhost.txt`. |
| What is `dinput8.dll`? | MacroQuest / eq-core-dll style hook infrastructure - automation/cheat-class capability. |
| Can I use community login? | Edit `eqhost.txt` - may break Ascendant; patcher may revert. |
| Where is login trust explained? | [01 login notice](../01-loginserver-brief/player-login-notice-v1.md). |

---

## Acknowledgement

Operators should embed [player-patcher-acknowledgement-block.md](../03-transparency-and-accountability/player-patcher-acknowledgement-block.md) **before the first `eqhost.txt` write**.

---

*Player notice v1.0 - 2026-06-13. Factual corrections welcome from operator.*
