# 14b - Preliminary RCA: Stable Hand Bridle Exchange

**Subject:** Preliminary RCA: Stable Hand Bridle Exchange Platinum Generation Issue

## Summary

I am providing a preliminary root cause analysis regarding a platinum-generation issue identified in the stable hand bridle exchange flow. Based on current observations, certain bridle exchange paths appear to return platinum bags with a value significantly higher than the expected item/vendor value. This creates a repeatable currency-generation path with potential economy-wide impact.

## Confirmed Behavior

The issue appears to involve specific stable hand interactions tied to bridle color/type. Observed behavior includes:

- The Amondson stable hand closest to the vendor appears to accept Tan Chain Bridles.
- A stable hand approximately three positions down appears to accept Black Chain Bridles.
- Tan Chain Bridles with an approximate vendor value of 5,000 platinum appear to return a platinum bag worth approximately 78,000 platinum.
- White Chain Bridles dropped from Seru do not appear to be accepted in the same exchange path.
- Bridles from Sanctus Seru mini encounters also appear to return platinum bags.

This suggests the issue may not be isolated to a single bridle item and may instead involve multiple stable hand exchange paths or reward-value mappings.

## Unconfirmed Scope

I have not confirmed whether similar behavior exists for:

- Ssra Commander bridle drops
- Other Luclin-era bridle sources
- Additional stable hand NPCs
- Other bridle color/type variants

Given the observed variance between accepted and rejected bridle types, I would recommend reviewing all bridle-related exchange scripts and reward mappings rather than only the confirmed cases.

## Observed Economy Indicators

Following discovery of this issue, I noticed several market indicators that may suggest the server economy has already been affected:

- Shards of Ascendant Power appear to have nearly doubled in price.
- Illegible tomes have become significantly harder to find.
- Marks of Ascendant Power appear to be largely unavailable in the Bazaar.
- Several high-value Bazaar items appear to have increased materially in price.
- Wealth leaderboard rankings appear to have shifted in a way that may warrant review.

These observations are not definitive proof of prior exploit usage, but they indicate that a broader economic audit may be appropriate.

## Leaderboard / Wealth Distribution Observations

Based on current leaderboard visibility, approximately 550,000 platinum appears sufficient to reach around rank #10. Based on extrapolation, rank #3 may be approximately 4.5 million platinum. I also observed several low-level or mule-like characters rising in global wealth rank. I cannot confirm whether those characters are connected to this issue, benefiting from others using it, or participating in unrelated market activity. However, given the timing and the known exploit path, these movements may be worth reviewing against recent platinum-generation logs.

For additional context, another player previously indicated that approximately 500,000 platinum placed them around rank #8 last week, which appears directionally consistent with the current leaderboard range.

## Testing Context

My testing was intended to determine:

- Whether the issue applied to one bridle type or multiple variants.
- Whether different stable hand NPCs behaved differently.
- Whether the reward discrepancy was isolated or systemic.
- Whether the current economy and leaderboard suggested prior usage by others.
- Whether parcel-held platinum impacted leaderboard accounting.

I recognize that I tested the issue far beyond what was appropriate. Once the issue was confirmed, I should have stopped immediately and reported it. I also moved platinum through parcels while attempting to understand leaderboard visibility. I understand that this looks like concealment and that I should not have moved the currency at all. The platinum was not spent and was left in parcels so it could be traced, removed, or rolled back cleanly.

## Current Recoverability

From memory:

- Approximately 500,000 platinum was parceled to several boxed characters.
- Approximately 1,000,000 platinum remains in my main character's parcel.
- Exact parcel destinations and amounts should be verifiable through logs.

I will not claim, move, spend, trade, or otherwise alter any related platinum unless instructed by staff.

## Recommended Review Areas

I recommend reviewing the following:

- Stable hand bridle exchange scripts.
- Item IDs and reward mappings for Tan Chain, Black Chain, White Chain, and related bridle variants.
- Platinum bag reward values.
- Vendor value versus exchange return value.
- Recent platinum bag generation logs.
- Parcel logs associated with my characters/accounts.
- Bazaar price movement for Shards of Ascendant Power, Marks of Ascendant Power, illegible tomes, and other high-value items.
- Wealth leaderboard movement over the past week.
- Low-level characters with sudden large platinum increases.
- Repeat bridle exchange activity across all accounts.

## Recommended Immediate Mitigation

Suggested short-term actions:

- Temporarily disable affected stable hand bridle exchanges.
- Audit all bridle-to-platinum-bag reward logic.
- Review recent platinum bag creation events.
- Trace and remove exploit-generated platinum.
- Review high-value Bazaar transactions funded by recently generated platinum.
- Consider temporarily suppressing or reviewing wealth leaderboard visibility if it can be used to benchmark exploit output.

## Closing

I acknowledge that my validation process went beyond what would be considered appropriate for responsible exploit reporting. Once the issue was confirmed, I should have stopped testing, preserved the state exactly as-is, and reported it immediately.

As context, I am currently on a work leave of absence and have been trying to avoid re-entering the same analytical/operational mindset in a game environment. However, my tendency to over-scope and over-validate issues clearly carried over here. That contributed to the excessive number of exchanges, multi-variant testing, parcel movement, and leaderboard/accounting checks. That context does not excuse the handling. I understand why the activity appears problematic and why staff would view it seriously.

My intent was to determine scope and potential economy impact, then report it privately, not to retain or benefit from the generated currency. I will fully cooperate with remediation and provide any character names, parcel destinations, approximate amounts, timestamps, or reproduction details needed to trace and remove the affected platinum. I will not take any further action involving the related currency unless instructed by staff.
