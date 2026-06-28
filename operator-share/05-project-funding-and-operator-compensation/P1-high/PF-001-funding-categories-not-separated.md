# PF-001: Funding categories are not separated in public appeals

| Field | Value |
|-------|-------|
| **Priority** | P1 - High |
| **Status** | Confirmed (Server Support + Ko-fi copy) |
| **Component** | Governance / fundraising messaging |
| **CWE** | N/A (disclosure) |

## Summary

Public funding asks blend **hosting, operating, maintenance, tooling, and development** into a single narrative and a **single runway divisor**. Contributors cannot tell how much of each dollar is intended for **cloud invoices**, **residential game uptime**, **software/services**, **hardware replacement**, **reserves**, or **people-time**. Blended messaging is not inherently improper - the gap is **category visibility**.

## Example

**Server Support / Ko-fi language (observed):**

> “…help cover the ongoing costs of running Ascendant EQ - things like **hosting**, operating, maintenance, tooling, and development.”

**Infrastructure reality (probe-confirmed - see RH-011):**

| Layer | Provider type | Publicly labeled in appeals? |
|-------|---------------|------------------------------|
| Website / CDN / downloads | Cloud (OVH US) | Implied by “hosting” |
| Live game world | Residential ISP | **Not separated** |
| Federation login | Cloud (OVH Canada) | Not in donation copy |

**Runway widget:** One `monthlyRate` and one `months` value - no stacked categories (**DT-010**).

## Player scenario

**Context:** Contributor donates to “keep the server online.”

**What happens:**

1. Copy references **hosting** without topology.
2. They cannot tell whether the payment primarily offsets **cloud bills**, **home broadband/power**, or **volunteer labor** framed as project cost.
3. Runway “Healthy” label may read as **datacenter uptime** funding.

**Not claimed:** Misuse of funds - **allocation clarity gap**.

## Implications

- **Informed giving:** Contributors cannot prioritize what they intend to support.
- **Pairs with DT-001** (hosting honesty), **PF-002**, **PF-006**.

## Remediation

1. Publish a **cost-category table** on Server Support (ranges or bands acceptable).
2. Map each category to **what donations may fund** vs **operator-covered** costs.
3. Align Ko-fi page description with the same categories on the same date.
4. Optionally show **stacked runway** segments instead of one blended bar.

## Verification

- New donor can name **at least three** funding categories from official text alone.
- Ko-fi and Server Support use **matching** category language.
