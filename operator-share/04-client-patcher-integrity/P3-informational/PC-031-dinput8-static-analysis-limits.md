# PC-031: `dinput8` - hook framework; no exfiltration found in static review

| Field | Value |
|-------|-------|
| **Priority** | P3 - Informational |
| **Status** | Confirmed (static analysis) - limits apply |
| **Component** | CDN `dinput8.dll` |
| **CWE** | N/A |

## Summary

Static review of deployed **`dinput8.dll`** confirms **MacroQuest / eq-core-dll** hook and detour capability (`WriteProcessMemory`, DirectInput proxy). **No credential-theft strings, no suspicious network imports, no exfiltration URLs** were found at import-table level.

Dynamic behavior (plugins, runtime network) was **not** fully exercised.

## Example

**Confirmed:** `InitializeMQ2Detours`, `MQ2Main.dll` path strings, 1,484 exports.

**Not found:** HTTP exfil endpoints, keylogger patterns.

**Note:** `MQ2Main.dll` was not present in install directory at analysis time - core hook DLL still loads detour infrastructure.

## Attack vector

**N/A** - assessment limit note.

## Implications

- Supports **transparency / rules** framing (PC-002), not **malware** framing.
- Runtime plugin load could change behavior - not assessed here.

## Remediation

Operator documents purpose and publishes hash; optional dynamic review if disputed.

## Verification

Third-party static analysis agrees; optional sandbox run for network telemetry.
