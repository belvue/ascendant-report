# 03b - Loginserver lab methodology

This chapter documents **how we tested** the `login.eqemulator.dev` stack. Chapter [03-login-trust-boundary](03-login-trust-boundary.md) states **what it means for players**. This one is for anyone who wants reproducibility, limits, and evidence IDs.

**Period:** 2026-06-13 (primary lab week), with static code review through Jun 2026.  
**Tester:** Report author (in-game handle **[redacted]** on legacy `login.eqemulator.net`).  
**Host:** Windows workstation; Docker Desktop.  
**Full artifact tree (internal):** `eqemulator_intel/lab/loginserver/` (compose, scripts, JSON/MD reports). Not required to read the public report.

---

## Why we built a lab

Ascendant's patcher routes credentials to operator-controlled login infrastructure. We needed to separate:

1. **Product behavior** (LSPX proxy, federation hash export) from  
2. **Covert malice** (hidden exfil endpoints, mystery backdoors).

A local Docker copy of the published loginserver stack let us run the same code paths **without** attacking production accounts or posting credentials to production web forms.

---

## Lab environment

### Stack

Minimal compose mirroring Straps' published images plus a **locally built** Next.js web app (lab-only cookie/MFA relaxations).

| Service | Host port | Role |
|---------|-----------|------|
| Web UI + `/api/*` | `13000` | Platform accounts, admin, federation HTTP |
| Loginserver HTTP API | `16000` | `/v1/account/...` (C++ layer) |
| Game login (LSPX) | `5999`, `5998`, `15900` | EQ client wire protocol |
| MariaDB | `13306` | `login_accounts`, federation tables |

**Images:** Pre-built `ghcr.io/straps-eq/*` for loginserver and MariaDB. Web built from reviewed source under `eqemulator_intel/evidence/eqemulator-loginserver/web`.

**Bootstrap:** `eqemulator_intel/lab/loginserver/bootstrap_lab.ps1` generates secrets (`.env`), initializes schema, starts compose. Optional `-ResetDb` for clean runs.

**Lab admin:** `create_lab_admin.ps1` seeds a platform admin for `/admin` tests (`labadmin`; password is lab-only, never committed).

### Production target (read-only)

| Host | Use in tests |
|------|----------------|
| `https://eqemulator.dev` | GET probes only (`/api/servers`, `/api/status`, federation heartbeat) |
| `login.eqemulator.net:5999` | **Upstream LSPX validate** when lab loginserver proxies credentials (real TCP from lab container) |
| `login.eqemulator.dev:5999` | TCP reachability noted; game login tests used **local** `:5999` |

We did **not** POST player passwords to production **web** routes. LSPX tests intentionally contact **real** `login.eqemulator.net` from inside the lab loginserver process (documented upstream behavior).

---

## Rules of engagement

| Allowed | Not done |
|---------|----------|
| Own legacy account (`belvue`) with env-var credentials | Passwords in report JSON or git |
| Read-only GETs against production public APIs | Mass credential stuffing |
| Local DB queries after own login | Writing to production admin APIs without authorization |
| Admin actions **inside lab only** | Claiming "hack" where behavior is documented product design |
| Static review of web + `LOGINSERVER.md` | Full C++ binary reverse engineering (gap noted below) |

Credentials passed via **`EQ_LOGIN_USER` / `EQ_LOGIN_PASS` environment variables** only (`pen_test_loginserver.py`).

---

## Test phases (what we ran)

### Phase 1: Bootstrap and pen test script

**Script:** `pen_test_loginserver.py --local --production`

**Local (`--local`):**

- Register/login on lab web (Turnstile off when unset → bot registration possible, SEC-01).
- `POST /v1/account/credentials/validate/local` → expected fail before row exists.
- **`POST /v1/account/credentials/validate/external`** with belvue → **200**, `account_id: **630868**` (matches upstream `.net` id).
- Confirms lab loginserver **proxied** password check to `login.eqemulator.net` without inserting a row yet.

**Production (`--production`):**

- GET `/api/servers`, `/api/status`, `/api/metrics`, federation heartbeat.
- No credential POSTs to production web.

**Report:** `lab/loginserver/reports/pen_test_20260613_*.json`, `Analysis/11_lab_pen_test_results.md`.

### Phase 2: Full EQ client login (game path)

**Setup:** `eqhost.lab.txt` → `127.0.0.1:5999`; RoF2 client; legacy username/password.

**Steps:**

1. Log in through **game port** (not HTTP validate-only).
2. Query lab MariaDB: `SELECT id, account_name, source_loginserver FROM login_accounts WHERE account_name='[redacted]'`.

**Result:**

| Check | Outcome |
|-------|---------|
| Client auth | Success |
| Row inserted | **Yes** |
| `id` | **630868** (same as upstream `.net`) |
| `source_loginserver` | **`eqemu`** (LSPX cache, not `local`) |
| `account_password` | scrypt hash (`$7$C6...` prefix) |
| Enter world | Failed (no world process in minimal lab; expected) |

**Impact:** Proves **first game login** through operator login path **copies** legacy `.net` accounts into operator MariaDB. Validate-only API is **not** enough to insert.

**Report:** `lab/loginserver/reports/lab_client_login_belvue_20260613.md` (E042, RF-029, SEC-06).

### Phase 3: Complete lab suite (federation + admin)

**Script:** `run_complete_lab_tests.ps1` → `lab_complete_tests.py` + `attack_vector_scan.py`

