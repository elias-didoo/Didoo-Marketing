---
name: meta-ads-healthcheck
description: "[Didoo AI] Fast on-demand campaign status check — answers \"are my ads working?\" using Green/Yellow/Red thresholds. For daily routine monitoring, use meta-ads-daily-pulse instead."
---

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching campaign and adset data |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
When user wants a quick "are my ads working?" check — not a deep diagnostic, just a status update. Use before a meeting, or any time something feels off.

|  | meta-ads-healthcheck | meta-ads-daily-pulse |
|---|---|---|
| Primary question | "Is this campaign healthy or not?" | "Did anything change vs. last week?" |
| Comparison basis | Fixed Green/Yellow/Red thresholds | Same day of prior week (WoW) |
| Best for | On-demand check when something feels wrong; meeting prep | Daily routine, morning scan |
| Output style | Campaign-by-campaign status report | Change alerts ranked by revenue impact |

---

## Step 1: Get the Basics
Pull the user's Meta Ads data for today and the past 7 days:
- Campaign name, status, spend
- Results, cost per result
- CTR, frequency
- Any alerts or issues (delivery low, learning limited, paused)

Use Meta Marketing API to fetch campaign and adset level data.

---

## Step 2: Traffic Light Assessment
Score each campaign as Green / Yellow / Red:

| Signal | Green | Yellow | Red |
|---|---|---|---|
| Delivery | Spending normally | < 80% of budget | < 50% or zero |
| Cost per result | At or below target | 10–30% above target (and variance is normal) | 30%+ above target **and sustained** (not just a spike) |
| Frequency | < 3 | 3–4 | > 4 |
| Learning | Out of learning | In learning (< 7 days) | Stuck in learning |
| CTR | > 1% | 0.5–1% | < 0.5% |

---

## Step 3: Check for Normal Performance Fluctuations
Before flagging a campaign as Yellow or Red, confirm it's not normal fluctuation.

**Normal fluctuation — monitor only:**
- CPA is bouncing around day-to-day (up to ±30%) but the rolling average is stable — this is normal Meta Ads volatility, not a problem
- Weekend vs. weekday differences
- Gradual changes over weeks
- Variation while in Learning Phase

**Concerning — investigate today:**
- CPA average has shifted up and stayed there for 3+ consecutive days (not just a one-day spike) — this is a real trend, not fluctuation
- Delivery dropping to near zero with no budget change
- Conversion rate declining while spend increases
- Performance degradation after a recent edit

Before diagnosing problems, ask:
1. Is the ad set still in Learning Phase? (if yes, delay judgment — data is unstable)
2. What's the baseline for normal variation for this campaign?
3. Are there external factors — seasonality, competitor activity, or platform changes?
4. Is sample size sufficient? (typically need 7+ days of data for stable ad sets)

---

## Step 4: Surface Key Issues
For each Yellow or Red campaign, identify:
- What is the specific problem?
- How urgent is it? (Needs attention today / this week / monitor)
- What likely caused it?

**Creative Refresh Trigger — Frequency > 3:**
When frequency reaches 3 or higher, flag the campaign as "Creative Refresh Needed":
"Campaign [X] has frequency at [N] — audience is seeing the same ads too often. This typically causes CTR to drop and CPL to rise. Creative refresh is indicated."

> Note: When this flag appears, the appropriate next step is `meta-ads-recommendation` — it will prescribe the specific creative refresh action based on the full analysis context. Do not trigger `meta-ads-builder` directly from this skill.

---

## Step 5: Quick Output
```
Overall Status: [N] campaigns running | [N] Green | [N] Yellow | [N] Red

For each Red/Yellow campaign:
- Campaign name
- Status: [Issue]
- Spend / Results / CPL
- What happened (1 sentence)
- Suggested focus area (1 sentence)

Everything else: brief healthy summary.
```

---

## Priority Sort — What to Check First
When you have 10+ active campaigns:
1. Red campaigns with spend > $50/day → Check today
2. Yellow campaigns where CPL is 20%+ above target → Check today
3. New campaigns (< 7 days, still in Learning) → Flag as "Learning Phase — Do Not Judge Yet"
4. Green campaigns → Review weekly

---

## When to Escalate
- If the health check surfaces a real problem: suggest meta-ads-analysis for a full diagnostic
- For daily routine monitoring: suggest meta-ads-daily-pulse instead

---

## Skill Boundary
- This is a quick scan, not a deep diagnostic — don't draw conclusions beyond what's clearly broken or healthy
- Do not make optimization recommendations in this skill — that's meta-ads-recommendation's job
- Do not re-fetch data if it was recently pulled in the same session
