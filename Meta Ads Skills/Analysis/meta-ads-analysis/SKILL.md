---
name: meta-ads-analysis
description: "[Didoo AI] Analyzes Meta Ads campaign performance in depth — metrics, funnel, trends, and anomalies. Use when user wants to understand how a campaign is performing, identify weak points, or get data before receiving recommendations. Outputs structured analysis only; never provides recommendations. Triggers on: \"analyze\", \"deep dive\", \"why is performance\", \"diagnose\", \"full audit\"."
---

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | All Meta Marketing API calls |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
Loaded when user wants to understand how their Meta Ads campaign is performing — either a specific aspect (audience, creative, overall) or a full diagnostic. Can run standalone or as prerequisite for meta-ads-recommendation.

---

## Step 0: Understand the Campaign
Before pulling any data, get context on what this campaign is trying to do.
Ask 1–2 quick questions if not already clear from conversation:
- "Is this an always-on campaign, or tied to a specific promotion?"
- "Are you testing new things, or trying to scale what's already working?"

Keep it conversational. If user doesn't want to answer, proceed with standard analysis.

---

## Step 1: Gather Campaign Structure
Ask the user for their Meta Ads account access. Use the Meta Marketing API to fetch:

### Campaign level
- Campaign name, ID, status (is it ACTIVE?)
- Objective (what counts as a "result" — leads, purchases, clicks?)
- Budget type (CBO = campaign-level budget optimization, or ABO = adset-level)
- Bid strategy, daily budget, start date

### Adset level
- Adset name, ID, status
- Optimization goal (this determines what "results" means for this adset)
- Targeting countries, budget, bid amount

### Ad level
- Ad name, ID, status
- Which adset it belongs to

Store this as `campaign_context`. Reuse across the session — don't re-fetch.

---

## Step 2: Confirm Time Range and Attribution Window
Always confirm the time period and attribution window before pulling performance data:

1. Time range: "What time period would you like to analyze? (e.g., last 7 days, last 30 days, or a specific date range)"
2. Attribution window: "What attribution window are you using? Default in Meta is 7-day click. For e-commerce, 1-day click often gives a clearer signal. For lead gen, 7-day click is usually better."

| Campaign type | Recommended attribution window |
|--------------|--------------------------------|
| E-commerce / purchase | 1-day click |
| Lead generation | 7-day click |
| App installs | 7-day click |
| Brand awareness | 28-day view |

Convert to `YYYY-MM-DD,YYYY-MM-DD` format.
Default granularity:
- ≤14 days → daily
- 15–60 days → weekly

---

## Step 3: Route the Analysis
| User is asking about | What to analyze |
|----------------------|-----------------|
| Overall / general health | Campaign-level metrics, no breakdown |
| Audience / country / platform / age / gender | Adset or ad level with breakdowns |
| Creative / which ad is better | Ad-level metrics |
| Everything / full diagnostic | All three in sequence |
| Follow-up on existing data | Use data already in session — don't re-fetch |

---

## Step 4: Pull and Interpret Performance Data
Use the Meta Marketing API to fetch metrics for the relevant level and time range.

### Key Metrics to Collect
**At campaign or adset level:**
- Spend, impressions, frequency
- CPM (cost per 1,000 impressions)
- CPC (cost per link click)
- CTR (link click rate)
- LPV rate (landing page view rate = landing page views / link clicks)
- Results, cost per result
- Conversion rate (results / landing page views)

**At ad level, add:**
- Individual ad performance
- Creative elements if identifiable

### What Each Metric Tells You
| Metric | Declining = | Growing = |
|--------|-------------|-----------|
| CPM | Cheaper reach | Competition up |
| CPC / cost_per_result | More efficient | Less efficient |
| CTR / LPV rate / conversion_rate | Problem | Healthy |
| Results volume | Dropping | Growing |
| Frequency | — | Fatigue risk if > 3 |

### Understanding "Results"
The meaning of "results" depends on the optimization goal:
- LEAD_GENERATION → results = leads
- LINK_CLICKS → results = link clicks
- LANDING_PAGE_VIEWS → results = landing page views
- PURCHASE / CONVERSIONS → results = purchases

Always clarify what "results" means when discussing cost_per_result.

