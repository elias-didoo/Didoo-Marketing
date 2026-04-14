---
name: meta-ads-fundamentals
description: "[Didoo AI] Core knowledge that underpins all Meta Ads decision-making — the Meta Auction, Pacing, Breakdown Effect, CBO vs ABO, Learning Phase, Auction Overlap, and Ad Relevance Diagnostics. Reference when explaining campaign behavior, troubleshooting anomalies, or justifying optimization decisions. For skill routing, see meta-ads-guide."
---

# Meta Ads Fundamentals

## The Meta Auction — Total Value Formula
Meta's ad auction is not a simple bid-vs-bid system. It uses a Total Value formula:

**Total Value = Bid × pAction + Quality Score**

Where:
- Bid: Your actual bid or the max you're willing to pay
- pAction: Probability that the right person will take the desired action (click, conversion, etc.)
- Quality Score: Meta's assessment of your ad's relevance and quality vs. competitors

**What this means in practice:**
- A high bid with a low-quality ad may lose to a lower bid with a highly relevant ad
- Two advertisers with identical bids can get different results based on their ad quality
- Optimization should address both bid strategy and creative relevance simultaneously

---

## Pacing — Why Your Budget Doesn't Spend Evenly
Meta's delivery system uses pacing to manage when and how your ads compete throughout the day.

**How pacing works:**
- Meta reserves some budget for later in the day to capture cheaper or higher-converting opportunities
- Early-day spending depends on how many high-value opportunities exist at that moment
- This is why you may see more spend at certain times and less at others — it's intentional

**What this means for your campaigns:**
- Don't panic if ads aren't spending in the first few hours — pacing is normal
- Campaigns may show irregular intra-day spend patterns — this is not a problem
- Focus on whether the daily/weekly budget achieves the expected results, not on hourly spend patterns

---

## The Breakdown Effect — Why High-CPA Segments Sometimes Get More Budget
Meta optimizes for marginal CPA (the cost of the next result), not for average CPA across all results. When looking at breakdown data (by age, placement, geo), you may see that a segment with higher average CPA is receiving more budget.

If that higher-CPA segment has a slightly higher marginal cost but also a higher probability of converting on the next unit of spend, Meta's algorithm may decide it's worth the extra investment to protect total campaign efficiency.

**What this means for you:**
- Do not make decisions based on average CPA in breakdown reports alone
- A segment with higher average CPA may still be generating efficient marginal results
- Only intervene if the overall campaign CPA is above target, not based on segment-level average CPA

---

## Campaign Budget Optimization (CBO) vs Adset Budget Optimization (ABO)

### How CBO Works
- Budget is set at the **campaign level**
- Meta distributes budget across all adsets automatically to maximize results
- Meta finds the best-performing audience combinations in real time

### How ABO Works
- Budget is set at the **adset level**
- You control exactly how much goes to each audience segment

> **CBO vs ABO decision table:** The full decision table (which structure to use for each scenario, with bidding strategy) is in **meta-ads-strategy → Step 4**. This section explains the mechanism only.

---

## Learning Phase — What It Is and Why It Matters
After any significant change to a campaign (new ad, targeting change, budget adjustment), Meta enters a Learning Phase.

**What happens during Learning Phase:**
- Meta's algorithm is actively testing different auction strategies
- Results are unstable — don't judge performance during this period
- The system is looking for the lowest-cost combination of audience, placement, and creative

**How to tell if you're in Learning Phase:**
- "Learning" or "Learning Limited" status appears in Ads Manager
- Results fluctuate significantly day to day

**How to exit Learning Phase:**
- Need ~50 results per week per adset to complete learning
- Smaller budgets = longer learning periods
- Larger budgets get through learning faster but cost more during testing

**What to do during Learning Phase:**
- Do not make changes — changing things resets the learning clock
- Wait at least 5–7 days and 50+ results before judging
- If stuck in Learning Phase beyond 7 days: increase budget OR simplify structure (fewer adsets)

**Key rule:** Budget must be realistic for the learning phase to complete efficiently. ~$10–15/day per adset minimum.

---

## Auction Overlap — When Multiple Ads Compete for the Same Person
When multiple ad sets in the same campaign share overlapping audiences, Meta excludes the lower-value ad from competing — preventing ads from entering auctions, ad sets from spending full budget, and achieving enough results to exit the learning phase.

**How to diagnose:**
1. Check Opportunity score in Account Overview
2. Look for multiple ad sets stuck in Learning Limited simultaneously
3. Use automated rules to detect and manage overlap

**How to fix:**
1. Combine similar ad sets — consolidates learning, faster stable results
2. Turn off overlapping ad sets — typically the learning-limited or lowest-result ones; move budget to the active ad set

Note: Separate Pages do not avoid overlap if the same ad account, campaign, or ad set shares audience or assets.

---

## Ad Relevance Diagnostics — What They Measure
Meta provides three relevance diagnostics that compare your ad to competitors targeting the same audience:

| Diagnostic | What it measures | Low ranking suggests |
|------------|------------------|----------------------|
| Quality Ranking | Perceived ad quality vs. competitors | Improve creative |
| Engagement Rate Ranking | Expected engagement vs. competitors | Test new angles, improve hook |
| Conversion Rate Ranking | Expected conversion vs. competitors with same optimization goal | Check landing page or audience-offer fit |

**Usage rules:**
- Requires 500+ impressions to be available — below that, diagnostics are not meaningful
- These are diagnostic signals only, not direct auction inputs
- When all three rankings are low simultaneously → strong audience-creative mismatch
- Quality Ranking is weighted most heavily in the auction

---

## Related Concepts
| Concept | Where to find details |
|---------|-----------------------|
| CBO vs ABO, bid strategy decisions | meta-ads-strategy Step 4 |
| Learning Phase behavior | meta-ads-scale-campaign Prerequisites |
| Auction Overlap diagnosis + action | meta-ads-recommendation Step 5 |
| Ad Relevance Diagnostics | meta-ads-analysis Step 5 |
| Lead gen LPV / CAPI | meta-ads-lead-gen-analysis |
