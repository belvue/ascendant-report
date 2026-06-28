# LS-032: Master-initiated remote upgrade/restart of federation peers

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (source) |
| **Component** | Federation - remote ops |
| **CWE** | CWE-94 (Code Injection) if master compromised |

## Summary

Federation master can trigger **remote upgrade**, **restart**, and **force_pull_restart** on peer nodes via signed federation POST endpoints, proxied to an upgrade-agent service. Compromise of master admin or master signing keys implies **supply-chain control** over peer infrastructure.

## Example

**Routes:** `/api/federation/remote_upgrade`, `remote_restart`, `force_pull_restart` (POST, signed peer auth).

**Reproduction:** Not exercised - upgrade-agent not included in minimal test stack.

## Attack vector

**Prerequisites:** Compromised master federation identity or master admin panel.

**Steps:**

1. Attacker controls master.
2. Sends signed upgrade command to peer.
3. Peer pulls and restarts containers from master-controlled registry/tag.

**Attacker role:** Master operator or master key thief.

## Implications

- **Supply chain:** Peers trust master for deployment commands.
- **Availability:** Remote restart is intentional - also denial-of-service vector if abused.

## Remediation

1. Document peer trust model - peers must trust master code supply chain.
2. Peers verify container image signatures independently where possible.
3. Restrict master admin to minimum personnel; MFA required.
4. Audit log all remote upgrade/restart actions.

## Verification

- Runbook documents peer consent to remote ops.
- Test peer rejects unsigned upgrade commands.
