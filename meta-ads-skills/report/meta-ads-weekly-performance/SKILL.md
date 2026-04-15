---
name: meta-ads-weekly-performance
description: "[Didoo AI] Generates a structured weekly performance report for Meta Ads accounts. Use at the end of each week to review performance, explain changes, and identify what needs attention. Pairs with meta-ads-daily-pulse for daily context."
homepage: https://didoo.ai/blog
metadata:
  {
    "openclaw":
      {
        "requires": { "env": ["META_ACCESS_TOKEN", "META_AD_ACCOUNT_ID"] },
      },
  }
---

## Required Credentials
| Credential | Where to Get | Used For | OAuth Scope |
|-----------|-------------|---------|-------------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching account and campaign insights | `ads_read` (read-only) |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query | — |

## When to Use
At the end of each week (every Monday) for ongoing campaigns. Also use after a significant change (new campaign launch, major budget shift, creative swap) to document the impact.

---

## Requirements
- account_id (required)
- date_range: last_7d (default)

---

## Step 1: Pull KPI Snapshot
Fetch account and campaign level metrics for this week and prior week:
- Spend, results, cost per result
- ROAS, conversion values
- CTR, CPM, CPC
- Frequency

Calculate week-over-week (WoW) change for each metric:
WoW = (This Week − Prior Week) / Prior Week × 100

---

## Step 2: Identify What Changed
Write 3-5 bullets explaining the biggest moves this week.

Format: "[What changed] — [Spend/ROAS/Revenue impact]"

Examples:
- "Increased budget on Pizza Campaign by 30% — spend up $1,200, ROAS held at 3.2x"
- "Creative rotation on Runner Audience — CTR up 0.8pp, CPL down $2.40"

---

## Step 3: Identify Performance Drivers
Identify the top 3 things driving results this week.

For each driver write one sentence with specific metrics:
Format: "[What drove results] – [Spend], [ROAS], [Revenue impact vs last week]"

---

## Step 4: Detect Issues
Run issue detection:
- Campaigns with ROAS drop > 30% vs prior week
- Campaigns with spend > budget but low conversion rate
- New campaigns still in Learning Phase
- Frequency > 4 on any campaign

---

## Step 5: Format Output
### Key Metrics Snapshot
| Metric | This Week | Prior Week | WoW Change |
|---|---|---|---|
| Spend | | | |
| ROAS | | | |
| Conversions | | | |
| CPA | | | |

### What Changed This Week
- [3-5 bullets]

### Performance Drivers
- [3 bullets]

### Issues and Actions Required
- [3 urgent issues, each with problem/solution format]

---

## Rules
- No executive summary
- Specific numbers on every line
- Separate problem from solution clearly in each issue
- This is a reporting tool — recommendations route to meta-ads-recommendation
