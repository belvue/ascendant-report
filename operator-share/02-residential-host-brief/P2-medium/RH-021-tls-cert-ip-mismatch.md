# RH-021: TLS certificate mismatch when accessing host by raw IP

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (probe) |
| **Component** | TLS / nginx |
| **CWE** | CWE-295 (Improper Certificate Validation) |

## Summary

HTTPS requests to **`https://eqemu.ascendanteq.com/`** directly (without hostname) produce a **TLS certificate name mismatch**. The service is intended for hostname access (`eqemu.ascendanteq.com`) but the IP is widely published in probes and DNS.

## Example

Direct IP HTTPS probe requires certificate bypass (`curl -k`) to retrieve content. Certificate does not match raw IP SAN.

Hostname access works with valid cert chain for `eqemu.ascendanteq.com`.

## Attack vector

**Prerequisites:** Users or tools hitting IP directly (common in security scans and misconfigured clients).

**Implications:** Phishing or MITM confusion if users accept cert warnings; indicator of manual/Let's Encrypt hostname-only cert provisioning.

## Implications

- **User training:** Players should use hostname, not IP.
- **Ops:** Expected for Let's Encrypt hostname certs - not critical if hostname is canonical.

## Remediation

1. Prefer hostname everywhere in docs and DNS; avoid publishing raw IP in player materials where possible.
2. nginx default server block: reject or redirect IP-only HTTPS to hostname.
3. Optional: include IP in cert only if truly needed (usually not).

## Verification

```bash
curl -v https://eqemu.ascendanteq.com/ 2>&1 | grep -i "subject\|SSL"
curl -v https://eqemu.ascendanteq.com/ 2>&1 | grep "OK"
```

Expected: hostname validates; IP request redirects or fails cleanly without serving admin UI on wrong cert.
