---
name: meta-lead-gen-analysis
description: "[Didoo AI] Specialized analysis module for Meta lead generation campaigns. Use when CPL is elevated, lead quality is unclear, or you need to diagnose why leads aren't converting downstream. For general campaign analysis, use meta-ads-analysis."
---

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching campaign and form integration data |
| META_AD_ACCOUNT_ID | Ads Manager URL: `adsmanager.facebook.com/act_XXXXXXXXX` | Identifying which account to query |

## When to Use
When running lead generation campaigns and standard e-commerce analysis logic doesn't apply. Specifically for:
- CPL is higher than expected or target
- Lead volume is healthy but conversion rate to closed deals is low (quality problem)
- LPV rate looks fine but form submit rate is unknown
- CAPI may not be sending offline lead data back to Meta
- Not sure if the problem is the ad, the audience, or the form itself

Lead gen has fundamentally different metrics than e-commerce — standard CVR and LPV benchmarks don't apply. Use this skill instead of applying e-commerce logic to lead gen campaigns.

---

## Key Differences from E-commerce Analysis
| Metric | E-commerce Benchmark | Lead Gen Benchmark | Why It Differs |
|---|---|---|---|
| LPV Rate | > 70% is healthy | > 50% is healthy | Many leads browse without intent to submit |
| Form Submit Rate | N/A | > 20% is healthy | Form friction is the primary drop-off point |
| CPL | Varies by industry | The primary metric | What you're actually paying for |
| CVR (post-click) | Purchase rate | Lead-to-qualified rate | Meta can't see what happens after form submit |
| Attribution | 1-day or 7-day click | 7-day click usually | Longer window for consideration |

---

## Step 1: Gather Lead Gen Specific Data
Pull these metrics at the campaign and adset level:
- Spend, impressions, CTR
- LPV Rate (Landing Page View rate = landing page views / link clicks)
- Form Submit Rate (if available — requires CAPI or form integration)
- CPL (cost per lead)
- Frequency
- Lead volume vs. prior period

Also ask the user:
- What does a "good lead" look like for their business?
- What's their lead-to-close rate historically?
- Are they using CAPI to send offline lead data back to Meta?

---

## Step 2: Diagnose the CPL Problem
CPL can be high due to three distinct root causes. Diagnose which one before recommending solutions:

### Root Cause A: Ad-to-Form Disconnect (LPV Rate < 50%)
If LPV rate is low, users click the ad but don't reach or engage with the form.

What to check:
- Does the landing page load quickly on mobile?
- Is the form visible above the fold without scrolling?
- Does the ad headline match the landing page promise?
- Is there too much friction before the form (popups, interstitials)?

### Root Cause B: Form Friction (Form Submit Rate < 20%)
If LPV rate is healthy but leads are low, the form itself is the bottleneck.

What to check:
- Number of form fields: > 4 fields causes severe drop-off
- Field types: too many required fields increases abandonment
- Trust signals: is it clear what happens after they submit?
- Mobile optimization: can users easily fill out the form on phone?

### Root Cause C: Audience or Offer Mismatch
If both LPV rate and form submit rate look healthy but CPL is still high, the problem is upstream.

What to check:
- Is the audience targeting too broad? (Getting unqualified leads)
- Is the offer compelling enough for this specific audience?
- Is the creative clearly communicating what they'll get?

---

## Step 3: CAPI Verification — Critical for Lead Gen
Without CAPI sending offline lead data back to Meta:
- Meta is optimizing for form submissions, not qualified leads
- CPL shown may be "form submit cost" not "actual lead cost"
- The algorithm can't learn which leads actually convert

**How to verify CAPI is connected:**
1. Go to Meta Events Manager → select your pixel
2. Check "About CAPI" — status should be green/active
3. Ask: are offline leads (calls, CRM-qualified leads) being sent back via CAPI?

