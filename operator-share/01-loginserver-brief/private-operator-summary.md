# Private operator summary - eqemulator.dev login platform

**Date:** 2026-06-13  
**Audience:** eqemulator.dev operator (private review)  
**Package:** [01-loginserver-brief/](.) - 21 findings in priority folders  

---

## Purpose

This is a **neutral security and transparency review** of the eqemulator-loginserver stack (web + federation + loginserver API). It is intended to support remediation before any public player advisory. It does **not** allege covert credential theft - **no hidden exfil endpoint was found** in reviewed web source.

---

## Executive summary

| Priority | Count | Action |
|----------|-------|--------|
| **P0 - Immediate** | 4 | Patch/config before next exposure |
| **P1 - High** | 6 | Trust, credentials, disclosure |
| **P2 - Medium** | 7 | Defense in depth |
| **P3 - Informational** | 5 | Document or accept |

**Top actions this week:**

1. **LS-003 / LS-004** - Upgrade Next.js (≥15.5.19) and drizzle-orm (≥0.45.2).
2. **LS-001** - Enable Turnstile in production; fail closed if unset.
3. **LS-002** - Fix federation-play internal guard (remove empty-IP trust).
4. **LS-015** - Publish login-path + LSPX disclosure; deploy [player-login-notice-v1.md](player-login-notice-v1.md) and [player-acknowledgement-block.md](player-acknowledgement-block.md).

---

## What we verified (positive)

- Admin APIs require platform admin session.
- Loginserver `/v1/*` requires bearer token except healthcheck.
- Federation `sync_data` requires Ed25519-signed approved peer - anonymous GET returns 401.
- Federation heartbeat minimal response - no public key leak.
- Covert third-party credential exfil in web TypeScript - **not found**.

---

## What we proved (high trust impact)

| Finding | One line |
|---------|----------|
| **LS-010** | Full EQ client login via LSPX **inserts local hash row** (`source_loginserver=eqemu`). |
| **LS-012** | Approved federation peer receives **all account password hashes** via `sync_data`. |
| **LS-011** | Leaked API token → create accounts / overwrite password hashes. |
| **LS-001** | Registration works with empty captcha when Turnstile unset. |
| **LS-002** | Internal federation-play accepts requests with empty client IP. |

---

## Suggested response

1. Review findings by ID in [README.md](README.md).
2. Complete [operator-response.md](../operator-response.md) with status per finding ID.
3. Reply with factual corrections or dispute specific IDs (evidence-based).
4. If you publish your own player notice, we can adjust public advisory timing accordingly.

---

## Out of scope

See [README.md](README.md).

---

## Contact

_(Operator fills channel in response template.)_
