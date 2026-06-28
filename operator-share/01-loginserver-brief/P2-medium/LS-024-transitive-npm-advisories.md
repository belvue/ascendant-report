# LS-024: Transitive npm advisories (dompurify, lodash, postcss, uuid)

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | CVE (lockfile audit) |
| **Component** | Dependencies - transitive |
| **CWE** | CWE-79, CWE-94, CWE-1321 (varies) |

## Summary

Beyond Next.js and drizzle-orm (P0), npm audit reports **moderate/high** issues in transitive packages: **dompurify** (via isomorphic-dompurify), **lodash** (via recharts), **postcss**, **uuid** (via resend→svix).

**Total lockfile buckets:** 12 vulnerable packages, 25 unique GHSA entries (2026-06-13 audit).

## Example

| Package | Via | Notable GHSA | Fix |
|---------|-----|--------------|-----|
| dompurify | isomorphic-dompurify | GHSA-v9jr-rg53-9pgp (XSS bypass) | ≥3.4.0 |
| lodash | recharts | GHSA-r5fr-rjxr-66jc (code injection) | >4.17.23 |
| postcss | direct + next | GHSA-qx2v-qp2m-jg93 (XSS) | ≥8.5.10 |
| uuid | resend→svix | GHSA-w5hq-g745-h8pq | ≥11.1.1 |

Run `npm audit` in `web/` for full list.

## Attack vector

**Prerequisites:** Varies - mostly requires attacker-controlled input reaching sanitizer/chart/template code paths.

**Steps:** Per individual GHSA; not exploited in this assessment.

**Attacker role:** Typically authenticated user submitting HTML/chart data, or dev-server exposure for build-tool issues.

## Implications

- **Integrity / XSS:** dompurify/postcss/lodash issues if user content reaches vulnerable APIs.
- **Lower immediate risk** than P0 Next.js middleware bypass if admin surfaces sanitize output.

## Remediation

1. `npm audit fix` where non-breaking; manual bumps for major transitive updates.
2. Upgrade **recharts** or override **lodash** version if needed.
3. Pin **isomorphic-dompurify** / **dompurify** to patched releases.
4. CI: fail on high/critical audit results for production builds.

## Verification

```bash
cd web && npm audit
```

Expected: zero high/critical after remediation pass.
