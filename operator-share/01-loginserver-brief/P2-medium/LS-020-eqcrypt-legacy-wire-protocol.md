# LS-020: Legacy eqcrypt wire encryption on game login port

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source) - inherited EQEmu protocol |
| **Component** | C++ loginserver - UDP/TCP :5998/:5999/:15900 |
| **CWE** | CWE-327 (Use of Broken Crypto) |

## Summary

EQ client login uses **eqcrypt** (DES-family) on the game login wire protocol. This is legacy EverQuest emulator design, not TLS. Anyone who can capture packets on the path may have reduced confidentiality compared to modern TLS-based auth.

## Example

**Documentation:** `LOGINSERVER.md` - client sends `OP_Login` encrypted with eqcrypt.

**Assessment:** UDP/TCP ports exposed for game clients; no wire fuzzing or packet capture performed in this review.

## Attack vector

**Prerequisites:** Network position to capture client↔loginserver traffic (same LAN, ISP, compromised router).

**Steps:**

1. Capture login packets on :5999 (or titanium/larion ports).
2. Apply known eqcrypt weaknesses / key material from protocol research.
3. Recover or replay credential material within protocol constraints.

**Attacker role:** Network adversary - not application-layer anonymous HTTP.

## Implications

- **Confidentiality:** Weaker than TLS for credential-adjacent material on the wire.
- **Inherited risk:** Common to EQEmu ecosystem - not unique to this fork.
- Does not replace LS-010 (hash still cached server-side after login).

## Remediation

1. Document wire protocol limits in operator security FAQ.
2. Recommend players use unique passwords; avoid password reuse.
3. Long-term: evaluate TLS wrapper or VPN for login path (major protocol change - coordinate with EQEmu community).
4. Ensure login ports are not exposed beyond necessary game client population.

## Verification

- Player-facing docs mention legacy wire encryption.
- Network exposure review documented.
