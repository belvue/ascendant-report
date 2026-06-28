# LS-022: Admin import-accounts creates placeholder password rows

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source); import tested in reproduction (200 success) |
| **Component** | Web - admin tooling |
| **CWE** | CWE-521 (Weak Password Requirements) |

## Summary

The admin `POST /api/admin/import-accounts` endpoint bulk-imports loginserver account rows from TSV/CSV with **`accountPassword: ""`** (empty placeholder). Imported accounts exist in `login_accounts` without valid password hashes until separately set.

## Example

**Reproduction (admin session):**

```http
POST /api/admin/import-accounts
{"data": "999001\timported_user_1\n999002\timported_user_2"}
→ HTTP 200 {"success": true, "imported": 2}
```

**Source:** `import-accounts/route.ts` - inserts with empty password field.

## Attack vector

**Prerequisites:** Platform admin or moderator session (see LS-021).

**Steps:**

1. Admin imports account names/IDs from external list.
2. Rows exist with empty passwords.
3. If loginserver allows login with empty hash or predictable recovery path, accounts may be weak - verify C++ loginserver behavior for empty hash rows.

**Attacker role:** Insider admin; or attacker if admin session compromised.

## Implications

- **Integrity:** Namespace squatting on lsaccount IDs / names.
- **Auth confusion:** Imported rows may interact unexpectedly with LSPX or client login.
- **Operational:** Intended migration tool - needs safeguards.

## Remediation

1. Require non-empty hash or force **password reset workflow** on first import.
2. Flag imported rows with `source_loginserver='import'` and block login until hash set.
3. Restrict import to **admin-only** (exclude moderator) - LS-021.
4. Log all imports with admin user id and row counts.

## Verification

- Imported row cannot authenticate until hash explicitly set.
- Moderator receives 403 on import endpoint (if policy changed).
