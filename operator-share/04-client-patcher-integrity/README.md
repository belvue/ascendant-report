# Client patcher & supply chain - security findings archive

**Product:** Ascendant EQ patcher (`EQAscendant.exe`) and CDN-deployed client files  
**Assessment date:** 2026-06-13  
**Scope:** Static analysis, supply-chain integrity, and technical client modifications

**Disclosure, consent, and UI transparency** (eqhost checkbox, file manifest, change summary) moved to **[03-transparency-and-accountability](../03-transparency-and-accountability/README.md)** (PC-011, PC-012, PC-022, PC-023).

---

## Priority index

### P0 - Immediate (supply chain / undisclosed client changes)

| ID | Title | File |
|----|-------|------|
| PC-001 | Patch CDN serves files over HTTP (no TLS) | [P0-immediate/PC-001-http-patch-cdn.md](P0-immediate/PC-001-http-patch-cdn.md) |
| PC-002 | MacroQuest-class `dinput8.dll` deployed without patcher UI disclosure | [P0-immediate/PC-002-dinput8-mq-hook-deployed-silently.md](P0-immediate/PC-002-dinput8-mq-hook-deployed-silently.md) |

### P1 - High (login routing mechanism)

| ID | Title | File |
|----|-------|------|
| PC-010 | CDN `eqhost.txt` routes login to `login.eqemulator.dev` | [P1-high/PC-010-eqhost-login-redirect-via-cdn.md](P1-high/PC-010-eqhost-login-redirect-via-cdn.md) |

### P2 - Medium (remote control, autoPatch behavior)

| ID | Title | File |
|----|-------|------|
| PC-020 | `autoPatch: true` re-applies CDN files on every patch cycle | [P2-medium/PC-020-autopatch-reapplies-cdn-files.md](P2-medium/PC-020-autopatch-reapplies-cdn-files.md) |
| PC-021 | Operator can change deployed files from CDN without patcher release | [P2-medium/PC-021-remote-cdn-file-swap.md](P2-medium/PC-021-remote-cdn-file-swap.md) |

### P3 - Informational (static analysis limits, positive notes)

| ID | Title | File |
|----|-------|------|
| PC-030 | Patcher binary - no credential-handling code found | [P3-informational/PC-030-patcher-no-credential-code.md](P3-informational/PC-030-patcher-no-credential-code.md) |
| PC-031 | `dinput8` - hook framework; no exfiltration found in static review | [P3-informational/PC-031-dinput8-static-analysis-limits.md](P3-informational/PC-031-dinput8-static-analysis-limits.md) |
| PC-032 | Login redirect is CDN `eqhost.txt`, not hardcoded in patcher exe | [P3-informational/PC-032-login-via-eqhost-not-exe-hardcode.md](P3-informational/PC-032-login-via-eqhost-not-exe-hardcode.md) |

---

## Player-facing materials

| Document | Purpose |
|----------|---------|
| [player-client-notice-v1.md](player-client-notice-v1.md) | What the patcher installs (technical summary) |

Acknowledgement blocks and trust hub: [03-transparency-and-accountability](../03-transparency-and-accountability/README.md).

---

## Response

[operator-response.md](../operator-response.md) (patcher security section) · [private-operator-summary.md](private-operator-summary.md)

---

## Out of scope

- **Login platform internals** - LSPX, federation ([01-loginserver-brief](../01-loginserver-brief/README.md))
- **Live game world hosting** - ATLAS, SSH ([02-residential-host-brief](../02-residential-host-brief/README.md))
- **Transparency rollout** - trust hub, acknowledgements ([03-transparency-and-accountability](../03-transparency-and-accountability/README.md))
- **Malware intent** - static analysis only
- **Ban or exploit disputes**
