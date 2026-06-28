# LS-034: C++ loginserver binary not fully auditable from published source

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Assessment gap |
| **Component** | C++ loginserver (GHCR image) |
| **CWE** | N/A (coverage gap) |

## Summary

The web layer and documentation were fully reviewed. The **C++ loginserver** shipped as `ghcr.io/straps-eq/eqemu-loginserver` is not completely represented in the public git clone - auth logging, memory handling of plaintext passwords during LSPX validate, and wire protocol edge cases were **not verified at binary level**.

## Example

**Reviewed:** TypeScript routes, `LOGINSERVER.md`, loginserver HTTP API and game-port behavior in isolated deployment (LSPX insert confirmed).

**Not reviewed:** Binary strings analysis, disassembly, packet fuzzing, container image CVE scan (Trivy/grype).

## Attack vector

**Unknown** - residual risk that binary logs credentials or contains undeclared network calls. **No evidence found** in web layer; binary review would increase confidence.

## Implications

- **Assessment confidence:** High for web/federation; medium for C++ runtime.
- **Operator action:** Optional third-party binary audit or publish complete matching source for tagged images.

## Remediation

1. Publish **source commit hash** matching each GHCR tag.
2. Run **container CVE scan** on loginserver image in CI.
3. Disable verbose auth logging in production; document logging policy.
4. Optional: commission binary review or reproduce build from source.

## Verification

- Image tag ↔ git tag mapping published.
- `strings` / log review shows no plaintext password logging (operator self-check).
