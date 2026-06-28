# RH-014: Player world path terminates on residential IP after federation login

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (architecture) |
| **Component** | Player session path |
| **CWE** | N/A (trust boundary) |

## Summary

Typical Ascendant player flow crosses **three networks**:

1. **Authentication** - patcher `eqhost.txt` → `login.eqemulator.dev:5999` (OVH Canada federation login - see project 01)
2. **Server list** - curated federation directory
3. **World gameplay** - client connects to `eqemu.ascendanteq.com:9000` → **residential AT&T**

Players may not understand that **credentials** and **gameplay** occur on different operator-controlled infrastructures.

## Example

**Patcher eqhost (observed):**

```
login.eqemulator.dev:5999   ← authentication
# eqemu.ascendanteq.com:5999  ← commented fallback
```

**After character select:**

```
World connection → eqemu.ascendanteq.com → 99.42.xxx.xxx:9000 (residential)
```

## Attack vector

**Prerequisites:** N/A - architectural transparency.

**Player confusion scenario:** User trusts `.dev` login security model but is unaware world runs on home ISP with ATLAS/SSH exposure (RH-001 - 003).

## Implications

- **Trust boundary:** Two hops, two risk profiles - must be disclosed together.
- **Incident response:** Login stack compromise ≠ game host compromise (and vice versa) - players need clarity.

## Remediation

1. Single **trust diagram** linking project 01 (login) and 02 (world host).
2. Patcher or website: "Auth: eqemulator.dev | World: eqemu.ascendanteq.com (residential)."
3. Cross-link notices; do not merge legal/compliance text without clear headings.

## Verification

Published diagram reviewed by third party for plain-language clarity.

Cross-reference: [01-loginserver-brief/LS-015](../../01-loginserver-brief/P1-high/LS-015-login-path-disclosure-gap.md).
