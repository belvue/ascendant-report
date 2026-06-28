# LS-021: Moderator role has admin privileges on many routes

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source); role split not tested in reproduction |
| **Component** | Web - RBAC |
| **CWE** | CWE-269 (Improper Privilege Management) |

## Summary

Multiple admin routes use `requireAdmin()` that accepts both **`admin`** and **`moderator`** platform roles. Moderators may perform sensitive operations (federation panel, loginserver account reset/delete, import accounts) depending on route - equivalent to full admin on those endpoints.

## Example

**Source:** `web/src/app/api/admin/loginserver-accounts/route.ts`:

```typescript
if (adminCheck.length === 0 || (adminCheck[0].role !== "admin" && adminCheck[0].role !== "moderator")) {
  return null;
}
```

Similar pattern on federation admin, server tiers, logs, etc.

**Reproduction:** Anonymous requests blocked (403); moderator session not tested.

## Attack vector

**Prerequisites:** Compromise of moderator account (phishing, weak password, insider).

**Steps:**

1. Attacker logs in as moderator.
2. Accesses `/admin` tabs and API routes gated by `requireAdmin`.
3. Resets loginserver passwords, imports accounts, views federation config.

**Attacker role:** Authenticated moderator.

## Implications

- **Integrity:** Account takeover via admin reset paths.
- **Least privilege violation:** Moderators often intended for content-only moderation.
- **Audit:** Actions may be attributed to moderator without distinguishing limited scope.

## Remediation

1. Split **`requireMasterAdmin()`** vs **`requireModerator()`** - sensitive routes (federation, password reset, import, system) → admin only.
2. Document moderator scope in operator runbook.
3. Enable MFA for all moderators (already for admin/server owners when Resend configured).
4. Audit log all admin POST actions with role field.

## Verification

- Moderator session receives 403 on federation initialize, password reset, import-accounts.
- Admin session still succeeds.
- Role matrix published internally.
