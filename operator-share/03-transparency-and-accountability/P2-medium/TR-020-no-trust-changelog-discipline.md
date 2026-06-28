# TR-020: No versioned changelog when trust-affecting client or login config changes

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (eqhost/CDN history) |
| **Component** | Governance / release comms |
| **CWE** | N/A |

## Summary

CDN **`eqhost.txt`**, **`dinput8.dll`**, and login hostname targets changed on **dated manifests** without a **player-facing changelog** explaining trust impact. Migrations (.net → Ascendant host → `.dev`) appear in **community threads**, not versioned operator release notes.

Cross-ref: eqhost churn absorbed into **PC-010**, **PC-020**, **LS-015** - this finding is **comms discipline** only.

## Example

| Change | Player-facing changelog observed? |
|--------|-----------------------------------|
| CDN eqhost → `login.eqemulator.dev` (2026-03-31 manifest) | No patcher/website entry in package |
| `dinput8` MQ compatibility update | Discord patch-notes only |
| Login password confusion wave | Bug threads - no migration notice |

## Player scenario

**Context:** Player returns after CDN eqhost update.

**What happens:** Unexpected login behavior - no “what changed” message.

## Remediation

1. **`trust-changelog.md`** on website - date, files affected, player action required.
2. Patcher post-patch summary references changelog entry (PC-023).
3. Email/Discord announcement **links** canonical changelog - not a substitute.

## Verification

- Last three CDN eqhost/file changes have **dated** public changelog entries.
