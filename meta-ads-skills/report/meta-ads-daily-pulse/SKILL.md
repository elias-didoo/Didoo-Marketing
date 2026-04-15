---
name: meta-ads-daily-pulse
description: "[Didoo AI] Rapid daily health scan for Meta Ads — detects week-over-week performance changes and flags urgent issues before meetings. Use every morning or before any daily review meeting. This is a change detector, not a full diagnostic."
---

# Meta Ads Daily Pulse

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching account and campaign insights |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

---

## When to Use
Use every morning or before any daily review meeting. This skill is a change detector — it answers: "Did anything important change overnight compared to the same day last week?"

Not sure which monitoring skill to use?

|  | meta-ads-daily-pulse | meta-ads-healthcheck |
|---|---|---|
| Primary question | "Did anything change vs. last week?" | "Is this campaign healthy or not?" |
| Comparison basis | Same day of prior week (WoW) | Fixed Green/Yellow/Red thresholds |
| Best for | Daily routine, morning check, meeting prep | Something feels off — quick on-demand check |
| Output style | Change alerts ranked by revenue impact | Campaign-by-campaign status report |

---

## Requirements
- account_id (required)
- date_range: yesterday (default) — fetches yesterday's data; compare to same day of prior week (WoW)
- campaign_ids (optional) — defaults to all active campaigns

---

## Step 1: Pull Account Baseline
Fetch account-level metrics for yesterday and the same day of the prior week:
- Spend, impressions, results, ROAS
- CTR, CPC, CPM, frequency
- Conversions, cost per conversion

Use exec + curl to call the Meta Graph API:

**Account-level (yesterday):**
```bash
# Cross-platform date helper (Python, works on macOS and Linux):
YESTERDAY=$(python3 -c "from datetime import date, timedelta; print((date.today() - timedelta(days=1)).isoformat())")
SAME_DAY_LAST_WEEK=$(python3 -c "from datetime import date, timedelta; print((date.today() - timedelta(days=8)).isoformat())")

curl -G "https://graph.facebook.com/v21.0/act_${META_AD_ACCOUNT_ID}/insights" \
  -d "fields=spend,impressions,results,roas,ctr,cpc,cpm,frequency,conversion_rate" \
  -d "time_range={'since':'${YESTERDAY}','until':'${YESTERDAY}'}" \
  -d "access_token=${META_ACCESS_TOKEN}"
```

**Same day of prior week (7 days back):**
```bash
# Same YESTERDAY and SAME_DAY_LAST_WEEK variables as above

curl -G "https://graph.facebook.com/v21.0/act_${META_AD_ACCOUNT_ID}/insights" \
  -d "fields=spend,impressions,results,roas,ctr,cpc,cpm,frequency,conversion_rate" \
  -d "time_range={'since':'${SAME_DAY_LAST_WEEK}','until':'${SAME_DAY_LAST_WEEK}'}" \
  -d "access_token=${META_ACCESS_TOKEN}"
```

**Campaign-level (yesterday, if you need per-campaign breakdown):**
```bash
# Same YESTERDAY variable as above

curl -G "https://graph.facebook.com/v21.0/act_${META_AD_ACCOUNT_ID}/insights" \
  -d "fields=campaign_name,spend,impressions,results,roas,ctr,cpc,cpm,frequency" \
  -d "level=campaign" \
  -d "time_range={'since':'${YESTERDAY}','until':'${YESTERDAY}'}" \
  -d "access_token=${META_ACCESS_TOKEN}"
```

**Important — Same Day of Prior Week comparison:**
Meta Ads performance follows day-of-week patterns (e.g., Tuesdays often differ from Saturdays). Always compare each day to its same day of prior week — not the prior day. Comparing to the prior day creates false alarms.

For each day in the date range, pull the equivalent day from 7 days prior:
- Monday this week → Monday last week
- Tuesday this week → Tuesday last week
- etc.

---

## Step 2: Run Detection Checks
Run all four detection checks:

| # | Issue | Detection Logic |
|---|---|---|
| 1 | Spend without conversions | spend > 0 AND conversions = 0 |
| 2 | Week-over-week drop | CTR, ROAS, or CVR down > 20% vs same day of prior week |
| 3 | Budget cap + strong ROAS | spend > 80% daily budget AND ROAS > 2x account average |
| 4 | Creative fatigue | frequency > 3 AND CTR declining vs same day of prior week |

For each flag, calculate estimated revenue impact:
- Lost conversions = (prior period CVR − current CVR) × current impressions
- Revenue impact = lost conversions × average order value

---

## Step 3: Prioritize by Revenue Impact
Sort alerts by estimated revenue impact. Do not exceed 3 items per section.

---

## Step 4: Format Output
```
🚨 Critical
- [Campaign/Ad ID]: [Issue] | [Metric] | [Immediate action]

⚠️ Watch
- [Trending decline not yet critical]

✅ Healthy
- [Brief confirmation]

💡 Opportunity
- [Quick win if found]
```

---

## Skill Boundary
- This is a detection tool only — do not output recommendations
- After flagging issues, route to meta-ads-recommendation for action plans
- If you need a full campaign-by-campaign status check (not change-focused), use meta-ads-healthcheck instead

---

## Rules
- Always compare to the same day of prior week (not prior day — day-of-week patterns matter in Meta Ads)
- Include specific campaign/ad IDs and numbers in every line
- One immediate action per issue
- Do not re-fetch data if it was recently pulled in the same session
