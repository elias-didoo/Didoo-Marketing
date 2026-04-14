---
name: meta-guide
description: Entry-point navigator for Didoo AI's Meta Ads workflow. Contains the Skill Relationship Map, Quick Routing Table, two core workflows, monitoring cadence, getting started guide, and first-timer mistakes. Start here if new to the system or unsure which skill to use.
---

> Published by Didoo AI — Your AI Media Buyer. One that actually knows you, built for SMBs.
> A skill system for Meta Ads strategy, execution, and analysis.

# Meta Ads Skill Routing Guide

## When to Use
This is the entry-point navigator for Didoo AI's Meta Ads workflow. New here? Start at the diagram below. Not sure which skill to use? Jump to the Quick Routing Table.

---

## Skill Relationship Map

```
+-------------------------------------------------------------------------+
|  REFERENCE / ENTRY                                                       |
|                                                                          |
|  +------------+   +--------------+   +-------------------------------+   |
|  | meta-guide |-->| meta-funda- |   |  meta-capi-setup             |   |
|  | (this file) |   | mentals     |   |  (tracking setup)            |   |
|  +------------+   +--------------+   +-------------------------------+   |
|          |                                                                  |
+----------+------------------------------------------------------------------+
           |
           v
+-------------------------------------------------------------------------+
|  PLAN -- NEW CAMPAIGN                                                    |
|                                                                          |
|  +--------------------+     +---------------------+                     |
|  |  meta-ads-         |---->|  meta-research       |                     |
|  |  strategy          |     |  (competitive intel) |                     |
|  |  (campaign design)  |     +---------------------+                     |
|  +--------+-----------+                                              |
|           |         +---------------------+                              |
|           +------->| meta-budget-         |                              |
|                     | planning            |                              |
|                     | (budget allocation)  |                              |
|                     +---------------------+                              |
+-------------------------------------------------------------------------+
           |
           v
+-------------------------------------------------------------------------+
|  EXECUTE -- LAUNCH & MANAGE                                             |
|                                                                          |
|  +--------------------+     +---------------------+                     |
|  |  meta-ads-builder   |---->|  [creative output]  |                     |
|  |  (creative gen)     |     +---------------------+                     |
|  +--------+-----------+                                              |
|           |                                                              |
|           v                                                              |
|  +--------------------+     +---------------------+                     |
|  |  meta-ads-         |<----|  meta-scale-         |                     |
|  |  publisher         |     |  campaign           |                     |
|  |  (create/modify)    |     |  (scaling ops)      |                     |
|  +--------------------+     +---------------------+                     |
+-------------------------------------------------------------------------+

          ^
          |
+-------------------------------------------------------------------------+
|  ANALYZE -- INTERPRET & DIAGNOSE                                        |
|                                                                          |
|  +----------------+  +------------------+  +------------------------+  |
|  | meta-daily-    |  |    meta-ads-     |  |  meta-weekly-           |  |
|  | pulse          |  |    healthcheck    |  |  performance            |  |
|  | (daily auto)   |  |    (on-demand)   |  |  (weekly auto)           |  |
|  +-------+--------+  +--------+---------+  +-----------+------------+  |
|          |                    |                         |                |
|          |               +----+----+                    |                |
|          |               v         v                    |                |
|          |  +---------------+  +-------------------+   |                |
|          +->| meta-drop-    |  |  meta-ads-         |   |                |
|              | diagnosis    |  |  analysis         |   |                |
|              | (sudden drop)|  |  (general deep)   |   |                |
|              +-------+------+  +--------+----------+   |                |
|                     |            +------+------+      |                |
|                     |            v            v       |                |
|                     |  +------------+  +---------+  +---------+  |
|                     |  | meta-      |  |  meta-  |  |  meta-  |  |
|                     |  | audience-  |  | lead-gen|  | creative|  |
|                     |  | analysis   |  | analysis|  | fatigue |  |
|                     |  | (targeting)|  | (leads) |  | (creative)| |
|                     |  +------------+  +---------+  +---------+  |
+---------------------+--------------------------------------------------+

          ^
          |
+-------------------------------------------------------------------------+
|  RECOMMEND -- SINGLE EXIT POINT FOR ALL ANALYSIS                        |
|                                                                          |
|  +--------------------------------------+                                |
|  |    meta-ads-recommendation           |<----- all analysis converges   |
|  |    (specific action plan + priority) |                                |
|  +---------------------+-----------------+                               |
+-----------------------+-----------------------------------------------+
                        |
                        v
               BACK TO EXECUTE
```

---

