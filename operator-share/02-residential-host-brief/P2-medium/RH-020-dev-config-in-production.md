# RH-020: Production deploy leaks dev configuration

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (probe) |
| **Component** | ATLAS / Next.js deploy |
| **CWE** | CWE-489 (Active Debug Code) |

## Summary

HTML metadata on the ATLAS deployment references **`http://localhost:3000`** for Open Graph images (e.g. `localhost:3000/atlas-overview.png`), indicating development configuration shipped to the internet-facing production host on the residential IP.

## Example

**Observed in page source (2026-06-12):**

- OG meta tags point to `http://localhost:3000/...` paths
- Suggests `.env` or Next.js config not updated for production URL

Same Next.js webpack chunk hashes appear on OVH portal (`147.135.10.69`) - shared pipeline with incomplete prod config.

## Attack vector

**Prerequisites:** None - visible in HTML source.

**Implications:** Signals immature deploy process; may correlate with other misconfigurations (RH-001, RH-012).

## Implications

- **Operational:** Increases confidence other security settings were not production-hardened.
- **Low direct exploit** - informational for attackers.

## Remediation

1. Set `NEXT_PUBLIC_*` and OG base URLs to production hostname.
2. Add CI check blocking `localhost` in production build artifacts.
3. Separate dev/staging/prod environment files.

## Verification

View source on `/admin/settings` - no `localhost` references in meta tags.
