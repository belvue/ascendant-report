# 14d - Anti-cheat layers (Ascendant fork reference)

Companion to the speed-hacker notes in [14-reporter-account](14-reporter-account.md). Source tree: [Ascendant-EQ-Emu/Ascendant-Server](https://github.com/Ascendant-EQ-Emu/Ascendant-Server) on GitHub (public fork; paths below use that repo).

There are **two anti-cheat layers** - one Straps-authored (quest Perl), one EQEmu core C++ (where the **1.5×** speed threshold lives).

---

## 1. Straps quest anti-warp (Profusion-adapted)

| File | Role |
|------|------|
| [`server/quests/plugins/ascendant_antiwarp.pl`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/server/quests/plugins/ascendant_antiwarp.pl) | Main logic - Author: Straps (adapted from Profusion) |
| [`server/quests/plugins/ascendant_antiwarp_readme.md`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/server/quests/plugins/ascendant_antiwarp_readme.md) | Setup/docs |
| [`server/quests/global/global_player.pl`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/server/quests/global/global_player.pl) | Wiring (Straps header on file) |

How it works: every 1s, compare 2D distance moved vs **150 units/second**; if over threshold, `MovePCInstance` snaps the player back. Whitelists teleport spell IDs and exempt zones (Bazaar, Guild Lobby, PoK, etc.). **No logging, staff alert, or persistent flag** in the plugin source - snap-back only, unlike EQEmu `LogCheat` / `POSSIBLE_HACK` events.

Important: in `global_player.pl`, start/stop are commented out:

```perl
# Start anti-warp system
#plugin::StartAntiWarp($client);
```

Timer handler and movement-spell grace periods are still wired (`HandleAntiWarpTimer`, `HandleMovementSpell` on `EVENT_CAST`), so the plugin may be **partially disabled** unless something else calls `StartAntiWarp`.

---

## 2. EQEmu `cheat_manager` - speed / warp detection (the 1.5× line)

Stock EQEmu movement cheat code in the Ascendant fork:

| File | Role |
|------|------|
| [`code/zone/cheat_manager.cpp`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/code/zone/cheat_manager.cpp) | Speed/warp math + `CheatDetected()` |
| [`code/zone/cheat_manager.h`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/code/zone/cheat_manager.h) | `MQWarp`, `MQWarpLight`, `MQZone`, etc. |
| [`code/common/ruletypes.h`](https://github.com/Ascendant-EQ-Emu/Ascendant-Server/blob/main/code/common/ruletypes.h) | `[Cheat]` rules (`EnableMQWarpDetector`, etc.) |

The **1.5× threshold** (referenced in public Discord re a speed report; player handle redacted as **`Player_1`**):

```cpp
else if (!GetExemptStatus(Port)) {
  if (estimated_speed > (run_speed * 1.5)) {
    CheatDetected(MQWarp, glm::vec3(m_target->GetX(), m_target->GetY(), m_target->GetZ()));
    ...
  } else {
    CheatDetected(MQWarpLight, glm::vec3(m_target->GetX(), m_target->GetY(), m_target->GetZ()));
  }
}
```

- `estimated_speed > run_speed × 1.5` → full `MQWarp` detection (log + `POSSIBLE_HACK` player event).
- Between normal and 1.5× → `MQWarpLight` (lighter flag).
- Called from `client_packet.cpp` on position updates (`MovementCheck`) and movement history packets.

Cheat rules (tunable in DB `rules` / `#rules`):

```cpp
RULE_CATEGORY(Cheat)
RULE_REAL(Cheat, MQWarpDetectionDistanceFactor, 9.0, ...)
RULE_BOOL(Cheat, EnableMQWarpDetector, true, ...)
RULE_BOOL(Cheat, EnableMQZoneDetector, true, ...)
```

Also handles zone/gate/ghost/fast-mem cheats (`zoning.cpp`, `zone.cpp` → `MQZone`, `MQGate`, etc.).

Upstream diff baseline: [EQEmu/EQEmu](https://github.com/EQEmu/EQEmu) `code/zone/cheat_manager.cpp`.

---

## Quick map

| Concern | Where |
|---------|--------|
| Speed hack (1.4 vs 1.5) | `cheat_manager.cpp` → `MovementCheck()` |
| Teleport snap-back (Straps) | `ascendant_antiwarp.pl` (no audit log; may be unwired) |
| Zone warp / illegal port | `cheat_manager.cpp` + `zoning.cpp` |
| Logs | `LogCheat(...)` + `player_event_logs` (`POSSIBLE_HACK`) |

---

Previous: [14-reporter-account](14-reporter-account.md) · [14c-may-5-ledger-audit](14c-may-5-ledger-audit.md)
