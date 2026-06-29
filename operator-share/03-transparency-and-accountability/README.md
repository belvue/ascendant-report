# Transparency & accountability - findings archive

**Product:** Player disclosure, informed consent, and fundraising honesty for Ascendant EQ  
**Assessment date:** 2026-06-13 (Ko-fi feed observations through 2026-06-16)  
**Scope:** Server Support / Ko-fi, trust hub rollout, patcher **disclosure & consent** (not supply-chain CVEs)

Technical patcher security (HTTP CDN, `dinput8`, remote file swap) lives in **[04-client-patcher-integrity](../04-client-patcher-integrity/README.md)**.

---

## Priority index

### P1 - High

| ID | Title | File |
|----|-------|------|
| **Donations** | | |
| DT-001 | “Hosting costs” messaging does not match split infrastructure | [P1-high/DT-001-hosting-cost-messaging-split-infrastructure.md](/operator-share/03-transparency-and-accountability/P1-high/DT-001-hosting-cost-messaging-split-infrastructure.md) |
| DT-010 | Server Runway bar uses undifferentiated burn - not tied to real cost buckets | [P1-high/DT-010-runway-meter-undifferentiated-burn-rate.md](/operator-share/03-transparency-and-accountability/P1-high/DT-010-runway-meter-undifferentiated-burn-rate.md) |
| DT-011 | No public breakdown of funds received by channel or period | [P1-high/DT-011-no-channel-receipt-transparency.md](/operator-share/03-transparency-and-accountability/P1-high/DT-011-no-channel-receipt-transparency.md) |
| KF-001 | Public Ko-fi feed is not a complete donation record | [P1-high/KF-001-public-kofi-feed-not-complete-donation-record.md](/operator-share/03-transparency-and-accountability/P1-high/KF-001-public-kofi-feed-not-complete-donation-record.md) |
| KF-002 | Same-name donor / character MOA jump warrants reconciliation | [P1-high/KF-002-same-name-moa-inventory-anomaly.md](/operator-share/03-transparency-and-accountability/P1-high/KF-002-same-name-moa-inventory-anomaly.md) |
| **Program rollout** | | |
| TR-001 | No unified player trust / transparency hub on official surfaces | [P1-high/TR-001-no-unified-trust-hub.md](/operator-share/03-transparency-and-accountability/P1-high/TR-001-no-unified-trust-hub.md) |
| TR-010 | Acknowledgement blocks not embedded at decision points | [P1-high/TR-010-acknowledgements-not-at-decision-points.md](/operator-share/03-transparency-and-accountability/P1-high/TR-010-acknowledgements-not-at-decision-points.md) |
| TR-011 | Trust-affecting facts primarily in Discord - not versioned official docs | [P1-high/TR-011-discord-primary-disclosure-pattern.md](/operator-share/03-transparency-and-accountability/P1-high/TR-011-discord-primary-disclosure-pattern.md) |
| **Patcher disclosure** | | |
| PC-011 | No patcher acknowledgement or opt-in before `eqhost.txt` overwrite | [P1-high/PC-011-no-eqhost-consent-in-patcher.md](/operator-share/03-transparency-and-accountability/P1-high/PC-011-no-eqhost-consent-in-patcher.md) |
| PC-012 | Modified client files deployed without player-visible manifest | [P1-high/PC-012-eqgame-deployed-without-file-list-ui.md](/operator-share/03-transparency-and-accountability/P1-high/PC-012-eqgame-deployed-without-file-list-ui.md) |

### P2 - Medium

