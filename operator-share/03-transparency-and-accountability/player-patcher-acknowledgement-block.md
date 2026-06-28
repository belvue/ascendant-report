# Ascendant Patcher - Client & Login Acknowledgement

Embed **before the patcher writes or updates `eqhost.txt`** (first run and when CDN eqhost changes). Link to [player-client-notice-v1.md](../04-client-patcher-integrity/player-client-notice-v1.md) and [01 login notice](../01-loginserver-brief/player-login-notice-v1.md).

---

## Full acknowledgement (checkbox before patch writes eqhost)

> **Ascendant Patcher - Client Changes Acknowledgement**
>
> Before patching, I understand the patcher may install or update:
>
> 1. **`eqhost.txt`** - sets my login server. Ascendant currently uses **`login.eqemulator.dev:5999`**, not the official community login at **`login.eqemulator.net`**. *(See login trust notice.)*
>
> 2. **`dinput8.dll`** - a **MacroQuest-class game hook DLL**, not the standard Windows DirectInput library. It can modify game behavior at launch.
>
> 3. **`eqgame.exe`** - a **modified** EverQuest client provided by the operator.
>
> 4. **Auto-patch** - with default settings, future patches may **overwrite** my `eqhost.txt` and other files when the operator updates their CDN.
>
> 5. **Downloads** - patch files are fetched from the operator CDN (currently **HTTP**).
>
> 6. **This is about transparency**, not an accusation that the patcher steals passwords. Passwords are validated on the login server named in `eqhost.txt`.
>
> **My choice (operator implements one):**
>
> - ☐ **Accept** - Apply Ascendant patch files including `eqhost.txt` pointing to `login.eqemulator.dev`
> - ☐ **Patch only** - Update other files but **do not change** my current `eqhost.txt`
> - ☐ **Decline** - Do not patch; I will manage files manually (`autoPatch: false`)
>
> ☐ I have read and understand the above.

---

## Short-form (patch notes / Discord pin)

> Ascendant patcher silently updates **`eqhost.txt`** (→ `login.eqemulator.dev`), **`dinput8.dll`** (MacroQuest-class hook), and a custom **`eqgame.exe`**. No in-patcher consent today. Details: [player-client-notice-v1.md](../04-client-patcher-integrity/player-client-notice-v1.md)

---

## Operator implementation checklist

- [ ] Modal **before first `eqhost.txt` write** with choices above (PC-011)
- [ ] File list UI: name, purpose, hash for each manifest entry (PC-012)
- [ ] Post-patch summary before launching game (PC-023)
- [ ] HTTPS patch CDN (PC-001)
- [ ] Mirror this block in patcher - not Discord-only (PC-022)

---

## Cross-references

- Login trust checkbox: [01-loginserver-brief/player-acknowledgement-block.md](../01-loginserver-brief/player-acknowledgement-block.md)
- Findings: PC-010, PC-011, PC-002
