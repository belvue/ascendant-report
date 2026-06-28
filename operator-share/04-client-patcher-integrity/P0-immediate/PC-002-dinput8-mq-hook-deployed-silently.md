# PC-002: MacroQuest-class `dinput8.dll` deployed without patcher UI disclosure

| Field | Value |
|-------|-------|
| **Priority** | P0 - Immediate |
| **Status** | Confirmed (static analysis) |
| **Component** | CDN-deployed client - `dinput8.dll` |
| **CWE** | CWE-506 (Embedded Malicious Code) - *governance classification only; not malware verdict* |

## Summary

The patcher manifest deploys **`dinput8.dll`** (deploy date 2026-04-11) to every auto-patching client. Static analysis identifies it as **MacroQuest / eq-core-dll** infrastructure - DirectInput proxy, detour engine, `WriteProcessMemory`, 1,484 exports - **not** the stock Microsoft DirectInput DLL.

The **patcher UI does not list** this file or explain that a game-hook framework is being installed.

## Example

**Manifest entry:**

```yaml
- name: dinput8.dll
  md5: 7b36b360aa2c6addcb45854cfb953cf8
  date: "20260411"
```

**Identity strings (representative):** `MacroQuest`, `InitializeMQ2Detours`, `MQ2CharacterType`, `%s\MQ2Main.dll`

**Patcher UI strings:** Patch progress only - no mention of `dinput8`, MacroQuest, or hooks.

## Attack vector

**N/A for covert exploit** - file is **deliberately deployed** by operator CDN.

**Player impact:** All patching clients load local `dinput8.dll` when `eqgame.exe` starts (standard DLL search order).

## Implications

- **Transparency:** Players cannot make informed consent from patcher alone.
- **Server integrity:** Automation/cheat-class capability shipped silently; may conflict with published server rules if those restrict unattended play or warping.
- **Trust:** Major client modification presented as routine patch.

**Not claimed:** Credential theft or network exfiltration in static review (see PC-031).

## Remediation

1. **List every deployed DLL/exe** in patcher UI before write (PC-012).
2. Plain-language description: purpose of `dinput8.dll`, MacroQuest relationship, what hooks do.
3. Optional: separate opt-in for hook DLL vs base patch files.
4. Publish file hashes on website for independent verification.

## Verification

- Patcher shows `dinput8.dll` with description before download/write.
- Post-patch manifest screen matches files on disk.

Cross-reference: PC-022 (Discord partial disclosure).
