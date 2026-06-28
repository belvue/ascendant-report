# LS-023: Shared Resend API key documented for @eqemulator.dev mail

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source - README) |
| **Component** | Operations - email |
| **CWE** | CWE-798 (Credential Management) |

## Summary

Project README instructs federation peers to request a **shared Resend API key** from the operator for sending `noreply@eqemulator.dev` mail. Shared email credentials across operators increase blast radius if any peer mishandles the key.

## Example

**README (paraphrased):** Contact operator for shared Resend key to send from `@eqemulator.dev` domain.

**Impact surface:** MFA codes, email verification, password reset emails (if implemented via Resend).

## Attack vector

**Prerequisites:** Leaked shared Resend key from any peer operator or chat log.

**Steps:**

1. Attacker uses key to send mail from verified `@eqemulator.dev` domain.
2. Phishing or MFA interception against platform users.

**Attacker role:** Key holder - not anonymous HTTP.

## Implications

- **Integrity / trust:** Forged official-looking email from legitimate domain.
- **Confidentiality:** Resend dashboard may expose send history.

## Remediation

1. **Per-node Resend keys** or centralized mail relay controlled by master only.
2. Never distribute production Resend key in Discord DMs - use secure channel + rotation.
3. Restrict Resend key to master deployment; peers use own domains for mail.
4. Monitor Resend for anomalous send volume.

## Verification

- Inventory who holds Resend key today.
- Rotate key; revoke old key in Resend dashboard.
- Document mail architecture in operator runbook.