## Quick Routing Table
| What you want to do | Use this skill |
|---|---|
| "My ads are running — are they healthy?" | meta-ads-healthcheck |
| "I want to understand why performance dropped suddenly" | meta-drop-diagnosis |
| "I need to analyze campaign performance in depth" | meta-ads-analysis |
| "I'm running lead generation campaigns" | meta-lead-gen-analysis |
| "I know what I want to do — just do it (launch/change)" | meta-ads-publisher |
| "I have analysis results — what should I actually do?" | meta-ads-recommendation |
| "I'm planning a new campaign — where do I start?" | meta-ads-strategy |
| "I need ad creative — copy, images, hooks" | meta-ads-builder |
| "I'm entering a new market — what are competitors doing?" | meta-research |
| "I have a winner and want to scale it" | meta-scale-campaign |
| "My creative performance is declining over time" | meta-creative-fatigue |
| "I want a structured weekly performance report" | meta-weekly-performance |
| "I want daily monitoring signals before my morning review" | meta-daily-pulse |
| "I need to understand how Meta's auction/bidding actually works" | meta-fundamentals |
| "I need to set up CAPI / server-side tracking" | meta-capi-setup |
| "I want to plan my ad budget across campaigns" | meta-budget-planning |
| "I need to audit targeting, audience mix, or placement" | meta-audience-analysis |

---

## Two Core Workflows

### Workflow A — New Campaign Launch
```
meta-ads-strategy -> meta-research -> meta-ads-builder -> meta-ads-publisher
                                                                    |
                                                         meta-daily-pulse
                                                                    |
                                                         meta-ads-analysis
                                                                    |
                                                         meta-ads-recommendation
```

### Workflow B — Problem Detection & Fix
```
meta-ads-healthcheck / meta-daily-pulse
          | [flags problem]
meta-drop-diagnosis
          | [root cause identified]
meta-ads-analysis / meta-lead-gen-analysis / meta-creative-fatigue / meta-audience-analysis
          | [analysis complete]
meta-ads-recommendation
          | [action accepted]
meta-ads-publisher
          | [change made]
meta-daily-pulse (monitor)
```

---

## Monitoring Cadence
| Frequency | Skill | When |
|---|---|---|
| Daily -- 2 min | meta-daily-pulse | Every morning |
| Weekly -- 10 min | meta-weekly-performance | Every Monday |
| On-demand | meta-ads-healthcheck | When something feels off |

---

## Getting Started -- First Time Setup
Before running your first campaign:

1. **Connect your Meta Ads account** -- Ad Account ID (act_XXXXXXXXX) from Ads Manager + Access Token from Meta Developer Console.
2. **Install Pixel + CAPI** -- Without these, Meta can't optimize. See meta-capi-setup.
3. **Define your first campaign goal** -- Lead gen / Conversions / Link Clicks / Awareness.
4. **Set a realistic budget** -- Minimum $10-15/day per adset. See meta-budget-planning.
5. **Launch** -- meta-ads-strategy -> meta-ads-builder -> meta-ads-publisher (always launch PAUSED, review, then activate).
6. **Wait 5-7 days** -- Don't judge during Learning Phase. Changing things during Learning resets the clock.

---

## Common First-Timer Mistakes
- Budget too low ($1-5/day) -- Learning Phase never completes
- Too many campaigns at once -- spread thin, none learns
- Expecting results in 24-48 hours -- give Meta time to optimize
- Changing too many things at once -- change one variable, then wait
- Not connecting CAPI -- CPA looks worse than reality

---

## Key Constraint -- Analysis vs. Recommendation Boundary
- meta-ads-analysis (and its variants) -- explains what happened. Data only. Never tells you what to do.
- meta-ads-recommendation -- tells you what to do, why, and in what order. Requires analysis data first.
- Never output recommendations without running analysis first (or having existing data in session context).

---

## Session Context Conventions

Analysis skills store their output in session context. Downstream skills read from there. This is the shared contract between skills.

### Analysis -> Recommendation Data Flow

| Skill (Writer) | Keys Written | Skill (Reader) |
|---------------|-------------|----------------|
| meta-ads-analysis | funnel_weak_points, trend_signals, anomalies, data_quality, lp_diagnosis | meta-ads-recommendation |
| meta-lead-gen-analysis | lp_diagnosis, capi_status, cpl_breakdown, recommended_fix_priority | meta-ads-recommendation |
| meta-audience-analysis | budget_reallocation_plan, audience_issues | meta-ads-recommendation |
| meta-creative-fatigue | rotation_pipeline, fatigue_level, days_until_death | meta-ads-recommendation |
| meta-drop-diagnosis | primary_root_cause, recovery_plan | meta-ads-recommendation |

### Cross-Skill Reference Rules
- Analysis skills never embed recommendation logic -- they store results and defer
- meta-ads-recommendation is the single exit point for all analysis outputs
- When one analysis skill routes to another (e.g., meta-ads-analysis -> meta-lead-gen-analysis), preserve the original skill's context keys before overwriting
- When routing from meta-ads-recommendation back to execution, use meta-ads-publisher for all campaign changes

### Session Key Collision Warning
> meta-ads-analysis and meta-lead-gen-analysis both write lp_diagnosis. If you run them both in the same session, copy needed values to your working notes before running the second one -- it will overwrite the first.