### Campaign Structure Matters
- CBO (campaign-level budget): Evaluate at campaign level first
- ABO (adset-level budget): Evaluate at adset level
- Multiple ads in one adset: Evaluate at adset level, not individual ads
- Active but less than 7 days old + less than 50 results since last significant edit: Treat as still in learning phase — data is unstable
- Breakdown Effect: Meta optimizes for marginal CPA (cost of the next result), not average CPA. A segment showing higher average CPA may be protecting overall campaign efficiency. Do not judge system decisions by average CPA in breakdown reports alone.

---

## Step 4b: Lead Generation Campaigns
> Use **meta-ads-lead-gen-analysis** instead — it has dedicated LPV benchmarks for lead gen, form friction analysis, CAPI verification, and lead quality diagnosis.

---

## Step 4c: Landing Page Diagnostic
> This section has moved to **meta-ads-recommendation → Step 4**. Landing page diagnosis is now part of the recommendation workflow for better action alignment.

---

## Step 5: Check Ad Relevance Diagnostics
When an ad's cost_per_result is elevated and basic metrics (CTR, LPV rate) don't fully explain it, check the Ad Relevance Diagnostics in Meta Ads Manager.

| Diagnostic | What it measures | Low ranking suggests |
|-------------|-------------------|-----------------------|
| Quality Ranking | Perceived ad quality vs. competitors | Improve creative |
| Engagement Rate Ranking | Expected engagement vs. competitors | Test new angles, improve hook |
| Conversion Rate Ranking | Expected conversion vs. competitors with same optimization goal | Check landing page or audience-offer fit |

**Usage rules:**
- Requires 500+ impressions to be available — below that, diagnostics are not meaningful
- These are diagnostic tools only, not auction inputs
- When all three rankings are low simultaneously → strong audience-creative mismatch

---

## Step 6: Assess for Problems
Flag these when you see them:
- Frequency > 3 → Audience fatigue risk
- CTR less than 1% → Weak creative hook or audience mismatch
- LPV rate less than 70% → Ad-to-landing-page disconnect
- cost_per_result rising → Efficiency problem
- Status is not ACTIVE → Explain impact
- Learning / Learning Limited → Warn: data not yet stable
- Spend less than 50% of budget → Delivery problem — audience may be too narrow or bid too low
- One segment vastly outperforming others → System is correctly finding winners

---

## Step 7: Structure Your Output
- **Campaign Context**: one sentence covering objective, budget type, and status.
- **Summary**: Spend, impressions, results, cost per result, CTR, CPM.
- **Funnel**: Impressions → Link Clicks → Landing Page Views → Results, with rates at each stage.
- **Trend Analysis**: Key metrics with direction and what it means for the business.
- **Key Issues**: Specific problems and why they matter.
- **Data Notes**: Any caveats — learning phase, insufficient data, zero-spend entities excluded.

---

## Tone and Language
- Match the language the user speaks (English or Chinese)
- Speak as a professional growth partner — translate numbers into business insights
- Every judgment must cite specific data. Never say "performance is bad" without a number
- When data is insufficient, say so: "2 days isn't enough for reliable trends — let's revisit in a week"

---

## Restrictions
- Analysis only — never output recommendations in this skill
- Do not recalculate metrics already computed by Meta (CTR, CPM, CPC are already calculated)
- Never conclude an ad or adset is "underperforming" based on CPA alone without comparing to its own historical baseline and the account average

---

## Session Context — What This Skill Writes

After completing analysis, store the following in session context:

| Key | Description | Example |
|-----|-------------|---------|
| funnel_weak_points | Where the biggest funnel drop-off occurs | "LPV rate 58%, well below 70% benchmark" |
| trend_signals | Direction of key metrics | "CPM up 18% WoW; CTR down 0.4pp" |
| anomalies | Anything unusual or unexpected | "Frequency 4.2 with CPL still at target" |
| data_quality | Whether there's enough data to act | "Only 2 days — too early to judge" |
| lp_diagnosis_general | Is the problem ad-side or landing page-side? | "Ad side — CTR declining, frequency stable" |

> **Routing:** If the campaign is Lead Gen, route to meta-ads-lead-gen-analysis and preserve these keys. meta-ads-recommendation reads these keys to produce the action plan.