If CAPI is NOT connected for offline leads:
- This is a priority fix — it will lower CPL over 2–3 weeks
- Recommend a CRM integration or Zapier/Make workflow
- Or use Meta's Lead Forms (form submission = conversion automatically tracked)

---

## Step 4: Audience Targeting Analysis for Lead Gen
### Cold Audiences (Interest-Based Targeting)
- Interest targeting works for cold audiences
- Frequency builds fast — limit budget to avoid rapid fatigue
- Test narrower interest layers (2–3 stacked interests, not broad categories)

### Custom Audiences (Retargeting and Lookalikes)
- Website visitors (pixel): typically highest conversion rate
- Email lists (matched audiences): strong for existing customer re-engagement
- Lookalikes (LAL): based on converter audiences, quality depends on seed list quality
- Test LAL layers 1–3% first — wider LAL = lower quality but more volume

---

## Step 5: Lead Gen LPV Rate Deep Dive
| LPV Rate | Diagnosis | Action |
|---|---|---|
| > 70% | Excellent — high-intent audience | Scale |
| 50–70% | Healthy — ad and page are aligned | Monitor |
| 30–50% | Below average — check form friction | Investigate form |
| < 30% | Poor — messaging mismatch or page issue | Fix ad-to-page alignment |

---

## Step 6: Format Output

**Lead Gen Health Summary:**
- Spend / CPL / Lead Volume
- LPV Rate vs. benchmark
- Form Submit Rate (if available)
- CAPI Status (connected / not connected)

### SECTION 1: Funnel Bottleneck Diagnosis
Identify which stage is causing CPL to exceed target:
- [Stage: Ad Click → Landing Page View] — if LPV < 50%
- [Stage: Landing Page View → Form Submit] — if LPV ≥ 50% but leads low
- [Stage: Form Submit → Qualified Lead] — if CPL OK but quality poor (offline issue)

### SECTION 2: CAPI Gap (if applicable)
- State whether offline lead data is being sent to Meta
- Impact: algorithm is optimizing for quantity, not quality
- Recommended action to close the gap

### SECTION 3: Audience and Targeting Issues
- Narrow/wide diagnosis based on frequency and CPL
- Recommendation on interest stacking or lookalike layers

### SECTION 4: Form and Landing Page Issues
- Form field count check
- Mobile friction check
- Messaging alignment check

---

## Data Passing to meta-ads-recommendation
After completing this analysis, store in session context:
- lp_diagnosis: Ad side vs. Landing Page side vs. Audience side
- capi_status: Connected / Not Connected / Partially Connected
- cpl_breakdown: Which stage is causing the CPL problem
- recommended_fix_priority: Audience / Creative / Form / CAPI (ranked by likely impact)

---

## Rules
- Apply lead gen benchmarks, not e-commerce benchmarks
- Always verify CAPI status before diagnosing CPL issues — missing CAPI is the most common cause of misleading CPL data
- Do not recommend creative changes if form friction is the bottleneck
- Do not recommend audience changes if LPV rate indicates a page problem
- This is analysis only — recommendations route to meta-ads-recommendation

---

## Session Context — What This Skill Writes

After completing analysis, store the following in session context:

| Key | Description | Example |
|-----|-------------|---------|
| lp_diagnosis | Funnel stage causing the CPL problem | "Form friction — submit rate 12%, below 20% benchmark" |
| capi_status | Whether offline leads are being sent to Meta | "Not connected — offline lead data not flowing" |
| cpl_breakdown | Which stage is the bottleneck | "LPV 65% OK; Form submit 11% is the bottleneck" |
| recommended_fix_priority | Ranked fix order | "1. CAPI  2. Form fields  3. Audience" |

> meta-ads-recommendation reads these keys to produce the lead gen action plan.

> ⚠️ Note: meta-ads-analysis writes overlapping keys (lp_diagnosis). If you have already run meta-ads-analysis in this session, copy any needed values to your working notes before running this skill — they will be overwritten.
