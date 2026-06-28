# DT-011: No public breakdown of funds received by channel or period

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (public page review) |
| **Component** | Governance / fundraising disclosure |
| **CWE** | N/A (transparency) |

## Summary

Server Support states the operator is **“making open how much donations are covering server costs,”** but the page does **not** publish **how much has been received**, **through which channels**, or **over what time windows**. Primary channel observed: **Ko-fi** (`ko-fi.com/ascendanteq`). Donors supporting **maintenance and uptime** cannot verify **inflows vs stated runway** from official sources alone.

## Example

**Stated intent (Server Support, observed):**

> “To provide transparency, I'm making open how much donations are covering server costs.”

**Observed channels:**

| Channel | Role | Public totals? |
|---------|------|----------------|
| Ko-fi | Linked “Support on Ko-fi” CTA | **No** aggregate on Server Support |
| Server Support page | Runway months only | **No** dollar inflows shown to visitors |
| Discord `#server-support` | Policy repost | **No** running totals in pinned policy |

**Funding API** (powers runway) exposes `amount` and `monthlyRate` to the frontend JSON - **not rendered** as receipt transparency on the page (see **DT-022**).

**What donors reasonably want (not accusatory):**

- Rolling **30 / 90 day** Ko-fi support total (or range)
- **Last updated** timestamp for receipts
- Confirmation whether **any other channels** exist (PayPal, direct, etc.) - if none, say so

## Player scenario

**Context:** Player donates monthly expecting contributions fund **server operations**.

**What happens:**

1. They see a **runway months** number but not **community inflows**.
2. They cannot reconcile **their payments + others’** against the bar without trusting opaque backend math.
3. Good-faith operators lose an easy trust win; skeptics fill the gap with assumptions.

**Not an exploit:** Disclosure gap - **not** a claim of hidden wallets or embezzlement.

## Implications

- **Informed giving:** Transparency promise on the page is **partially fulfilled** (outflow metaphor via runway) without **inflow accounting**.
- **Community relations:** Recurring donors (e.g. auto-support tiers) may expect **periodic public summaries** - common on volunteer-run game servers.

Pairs with **DT-010**, **DT-022**, **KF-001**.

## Remediation

1. Add a **“Community support summary”** block on Server Support:
   - **Ko-fi - last 30 days:** $X (or range band)
   - **Ko-fi - last 12 months:** $X
   - **Other channels:** none listed / or itemized
   - **As of:** ISO date
2. Optional: **quarterly** pinned update in Discord linking to the same numbers.
3. Clarify relationship between **inflows**, **reserve balance**, and **runway months** (see **DT-010** formula).
4. If exact dollars are sensitive, publish **rounded bands** ($X - $Y) - better than silence.

## Verification

- Visitor can see **at least one channel’s received total** and **last update date** without Ko-fi login.
- Runway months can be traced to **published reserve + burn** figures on the same page.
