# Login server - security findings archive

**Product:** eqemulator.dev / eqemulator-loginserver  
**Assessment date:** 2026-06-13  
**Scope:** Web application, federation APIs, C++ loginserver HTTP/game ports, dependency lockfile  

---

## Priority index

### P0 - Immediate (patch or config before next public exposure)

| ID | Title | File |
|----|-------|------|
| LS-001 | Turnstile registration bypass when captcha unset | [P0-immediate/LS-001-turnstile-registration-bypass.md](/operator-share/01-loginserver-brief/P0-immediate/LS-001-turnstile-registration-bypass.md) |
| LS-002 | Internal federation-play route accepts empty client IP | [P0-immediate/LS-002-federation-play-internal-bypass.md](/operator-share/01-loginserver-brief/P0-immediate/LS-002-federation-play-internal-bypass.md) |
| LS-003 | Next.js 15.5.14 - multiple high-severity advisories | [P0-immediate/LS-003-nextjs-vulnerable-dependencies.md](/operator-share/01-loginserver-brief/P0-immediate/LS-003-nextjs-vulnerable-dependencies.md) |
| LS-004 | drizzle-orm SQL identifier injection (CVE) | [P0-immediate/LS-004-drizzle-orm-sql-injection-cve.md](/operator-share/01-loginserver-brief/P0-immediate/LS-004-drizzle-orm-sql-injection-cve.md) |

### P1 - High (trust, credentials, or material exposure)

| ID | Title | File |
|----|-------|------|
| LS-010 | LSPX caches legacy account hashes on game login | [P1-high/LS-010-lspx-credential-cache-game-login.md](/operator-share/01-loginserver-brief/P1-high/LS-010-lspx-credential-cache-game-login.md) |
| LS-011 | Loginserver API bearer token enables account takeover | [P1-high/LS-011-loginserver-api-token-privilege.md](/operator-share/01-loginserver-brief/P1-high/LS-011-loginserver-api-token-privilege.md) |
| LS-012 | Federation sync_data exports all password hashes to approved peers | [P1-high/LS-012-federation-sync-data-hash-export.md](/operator-share/01-loginserver-brief/P1-high/LS-012-federation-sync-data-hash-export.md) |
| LS-013 | Public /api/status and /api/servers without authentication | [P1-high/LS-013-public-status-server-enumeration.md](/operator-share/01-loginserver-brief/P1-high/LS-013-public-status-server-enumeration.md) |
| LS-014 | Web link-loginserver accepts plaintext legacy passwords | [P1-high/LS-014-link-loginserver-plaintext-flow.md](/operator-share/01-loginserver-brief/P1-high/LS-014-link-loginserver-plaintext-flow.md) |
| LS-015 | Players not informed of third-party login path (disclosure gap) | [P1-high/LS-015-login-path-disclosure-gap.md](/operator-share/01-loginserver-brief/P1-high/LS-015-login-path-disclosure-gap.md) |

### P2 - Medium (defense in depth, role model, legacy crypto)

| ID | Title | File |
|----|-------|------|
| LS-020 | Legacy eqcrypt wire encryption on game login port | [P2-medium/LS-020-eqcrypt-legacy-wire-protocol.md](/operator-share/01-loginserver-brief/P2-medium/LS-020-eqcrypt-legacy-wire-protocol.md) |
| LS-021 | Moderator role has admin privileges on many routes | [P2-medium/LS-021-moderator-admin-equivalence.md](/operator-share/01-loginserver-brief/P2-medium/LS-021-moderator-admin-equivalence.md) |
| LS-022 | Admin import-accounts creates placeholder password rows | [P2-medium/LS-022-import-accounts-empty-password.md](/operator-share/01-loginserver-brief/P2-medium/LS-022-import-accounts-empty-password.md) |
| LS-023 | Shared Resend API key documented for @eqemulator.dev mail | [P2-medium/LS-023-shared-resend-api-key.md](/operator-share/01-loginserver-brief/P2-medium/LS-023-shared-resend-api-key.md) |
| LS-024 | Transitive npm advisories (dompurify, lodash, postcss, uuid) | [P2-medium/LS-024-transitive-npm-advisories.md](/operator-share/01-loginserver-brief/P2-medium/LS-024-transitive-npm-advisories.md) |
| LS-025 | Curated server directory and trust flags | [P2-medium/LS-025-directory-curation-trust-labels.md](/operator-share/01-loginserver-brief/P2-medium/LS-025-directory-curation-trust-labels.md) |
| LS-035 | No end-user access or activity history in account settings | [P2-medium/LS-035-no-user-access-history-dashboard.md](/operator-share/01-loginserver-brief/P2-medium/LS-035-no-user-access-history-dashboard.md) |

