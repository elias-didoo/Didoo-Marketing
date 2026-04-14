---
name: meta-ads-recommendation
description: "[Didoo AI] Produces specific, prioritized action plans based on campaign analysis data. Use when user wants to know what to do about their campaign performance — after seeing analysis results, or alongside analysis when they ask for both. All analysis skills route here for the final recommendation output."
---

# Meta Ads Optimization Recommendations

## When to Use
Loaded when user wants to know what to do about their campaign performance — after seeing analysis results, or alongside analysis when they ask for both.

---

## Prerequisites

Before giving recommendations, you need performance data. Either:
1. Run an analysis skill first (meta-ads-analysis, meta-ads-lead-gen-analysis, meta-ads-audience-analysis, etc.)
2. Or use the analysis data already in the conversation

If no analysis data exists: *"I need to look at your campaign data before I can give recommendations. What time period should I analyze?"*

---

## Session Context Keys This Skill Reads

This skill is the single exit point for all analysis outputs. Read the relevant keys from session context before producing recommendations. See the full table at the bottom of this file.

---

## Step 1: Understand the Full Picture
Review the analysis conclusions and extract:
- Funnel weak points
- Trend signals
- Anomalies
- Data quality

Factor in brand or campaign context:
- Early-stage brand → higher CPL is normal initially
- Testing campaign → focus on learning, not scaling
- Scaling campaign → focus on efficiency and volume capacity

---

## Step 2: Map Problems to Actions
| Problem You See | Likely Cause | Recommended Action |
|---|---|---|
| CTR less than 1% or declining | Creative fatigue, weak hook, audience mismatch | Test new creative angles; narrow audience if broad |
| LPV rate less than 70% or declining | Ad doesn't match landing page; page loads slowly | Check landing page relevance and speed; align messaging |
| Conversion rate declining | Landing page or offer issue | Review page UX, form friction, offer clarity |
| cost_per_result rising | Saturation, competition, fatigue | Check frequency; if > 3, refresh creative or widen audience |
| Frequency > 3 | Audience seeing ads too often | Expand audience or rotate in new creative |
| Spend less than 50% of budget | Audience too narrow, bid too low, ad quality issues | Widen targeting; check if ad is stuck in learning |
| Stuck in Learning phase | Not enough results to exit | Increase budget or consolidate to fewer adsets |
| One segment vastly outperforming | System finding winners correctly | Shift more budget to winner; pause underperformers |
| All healthy but low volume | Budget ceiling | Increase budget gradually — max 20% at a time |

---

## Step 3: Prioritize — Pick 1 to 3 Actions
Prioritization rules:
1. Severity — How much is this hurting results right now?
2. Actionability — Can it be fixed with tools/data available?
3. Confidence — Is the data clear enough to be sure?

Optional additions (0–2, only if genuinely useful):
- Things worth monitoring but not urgent
- Early warning signs to watch

If nothing needs fixing: say so and give 1–2 things to keep an eye on.

---

## Step 4: Landing Page Diagnostic
When funnel data points to the landing page (LPV rate or CVR is the weak point), run this diagnostic before recommending creative or audience changes.

### Step 4a: LPV Rate Check

**First — determine campaign type:**
- Lead gen / Conversions objective → use Lead gen benchmarks below
- E-commerce / Purchase objective → use E-commerce benchmarks below

| Campaign Type | LPV Rate | Indicates |
|---|---|---|
| E-commerce | < 70% | Landing page issue likely |
| E-commerce | ≥ 70% | Ad-to-page alignment is healthy |
| Lead gen | < 50% | Investigate form or page |
| Lead gen | ≥ 50% | LPV is not the bottleneck |

**E-commerce only:** If LPV is healthy but CVR is low → problem is deeper in the funnel (offer, pricing, trust signals).

**Lead gen only:** If LPV is healthy but CPL is still high → check form friction (Step 4c) and CAPI status (Step 4b) before concluding the landing page is fine.

### Step 4b: CAPI Connection Check
Before diagnosing an LPV or CVR problem:
1. Verify Meta pixel is firing on the landing page (check Events Manager → Test Events)
2. Verify Conversions API is connected and sending events back
3. If CAPI is not connected, CPL shown will be artificially high — this is a data problem, not an ad problem

### Step 4c: Form Friction Check (Lead Gen)
- Number of form fields: > 4 fields causes severe drop-off
- Form submit rate benchmark: > 20% is healthy
- If below 20%: recommend reducing fields to essential only
- Mobile usability: can users easily complete the form on phone?

