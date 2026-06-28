# 01b - Before going public

This chapter is why a public report exists at all. Sharing notes are at the bottom.

## Remediation first

When the investigation matured, the plan was not a Reddit dossier. It was findings, a remediation list, and a private presentation to Straps first. The list was concrete:

- **Login transparency:** Tell every patched client, at first run, that credentials validate on `login.eqemulator.dev`, not only in buried Discord threads.
- **Hosting disclosure:** Break out what donors fund: OVH web, residential world, tools, operator/dev time, volunteer coordination, and reserve. **~$411/mo** can be a fair **whole-project** burn when real labor and tooling are included; it is not fair to present that figure as pure **hosting** when a **infra-only** estimate is **~$55/mo** ([11-hosting-cost-gap](11-hosting-cost-gap.md)). Actual costs happened. Donors need labeled buckets, not one runway line that reads like datacenter rent.
- **Ko-fi feed integrity:** Stop selective hiding and blaming the platform; treat the public feed as optional social proof, not an audit log, or publish an honest accounting.
- **Enforcement consistency:** Investigate and enforce unattended play with **operator-run** tooling, log review, and audits. Apply the Discord AFK ladder to the long high-AFK tail, not only selective ban waves ([06-enforcement-asymmetry](06-enforcement-asymmetry.md)). BotWatch is not a handout. That onus is on staff. This report shows what a third party can reconstruct from public sources when someone actually tries.
- **Backend and security maturity:** Harden the residential stack; auth-gate admin and character APIs; separate game/web/login networks; publish trust docs instead of Discord-only pins ([04-infrastructure-security](04-infrastructure-security.md), [04b-operational-security](04b-operational-security.md)).

That was the good-faith path: fix the gaps, keep the server, give players a reason to trust the operator again. No public humiliation required.

## Why private presentation became moot

In May 2026 I reported a large platinum-generation bug in private with a full post-mortem ready. I was permabanned within about twenty minutes. The next day staff posted mockery and a disputed ledger ([14-reporter-account](14-reporter-account.md)). That was when I stopped taking the public posture at face value and kept digging.

By mid-June Straps went quiet in Discord when donation and login questions heated up. The post-mortem I sent met silence. Harassment continued in public channels. Straps had been absent from the server and from hard questions for too long.

The Ko-fi page still sold runway months on "hosting" while the live world sat on a **weak residential backend**: exposed ATLAS admin UI, open SSH and game ports, partial SQLi fixes, and **no visible hardening on re-probe** ([04-infrastructure-security](04-infrastructure-security.md)). Planes of Power still went up with on-record **not fully tested yet** language while item tiers showed a **flat PoP ladder** after Luclin's spike and **LDoN had shipped before Luclin** ([07-anti-cheat-and-patcher](07-anti-cheat-and-patcher.md)). The visible record showed more public evidence of donation-feed curation than of transparent anti-cheat disclosure or audit-backed enforcement ([05-donations-moa-kofi](05-donations-moa-kofi.md), [07-anti-cheat-and-patcher](07-anti-cheat-and-patcher.md)).

Presenting a structured remediation brief felt pointless by then. Not because the findings were weak. The public line is still **tell us what to fix**. What the record showed was mockery toward people who did, **half-tested expansion tiers** pushed out anyway, and **steady effort on Ko-fi runway and feed optics** instead of login disclosure, hosting honesty, or enforcement.

## Why public release

Public release is a fallback, not revenge or score-settling. I moved to it when private outreach failed and operator involvement stayed missing for too long. Players and donors deserve to know where login goes, where the world runs, what Ko-fi money implies, and how enforcement actually lands. If the operator will not engage privately on those items, the honest record belongs with the people funding and playing the server.

This package is redacted on purpose. Staff on-record Discord quotes stay in. Player names, donor names, recipient-traceable DMs, and named Ko-fi feed captures stay out. Redaction rules (player/donor names out, staff on-record quotes in) apply throughout this package.

## Findings we would have presented

Category summary only. Detail lives in the linked chapters and the [operator-share briefs](operator-share/README.md) (projects 01-05).

### Login - player trust (Ascendant patcher)

What patched clients and donors needed first:

