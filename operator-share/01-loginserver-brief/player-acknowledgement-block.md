# EQ Login Trust Acknowledgement

Use this block in forum posts, wiki pages, patcher first-run, or the web account-link flow. Link to [player-login-notice-v1.md](player-login-notice-v1.md) for full detail.

---

## Full acknowledgement (checkbox / sign-off)

> **EQ Login Trust Acknowledgement**
>
> I understand that:
>
> 1. **Third-party login.** Ascendant’s patcher configures my client to use **`login.eqemulator.dev`**, operated by the eqemulator.dev project owner (Straps), not the official EQEmu Foundation login at **`login.eqemulator.net`**.
>
> 2. **Credential handling.** When I log in or link an account on eqemulator.dev, my password is validated on infrastructure controlled by that operator. Password **hashes** may be stored in their database. Legacy accounts may be validated against `.net` via LSPX and then cached locally.
>
> 3. **Federation.** Approved federation peers may receive password hash copies as part of the platform’s documented sync design.
>
> 4. **Directory curation.** The server list I see after login may be filtered or trust-labeled by the eqemulator.dev operator.
>
> 5. **Alternatives.** I can manually set `eqhost.txt` to `login.eqemulator.net:5999` if I accept that Ascendant’s world may not accept sessions from the community login path (compatibility not guaranteed).
>
> 6. **Risk scope.** This acknowledgement is about **trust and transparency**. It does not assert that the operator has stolen credentials - only that the **architecture allows** operator access to authentication material.
>
> ☐ I have read and understand the above. I choose to continue using this login path.

---

## Short-form (Discord pin / social)

> Ascendant routes logins through **login.eqemulator.dev** (Straps-operated), not **login.eqemulator.net**. Passwords are validated/stored on that stack; hashes may sync to federation peers. Patcher UI doesn’t spell this out. Details: [link to player-login-notice-v1.md]

---

## Operator implementation checklist

- [ ] First-run patcher modal with full acknowledgement + “Learn more” link - see [04 patcher acknowledgement](../03-transparency-and-accountability/player-patcher-acknowledgement-block.md) for eqhost opt-in/out
- [ ] Web `link-loginserver` page: mandatory checkbox before submit (LS-014)
- [ ] Persistent “Login path: eqemulator.dev” indicator in launcher or patch notes
- [ ] Versioned URL (`…/trust-notice/v1`) with changelog when text changes
