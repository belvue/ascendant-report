# PC-020: `autoPatch: true` re-applies CDN files on every patch cycle

| Field | Value |
|-------|-------|
| **Priority** | P2 - Medium |
| **Status** | Confirmed (source + config) |
| **Component** | `eqemupatcher.yml` |
| **CWE** | N/A |

## Summary

Default **`autoPatch: true`** in `eqemupatcher.yml` causes the patcher to fetch the remote manifest on startup and **overwrite local files** whose MD5 differs from CDN. This includes **`eqhost.txt`** - so login routing and client files are **re-applied silently** whenever the operator updates the CDN.

## Example

**Typical local config:**

```yaml
autoPatch: true
clientVersion: Rain_Of_Fear_2
```

**Behavior:** Any CDN change to `eqhost.txt`, `dinput8.dll`, or `eqgame.exe` redeploys on next patch without version bump of `EQAscendant.exe`.

**Disable path (not in UI):** Edit `eqemupatcher.yml` → `autoPatch: false` - only string reference is a generic autoplay hint, not login consent.

## Attack vector

**N/A** - operational behavior.

## Implications

- Manual player fixes (`.net` eqhost) are **fragile**.
- Operator has **ongoing remote control** of client directory via CDN.
- Pairs with PC-021; consent UX in [03 PC-011](../../03-transparency-and-accountability/P1-high/PC-011-no-eqhost-consent-in-patcher.md).

## Remediation

1. Separate **autoPatch** toggle in patcher settings UI with explanation.
2. Do not overwrite `eqhost.txt` if player chose opt-out path (PC-011).
3. Notify when CDN pushes breaking file changes.

## Verification

- Player preference persists across patch cycles.
- CDN eqhost change triggers prompt, not silent overwrite (after fix).
