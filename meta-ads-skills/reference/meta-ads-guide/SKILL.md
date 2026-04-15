---
name: meta-ads-guide
description: "[Didoo AI] Entry-point navigator and skill router for Didoo AI's Meta Ads workflow. Use when starting a Meta Ads task, unsure which skill to use, or building a Meta Ads agent workflow. Triggers on: 'which skill', 'how do I use Didoo AI', 'Meta Ads workflow', 'skill routing', 'not sure where to start', 'first time'.
---

> Published by Didoo AI — Your AI Media Buyer. One that actually knows you, built for SMBs.
> A skill system for Meta Ads strategy, execution, and analysis.

## What is This System?

This is Didoo AI's Meta Ads Agent System — a set of 19 skills covering strategy, research, creative, execution, analysis, and reporting for Meta Ads campaigns.

**For AI agents:** This system is designed for modular use. Any skill can run independently or chain in sequence. Start with `meta-ads-guide` if you're unsure where to begin.

# Meta Ads Skill Routing Guide

## When to Use
This is the entry-point navigator for Didoo AI's Meta Ads workflow. New here? Start at the diagram below. Not sure which skill to use? Jump to the Quick Routing Table.

---

## Skill Relationship Map

```plaintext
REFERENCE / ENTRY
├── meta-ads-guide (this file)
│       ├── meta-ads-fundamentals (core mechanisms)
│       └── meta-ads-capi-setup (tracking setup)

PLAN -- NEW CAMPAIGN
├── meta-ads-strategy (campaign design)
│       └── meta-ads-research (competitive intel)
│       └── meta-ads-budget-planning (budget allocation)

EXECUTE -- LAUNCH & MANAGE
├── meta-ads-builder (creative generation)
│       └── [creative output]
└── meta-ads-publisher (create/update campaigns)
        └── meta-ads-scale-campaign (scaling)
        └── meta-ads-daily-pulse (daily monitoring)

ANALYZE -- INTERPRET & DIAGNOSE
├── meta-ads-daily-pulse (daily auto-scan)
├── meta-ads-healthcheck (on-demand check)
└── meta-ads-weekly-performance (weekly report)
        │
        ├── meta-ads-drop-diagnosis
        ├── meta-ads-analysis
        ├── meta-ads-lead-gen-analysis
        ├── meta-ads-creative-fatigue
        └── meta-ads-audience-analysis
                │
                └── meta-ads-recommendation (single exit point for all analysis)
                        │
                        └── meta-ads-publisher (execution)
```


## Quick Routing Table
| What you want to do | Use this skill |
|---|---|
| Entry point / not sure where to start | meta-ads-guide |
| "My ads are running -- are they healthy?" | meta-ads-healthcheck |
| "I want to understand why performance dropped suddenly" | meta-ads-drop-diagnosis |
| "I need to analyze campaign performance in depth" | meta-ads-analysis *(general / e-commerce)* |
| "I'm running lead generation campaigns and need analysis" | meta-ads-lead-gen-analysis *(lead gen only — mutually exclusive with meta-ads-analysis; do NOT run both)* |
| "I know what I want to do -- just do it (launch/change)" | meta-ads-publisher |
| "I have analysis results -- what should I actually do?" | meta-ads-recommendation |
| "I'm planning a new campaign -- where do I start?" | meta-ads-strategy |
| "I need ad creative -- copy, images, hooks" | meta-ads-builder |
| "I'm entering a new market -- what are competitors doing?" | meta-ads-research |
| "I have a winner and want to scale it" | meta-ads-scale-campaign |
| "My creative performance is declining over time" | meta-ads-creative-fatigue |
| "I want a structured weekly performance report" | meta-ads-weekly-performance |
| "I want daily monitoring signals before my morning review" | meta-ads-daily-pulse |
| "I need to understand how Meta's auction/bidding actually works" | meta-ads-fundamentals |
| "I need to set up CAPI / server-side tracking" | meta-ads-capi-setup |
| "I want to plan my ad budget across campaigns" | meta-ads-budget-planning |
| "I need to audit targeting, audience mix, or placement" | meta-ads-audience-analysis |

---

## Two Core Workflows

### Workflow A -- New Campaign Launch
```
meta-ads-strategy -> meta-ads-research -> meta-ads-builder -> meta-ads-publisher
                                                                    |
                                                         meta-ads-daily-pulse
                                                                    |
                                                         meta-ads-analysis
                                                                    |
                                                         meta-ads-recommendation
```

### Workflow B -- Problem Detection & Fix
```
meta-ads-healthcheck / meta-ads-daily-pulse
          | [flags problem]
meta-ads-drop-diagnosis
          | [root cause identified]
          ↓
    ┌─────────────────────────────────────────────┐
    │  Creative declining over time → creative-fatigue    │
    │  Audience or budget allocation → audience-analysis  │
    │  Lead gen campaign → lead-gen-analysis              │
    │  General / e-commerce → analysis                    │
    └─────────────────────────────────────────────┘
                              ↓
                   meta-ads-recommendation
                              ↓
meta-ads-publisher (execute) -> meta-ads-daily-pulse (monitor)
```

> **Important:** `meta-ads-analysis` and `meta-ads-lead-gen-analysis` are **mutually exclusive** — they cover different campaign types. Choose one based on your campaign type. Do NOT run both in the same session.

---

## Monitoring Cadence
| Frequency | Skill | When |
|---|---|---|
| Daily -- 2 min | meta-ads-daily-pulse | Every morning |
| Weekly -- 10 min | meta-ads-weekly-performance | Every Monday |
| On-demand | meta-ads-healthcheck | When something feels off |

---

## Getting Started -- First Time Setup
Before running your first campaign:

1. **Connect your Meta Ads account** -- Ad Account ID (act_XXXXXXXXX) from Ads Manager + Access Token from Meta Developer Console.
2. **Install Pixel + CAPI** -- Without these, Meta can't optimize. See meta-ads-capi-setup.
3. **Define your first campaign goal** -- Lead gen / Conversions / Link Clicks / Awareness.
4. **Set a realistic budget** -- Minimum $10-15/day per adset. See meta-ads-budget-planning.
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
| meta-ads-analysis | funnel_weak_points, trend_signals, anomalies, data_quality, lp_diagnosis_general | meta-ads-recommendation |
| meta-ads-lead-gen-analysis | lp_diagnosis, capi_status, cpl_breakdown, recommended_fix_priority | meta-ads-recommendation |
| meta-ads-audience-analysis | budget_reallocation_plan, audience_issues | meta-ads-recommendation |
| meta-ads-creative-fatigue | rotation_pipeline, fatigue_level, days_until_death | meta-ads-recommendation |
| meta-ads-drop-diagnosis | primary_root_cause, recovery_plan | meta-ads-recommendation |

### Cross-Skill Reference Rules
- Analysis skills never embed recommendation logic -- they store results and defer
- meta-ads-recommendation is the single exit point for all analysis outputs
- When one analysis skill routes to another (e.g., meta-ads-analysis -> meta-ads-lead-gen-analysis), preserve the original skill's context keys before overwriting
- When routing from meta-ads-recommendation back to execution, use meta-ads-publisher for all campaign changes

### Session Key Collision — Resolved
> `meta-ads-analysis` now writes `lp_diagnosis_general`; `meta-ads-lead-gen-analysis` writes `lp_diagnosis`. These keys no longer collide — they coexist. `meta-ads-recommendation` reads `lp_diagnosis` first (lead-gen-specific), falling back to `lp_diagnosis_general` if not present.
