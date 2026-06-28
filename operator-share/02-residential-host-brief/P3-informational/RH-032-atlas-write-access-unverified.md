# RH-032: Anonymous ATLAS write access not demonstrated

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Assessment limit (intentional) |
| **Component** | ATLAS control plane |
| **CWE** | N/A |

## Summary

Assessment **deliberately stopped at read-only GET**. Anonymous **write** access to ATLAS (settings changes, API key creation, RBAC edits, federation import) was **not tested** and **not claimed**.

**Proven:** Public admin UI shell (RH-001) and unauthenticated read of `/api/admin/settings` (RH-002).

**Not proven:** Strangers can modify operator systems without credentials.

## Example

**Not probed (by policy):**

- POST to settings or admin APIs
- API key creation forms
- RBAC or federation import actions
- SSH login attempts

**Probed (read-only):**

- GET `/admin/*` routes → 200 with UI shell
- GET `/api/admin/settings` → 200 with status JSON

## Attack vector

**Prerequisites for write (unknown):** May require session cookies, API keys, or network position not exercised in this assessment.

**Risk:** Read exposure plus degraded backend (RH-012) increases **likelihood** other endpoints are misconfigured - not confirmed here.

## Implications

- Player notices must **not** claim the server is "already hacked."
- Operator should still treat RH-001/RH-002 as **critical** - read exposure is sufficient for urgent remediation.

## Remediation

Same as RH-001/RH-002 - auth on all admin routes; assume write paths may exist behind same host.

## Verification

After auth gate deployed, attempt anonymous GET (expect deny) and confirm write endpoints require credentials in operator test environment.
