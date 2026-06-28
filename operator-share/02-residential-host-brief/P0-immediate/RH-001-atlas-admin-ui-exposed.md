# RH-001: ATLAS admin UI reachable without authentication gate

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (probe) - 2026-06-12 and re-probe 2026-06-13 |
| **Component** | ATLAS control plane - HTTPS :443 |
| **CWE** | CWE-306 (Missing Authentication for Critical Function) |

## Summary

The **ATLAS - Capability & Context Control Plane** admin interface is served on the **public residential game IP** without an visible login wall. Multiple `/admin/*` routes return HTTP 200 and render full navigation (Settings, API Keys, RBAC, Federation, Webhooks, etc.) to unauthenticated GET requests.

This is **not** the eqemulator.dev federation admin (`eqemulator.dev/admin/settings` returns 404).

## Example

**Probe (read-only GET, 2026-06-13):**

| URL | Status | Notes |
|-----|--------|-------|
| `https://eqemu.ascendanteq.com/admin/settings` | 200 | ~29 KB HTML - "ATLAS" title, full admin nav |
| `https://eqemu.ascendanteq.com/admin/settings` (by hostname; same host as masked residential IP) | 200 | Identical response |
| `/admin/api-keys` | 200 | UI shell |
| `/admin/rbac` | 200 | UI shell |
| `/admin/federation` | 200 | Federation & Import UI |

Page title: *"ATLAS - Capability & Context Control Plane"*. Settings body shows client-side loading spinner fetching backend APIs.

## Attack vector

**Prerequisites:** Internet access; no credentials.

**Steps:**

1. Browse to `https://eqemu.ascendanteq.com/admin/settings` (or raw IP with TLS bypass).
2. View admin navigation, page structure, and client-side routes.
3. Attempt further interaction (forms, imports) - **not tested** in this assessment to avoid unauthorized access.

**Attacker role:** Anonymous reconnaissance; potential escalation if write APIs lack auth (RH-032).

## Implications

- **Confidentiality:** Exposes operator control-plane architecture to anyone.
- **Integrity / availability:** Increases risk if write endpoints are also unauthenticated or weakly protected.
- **Co-location risk:** Same IP as live EQEmu world server (:9000) and SSH (:22).
- **Legal exposure:** Operator and players on a consumer ISP line with enterprise admin software internet-facing.

## Remediation

1. **Immediately** place all `/admin/*` behind authentication (SSO, basic auth at nginx, or ATLAS login gate).
2. **Remove ATLAS from public internet** - bind to VPN, Tailscale, or OVH management network only.
3. Do not co-locate control-plane UIs on the game world IP.
4. Add nginx `deny all` for `/admin` from non-management IPs until proper auth is deployed.
5. After fix, confirm with external GET (expect 401/403/302 to login).

## Verification

```bash
curl -s -o /dev/null -w "%{http_code}" https://eqemu.ascendanteq.com/admin/settings
```

Expected after fix: **401**, **403**, or **302** to login - not **200** with full admin shell.

Cross-check: [RH-002](RH-002-atlas-admin-api-unauthenticated.md) API must also require auth.
