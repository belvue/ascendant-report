# Ascendant Game Host Trust Acknowledgement

Use this block in forum posts, wiki pages, patcher first-run, donation page, or server rules. Operators may link to [player-hosting-notice-v1.md](player-hosting-notice-v1.md) for full detail.

---

## Full acknowledgement (checkbox / sign-off)

> **Ascendant Game Host Trust Acknowledgement**
>
> I understand that:
>
> 1. **Residential world server.** After login, my game client connects to **`eqemu.ascendanteq.com`**, which currently resolves to a **consumer residential internet** address (AT&T), not the OVH datacenter that hosts the Ascendant **website** and patch downloads.
>
> 2. **Split infrastructure.** Authentication may route through **`login.eqemulator.dev`** (separate notice) while gameplay runs on the **residential host** - two different networks and trust boundaries.
>
> 3. **Reliability.** Home-hosted worlds may experience **more downtime** (ISP outages, power, IP changes) than datacenter-hosted servers.
>
> 4. **Security exposure.** Investigators documented **internet-facing SSH** and a **public ATLAS admin interface** on the same IP as the game server, including an **unauthenticated read API** - a **misconfiguration**, not proof the server is already compromised.
>
> 5. **Donations.** For funding appeals, runway, and Ko-fi transparency, see the [donation notice](../03-transparency-and-accountability/player-donation-notice-v1.md). **Hosting:** the live game runs on home broadband; website/CDN use cloud hosting.
>
> 6. **Risk scope.** This acknowledgement is about **transparency and informed choice**. It does not assert the operator has defrauded donors or intentionally harmed players - only that the **observed architecture** differs from what many players assume.
>
> ☐ I have read and understand the above. I choose to continue playing on Ascendant with this hosting model.

---

## Short-form (Discord pin / social)

> Ascendant’s **live game world** runs on **residential AT&T** (`eqemu.ascendanteq.com`), not OVH - while the **website/patcher** stay on OVH. Same IP exposes **SSH** and a **public ATLAS admin UI** (misconfiguration confirmed Jun 2026). Login path is separate: [link to login notice]. Details: [link to player-hosting-notice-v1.md]

---

## Operator implementation checklist

- [ ] Donation / Ko-fi: use [03 donation acknowledgement](../03-transparency-and-accountability/player-donation-acknowledgement-block.md) + hosting topology link
- [ ] Patcher or launcher: “World host: residential - see [link]” persistent indicator
- [ ] Server rules or `#helpful-info` pin with full acknowledgement + remediation status for ATLAS/SSH
- [ ] Versioned URL (`…/hosting-notice/v1`) with changelog when text changes
