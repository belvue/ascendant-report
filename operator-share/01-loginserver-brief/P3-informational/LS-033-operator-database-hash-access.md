# LS-033: Operator can read all credential hashes via database access

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (reproduction) - architectural |
| **Component** | Infrastructure - MariaDB |
| **CWE** | N/A (trust model) |

## Summary

The loginserver stores password **hashes** (scrypt and legacy formats) in `login_accounts.account_password` and platform hashes in `platform_accounts`. Anyone with MariaDB root/application credentials - including the operator via normal administration - can read all hashes. **No vulnerability is required** for this access.

## Example

**Reproduction:**

```sql
SELECT COUNT(*) FROM login_accounts;   -- 5+ rows at assessment
SELECT id, account_name, source_loginserver, LEFT(account_password, 16) FROM login_accounts;
```

**Also:** Admin password reset (session required), API token hash overwrite (LS-011), federation sync_data (LS-012).

## Attack vector

**Prerequisites:** DB access, backup access, or host compromise.

**Steps:** Query or exfiltrate `login_accounts`; offline cracking attempt on scrypt hashes.

**Attacker role:** Operator / insider / attacker after infra breach.

## Implications

- **Confidentiality:** Hashes are available to operator by design.
- **Not proof of theft:** Architectural capability ≠ malicious intent.
- Players should use **unique passwords** not shared with other services.

## Remediation

1. **Transparency:** Publish that operator can access hashes (LS-015).
2. Harden DB: strong passwords, no public 3306, encrypted backups, access logging.
3. Minimize personnel with DB root.
4. Consider alerting on bulk SELECT/export patterns.

## Verification

- DB not internet-exposed.
- Backup encryption documented.
- Trust page states hash storage plainly.
