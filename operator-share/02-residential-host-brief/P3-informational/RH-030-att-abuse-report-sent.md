# RH-030: ISP abuse report sent to AT&T (2026-06-12)

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (disclosure record) |
| **Component** | Responsible disclosure process |
| **CWE** | N/A |

## Summary

On **2026-06-12**, a **security misconfiguration disclosure** was sent to **abuse@att.net** regarding the public ATLAS admin interface on **`99.42.xxx.xxx`**. This was responsible disclosure to the ISP - not a player-facing action and not proof of malicious operator intent.

## Example

**Transmission record:**

| Field | Value |
|-------|-------|
| Sent (UTC) | 2026-06-12T04:54:27 |
| To | abuse@att.net |
| Subject | Security Misconfiguration Disclosure: Public Admin Interface on 99.42.xxx.xxx |
| Attachments | None |

Report described public admin UI and unauthenticated status API on consumer broadband line.

## Attack vector

**N/A** - process note for operator context.

## Implications

- **Operator:** ISP may contact subscriber about open services on residential line.
- **Players:** No takedown observed as of 2026-06-13 re-probe (RH-031).
- **Assessment:** Independent of operator brief - documents that exposure was reported through proper channels.

## Remediation

Operator should treat ISP contact (if any) as additional urgency to close RH-001, RH-002, RH-003.

## Verification

Operator confirms receipt of ISP notice (if received) and remediation timeline.

**Do not** include abuse email body in player-facing materials.