### Step 4d: Disconnect Diagnosis
| Signal | Likely Cause | Recommended Action |
|---|---|---|
| LPV < 70%, CVR OK | Ad-to-page messaging mismatch | Review headline and CTA alignment |
| LPV OK, CVR < 50% | Offer or landing page UX issue | Investigate page content and trust signals |
| Both LPV and CVR low | Funnel-wide problem | Fix landing page first before changing ads |
| LPV and CVR OK but CPL high | Audience too broad or CAPI not connected | Check targeting or verify CAPI is sending offline data |

---

## Step 5: Format Each Recommendation
Every recommendation needs:
1. **What** — Specific action (which ad, which adset, what change, exact numbers)
2. **Why** — The data point that led to this conclusion

Optional:
3. **Expected outcome** — What should improve and by how much (only if it adds real clarity)

Good example:
> Pause ad "Blue Banner v2" — Why: Spent $320 but only 2 leads (CPL $160) vs. account average CPL $45. CTR 0.6% vs. 1.8% average. Drags down overall efficiency.

Bad example:
> "Consider optimizing your creative." (no specific action, no data)

---

## Step 6: Constraints

### Budget change exceptions — when to break the 20% rule
| Condition | Max adjustment per change |
|---|---|
| cost_per_result is 15%+ below target (winner signal) | +50% |
| cost_per_result is above target | 20% (standard) |
| Newly launched, spending < 50% of budget | Check delivery first, then adjust |

### General rules
- Budget changes: Max 20% per adjustment — larger jumps disrupt learning
- Pausing ads: Pause only when CPL is 2x+ higher than average AND meaningful spend. Don't pause during learning phase unless clearly failing.
- Audience changes: Major targeting changes reset learning — prefer creating a new adset.
- Creative recommendations: Be specific about direction: "Test a hook around [specific pain point]", not just "new creative". Suggest 2–3 new variations.
- Landing page issues: Acknowledge we can't see the page directly. Infer from LPV rate and conversion rate data.

### Auction Overlap
When multiple ad sets share overlapping audiences, Meta excludes the lower-value ad from competing.

**Diagnosis:**
1. Check Opportunity score in Account Overview
2. Look for multiple ad sets stuck in Learning Limited simultaneously

**Solutions:**
1. Combine similar ad sets — consolidates learning
2. Turn off overlapping ad sets — move budget to the active ad set

---

## Step 7: Output Format
Brief context sentence, then:

Recommendations:
1. [Action title] — Why: [data-backed reason] — How: [specific steps in Ads Manager]
2. [Action title] — ...
3. [Action title] — ...

Other observations: Things worth watching, not urgent.

---

## Multi-turn Refinement
If user pushes back ("too aggressive", "I don't want to pause that"):
- Adjust only that specific point
- Don't repeat the full list unless asked

---

## Tone
Advisory — professional but warm. Specific and data-driven.
"Execution needs to happen in Meta Ads Manager — I can walk you through the steps."
"Once you've made these changes, I can re-analyze in a few days to see the impact."

---

## Restrictions
- Every recommendation must cite specific data — never vague
- Never recommend without data support
- Maximum 1–3 recommendations — quality over quantity

---

## Session Context — What This Skill Reads

This skill is the single exit point for all analysis outputs. Read the relevant keys from session context before producing recommendations:

| Key | Written By | Description |
|-----|-----------|-------------|
| funnel_weak_points | meta-ads-analysis | Where the biggest funnel drop-off occurs |
| trend_signals | meta-ads-analysis | Direction of key metrics |
| anomalies | meta-ads-analysis | Unusual findings |
| data_quality | meta-ads-analysis | Whether data is sufficient to act on |
| lp_diagnosis | meta-ads-lead-gen-analysis (primary) | Ad side vs. landing page side — lead-gen-specific diagnosis |
| lp_diagnosis_general | meta-ads-analysis (fallback) | Ad side vs. landing page side — general diagnosis |
| capi_status | meta-ads-lead-gen-analysis | CAPI connection status |
| cpl_breakdown | meta-ads-lead-gen-analysis | Which funnel stage is the CPL bottleneck |
| recommended_fix_priority | meta-ads-lead-gen-analysis | Ranked fix order for lead gen |
| budget_reallocation_plan | meta-ads-audience-analysis | Specific audience budget shifts |
| audience_issues | meta-ads-audience-analysis | Overlap and misallocation findings |
| rotation_pipeline | meta-ads-creative-fatigue | Creative inventory by status |
| fatigue_level | meta-ads-creative-fatigue | Per-creative fatigue classification |
| days_until_death | meta-ads-creative-fatigue | Estimated creative lifespan |
| primary_root_cause | meta-ads-drop-diagnosis | Root cause of sudden performance drop |
| recovery_plan | meta-ads-drop-diagnosis | Structured recovery steps |

> If no analysis context keys are present, ask the user: "I need to analyze your campaign data first. What time period should I look at?"
