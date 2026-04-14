---
name: meta-ads-audience-analysis
description: "[Didoo AI] Analyzes Meta Ads audience efficiency across audience types, overlap, demographics, and budget allocation. Use when reviewing targeting strategy, planning audience expansion or narrowing, or auditing budget distribution across audience segments."
---

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching audience breakdown and overlap data |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
Before launching new campaigns, during quarterly planning, or when a campaign is underperforming and audience mismatch is suspected.
Run standalone or after `meta-ads-weekly-performance` identifies audience-related issues.

---

## Requirements
- account_id (required)
- campaign_id (optional — defaults to all campaigns)
- date_range: last_30d (default)

---

## Step 1: Fetch Audience Data
Pull data broken down by audience dimension:

### Audience Type Performance (via breakdown by audience_type)
- Lookalike, Custom, Broad, Interest, Retargeting
- Spend, CTR, CPA, ROAS for each

### Audience Overlap (via Meta's overlap analysis tool)
- Pairs of overlapping audiences
- Overlap percentage, combined spend, ROAS

### Demographic Breakdown (via breakdown by age and gender)
- Age × Gender cells
- Spend, conversions, CPA, ROAS

### Placement Breakdown
- Feed, Stories, Reels, Instant Articles, Audience Network, Messenger
- CTR, CVR, CPA, ROAS for each

---

## Step 2: Audience Type Efficiency
| Audience Type | Spend | % of Budget | CTR | CPA | ROAS | Budget Should Be |
|--------------|------|------------|-----|-----|------|------------------|
|              |      |            |     |     |      |                  |

Identify:
- Which audience type delivers best ROAS
- Which is over/underallocated relative to efficiency
- Lookalike quality (which LAL layers performing)

---

## Step 3: Audience Overlap Analysis
| Audience Name | Overlaps With | Overlap % | Combined Spend | ROAS | Action |
|---------------|---------------|-----------|-----------------|------|--------|
|               |               |           |                 |      | Consolidate / Exclude / Separate / Monitor |

Flag:
- Overlap > 30% with high combined spend → Consolidate
- Overlap > 50% → Exclude one
- Different audiences with same LAL source → Separate

---

## Step 4: Demographic Deep Dive
| Age and Gender | Spend | Conversions | CPA | ROAS | Action |
|-----------------|------|-------------|-----|------|--------|
|                 |      |             |     |      | Increase Bid / Decrease Bid / Exclude / Maintain |

Identify:
- Highest and lowest ROAS demographic cells
- Unexpected performance by gender or age
- Bid adjustment opportunities

---

## Step 5: Placement Breakdown
| Placement | Spend | % of Budget | CTR | CVR | CPA | ROAS | Action |
|-----------|------|------------|-----|-----|-----|------|--------|
|           |      |            |     |     |     |      | Scale / Maintain / Reduce / Exclude |

Flag:
- Audience Network with low ROAS → Exclude
- Stories vs Feed efficiency gap > 30% → Reduce lower performer
- Reels growing but underallocated → Scale

---

## Step 6: Budget Reallocation Plan
| From Audience/Placement | To Audience/Placement | Shift Amount | Expected ROAS Impact | Revenue Impact | Priority |
|---------------------------|-------------------------|---------------|-----------------------|-----------------|----------|
|                           |                         |               |                       |                 | High / Medium / Low |

Prioritize moves by:
1. Highest ROAS differential
2. Enough data to trust the signal (minimum 50 results per segment)
3. Realistic shift amounts (don't move more than 20% at once)

---

## Step 7: Output Format
### SECTION 1: Audience Type Efficiency
2–3 audience type issues in problem/solution format.

### SECTION 2: Audience Overlap and Waste
2 overlap issues in problem/solution format.

### SECTION 3: Placement Breakdown
2 placement issues in problem/solution format.

### SECTION 4: Demographic Deep Dive
2–3 demographic issues in problem/solution format.

### SECTION 5: Budget Reallocation Plan
1–2 reallocation strategy issues in problem/solution format.

---

## Rules
- No executive summary
- Minimum 50 results per segment before drawing conclusions on any audience slice
- Action must be specific: "Increase bid 20% on Women 25-34" not "adjust demographics"
- This is analysis only — recommendations route to `meta-ads-recommendation`

---

## Session Context — What This Skill Writes

After completing analysis, store the following in session context:

| Key | Description | Example |
|-----|-------------|---------|
| budget_reallocation_plan | Specific shifts between audience segments | "$200/day from Broad to LAL 1% Women 25-34" |
| audience_issues | Overlap, waste, or misallocation findings | "Audience A overlaps with B by 41% — consolidate" |

> meta-ads-recommendation reads these keys to produce the audience action plan.
