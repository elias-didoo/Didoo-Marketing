---
name: meta-ads-publisher
description: "[Didoo AI] Executes Meta Ads campaign management tasks — creates new campaigns, makes changes, pauses/activates, changes budgets, duplicates campaigns. Use when user wants to launch or modify campaigns. Requires META_ACCESS_TOKEN."
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
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | All Meta Marketing API calls | `ads_management` (read+write) |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | All campaign operations | — |

> META_APP_ID and META_APP_SECRET are not needed — the access token alone is sufficient for all API operations.

## How to Call the Meta API
Use exec + curl with your META_ACCESS_TOKEN. All requests go to `https://graph.facebook.com/v21.0/`.

Example structure:
```bash
curl -X POST "https://graph.facebook.com/v21.0/act_{AD_ACCOUNT_ID}/campaigns" \
  -d "access_token={META_ACCESS_TOKEN}" \
  -d "name=My Campaign" \
  -d "objective=LEADS" \
  -d "status=PAUSED"
```

---

## When to Use
When user wants to launch a new campaign, make changes to an existing one, or do any campaign management task. Requires META_ACCESS_TOKEN and META_AD_ACCOUNT_ID.

---

## Step 1: Authenticate
Required credentials:
- META_ACCESS_TOKEN — from Meta Developer Console (Graph API Explorer)
- META_AD_ACCOUNT_ID — format: act_XXXXXXXXX (from Ads Manager URL)

If credentials are not yet configured: Guide the user to get them:
1. Access Token: Go to developers.facebook.com/tools/explorer → select App → Generate Token → copy
2. Ad Account ID: adsmanager.facebook.com → Account Settings → find act_XXXXXXXXX

Store credentials securely. Never paste tokens into open conversation unless absolutely necessary.

---

## Step 2: Create New Campaign

### Campaign Level
POST to /act_{AD_ACCOUNT_ID}/campaigns
- objective: LEADS / PURCHASE / LINK_CLICK / AWARENESS
- status: PAUSED (create paused, activate after review)
- name: [Descriptive name: Product + Audience + Date]
- budget_type: DAILY or LIFETIME

### Adset Level
POST to /act_{AD_ACCOUNT_ID}/adsets
- campaign_id: [From step above]
- status: PAUSED
- optimization_goal: corresponds to campaign objective
- billing_event: IMPRESSIONS / LINK_CLICK / etc.
- bid_amount: [If using manual bid]
- targeting: age, gender, geo, interests, behaviors
- daily_budget or lifetime_budget

### Creative / Ad Level
Upload image first: POST /act_{AD_ACCOUNT_ID}/adimages
Then reference image_hash in creative.

POST to /act_{AD_ACCOUNT_ID}/ads
- adset_id: [From adset step]
- creative: reference to adcreative
- status: PAUSED

---

## Step 3: Pre-flight Checklist — Before Activating
Before setting status to ACTIVE, verify all of the following:
1. Campaign status is PAUSED — never launch in ACTIVE status; always review first
2. Budget confirmed with user — confirm the daily budget amount before going live
3. Targeting is appropriate — not too broad (> 1M people is too broad for interest-based) or too narrow (< 100K people may not have enough volume)
4. Creative passed Quality Check — confirm the ad creative has been reviewed
5. Pixel and CAPI installed — verify Meta pixel is firing and CAPI is connected
6. Bid strategy matches the objective — lowest cost for testing, cost cap for CPL control, target cost for scaling

---

## Step 4: Campaign Management Tasks

### Pause / Activate
PATCH /act_{AD_ACCOUNT_ID}/adsets?ids=[ADSET_ID]
Set status to PAUSED or ACTIVE.

### Change Budget
PATCH /act_{AD_ACCOUNT_ID}/adsets?ids=[ADSET_ID]
Set new daily_budget or lifetime_budget.
Rule: Change by max 20% per adjustment to avoid disrupting learning.

### Check Campaign List
GET /act_{AD_ACCOUNT_ID}/campaigns?fields=id,name,status,objective,daily_budget

### Get Performance
GET /act_{AD_ACCOUNT_ID}/insights?fields=spend,impressions,clicks,results,cpc,cpm,ctr&level=[campaign|adset|ad]&time_range={'since':'YYYY-MM-DD','until':'YYYY-MM-DD'}

### Duplicate / Clone Campaign
When you have a winner campaign and want to replicate it to a new audience, geo, or offer, follow these 6 steps:

**Step 1: Get source campaign structure**
```
GET https://graph.facebook.com/v21.0/[CAMPAIGN_ID]?fields=id,name,status,objective,daily_budget&access_token=[TOKEN]
```

**Step 2: Get source adsets**
```
GET https://graph.facebook.com/v21.0/[ADSET_ID]?fields=id,name,status,optimization_goal,targeting,daily_budget,bid_amount&access_token=[TOKEN]
```

**Step 3: Get source ads**
```
GET https://graph.facebook.com/v21.0/[AD_ID]?fields=id,name,status,creative&access_token=[TOKEN]
```

**Step 4: Create new campaign (PAUSED)**
```
POST https://graph.facebook.com/v21.0/act_[AD_ACCOUNT_ID]/campaigns
  access_token=[TOKEN]
  name=[NEW_NAME — e.g. "Didoo AI — US — Interest — 2026-04-14"]
  objective=[SAME AS SOURCE]
  status=PAUSED
  budget_type=DAILY
```

**Step 5: Create new adset(s) with modified targeting/budget**
```
POST https://graph.facebook.com/v21.0/act_[AD_ACCOUNT_ID]/adsets
  access_token=[TOKEN]
  campaign_id=[NEW CAMPAIGN ID]
  name=[NEW ADSET NAME]
  optimization_goal=[SAME AS SOURCE — e.g. LEAD_GENERATION]
  targeting=[MODIFIED — change geo, age, interests, etc.]
  daily_budget=[NEW BUDGET — integer in cents, e.g. 5000 = $50/day]
  status=PAUSED
```

**Step 6: Create new ad(s)**
```
POST https://graph.facebook.com/v21.0/act_[AD_ACCOUNT_ID]/ads
  access_token=[TOKEN]
  adset_id=[NEW ADSET ID]
  name=[NEW AD NAME]
  creative={'creative_id': '[NEW_CREATIVE_ID]'}
  status=PAUSED
```

**Review BEFORE activating:**
1. Verify targeting is correct for the new audience
2. Confirm budget is realistic
3. Review creative and copy
4. Set status to ACTIVE only after review

---

## Step 5: Common Issues
| Issue | Check |
|-------|-------|
| Campaign not delivering | Is status ACTIVE? Is budget more than $5/day? Is targeting too narrow? |
| Ads in Learning Limited | Need ~50 results/week to exit; increase budget or consolidate adsets |
| High frequency | Expand audience or rotate new creative |
| Low CTR | Test new creative; check if audience matches the hook |
| Cost per result too high | Check if budget is realistic; test narrower targeting |

---

## Output Rules
- Always confirm what was done: "Campaign created in PAUSED status. Ready to review before I activate it."
- After any change, summarize what changed and what to expect
- If user asks for a campaign list, format as a readable table

---

## Security Rules
- Never log or print access tokens in plain text
- Store credentials in platform's secret management only
- Only use credentials for Meta Marketing API — never for other purposes

---

## Key Numbers to Know
- Budgets in cents (5000 = $50.00) in API calls
- Ad Account ID format: act_XXXXXXXXX
- Minimum viable daily budget for learning: ~$10–15/day per adset
- Need ~50 results per week per adset to exit learning phase
