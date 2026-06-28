# 14c - May 5 ledger audit (reference)

Technical companion to [14-reporter-account](14-reporter-account.md). Straps posted a **"Belvue Cluster - Final Accounting"** embed on **2026-05-09** (screenshots in chapter 14). It labels income **since May 5** and assigns **59%** of exploit plat to **Harley Wynn gambling at 1,500pp per Lucky Coin**.

I (**Belvue**, report author; name used with my consent) dispute that breakdown. My read: **AA transmutes into Lucky Coins** on the cluster, not thousands of **1,500pp** Harley purchases. The **May 5** income bucket implies spending **May 8** bridle plat on **May 5** purchases. I lack confirmation I can travel through time; I suspect I cannot. Straps and staff are the **public face** of the service; their on-record Discord accounting is fair game. Other players on the cluster are **not** named here without consent (see tokens below).

---

## Open request to Ascendant staff

We do **not** have database access. The community should not have to guess whether a ban was backed by logs or by a Discord spreadsheet.

On **2026-05-09**, Straps published an **AI-generated "Final Accounting"** ledger that assigned **59%** of exploit plat to **1,543 Harley gambles at 1,500pp per coin**. That was not a SQL export. It was narrative packaging. **Do not answer this dispute with another generated table.** Publish **counts from `player_event_logs`** (or admit logging was off).

Copy-paste audit for **`Belvue`** (reporter-consented name) and, if you extend to the cluster, run the same by **`account_id`** internally without publishing other players' handles:

```sql
-- Window: Straps "since May 5" bucket vs May 8 bridle disclosure
-- Character: Belvue (public); use character_id for other alts internally

-- 1) Row counts by event type (publish these four integers in Discord)
SELECT pel.event_type_name, COUNT(*) AS row_count
FROM player_event_logs pel
JOIN character_data cd ON cd.id = pel.character_id
WHERE cd.name = 'Belvue'
  AND pel.created_at >= '2026-05-05 00:00:00'
  AND pel.created_at <  '2026-05-09 00:00:00'
  AND pel.event_type_name IN (
    'Merchant Purchase', 'AA Purchase', 'NPC Handin', 'Trader Purchase', 'Trader Sell'
  )
GROUP BY pel.event_type_name
ORDER BY row_count DESC;

-- 2) Harley merchant purchases on May 5 only (tests 1500pp NPC path)
SELECT COUNT(*) AS harley_merchant_buys_may5
FROM player_event_logs pel
JOIN character_data cd ON cd.id = pel.character_id
WHERE cd.name = 'Belvue'
  AND pel.event_type_name = 'Merchant Purchase'
  AND pel.created_at >= '2026-05-05 00:00:00'
  AND pel.created_at <  '2026-05-06 00:00:00'
  AND JSON_UNQUOTE(JSON_EXTRACT(pel.event_data, '$.npc_name')) LIKE '%Harley%';

-- 3) AA purchases on May 5 (tests AA transmute path)
SELECT COUNT(*) AS aa_purchase_rows_may5
FROM player_event_logs pel
JOIN character_data cd ON cd.id = pel.character_id
WHERE cd.name = 'Belvue'
  AND pel.event_type_name = 'AA Purchase'
  AND pel.created_at >= '2026-05-05 00:00:00'
  AND pel.created_at <  '2026-05-06 00:00:00';

-- 4) Bridle / stable-hand turn-ins on May 8+ (May 8 disclosure path)
SELECT COUNT(*) AS npc_handin_rows_may8_on
FROM player_event_logs pel
JOIN character_data cd ON cd.id = pel.character_id
WHERE cd.name = 'Belvue'
  AND pel.event_type_name = 'NPC Handin'
  AND pel.created_at >= '2026-05-08 00:00:00';
```

**What we are asking for publicly:** post the four result numbers (or a screenshot of the query output with other player names redacted). If **`harley_merchant_buys_may5` ≈ 1,543**, the 1,500pp gambling line holds. If **`aa_purchase_rows_may5`** dominates and Harley buys are near zero, the spreadsheet was wrong.

If `player_event_logs` was empty in May 2026, say so on the record. Banning off an unaudited ledger while SQL was one login away is the story.

---

## What I can and cannot prove from my side

| Source | Status |
|--------|--------|
| Straps Discord accounting embed | **Published** (chapter 14) |
| My memory of AA transmute path vs 1500pp gambling | **On record** in chapter 14; not a log export |
| Client `eqlog_*` for cluster characters | **Not available** - I did not have `/log` enabled on those alts during May 2026 |
| Staff `player_event_logs` / QueryServ | **Not accessible** to me; staff request above would settle it |

---

