---
name: meta-ads-drop-diagnosis
description: "[Didoo AI] Diagnoses sudden performance drops in Meta Ads campaigns. Use when CTR, ROAS, or conversions suddenly decline with no obvious explanation, or when a previously healthy campaign starts underperforming."
---

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching campaign performance data |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
Triggered by meta-ads-daily-pulse flagging a week-over-week drop > 20%, or when user reports "performance suddenly dropped." Not for gradual decline over weeks (that's creative fatigue territory).

---

## Requirements
- account_id (required) — your META_AD_ACCOUNT_ID (format: act_XXXXXXXXX)
- campaign_id or ad_id of the declining entity (required) — find in Meta Ads Manager URL, or via GET /act_{AD_ACCOUNT_ID}/campaigns?fields=id,name
- date_range: last_7d (default)

---

## Step 1: Establish the Drop
Confirm the drop with a Performance Snapshot mini-table:

| Metric | Last Week | This Week | WoW Change |
|--------|-----------|-----------|------------|
| Impressions | | | |
| CPM | | | |
| CTR | | | |
| Frequency | | | |
| CVR | | | |
| CPA | | | |
| ROAS | | | |

Confirm it's real: WoW drop must be > 20% on at least 2 metrics to qualify as a "drop" (not normal fluctuation).

---

## Step 2: Root Cause Analysis
Based on the metric pattern, identify the top 1-3 most likely root causes.

Use this decision tree:

| Pattern | Likely Root Cause |
|---------|-------------------|
| CTR down, frequency up | Creative fatigue |
| CTR down, frequency stable | Creative burnout or audience mismatch |
| CTR stable, CVR down | Landing page issue or offer problem |
| CTR up, CVR down | Landing page can't handle higher volume |
| Impressions down, CPM up | Competition increased, bid not competitive |
| Frequency up, ROAS down | Retargeting audience exhausted |
| All metrics down | External factor (seasonality, iOS, pixel issues) |

For each root cause, write 1-3 short sentences explaining what the data shows, why it points to this cause, and what other evidence supports or contradicts it.

---

## Step 3: Recovery Plan
Format:

**Immediate (next 24 hours):**
- [Specific action: pause, scale down, refresh, shift budget]
- Why this helps now

**Short-term (this week):**
- [New creative, audience expansion, placement changes]
- What to test and why

**Prevention (ongoing):**
- [Rotation schedule, budget cap rules, monitoring thresholds]
- How to catch this earlier next time

---

## Step 4: Output Format
```
Title: Summary
Declining campaign: [Name]
Performance Snapshot: [mini-table above]

Title: Root Cause Analysis
[1-3 root causes with supporting evidence]

Title: Recovery Plan
Immediate: [actions]
Short-term: [actions]
Prevention: [actions]
```

---

## Rules
- Root cause must be supported by data — do not speculate without evidence
- If data is insufficient (e.g., < 3 days since change), say so and recommend waiting
- This is diagnosis only — specific action recommendations route to meta-ads-recommendation
- One immediate action maximum — don't overwhelm the user

---

## Session Context — What This Skill Writes

After completing diagnosis, store the following in session context:

| Key | Description | Example |
|-----|-------------|---------|
| primary_root_cause | The most likely cause of the drop | "Creative fatigue — frequency 4.8, CTR dropped 38% WoW" |
| recovery_plan | Immediate + short-term + prevention actions | "Rotate new creative within 24h; set frequency cap at 3" |

> meta-ads-recommendation reads these keys to produce the structured recovery action plan.
