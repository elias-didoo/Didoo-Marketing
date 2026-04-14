---
name: meta-ads-creative-fatigue
description: "[Didoo AI] Analyzes creative fatigue signals across Meta Ads campaigns. Use when reviewing declining CTR or ROAS, planning creative rotation schedules, or managing creative lifecycle. Run standalone or after meta-ads-daily-pulse flags creative fatigue."
---

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching ad performance and frequency data |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
When creative has been running for 7+ days and performance is declining — or proactively when managing a portfolio of creatives. Use before scheduling creative refresh.

---

## Requirements
- account_id (required)
- campaign_id or ad_ids (optional — defaults to all active ads in account)
- date_range: last_30d (default)

---

## Step 1: Fetch Active Ads
Get list of all active ads with performance data:
- get_ads_list → ad names, IDs, campaign association
- get_ads_insights per ad: frequency, CTR, ROAS, spend, impressions

---

## Step 2: Calculate Fatigue Signals
For each ad, compute:
- Days active: from ad creation date
- CTR drop %: (Week 1 CTR − Current CTR) / Week 1 CTR × 100
- Frequency decay: CTR at freq 1-2x vs CTR at freq 5x+
- Decay rate: CTR drop % per day since launch
- Days until death: estimate based on current decay rate and "Dead" threshold
- Lifespan status: see Classification table below

---

## Step 3: Fatigue Classification
| Level | Criteria | Action |
|-------|----------|--------|
| Fresh | < 7 days OR CTR drop < 10% | Monitor |
| Mild | 7-14 days, CTR drop 10-20% | Monitor |
| Moderate | 14-21 days, CTR drop 20-35% | Rotate Soon |
| Severe | 21-28 days, CTR drop 35-50% | Refresh Now |
| Dead | > 28 days OR CTR drop > 50% OR Freq > 5x with CTR < 50% of baseline | Kill Immediately |

---

## Step 4: Lifespan Status
| Status | Criteria |
|--------|----------|
| Growing | ROAS within 10% of peak, < 14 days |
| Peak | ROAS at maximum, 7-21 days |
| Declining | ROAS decay > 5%/day, > 21 days |
| Expired | ROAS < 50% of peak OR > 35 days |

---

## Step 5: Build Rotation Pipeline
Classify all active creatives into pipeline buckets:

| Status | Count |
|--------|-------|
| Active Winners | |
| Active Decaying | |
| Recently Killed | |
| In Pipeline | |

Calculate:
- Creatives dying this week
- Creatives dying next week
- New creatives needed
- Testing velocity (new creatives launched per week)

---

## Step 6: Output Format

### SECTION 1: Fatigue Status Overview
Fatigue Health Table (top 15-20 creatives):

| Creative | Days Active | Current Freq | CTR Week 1 | CTR Now | Drop % | Fatigue Level | Action |
|----------|-------------|--------------|------------|----------|---------|---------------|--------|

### SECTION 2: Frequency Decay Analysis
Frequency Impact Table (top 10-12 highest frequency):

| Creative | Freq 1-2x CTR | Freq 3-4x CTR | Freq 5x+ CTR | Current Freq | Decay Rate |
|----------|---------------|---------------|--------------|--------------|------------|

### SECTION 3: Performance Degradation Timeline
ROAS Decay Table (top 12-15):

| Creative | Launch Date | Peak ROAS | Current ROAS | Days to Peak | Days Since Peak | Decay Rate | Lifespan Status |
|----------|-------------|-----------|--------------|--------------|-----------------|------------|------------------|

### SECTION 4: Rotation Pipeline Status
Creative Inventory Table:

| Status | Count |
|--------|-------|
| Active Winners | |
| Active Decaying | |
| Recently Killed | |
| In Pipeline | |

Refresh Needs: Dying this week, dying next week, new needed, testing velocity.

### SECTION 5: Refresh Recommendations
Action Priority Table (top 10-12):

| Creative | Current Status | Days Until Death | Action | Priority | New Direction |
|----------|----------------|------------------|--------|----------|----------------|

Priority: Urgent (24h), High (3d), Medium (7d), Low (monitor).

---

## Rules
- Prioritize by: Severity > Days Until Death > Spend
- New Direction must be specific angles, not generic advice ("Test urgency angle" not "new creative")
- No executive summary
- This is analysis only — recommendations route to meta-ads-recommendation

---

## Session Context — What This Skill Writes

After completing analysis, store the following in session context:

| Key | Description | Example |
|-----|-------------|---------|
| rotation_pipeline | Creative inventory by status bucket | "2 growing, 3 declining, 1 dead, 2 in pipeline" |
| fatigue_level | Per-creative fatigue classification | "Blue Banner v2: Severe; CTR drop 52%" |
| days_until_death | Estimated days before each creative expires | "Banner v3: 8 days; Banner v4: 22 days" |

> meta-ads-recommendation reads these keys to produce the creative rotation action plan. For new creative generation, route to meta-ads-builder.
