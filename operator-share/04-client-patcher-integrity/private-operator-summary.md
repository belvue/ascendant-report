# Private operator summary - Ascendant client patcher & supply chain

**Date:** 2026-06-13  
**Audience:** Ascendant operator (private review)  
**Package:** [04-client-patcher-integrity/](.) - **8 findings** (security & supply chain only)  

---

## Purpose

Technical review of **what the patcher delivers over the network** and **what it modifies on disk** - HTTP CDN, `dinput8.dll`, autoPatch overwrite, remote CDN control, eqhost mechanism (PC-010).

**Disclosure UX** (consent modal, file list UI, post-patch summary) is in **[03-transparency-and-accountability](../03-transparency-and-accountability/README.md)**.

---

## Executive summary

| Priority | Count | Action |
|----------|-------|--------|
| **P0 - Immediate** | 2 | HTTPS CDN + disclose `dinput8` purpose (also PC-012 in 03) |
| **P1 - High** | 1 | eqhost CDN mechanism - align with LS-015 |
| **P2 - Medium** | 2 | autoPatch behavior + CDN swap policy |
| **P3 - Informational** | 3 | Static limits / positive notes |

**Top actions:**

1. **PC-001** - Move patch CDN to HTTPS (or sign manifests).
2. **PC-002** - Document `dinput8` in UI (see also 03 PC-012).
3. **PC-010** - Cross-ref LS-015 / 03 PC-011 for player consent at eqhost write.

---

## Suggested response

1. Review **PC-xxx** in [README.md](README.md).
2. Complete [operator-response.md](../operator-response.md) section **04**.

---

## Contact

_(Operator fills channel in response template.)_
