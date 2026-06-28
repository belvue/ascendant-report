# RH-011: Split infrastructure not disclosed to players

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (probe + public materials review) |
| **Component** | Governance / transparency |
| **CWE** | N/A |

## Summary

Ascendant operates a **split hosting model**: public **website, CDN, and patch downloads** on **OVH US**, while the **live game world** runs on **residential AT&T**. Player-facing materials (website, Ko-fi, patcher UI) do **not** clearly explain this split. Average players may believe the game world is datacenter-hosted.

## Example

**Infrastructure map:**

| Layer | Hostname | IP | Provider |
|-------|----------|-----|----------|
| Website / CDN / patcher downloads | `ascendanteq.com`, `downloads.*` | 147.135.10.69 | OVH US |
| **Game world** | `eqemu.ascendanteq.com` | 99.42.xxx.xxx | AT&T residential |
| Federation login (separate brief) | `login.eqemulator.dev` | 144.217.76.180 | OVH Canada |

**Disclosure check:**

| Location | Game = residential disclosed? |
|----------|-------------------------------|
| Patcher UI | No |
| Public website / Ko-fi | "Hosting costs" - implies datacenter; no breakdown |
| Discord `#helpful-info` | Partial - staff discussed move in community context |

## Attack vector

**Prerequisites:** N/A - transparency finding.

**Player impact:** Uninformed consent on uptime, security, and donation allocation.

## Implications

- **Trust:** Material optics gap.
- **Donations:** Donors may fund OVH (real) while game runs on home broadband (low marginal cost). See **[03-transparency-and-accountability](../../03-transparency-and-accountability/README.md)** (DT-001).
- Pairs with RH-010. Hosting topology: RH-011; fundraising honesty: DT-001.

## Remediation

1. Publish **hosting topology diagram** on website (link from Server Support if desired).
2. Patcher splash or `#helpful-info` pin: "World server: residential ISP; Web/CDN: OVH."
3. Use [player-acknowledgement-block.md](../player-acknowledgement-block.md) before long-term play.
4. **Donation / Ko-fi copy and runway:** see [03-transparency-and-accountability](../../03-transparency-and-accountability/README.md) (DT-001, DT-010, DT-011).

## Verification

- New player can answer "where does my character data live?" from official sources alone.
- Hosting topology published with date. Donation/runway transparency: [03-transparency-and-accountability](../../03-transparency-and-accountability/README.md).