## Lucky Coin economics (why 1,500pp matters)

Straps priced **1,543 coins × 1,500pp ≈ 2.31M pp** of gambling sink. Ascendant's own Harley feature documents **3 AA per coin** as the normal path; the **1,500pp NPC buy** is the inefficient option players avoid.

Public market asks in the same era were **~530 - 1,400pp** per coin on Discord `#auction` and **~500 - 600pp** on bazaar snapshots, not 1,500pp unless someone deliberately paid the NPC premium. Detail and source table: [lucky-coin-market-reference](data/lucky-coin-market-reference.md).

**Market sweep check:** Straps blamed the cluster for **economy-scale plat** while assigning coin acquisition to the **worst possible price**. If we really held that much plat and needed **1,543 coins**, we would have **cleared sub-1k bazaar and `#auction` listings first** (saving roughly **half** the plat vs NPC). Public sellers kept posting **530 - 700pp** stacks through May; we did not buy them out. Server-wide Lucky Coin totals stayed visible on roster later ([09b](09b-botwatch-capabilities.md)). That fits **AA transmutes**, not a fabricated **1,500pp × 1,543** binge. See [Market sweep vs NPC premium](data/lucky-coin-market-reference.md#market-sweep-vs-npc-premium-common-sense-check).

Auction pricing for that window is in [lucky-coin-market-reference](data/lucky-coin-market-reference.md) (tokenized sellers; no per-player trade CSV in the public package).

---

## Harley Wynn: log strings that distinguish paths

Standard EQ client lines (any server using the same NPC script):

**AA tokens (0 plat):**

```text
You transmute 3 AA points into a Lucky Coin
```

**Plat spent at Harley (1,500pp path, if it existed):**

```text
Harly slips her hand into your coin purse
```

**Math check:** If Straps claims `N × 1500pp` gambling, you need **N** `Harly slips...` lines (or equivalent merchant plat-out events). If you only see transmutes, cost is **`N × 3 AA`**, not plat.

If anyone still holds a May 2026 client eqlog for a cluster alt:

```bash
rg -i "You transmute 3 AA points into a Lucky Coin" eqlog_Reporter_Alt_1_ascendant.txt
rg -i "Harly slips her hand into your coin purse" eqlog_Reporter_Alt_1_ascendant.txt
```

---

## Full SQL templates (detail rows)

Event-type reference: [EQEmu player event logging](https://docs.eqemu.dev/server/logging-system/player-event-logging/).

Plat timeline (**Belvue**):

```sql
SELECT pel.created_at, pel.event_type_name, pel.zone_id, pel.event_data
FROM player_event_logs pel
JOIN character_data cd ON cd.id = pel.character_id
WHERE cd.name = 'Belvue'
  AND pel.created_at >= '2026-05-05 00:00:00'
  AND pel.created_at <  '2026-05-09 00:00:00'
  AND pel.event_type_id IN (15, 16, 22, 27, 35, 38, 39)
ORDER BY pel.created_at;
```

Harley merchant lines (May 5):

```sql
SELECT pel.created_at,
  JSON_UNQUOTE(JSON_EXTRACT(pel.event_data, '$.npc_name')) AS npc,
  JSON_EXTRACT(pel.event_data, '$.platinum') AS plat_out
FROM player_event_logs pel
JOIN character_data cd ON cd.id = pel.character_id
WHERE cd.name = 'Belvue'
  AND pel.event_type_name = 'Merchant Purchase'
  AND pel.created_at BETWEEN '2026-05-05' AND '2026-05-06'
ORDER BY pel.created_at;
```

Other cluster characters: run by internal `character_id`; public exports use **`Reporter_Alt_1`**, **`Reporter_Alt_2`**, etc.

---

## What clears the May 5 vs May 8 story

| Straps claim | What logs should show | What AA-token path shows |
|--------------|----------------------|---------------------------|
| ~1,543 gambles × 1500pp | ~1,543 Harley **Merchant Purchase** rows on May 5 | **AA Purchase** rows; ~0 Harley plat buys |
| Plat from bridle bug | **NPC Handin** spikes on **May 8** | May 5 gambling bucket unrelated to May 8 disclosure |

---

## Other paths (no DB)

| Path | Realistic for May 2026? |
|------|-------------------------|
| Staff SQL counts (above) | **Best** |
| `https://ascendanteq.com/leaderboard` | Current tops only; no historical May API found |
| Portal `/api/characters/{name}` | Snapshot, not ledger ([04b](04b-operational-security.md)) |
| Wayback on leaderboard SPA | Unknown; not captured in this package |

---

Previous: [14-reporter-account](14-reporter-account.md) · [15-closing-statements](15-closing-statements.md)
