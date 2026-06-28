# LS-015: Players not informed of third-party login path (disclosure gap)

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (source + ecosystem review) |
| **Component** | Governance / UX - cross-product |
| **CWE** | N/A (transparency) |

## Summary

Ascendant’s patcher deploys `eqhost.txt` pointing to **login.eqemulator.dev:5999** while commenting out **login.eqemulator.net:5999**. The patcher UI does not disclose this change. Players may believe they still authenticate solely against the community eqemulator login.

eqemulator.dev is **operator-owned** infrastructure, not EQEmu Foundation official login.

**Player-facing copy** (notice + acknowledgement) lives in [player-login-notice-v1.md](../player-login-notice-v1.md) and [player-acknowledgement-block.md](../player-acknowledgement-block.md) - separate from this finding doc.

## Example

**Patcher-deployed eqhost (observed):**

```
loginserver:login.eqemulator.dev:5999
#loginserver:login.eqemulator.net:5999
```

**Patcher binary:** No UI strings disclosing login redirection.

**Operator README:** Describes LSPX and federation in technical terms - not shown to players at login time.

**Combined with:** LS-010 (hash cache on game login), LS-012 (federation replication).

## Player scenario

**Context:** Player uses Ascendant patcher or operator-provided eqhost without reading server-side docs.

**What happens:**

1. Player patches and launches client.
2. Credentials validated on operator login stack (LSPX to `.net` on first login).
3. Player unaware of trust boundary shift.

**Not an exploit:** No attacker prerequisite - informed-consent gap only.

## Implications

- **Trust / consent:** Informed consent gap for credential handling.
- **Phishing-adjacent optics:** `.dev` naming adjacent to official `.org` / `.net`.
- **Operator liability / community relations:** Disclosure expected for third-party auth routing.

Does **not** prove malicious intent or credential theft.

## Remediation

1. **Patcher first-run modal** - login path, link to [player-login-notice-v1.md](../player-login-notice-v1.md). **At eqhost write:** use [04 patcher acknowledgement](../../03-transparency-and-accountability/player-patcher-acknowledgement-block.md) (PC-011).
2. **Persistent indicator** - "Authenticating via eqemulator.dev".
3. Publish **`/about/trust`** page: diagram, LSPX, federation peers, data retention.
4. Provide **eqhost choice** documentation (tradeoffs with `.net` direct).
5. Implement [player-acknowledgement-block.md](../player-acknowledgement-block.md) before account link (LS-014).
6. Add `/account` **access & activity** panel - sessions, login history, linked-account dates (LS-035).

## Verification

- New player can state where password is validated without reading GitHub.
- Patcher changelog / UI mentions login host.
- Trust page live and linked from login/register.
- Player-facing materials published (not only this finding doc).
