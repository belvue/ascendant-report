# LS-025: Curated server directory and trust labels

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (reproduction + architecture) |
| **Component** | Web / federation - directory |
| **CWE** | N/A (governance / transparency) |

## Summary

The login platform presents a **curated server directory** via `/api/servers` and related UI. Servers carry flags such as **trusted** and **claimed**. The federation master controls visibility and labels - the list is not guaranteed to match the full eqemulator.net roster.

## Example

**Reproduction:** `GET /api/servers` → 200 with seeded server entry (`is_server_trusted`, player counts).

**Architecture:** Master admin sets tiers, trust, federation source; mesh peers pull read-only views.

**Production:** Roster size and membership differ from broader community login lists (documented in platform audit).

## Attack vector

**Prerequisites:** None for **reading** directory; **writing** trust flags requires master admin.

**Steps (misuse scenario):**

1. Master marks operator-owned server as "trusted".
2. Players infer official endorsement.
3. Competing servers omitted from directory.

**Attacker role:** Operator governance - not CVE.

## Implications

- **Trust:** Players may conflate directory with official eqemu server list.
- **Competition / fairness:** Curation affects player routing and perceived legitimacy.
- Pairs with LS-015 (disclosure).

## Remediation

1. Publish **directory policy** - how servers get listed, trusted, claimed.
2. UI disclaimer: "Curated federation directory - not official EQEmu Foundation list."
3. Separate **community** vs **federated** server views if product allows.
4. Audit trail for trust flag changes.

## Verification

- Public policy page exists.
- UI shows disclaimer on server browser.
- Sample trust change appears in admin audit log.
