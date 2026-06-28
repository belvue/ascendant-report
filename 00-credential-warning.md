# 00 - Credential warning

**Read this before anything else.**

If you play on Ascendant EQ (or any server that patches you through `downloads.ascendanteq.com`), your EverQuest login traffic may route through **`login.eqemulator.dev`**, infrastructure operated by the same person who runs the server.

That is not eqemulator.org or eqemulator.net. It is a separate login stack under operator control.

When you log in, your client sends your username and password to whatever host `eqhost.txt` names. EQEmu loginservers store **password hashes**, not plaintext passwords, in normal operation. Because the operator runs that stack, they **control the login database**: hashes, backups, account resets, and federation copies to approved peers. That is standard loginserver design on operator-controlled infrastructure. It does **not** mean staff routinely see your password in cleartext. It **does** mean you are trusting their systems with credentials you type into the client.

One successful game login through the `.dev` path can **copy your account record into the operator login database** (same account id, password hash stored there). You do not need a separate signup. See [03-login-trust-boundary - LSPX](03-login-trust-boundary.md#lspx-first-dev-login-copies-your-net-account).

The patcher can rewrite `eqhost.txt` on every update cycle ([PC-010](operator-share/04-client-patcher-integrity/P1-high/PC-010-eqhost-login-redirect-via-cdn.md), [PC-020](operator-share/04-client-patcher-integrity/P2-medium/PC-020-autopatch-reapplies-cdn-files.md)). Manual edits back to `.net` may not stick.

Treat Ascendant credentials like credentials you would give a stranger who also runs your game world. Use a **unique password** not reused on `login.eqemulator.net` or anywhere else.

**This report is not legal advice.** It documents artifacts and trust boundaries so you can decide whether to keep playing, donate, or ask questions in public.

Next: [01-why-this-exists](01-why-this-exists.md)
