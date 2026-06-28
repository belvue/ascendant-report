# LS-012: Federation sync_data exports all password hashes to approved peers

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (reproduction) - intentional federation design |
| **Component** | Web - federation API |
| **CWE** | N/A (trust / data sharing) |

## Summary

The endpoint `GET /api/federation/sync_data` returns a full export of loginserver accounts **including `account_password` hashes**, world servers, server admins, and related metadata. Access requires an **approved federation peer** with valid Ed25519 signed request - anonymous access correctly returns 401.

## Example

**Reproduction (2026-06-13):**

1. Initialize federation master; admin `add_peer` + bootstrap registration.
2. Mock peer signs GET `/api/federation/sync_data` with Ed25519 headers.
3. **Response:** `HTTP 200` with `accounts[]` - **5/5 rows included hash fields** (scrypt prefixes observed).

**Anonymous GET (no signature):** `HTTP 401`

**Source comment:** `handleSyncData` - *"Export loginserver accounts (with password hashes for auth replication)"*.

## Attack vector

**Prerequisites:**

- Attacker operates or compromises an **approved federation node**, **or**
- Master admin approves a malicious peer via bootstrap token, **or**
- Stolen Ed25519 private key of approved peer.

**Steps:**

1. Obtain approved node identity.
2. Sign federation GET request per middleware spec (`X-Federation-PublicKey`, `Timestamp`, `Signature`).
3. Pull full hash dataset on interval (mesh tier pulls from master).

**Attacker role:** Federation peer operator - not anonymous.

## Implications

- **Confidentiality:** All player password hashes replicated to every approved mesh/official peer.
- **Blast radius:** Compromise of any peer with sync_data pull = credential hash exposure for entire synced population.
- **Governance:** Master controls who gets approved - high trust concentration.

## Remediation

1. **Publish federation peer list** - who receives hash exports today.
2. Tighten peer approval - short-lived bootstrap tokens, out-of-band key verification (fingerprint).
3. Consider **hash sync scope reduction** for mesh tier if product allows (product change).
4. Monitor `sync_data` pull frequency and audit log (`federation_audit_log`).
5. Suspend/revoke peers promptly (`suspend_node`, `revoke_node` admin actions).

## Verification

- Anonymous GET returns 401.
- Documented peer list matches `federation_nodes` where `is_approved=1`.
- Revoked peer receives 403 on signed request.