**Federation (E041, [LS-012](operator-share/01-loginserver-brief/P1-high/LS-012-federation-sync-data-hash-export.md)):**

1. Initialize federation master (`lab-master`).
2. Admin `add_peer` → bootstrap token → mock peer registers Ed25519 key.
3. Signed `GET /api/federation/sync_data` as approved peer → **200**, accounts array includes **password hash fields** (scrypt prefixes; sample usernames truncated in report).
4. Anonymous `sync_data` → **401**.

**Admin (lab session):**

- List loginserver accounts, reset password on test row, import-accounts TSV (empty-password rows, [LS-022](operator-share/01-loginserver-brief/P2-medium/LS-022-import-accounts-empty-password.md)).
- Container logs → **502** (no upgrade-agent in lab; expected).

**Report:** `lab/loginserver/reports/lab_complete_20260613_171214.md` (**13/13 PASS**).

### Phase 4: Attack vector matrix

**Script:** `attack_vector_scan.py` (39 vectors: anonymous web, admin, federation, loginserver API token, DB insider, wire).

**High-severity lab confirmations (selected):**

| ID | Attacker model | Result |
|----|----------------|--------|
| AV-LS-006 | Bearer `LOGINSERVER_API_TOKEN` | Create arbitrary local account (**200**) |
| AV-LS-007 | Same token | `POST /v1/account/credentials/update/external` overwrites hash (**200**) |
| AV-OPS-001 | MariaDB access | Read all `login_accounts.account_password` prefixes |
| AV-WIRE-002 | LSPX operator path | Validate/external + full client login (Phase 1 - 2) |
| AV-FED-003 (sync_data) | Approved Ed25519 peer | Hash export (Phase 3) |

**Anonymous blocked:** Admin routes, unauthenticated `sync_data`, loginserver API without token.

**Report:** `lab/loginserver/reports/attack_vectors_20260613_171217.md`.

### Phase 5: Static code review (ongoing)

Reviewed Next.js federation routes, `link-loginserver` (plaintext password → scrypt insert + LSPX fallback), `password_sync`, `sync_data`, and operator `LOGINSERVER.md`.

**Gap:** C++ loginserver binary not fully reversed ([LS-034](operator-share/01-loginserver-brief/P3-informational/LS-034-cpp-binary-review-gap.md)). Wire logging of plaintext during LSPX not verified at binary level.

**Reports:** `Analysis/10_loginserver_security_audit.md`, `Analysis/13_loginserver_cve_attack_vectors.md`.

---

## Findings map (lab → report chapters)

| Lab evidence | Finding | Public chapter |
|--------------|---------|----------------|
| E042 client login + row 630868 | LSPX inserts `.net` account on first game login | [03](03-login-trust-boundary.md) LSPX section |
| E040 validate/external | Upstream password check via operator stack | [03](03-login-trust-boundary.md), [00](00-credential-warning.md) |
| E041 sync_data | Approved peer exports all password hashes | [03](03-login-trust-boundary.md) federation section, [04b](04b-operational-security.md) |
| AV-LS-007 | API token can overwrite `login_accounts` hash | [03](03-login-trust-boundary.md) two-password section |
| AV-ACC-003 blocked | Link-loginserver requires session | [LS-014](operator-share/01-loginserver-brief/P1-high/LS-014-link-loginserver-plaintext-flow.md) (web plaintext flow; separate from game path) |
| Production GET `/api/servers` | Curated roster public | [03](03-login-trust-boundary.md) six-server exhibit |
| SEC-01 Turnstile off | Registration open when unset | [04b](04b-operational-security.md) |

---

## What we did not prove

- That Straps or any peer **used** hash export or `password_sync` against a specific player.
- That changing a password on `.dev` **writes through** to `login.eqemulator.net` (staff quotes and code review say **local** store; `.net` → local re-sync on client login is documented on operator FAQ).
- Full eqcrypt wire security (AV-WIRE-001 not fuzzed).
- Production federation **peer list** (approved nodes not published by operator).

---

## Reproduce (investigator checkout)

From repo root, internal tree:

```powershell
cd eqemulator_intel\lab\loginserver
.\bootstrap_lab.ps1

$env:EQ_LOGIN_USER = "your_legacy_username"
$env:EQ_LOGIN_PASS = "your_password_here"
python pen_test_loginserver.py --local --production

# Full suite (after bootstrap):
.\run_complete_lab_tests.ps1

# Optional: EQ client + eqhost.lab.txt → 127.0.0.1:5999, then query MariaDB on port 13306
```

Never commit `.env`, pen-test JSON with credentials, or bootstrap tokens from federation init.

---

## Relation to BotWatch

BotWatch (chapter [09-botwatch-methodology](09-botwatch-methodology.md)) monitors **Ascendant game telemetry and Ko-fi**. The loginserver lab is a **separate** effort focused on **credential trust boundaries**. Methodology detail: [03b-loginserver-lab-methodology](03b-loginserver-lab-methodology.md). Overlap: both run on investigator infrastructure; login lab does not use BotWatch DB.

---

## Evidence IDs (internal index)

| ID | Description |
|----|-------------|
| E040 | LSPX validate/external pen test (belvue → 630868) |
| E041 | Federation `sync_data` hash export in lab |
| E042 | Full client login → `login_accounts` INSERT |
| RF-029 | Credential trust boundary (rollup) |
| SEC-06 | LSPX architecture / cache-on-login |

Next: [04-infrastructure-security](04-infrastructure-security.md)