### P3 - Informational (document, monitor, or accept risk)

| ID | Title | File |
|----|-------|------|
| LS-030 | Unauthenticated loginserver healthcheck endpoint | [P3-informational/LS-030-loginserver-healthcheck-unauthenticated.md](/operator-share/01-loginserver-brief/P3-informational/LS-030-loginserver-healthcheck-unauthenticated.md) |
| LS-031 | Platform users lack MFA (admins/server owners have MFA) | [P3-informational/LS-031-platform-users-no-mfa.md](/operator-share/01-loginserver-brief/P3-informational/LS-031-platform-users-no-mfa.md) |
| LS-032 | Master-initiated remote upgrade/restart of federation peers | [P3-informational/LS-032-remote-upgrade-supply-chain.md](/operator-share/01-loginserver-brief/P3-informational/LS-032-remote-upgrade-supply-chain.md) |
| LS-033 | Operator can read all credential hashes via database access | [P3-informational/LS-033-operator-database-hash-access.md](/operator-share/01-loginserver-brief/P3-informational/LS-033-operator-database-hash-access.md) |
| LS-034 | C++ loginserver binary not fully auditable from published source | [P3-informational/LS-034-cpp-binary-review-gap.md](/operator-share/01-loginserver-brief/P3-informational/LS-034-cpp-binary-review-gap.md) |

---

## Player-facing materials

| Document | Purpose |
|----------|---------|
| [player-login-notice-v1.md](player-login-notice-v1.md) | Plain-language summary for players |
| [player-acknowledgement-block.md](player-acknowledgement-block.md) | Checkbox consent text for patcher, web link, or forum |

---

## Controls verified

These behaved as expected - listed for completeness, not as findings:

- Admin API routes return 401/403 without platform admin session
- Loginserver HTTP API (`/v1/*`) returns 401 without bearer token (except healthcheck)
- Anonymous `GET /api/federation/sync_data` returns 401 (signed peer required)
- Federation heartbeat returns minimal JSON only (no node key leak)
- `/api/metrics` returns 403 for unauthenticated external requests

---

## Response

[operator-response.md](../operator-response.md) (login section) · [private-operator-summary.md](private-operator-summary.md)

---

## Out of scope

This archive covers **authentication and login platform** (`login.eqemulator.dev` / eqemulator-loginserver). It does **not** assess:

- **Live game world** after login - zone traffic, world server, character data on `eqemu.ascendanteq.com` ([02-residential-host-brief](../02-residential-host-brief/README.md))
- **ATLAS / SSH on the game host** - co-located admin exposure on the residential IP (same cross-link)
- **OVH website and patch CDN** - `ascendanteq.com` / downloads stack (hosting split context only, where noted in LS-015)
- **Full C++ loginserver binary audit** - image vs published source gap only (LS-034)
- **Patcher and client supply chain** - `EQAscendant.exe`, `dinput8.dll`, CDN eqhost deploy ([04-client-patcher-integrity](../04-client-patcher-integrity/README.md); LS-015 covers disclosure gap only)
- **Player transparency rollout** - trust hub, acknowledgements, publication ([03-transparency-and-accountability](../03-transparency-and-accountability/README.md))
- **Active exploitation** - no unauthorized writes, credential dumping, or production middleware bypass PoCs
- **Governance disputes** - ban narrative, exploit accusations, unproven fraud or covert theft (architectural hash access is documented; malice is not claimed)
