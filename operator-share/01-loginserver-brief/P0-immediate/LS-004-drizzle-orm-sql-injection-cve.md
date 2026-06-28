# LS-004: drizzle-orm SQL identifier injection (CVE)

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | CVE (lockfile); Not exploited in assessment |
| **Component** | Dependencies - ORM |
| **CWE** | CWE-89 (SQL Injection) |

## Summary

The project uses **drizzle-orm@0.33.0**, which is below the patched floor for [GHSA-gpj5-g38j-94v9](https://github.com/advisories/GHSA-gpj5-g38j-94v9) (CVSS 7.5). The advisory covers **SQL injection via improperly escaped SQL identifiers** in ORM-generated queries.

## Example

**Lockfile:**

```json
"drizzle-orm": "^0.33.0"
```

**npm audit:**

```
drizzle-orm  <0.45.2
SQL injection via improperly escaped SQL identifiers
Fix: drizzle-orm@0.45.2
```

**Context:** A separate federation SQLi via sync column names was fixed in author self-audit (whitelist in `sync.ts`). This CVE is an **ORM-layer** identifier escaping issue - different code path.

## Attack vector

**Prerequisites:** Application code path that passes **user-controlled** or **peer-controlled** values into Drizzle SQL identifier positions (table/column names), not just bound parameters.

**Steps:** Identify any dynamic identifier construction; attempt injection per GHSA guidance.

**Attacker role:** Depends on exposed input - potentially federation peer payloads or admin-configured fields.

**Assessment note:** No successful exploitation demonstrated during this review.

## Implications

- **Confidentiality:** Database read via identifier injection.
- **Integrity:** Data modification or destructive SQL if injection reachable.

## Remediation

1. Upgrade **drizzle-orm** to **≥0.45.2** (major bump - run full regression suite).
2. Audit all `sql` template usage and dynamic table/column references.
3. Keep federation sync column whitelists (`ALLOWED_COLUMNS`) - defense in depth.
4. Add CI dependency scanning.

## Verification

```bash
npm ls drizzle-orm
npm audit --json | jq '.vulnerabilities["drizzle-orm"]'
```

Expected: version ≥0.45.2; advisory absent.
