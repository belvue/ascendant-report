# Operator response

**From:**  
**Date:**  
**Re:** Disclosure package review (01 login · 02 hosting · 03 transparency · 04 patcher · 05 funding)

---

## Acknowledgement

- [ ] Received materials dated ______
- [ ] Factual corrections (by finding ID if applicable):

---

## Remediation - login server (01)

| Item | Status | Notes (LS-xxx) |
|------|--------|----------------|
| Turnstile required in production; registration fails closed if unset | | LS-001 |
| `/api/internal/federation-play` rejects empty client IP | | LS-002 |
| Next.js upgraded (≥15.5.19); npm high advisories cleared | | LS-003 |
| drizzle-orm upgraded (≥0.45.2) | | LS-004 |
| Login path + LSPX disclosed to players | | LS-010, LS-015 |
| `LOGINSERVER_API_TOKEN` secured; loginserver HTTP API not public | | LS-011 |
| Federation peer list published; sync_data access reviewed | | LS-012 |
| `/api/status` and `/api/servers` access policy documented or restricted | | LS-013 |
| Account-link page disclosure + acknowledgement before submit | | LS-014, LS-015 |
| Account settings show access / login history for end users | | LS-035 |
| Player notice / acknowledgement published or embedded | | player-login-notice-v1.md |

---

## Remediation - residential game host (02)

| Item | Status | Notes (RH-xxx) |
|------|--------|----------------|
| ATLAS `/admin/*` requires authentication | | RH-001 |
| `/api/admin/settings` requires authentication | | RH-002 |
| SSH :22 restricted (VPN, allowlist, or key-only) | | RH-003 |
| Hosting topology published to players | | RH-011 |
| Player hosting notice / acknowledgement published or embedded | | player-hosting-notice-v1.md |

---

## Remediation - transparency & accountability (03)

| Item | Status | Notes |
|------|--------|-------|
| Cost categories published (OVH vs residential vs dev/tooling) | | DT-001 |
| Server Runway tied to documented burn buckets | | DT-010 |
| Ko-fi / channel receipt summary public (30-day / 12-month + last updated) | | DT-011 |
| Ko-fi activity feed stated as non-ledger; Jun 15 empty feed explained | | KF-001 |
| Same-name MOA inventory anomaly reconciled; MOA grant policy published | | KF-002 |
| Jun 12 - 14 selective feed removals explained if intentional | | KF-003 |
| Runway shows formula, reserve, monthlyRate, updatedAt; fallback labeled | | DT-022 |
| Character-name field policy published or field removed | | DT-020 |
| EQEmu Server Operator Terms status documented or page adjusted | | DT-021 |
| `/about/trust` links all player notices | | TR-001 |
| Acknowledgements at account link, eqhost write, Ko-fi redirect | | TR-010, PC-011 |
| Trust facts on web - not Discord-only | | TR-011 |
| Trust changelog for eqhost / CDN / client file changes | | TR-020 |
| Player notices live at published URLs | | TR-021 |
| Patcher file manifest + post-patch change summary | | PC-012, PC-023 |
| `dinput8` purpose disclosed in patcher UI (pairs with 04 PC-002) | | PC-022 |
| Donation / patcher acknowledgement blocks embedded | | player-donation-notice-v1.md, player-patcher-acknowledgement-block.md |

---

## Remediation - project funding & compensation (05)

| Item | Status | Notes (PF-xxx) |
|------|--------|----------------|
| Funding categories published (cloud / residential / dev / reserves) | | PF-001 |
| Monthly burn distinguishes vendor bills vs labor | | PF-002 |
| Reserve target, rationale, and formula public | | PF-003 |
| Capital hardware vs monthly opex policy published | | PF-004 |
| Staff / steward / dev compensation policy published | | PF-005 |
| Funding model - priorities and surplus rules defined | | PF-006 |
| Optional category disclosure template adopted | | PF-007 |

---

## Remediation - client patcher security (04)

| Item | Status | Notes (PC-xxx) |
|------|--------|----------------|
| Patch CDN served over HTTPS | | PC-001 |
| `dinput8.dll` supply chain documented; see 03 for UI disclosure | | PC-002 |
| Login path via CDN eqhost documented (pairs with LS-015, 03 PC-011) | | PC-010 |
| autoPatch overwrite policy documented or gated | | PC-020 |
| CDN file change policy / signing | | PC-021 |
| Player client install notice published | | player-client-notice-v1.md |

---

## Publication

- [ ] We will publish our own player notice by ______
- [ ] We dispute the following (specific, evidence-based):

---

## Contact

**Channel:**
