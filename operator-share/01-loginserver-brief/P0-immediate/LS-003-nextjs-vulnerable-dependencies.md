# LS-003: Next.js 15.5.14 - multiple high-severity advisories

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | CVE (lockfile audit); Confirmed (reproduction) |
| **Component** | Dependencies - web runtime |
| **CWE** | Multiple (CWE-288, CWE-918, CWE-770, CWE-79) |

## Summary

The web application pins **next@15.5.14**. npm audit reports **14 advisories** on this package, including **middleware/proxy bypass**, **SSRF via WebSocket upgrades**, and **denial-of-service** issues in Server Components. Installed version confirmed in test deployment.

## Example

**Lockfile:** `web/package.json` → `"next": "15.5.14"`

**Deployment check:**

```bash
node -e "console.log(require('next/package.json').version)"
# Observed: 15.5.14
```

**Representative advisories (not exhaustive):**

| GHSA | CVSS | Title |
|------|------|-------|
| [GHSA-492v-c6pp-mqqv](https://github.com/advisories/GHSA-492v-c6pp-mqqv) | 8.1 | Middleware bypass via dynamic route parameter injection |
| [GHSA-c4j6-fc7j-m34r](https://github.com/advisories/GHSA-c4j6-fc7j-m34r) | 8.6 | SSRF via WebSocket upgrades |
| [GHSA-26hh-7cqf-hhc6](https://github.com/advisories/GHSA-26hh-7cqf-hhc6) | 7.5 | Middleware bypass (segment-prefetch follow-up) |
| [GHSA-mg66-mrh9-m8jx](https://github.com/advisories/GHSA-mg66-mrh9-m8jx) | 7.5 | DoS via connection exhaustion |

**Fix version:** `next@15.5.19` or later (per npm audit recommendation).

## Attack vector

**Prerequisites:** Varies by CVE - generally unauthenticated HTTP access to the Next.js app; middleware bypass CVEs may allow reaching routes intended to be protected.

**Steps:** Follow public GHSA proof-of-concept for each advisory after upgrade testing.

**Attacker role:** Anonymous (for most listed issues).

**Note:** Middleware bypass was **not exploited** against `/api/admin/*` during this assessment.

## Implications

- **Confidentiality:** Middleware bypass / SSRF class issues may expose admin or internal routes.
- **Availability:** DoS advisories affect service stability.
- **Integrity:** XSS-class issues where user-controlled content reaches App Router paths.

## Remediation

1. Upgrade **next** to **≥15.5.19** in `web/package.json`; rebuild and redeploy web tier.
2. Run `npm audit` in CI; fail build on high/critical for production images.
3. After upgrade, regression-test admin session gates and federation routes.
4. Subscribe to Next.js security releases for the 15.5.x line.

## Verification

```bash
npm audit --json | jq '.vulnerabilities.next'
node -e "console.log(require('next/package.json').version)"
```

Expected: no high next advisories; version ≥15.5.19.
