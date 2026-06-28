# LS-035: No end-user access or activity history in account settings

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source + live account) |
| **Component** | Web - account UX / transparency |
| **CWE** | N/A (disclosure / self-service gap) |

## Summary

After registering on **eqemulator.dev**, the **Account Settings** page (`/account`) shows username, account ID, password change, OAuth providers, loginserver linking, world-server tools, and `eqhost.txt` instructions. There is **no section for end users to review access or activity** - no login history, active sessions, linked-account timeline, LSPX/federation context, or “who can see my data.”

For a **new account**, linked loginserver and world-server sections are empty; nothing explains what will be stored when the user later links a legacy account or logs in through the client.

## Example

**Account nav (source):** Platform Account · Connected Accounts · Login Server Accounts · World Server Accounts · EQ Client Setup - no “Access,” “History,” “Sessions,” or “Data & privacy.”

**New user experience (2026-06):** Register → verify email → `/account` shows ID and empty link sections only.

**Backend exists, UI absent:**

- `platform_sessions` table stores web sessions (IP, user agent, expiry) - **no user-facing session list or revoke**
- `/api/account/loginserver-accounts` returns `lastLoginDate`, `linkedAt`, `sourceLoginserver` - **UI shows name and source badge only**, not dates or last activity
- No player-visible federation peer list, hash-cache notice, or LSPX explanation on the account page (see LS-015)

## Attack vector

**N/A** - transparency and informed-consent gap, not an exploit.

**User impact:** Players cannot self-audit where credentials were used, what is linked, or revoke stale web sessions without operator support.

## Implications

- **Trust:** Hard to exercise informed consent when LS-010, LS-012, and LS-014 describe data flows the account UI never surfaces.
- **Security hygiene:** No session management for regular users (admins get MFA; users get no visibility).
- **Pairs with LS-015:** Disclosure gap is both **out-of-band** (patcher) and **in-product** (account area incomplete).

## Remediation

1. Add **Access & activity** section on `/account`: recent logins (time, method), active sessions with revoke, linked loginserver rows with `linkedAt` / `lastLoginDate`.
2. Add **Your data** summary: LSPX behavior, hash storage, federation sync in plain language (link to trust notice).
3. On empty state for new users, explain what linking or client login will store **before** password submit (LS-014 acknowledgement).
4. Optional: export/delete request contact or self-service data export.

## Verification

- New and linked accounts see login history and session list on `/account`.
- Link-loginserver flow shows disclosure + checkbox before password entry.
- Trust/LSPX copy reachable from account settings without reading GitHub.
