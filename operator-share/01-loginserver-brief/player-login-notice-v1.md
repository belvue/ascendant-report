# Player Advisory: EQ Login Routing & Credential Trust (eqemulator.dev)

**Version:** 1.0  
**Date:** 2026-06-13  
**Scope:** `login.eqemulator.dev` / eqemulator-loginserver stack  
**Companion notice:** Live game world hosting (`eqemu.ascendanteq.com`) is covered in a **separate advisory** (02-residential-host-brief).

---

## Summary (read this first)

**Your login path may not be what the patcher implies.**

Ascendant’s patcher deploys `eqhost.txt` pointing to **`login.eqemulator.dev:5999`**, while commenting out **`login.eqemulator.net:5999`**. The patcher UI does **not** disclose this change. **`eqemulator.dev` is operator-owned infrastructure** - not the EQEmu Foundation official login.

When you log in through this path, passwords are validated on operator-controlled infrastructure. Password **hashes** may be stored locally. Legacy accounts may be checked against `.net` via **LSPX** and then **cached** on the operator database.

**One-sentence takeaway:** You may be trusting a third-party login stack that the patcher never clearly names.

---

## Who operates what (login map)

| Layer | Hostname | Operator relevance |
|-------|----------|-------------------|
| **Community login (official path)** | `login.eqemulator.net:5999` | EQEmu community login - commented out in Ascendant patcher |
| **Federation login (patcher default)** | `login.eqemulator.dev:5999` | Straps-operated eqemulator.dev stack |
| **Live game world** | `eqemu.ascendanteq.com` | Separate network - see hosting advisory |

```
EQ Client (Ascendant patcher)
  └─ login.eqemulator.dev:5999     ← credential validation (this notice)
       ├─ LSPX → may validate against .net on first login
       ├─ Hash may be stored in operator DB
       └─ Federation peers may receive hash copies (sync_data)
  └─ eqemu.ascendanteq.com:9000    ← world traffic [separate notice]
```

---

## What happens when you log in

1. Patcher sets `eqhost.txt` to `login.eqemulator.dev:5999`.
2. Client sends credentials to the `.dev` loginserver.
3. For legacy accounts, **LSPX** may validate against `login.eqemulator.net`, then **insert a local hash row** on success.
4. Federation may **replicate password hashes** to approved peer nodes.
5. After login, world connection may route to a **different host** than the login server.

**What this notice documents:** A **disclosure gap** - players are not clearly told where passwords are validated or stored.

**What this notice does not claim:** That passwords were stolen, that the operator acted in bad faith, or that a break-in has already happened. This package documents **where login runs and what the platform is built to access** - not proof of hidden theft or misuse.

---

## Security and trust topics (June 2026)

| Topic | Summary |
|-------|---------|
| **LSPX hash cache** | First game login can store a scrypt hash locally after upstream `.net` validation |
| **Federation sync** | Approved peers can pull full hash exports - by documented design |
| **API token** | Leaked loginserver API token enables account create/hash overwrite - insider/token leak risk |
| **Registration** | Captcha may be skipped when Turnstile is unset - bot registration risk on misconfigured nodes |
| **Public status APIs** | Some read-only routes expose server roster and health metrics without auth |

---

## Disclosure gap (why this notice exists)

| Location | Third-party login disclosed? | LSPX / hash storage disclosed? |
|----------|------------------------------|--------------------------------|
| Patcher UI | **No** | **No** |
| Ascendant website | **No** clear trust page | **No** |
| eqemulator.dev (player-facing) | Partial (technical README) | Not at login time |
| Account settings (`/account`) | **No** trust or activity history for end users | **No** sessions, login history, or LSPX summary (LS-035) |

This notice fills the gap until operators publish trust documentation and in-product acknowledgement.

---

## What you can do

| Option | Notes |
|--------|-------|
| **Continue with patcher default** | Works out of box - third-party login trust |
| **Manual eqhost → `.net`** | Community login path; Ascendant may break |
| **Use a unique password** | Limits cross-service reuse |
| **Don’t link legacy account on `.dev` web** | Avoids plaintext web submit; does not stop LSPX on game login |
| **Ask for a written data policy** | Reasonable transparency request |
| **Play a server using `.net` login only** | Clearer community trust model |

---

## What we’re asking operators to publish

1. **Login path diagram** on eqemulator.dev `/about/trust` or patch notes.
2. **Data retention** - how long `login_accounts`, logs, and backups are kept.
3. **Federation peer list** - who receives hash replication today.
4. **LSPX disclosure** - “first login may validate against `.net` and cache hash locally.”
5. **Curated directory policy** - how servers get `trusted` / `claimed` flags.
6. **Security contact** - email or Discord role for login-trust questions.

A player should answer “who touched my password?” without reading GitHub.

---

## FAQ

| Question | Answer |
|----------|--------|
| Is this a hack / breach? | **No active breach claimed.** Architectural transparency about login routing. |
| Did the operator steal passwords? | **Not claimed.** Passwords are validated on operator infrastructure; hashes may be stored and synced as documented. This notice is about **transparency**, not proof of theft or misuse. |
| Is eqemulator.dev official EQEmu? | **No.** Community project is `.org`; `.dev` is Straps’ federated platform. |
| Can I switch back to `.net`? | Edit `eqhost.txt`; Ascendant may require `.dev` for world auth - test at your risk. |
| What about the game server host? | **Separate issue** - see companion hosting advisory. |

---

## Acknowledgement

Operators may embed [player-acknowledgement-block.md](player-acknowledgement-block.md) in patcher, web link flow, or server rules.

---

*Player notice v1.0 - 2026-06-13. Factual corrections welcome from operator before or after publication.*
