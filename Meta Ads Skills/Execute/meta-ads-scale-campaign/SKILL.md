---
name: meta-ads-scale-campaign
description: "[Didoo AI] Guides Meta Ads campaign scaling — increases budget, expands audiences, replicates to new geos. Use when a testing campaign has exited Learning Phase (~50+ results in the past 7 days, CPL at or below target, stable metrics across 3–5 days) and you want to scale it. Not for campaigns still in testing phase."
---

# Meta Ads Scale Campaign

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching campaign status and insights |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
Loaded when a testing campaign has proven results and you want to scale it — increase spend, expand audience, or replicate to new geos. Not for campaigns still in testing phase.

---

## Prerequisites
Before scaling, confirm:
1. Testing phase is complete — campaign has generated ~50+ results in the past 7 days and exited Learning Phase (not just 50+ lifetime results)
2. Cost per result is at or below target CPL/CPA
3. Data is stable — metrics are consistent across at least 3–5 days

If any of the above are not confirmed, do not scale — return to meta-ads-analysis + meta-ads-recommendation to fix problems first.

---

## Step 1: Confirm Scale Readiness
Check the following signals:

**Must have:**
- Learning Phase has exited — need ~50+ results in the past 7 days (per adset for ABO, or per campaign for CBO). A campaign with 50+ lifetime results but only 2 results this week has NOT exited Learning Phase.
- CPL/CPA at or below target (or acceptable for the business)
- Delivery is consistent — not frequently limited by budget or audience

**Should have:**
- Frequency below 3 (if above, scale will hit fatigue faster)
- CTR stable and acceptable (not declining trend)
- LPV rate healthy (> 70% for e-commerce, > 50% for lead gen)

If these signals are not confirmed, return to meta-ads-analysis + meta-ads-recommendation to fix problems first.

---

## Step 2: Define Scale Strategy

### Strategy A: Increase Budget on Winner Campaign
When: CPL is stable and below target, delivery is near cap
- Increase budget by max 20% per adjustment
- Wait 2–3 days between adjustments to let Meta re-optimize
- If CPL starts rising after budget increase → stop, return to optimization

### Strategy B: Replicate to New Audience
When: Current audience is getting fatigued (frequency > 3) or you want to test new segments
- Duplicate the winning campaign (see meta-ads-publisher → Duplicate / Clone Campaign)
- Modify one dimension: new audience, new geo, or new placement
- Keep the same winning creative — creative is proven, not the variable

### Strategy C: Replicate to New Geo
When: Campaign is working in one market and you want to expand
- Duplicate campaign with same structure
- Change only the geo targeting
- Keep same audience parameters where possible
- New geo = new audience data → expect new learning phase

### Strategy D: Expand Audience Within Campaign
When: Targeting is too narrow and limiting volume
- Add new interest layers or lookalike audiences
- Do NOT broad-target — define the expansion clearly
- Monitor closely after expansion — may change CPL

---

## Step 3: Execute Scaling in Meta Ads Manager
**For budget increases:**
1. Go to the ad set in Meta Ads Manager
2. Change daily budget — increase by 20% max
3. Wait 2–3 days before next adjustment
4. Monitor CPL closely — if it rises > 15%, pause the increase

**For campaign duplication:**
1. Use meta-ads-publisher → Duplicate / Clone Campaign
2. Create PAUSED, review before activating
3. Confirm targeting/budget/geos are correct for new context
4. Activate and monitor

---

## Step 4: Monitor Post-Scale
Post-scale, run meta-ads-healthcheck or meta-ads-analysis daily for 3–5 days:

**Green signals:**
- CPL stable or improving
- Frequency still below 3
- Spend increasing proportionally to budget increase
- Results volume growing

**Red signals — stop and reassess:**
- CPL rising > 15% after budget increase
- Frequency > 3 and climbing
- Spend not increasing despite budget increase (delivery problem)
- Learning phase re-triggered

---

## Constraints
- Never scale a campaign still in Learning phase — wait for it to complete
- Never scale a campaign with CPL significantly above target — fix the problem first
- Budget increases: max 20% per adjustment, 2–3 days between changes
- When scaling to new geos: expect new learning phase — don't judge too early
- If 2+ scaling attempts fail: return to meta-ads-recommendation for structural diagnosis

---

## Scale Failure — Quick Reference
| Symptom | Most Likely Cause | Next Action |
|---------|-------------------|-------------|
| CPL rises > 15% after budget increase | Audience is saturated | Pause increase; rotate new creative or expand audience |
| Budget increased but spend stays flat | Delivery constrained — audience too narrow or bid too low | Check delivery preview; widen targeting or raise bid |
| CPL stable but frequency jumps above 4 | Audience fatigue | Rotate new creative immediately; expand to lookalike |
| Campaign re-enters Learning Phase after changes | Too many simultaneous changes | Pause; wait for re-stabilization; reduce changes next time |
| All metrics flat — no response to budget increase | Bid ceiling reached | Switch to lowest cost bidding; or campaign has hit its natural ceiling |
| Performance drops day after budget increase | Over-delivered early in day (pacing issue) | Check 3-day average, not single-day |
| CPL acceptable but volume too low to scale | Audience too narrow | Replicate to new audience (Strategy B), don't just increase budget |

**When to abandon a scaling attempt:**
- CPL rises > 30% despite 2 budget adjustments
- Delivery becomes erratic / spend unpredictable
- Frequency reaches 5+ across all adsets
- 2+ consecutive days with no spend increase despite budget increase

**When to keep trying:**
- CPL rises < 15% and stabilizes — this is normal as you scale
- Volume is growing proportionally to budget
- Frequency stays below 4
