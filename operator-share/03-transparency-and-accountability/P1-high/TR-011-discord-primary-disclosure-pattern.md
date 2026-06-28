# TR-011: Trust-affecting facts primarily in Discord - not versioned official docs

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (materials review) |
| **Component** | Governance / comms |
| **CWE** | N/A |

## Summary

Multiple **trust-affecting** facts appear first (or only) in **Discord** - eqhost changes, `dinput8` purpose, hosting move to residential, login workarounds - while **website and patcher stay silent**. Discord pins **age out** when hostnames or CDN files change; players without Discord inherit **information asymmetry**.

## Example

**Discord-disclosed vs product-silent (observed pattern):**

| Topic | Discord | Patcher / website |
|-------|---------|-------------------|
| eqhost re-patch behavior | `#helpful-info` (2026-03) | Silent autoPatch |
| `dinput8` / MQ compatibility | `#server-files`, `#patch-notes` | No file list in UI |
| Cloud → home game host | Staff discussion | Not on donation page |
| Login password mismatch workarounds | Bug threads, `#mq-discussion` | No troubleshooting page |

## Player scenario

**Context:** Player does not use Ascendant Discord.

**What happens:**

1. Misses migration guidance.
2. Blames self or server for login failures.
3. Discovers hosting facts from Reddit or external notices.

## Implications

- **Equity:** Discord-regulars vs everyone else.
- Remediation is **canonical docs + TR-001 hub**, not more pins alone.

Pairs with **TR-020**, **TR-021**.

## Remediation

1. Every Discord “trust pin” → **mirrored** on `/about/trust` with **ISO date**.
2. Deprecate stale pins when CDN eqhost or files change.
3. Patcher links to web docs - not “join Discord for login help.”

## Verification

- Player without Discord completes login recovery using **website only**.