| ID | Title | File |
|----|-------|------|
| DT-020 | Ko-fi character-name field creates donor-tracking optics | [P2-medium/DT-020-character-name-field-donor-optics.md](/operator-share/03-transparency-and-accountability/P2-medium/DT-020-character-name-field-donor-optics.md) |
| KF-003 | Selective supporter messages removed from public feed Jun 12 - 14 | [P2-medium/KF-003-selective-feed-masking.md](/operator-share/03-transparency-and-accountability/P2-medium/KF-003-selective-feed-masking.md) |
| DT-021 | EQEmu Server Operator Terms question raised - not resolved in public | [P2-medium/DT-021-eqemu-terms-compliance-unresolved.md](/operator-share/03-transparency-and-accountability/P2-medium/DT-021-eqemu-terms-compliance-unresolved.md) |
| DT-022 | Runway widget hides data source, formula, and last update | [P2-medium/DT-022-runway-data-provenance-not-shown.md](/operator-share/03-transparency-and-accountability/P2-medium/DT-022-runway-data-provenance-not-shown.md) |
| TR-020 | No versioned changelog when trust-affecting client or login config changes | [P2-medium/TR-020-no-trust-changelog-discipline.md](/operator-share/03-transparency-and-accountability/P2-medium/TR-020-no-trust-changelog-discipline.md) |
| TR-021 | Player notice package ready - not published or linked from product UI | [P2-medium/TR-021-player-notices-not-published.md](/operator-share/03-transparency-and-accountability/P2-medium/TR-021-player-notices-not-published.md) |
| PC-022 | Discord discusses `dinput8`; patcher UI does not | [P2-medium/PC-022-discord-disclosure-patcher-ui-silent.md](/operator-share/03-transparency-and-accountability/P2-medium/PC-022-discord-disclosure-patcher-ui-silent.md) |
| PC-023 | Patcher auto-launches game after patch with no change summary | [P2-medium/PC-023-no-post-patch-change-summary.md](/operator-share/03-transparency-and-accountability/P2-medium/PC-023-no-post-patch-change-summary.md) |

### P3 - Informational

| ID | Title | File |
|----|-------|------|
| DT-030 | Donor notes may link Ko-fi identity to in-game characters | [P3-informational/DT-030-donor-identity-linking-transparency.md](/operator-share/03-transparency-and-accountability/P3-informational/DT-030-donor-identity-linking-transparency.md) |
| DT-031 | Page states no in-game perks for donations (clarity of intent) | [P3-informational/DT-031-stated-no-in-game-perks.md](/operator-share/03-transparency-and-accountability/P3-informational/DT-031-stated-no-in-game-perks.md) |
| TR-030 | Transparency topics span briefs - needs single rollout owner | [P3-informational/TR-030-cross-brief-rollout-index.md](/operator-share/03-transparency-and-accountability/P3-informational/TR-030-cross-brief-rollout-index.md) |

---

## Player-facing materials

| Document | Purpose |
|----------|---------|
| [player-transparency-index-v1.md](player-transparency-index-v1.md) | One-page index - all notices and acknowledgements |
| [transparency-rollout-checklist.md](transparency-rollout-checklist.md) | Operator implementation checklist |
| [player-donation-notice-v1.md](player-donation-notice-v1.md) | Informed giving - Ko-fi, runway, cost split |
| [player-donation-acknowledgement-block.md](player-donation-acknowledgement-block.md) | Pre-donate checkbox |
| [player-patcher-acknowledgement-block.md](player-patcher-acknowledgement-block.md) | Pre-`eqhost` write checkbox + opt-in/out |

Login and hosting notices remain in **01** and **02**; client install summary in **04**.

---

## Response

[operator-response.md](../operator-response.md) (transparency & accountability section) · [private-operator-summary.md](private-operator-summary.md)

---

## Out of scope

- **Patcher supply-chain security** - HTTP CDN, `dinput8` hook, remote file swap ([04-client-patcher-integrity](../04-client-patcher-integrity/README.md))
- **Login platform CVEs / federation** - [01-loginserver-brief](../01-loginserver-brief/README.md)
- **ATLAS, SSH, residential hosting exposure** - [02-residential-host-brief](../02-residential-host-brief/README.md)
- **Criminal fraud allegations** - optics and informed-consent gaps only

---

## Related disclosure in other briefs

| Topic | Brief | Finding |
|-------|-------|---------|
| Login path / LSPX | 01 | LS-015 |
| Hosting topology | 02 | RH-011 |
| eqhost CDN mechanism | 04 | PC-010 (cross-ref LS-015) |
