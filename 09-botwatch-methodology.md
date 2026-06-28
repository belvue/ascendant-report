# 09 - BotWatch methodology

## What BotWatch is

BotWatch is an independent monitoring stack (presence sampling, AFK scoring, Ko-fi feed sync, **RMTkr** donation/market ledger, investigation queue) run by this report's author on Ascendant telemetry. It is not affiliated with Ascendant staff.

## Sources and scope

Full impact framing: [02-executive-summary - Outside observer](02-executive-summary.md#outside-observer).

| Source | What it rebuilt |
|--------|-----------------|
| `/who` + inventory sampling | Zone time, camp streaks, bag pressure, loot deltas |
| Public Ko-fi HTML | Feed rows, selective hides, runway math |
| Discord exports | Staff policy quotes, dismissals, hosting admissions |
| Client eqlogs | Economy paths, gambling vs transmute disputes |
| Patcher / CDN / eqhost | Login routing, directory curation |
| Public forum / RMT boards | Plat pricing, operator conduct threads |

No staff SQL, zone console, or insider tickets were required. BotWatch schedules the above and adds AFK scoring, signal rules, and investigation queue UI. **Activity timeline timestamps are UTC (GMT+0).** Known sampler outages appear as empty zone lanes; those gaps are tracking holes, not labeled offline events unless stated otherwise in the chapter crop.

Visual proof (masked crops + Chardok pair): [09b-botwatch-capabilities](09b-botwatch-capabilities.md).

## Ko-fi feed sync

Job `ascendant-donation-feed-sync` polls the public Ko-fi HTML feed every ~15-30 minutes.

When rows disappear from the public page, BotWatch **does not delete** DB rows. It flags `hidden_from_kofi_feed` and logs purge cycles. That is how we know the Jun 15 empty feed still had 15 stored rows internally.

The same sync backs the **RMTkr Ledger** UI ([09b-botwatch-capabilities](09b-botwatch-capabilities.md#rmtkr-donation-ledger-and-market-intel)).

## AFK / presence

Jun 27 snapshot: 192,801 presence samples; 9,013 signal rows with AFK score ≥ 65; 151 watched characters with max score ≥ 75.

The watch list is curated, not full roster. Numbers are lower bounds on automation suspicion, not proof of botting alone.

## Investigator cost

Context for **this report's build effort**, not a claim against Ascendant's billing. From [data/investigator-cost.json](data/investigator-cost.json):

| Item | Value |
|------|------:|
| Period | 2026-05-08 to 2026-06-27 |
| Hours logged | 612 |
| Opportunity cost (@ $220k/yr) | ~$65,000 |
| OVH infra (author lab) | $15-80 / mo range |
| Author belvue-vps (OVH web edge only) | **~$50/yr** (~$4/mo amortized); see [11-hosting-cost-gap](11-hosting-cost-gap.md#investigator-baseline-same-class-of-ovh-web-edge) |
| Cursor API retail equivalent (estimate) | **~$24,766** |
| Cursor on-demand billed (CSV) | $20.46 |
| Cursor actual spend (author) | **Ultra ~$200/mo** + on-demand; **~low hundreds USD** for May 8 - Jun 27 (not the retail-equivalent figure) |
| Included-plan tokens (May 8-Jun 27) | ~3.09B |

**Method:** On-demand `Cost` column plus Included rows valued at **$8 / 1M tokens** (retail-equivalent; not actual invoice). Parsed output: [data/investigator-cost.json](data/investigator-cost.json).

The retail-equivalent figure (~$24.8k on ~3.09B tokens) measures compute consumed, not the invoice. Actual Cursor spend for the window was **low hundreds USD** (Ultra subscription + $20.46 on-demand).

**Model families used** (investigation + engineering; not an exhaustive CSV slug list): Claude (Opus, Sonnet, Haiku), Composer, Gemini (Flash, Pro), GPT / Codex, GLM, Kimi K2, DeepSeek, Gemma, Llava, Qwen / Qwen-VL. BotWatch runtime assessment (when enabled): `gpt-4.1-mini`. Offline: Ollama, Hugging Face.

This investigation is expensive because operators make transparency expensive. Feed hides, login moves, and forum spin each cost hours to unwind.

Loginserver trust-boundary tests (Docker lab, LSPX, federation): [03b-loginserver-lab-methodology](03b-loginserver-lab-methodology.md).

Visual tour of roster, timelines, signals, and loot reconstruction: [09b-botwatch-capabilities](09b-botwatch-capabilities.md).

Next: [09b-botwatch-capabilities](09b-botwatch-capabilities.md) · then [10-witness-summary](10-witness-summary.md)
