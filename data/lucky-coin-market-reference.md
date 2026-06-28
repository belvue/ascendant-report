# Lucky Coin market reference (public sources)

Context for [14c-may-5-ledger-audit](../14c-may-5-ledger-audit.md) May 5 ledger dispute. Straps assigned **59%** of **Belvue Cluster** exploit plat to **Harley Wynn gambling at 1,500pp per Lucky Coin** (1,543 coins in the Discord accounting embed).

Harley Wynn accepts coins two ways per Ascendant's own feature copy (Guild Lobby NPC):

| Source | Cost per coin |
|--------|---------------|
| **AA transmute** (`Manifest Experience: Lucky Coin`) | **3 unspent AA** (0 plat) |
| **Buy from Harley NPC** | **1,500 platinum** |

Players routinely **undercut the NPC** on bazaar and Discord `#auction` / OOC connector because 1,500pp is a bad deal at scale.

## Seller tokens

Quotes below are from Ascendant's public Discord OOC auction mirror. Player handles are **tokenized** (no consent on file). **Belvue** (reporter, consented) and staff names stay on-record; other player handles are tokenized.

| Token | Role in this table |
|-------|-------------------|
| `Auction_Seller_A` | High-volume `#auction` seller, Apr - May 2026 |
| `Auction_Seller_B` | `#auction` seller, ~550pp ask |
| `Auction_Seller_C` | `#auction` seller, ~530pp ask |
| `Auction_Seller_D` | `#auction` seller, ~1k ask |
| `Auction_Seller_E` | `#auction` seller, ~1.3k ask |
| `Auction_Seller_F` | `#auction` seller, ~1.4k ask |
| `Auction_Seller_G` | OOC seller, AA-or-plat offer |

## Public auction / bazaar ask prices (Apr - Jun 2026)

| When (UTC) | Seller | Ask | Implied pp/coin |
|------------|--------|-----|-----------------|
| 2026-04-27 | `Auction_Seller_A` | 75k per 100 stack | **~750** |
| 2026-05-02 | `Auction_Seller_A` | 700pp each | **700** |
| 2026-05-21 | `Auction_Seller_A` | 700pp each | **700** |
| Discord `#auction` | `Auction_Seller_B` | 550 each | **550** |
| Discord `#auction` | `Auction_Seller_C` | 530 each | **530** |
| Discord `#auction` | `Auction_Seller_D` | 1k each ("save 500plat") | **1,000** |
| Discord `#auction` | `Auction_Seller_E` | 1.3k each ("save 200pp") | **1,300** |
| Discord `#auction` | `Auction_Seller_F` | 1,400pp | **1,400** |
| Discord OOC | `Auction_Seller_G` | 3 AA **or** 1kpp | **0 plat or 1,000** |

## Bazaar snapshot band (Jun 10 - 11 2026)

Investigator bazaar captures (not shipped in public package body) show **Lucky Coin** trader listings mostly **500 - 600pp** per coin with occasional **1k - 5k** outlier stacks; seller handles omitted. No May 2026 bazaar CSV exists in the capture set. Jun prices still show the market **far below 1,500pp NPC**.

## Why this matters for the Straps ledger

If **1,543 coins** were acquired via:

| Path | Cost |
|------|------|
| Straps spreadsheet (NPC Harley) | **1,543 × 1,500pp = ~2.31M pp** |
| AA transmute (feature text) | **1,543 × 3 AA = ~4,629 AA** (0 plat) |
| Typical player market (~700pp) | **~1.08M pp** equivalent if bought secondhand |

Assigning the **1,500pp NPC path** to the whole bucket without SQL or eqlog proof reads as **worst-case plat burn**, not an audited fact. Staff can disprove or confirm with the counts in [14c-may-5-ledger-audit](../14c-may-5-ledger-audit.md#open-request-to-ascendant-staff).

## Market sweep vs NPC premium (common-sense check)

Straps framed the cluster as an **economy disaster**: millions of plat, **59%** routed through Lucky Coin gambling at the **1,500pp Harley price**. That story only works if we ignored every cheaper coin on the server.

Public `#auction` and bazaar asks in the same era were **~530 - 750pp** per coin (table above). Harley NPC is **1,500pp**. AA transmute is **0 plat**. If the cluster really held bridle-scale plat and needed coins to gamble, the rational move is:

1. **Transmute from AA** where possible (documented feature path).
2. **Buy every undercut listing** on bazaar and `#auction` before touching the NPC.
3. Only pay **1,500pp** for whatever the market could not supply.

At **~700pp** market, **1,543 coins** costs on the order of **~1.08M pp**, not **~2.31M pp**. At **~550pp**, closer to **~850k pp**. Either way, a player accused of **cratering the economy with plat** does not voluntarily pick the **most plat-inefficient** acquisition path for fifteen hundred coins unless the logs show they did.

We did **not** buy out the Lucky Coin market. Tokenized sellers kept posting **700pp**, **550pp**, and **530pp** stacks in public auction channels through May and June. BotWatch roster rollups later show **thousands of Lucky Coins** still on characters server-wide (economy column in [09b](../09b-botwatch-capabilities.md) crops). The supply was visible and cheap relative to Harley. That fits **AA transmutes and normal player trading**, not a secret **1,500pp × 1,543** NPC binge invented for a Discord ledger.

If staff believe otherwise, **`Trader Purchase` / bazaar rows** in `player_event_logs` for **Belvue** on May 5 should show a market sweep. Publish those counts with the [open staff request](../14c-may-5-ledger-audit.md#open-request-to-ascendant-staff).

## Note on trade CSVs

No per-seller trade CSV exports exist in the public package. Pricing for the May window was reconstructed from **public Discord auction lines** above (tokenized), not from a private spreadsheet.