- **[PC-010](operator-share/04-client-patcher-integrity/P1-high/PC-010-eqhost-login-redirect-via-cdn.md) / [LS-015](operator-share/01-loginserver-brief/P1-high/LS-015-login-path-disclosure-gap.md):** Patcher CDN rewrites `eqhost.txt` to `login.eqemulator.dev`; no first-run consent in the client path. Buried Discord posts are not disclosure ([03-login-trust-boundary](03-login-trust-boundary.md), [00-credential-warning](00-credential-warning.md)).
- **[LS-025](operator-share/01-loginserver-brief/P2-medium/LS-025-directory-curation-trust-labels.md):** Curated six-server directory and trust flags instead of the community `.net` list ([03-login-trust-boundary](03-login-trust-boundary.md#curated-server-directory-six-servers-not-the-community-list)).
- **Operator-controlled login:** Password hashes, resets, and federation copies live on infrastructure Straps runs. That is not routine cleartext password access. It is still a trust boundary players were not walked through at patch time.

### Operator-share briefs (Jun 2026)

Full priority indexes, remediation steps, and player-notice drafts live under [operator-share/](operator-share/README.md). This section is the map, not a second copy of every finding ID.

| Brief | Scope | Start here |
|-------|--------|------------|
| [01-loginserver-brief](operator-share/01-loginserver-brief/README.md) | `login.eqemulator.dev` web, federation APIs, loginserver ports | Lab: [03b](03b-loginserver-lab-methodology.md) · Narrative: [03](03-login-trust-boundary.md) |
| [02-residential-host-brief](operator-share/02-residential-host-brief/README.md) | Residential game host, ATLAS, SSH, hosting topology | [04](04-infrastructure-security.md) · [11](11-hosting-cost-gap.md) |
| [03-transparency-and-accountability](operator-share/03-transparency-and-accountability/README.md) | Ko-fi feed, runway copy, trust hub, patcher consent | [05](05-donations-moa-kofi.md) · [08](08-referrals-leaderboards.md) |
| [04-client-patcher-integrity](operator-share/04-client-patcher-integrity/README.md) | CDN `eqhost.txt`, autoPatch, client file supply chain | [03](03-login-trust-boundary.md) · [07](07-anti-cheat-and-patcher.md) |
| [05-project-funding-and-operator-compensation](operator-share/05-project-funding-and-operator-compensation/README.md) | Infra vs labor vs reserve buckets on appeals | [11](11-hosting-cost-gap.md) · [05](05-donations-moa-kofi.md) |

**Enforcement asymmetry** and **expansion readiness** were not separate briefs. Public record: [06](06-enforcement-asymmetry.md), [07](07-anti-cheat-and-patcher.md), [12](12-operator-presence.md).

**Operator response template:** [operator-response.md](operator-share/operator-response.md) (checkbox remediation list by finding ID).

If Straps publishes a login-path notice, hosting breakdown, or Ko-fi audit, link it. This report should age with new facts.

## If you share this package

Recommended reading order for your audience:

1. [00-credential-warning](00-credential-warning.md) if they still play on Ascendant.
2. [02-executive-summary](02-executive-summary.md) for the one-page version.
3. Deep dives by interest: login (03, 03b), hosting and opsec (04, 04b, 11, 12), donations (05, 08), enforcement (06, 09b), patcher and integrity (07), methodology (09), witness (10, withheld), actions (13), reporter account (14), closing (15). Operator remediation briefs: [operator-share/](operator-share/README.md).

Use [00-credential-warning](00-credential-warning.md) and [02-executive-summary](02-executive-summary.md) as entry points rather than dumping every chapter at once.

This report is written to be boring on purpose. Hot language gets dismissed. Artifact IDs and probe dates survive moderation threads.

**Safety:** Do not repost withheld exhibits (player screenshots, named Ko-fi feed captures, verbatim recipient-traceable DMs). The placeholder means the proof exists privately, not that you should hunt it down. Staff names are already on their own Discord. On-record staff quotes with permalinks are fine where the chapter includes them. Harassment helps nobody. Ask operators questions in public channels if you want answers on the record.

**Corrections:** Send factual updates with artifacts. Update beats spin.

Previous: [01-why-this-exists](01-why-this-exists.md) · Next: [02-executive-summary](02-executive-summary.md)
