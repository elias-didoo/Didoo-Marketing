---
name: meta-ads-builder
description: "[Didoo AI] Generates Meta Ads creative — ad copy, headlines, hooks, and visual concepts. Use when user needs ad creative for a new campaign or to refresh existing ads. Works after meta-ads-strategy defines the campaign direction."
---

## Allowed Tools
web_search, web_fetch, image-tools

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | Fetching creative performance data (optional) |
| GEMINI_API_KEY | Google AI Studio → Create API Key | AI-powered image generation (optional — only if using AI image generation) |

> **Credentials note:** No Meta API credentials are required for basic creative copy generation. META_ACCESS_TOKEN is optional — only needed if you want to pull existing creative performance data (CTR, frequency by creative) to inform the new variations. Image generation tools (GEMINI_API_KEY) are also optional enhancements only.

## When to Use
When user needs actual ad creative — images, copy, headlines, or full ad variations. Works after meta-ads-strategy defines the campaign direction.

---

## Step 1: Get Campaign Context
Before generating creative, understand:
- What is the campaign goal? (Leads, sales, traffic?)
- Who is the target audience? (Their pain points, desires, language they use)
- What's the offer? (What does someone get when they click?)
- What's the CTA? (Sign up, buy, download, book call?)

If strategy already exists from meta-ads-strategy, use that. If not, ask a few quick questions.

---

## Step 2: Define Creative Direction

### Hook Formulas That Work on Meta
**Problem-focused hooks:**
- "Tired of [pain point]?" — speaks directly to frustration
- "What nobody tells you about [topic]" — curiosity gap
- "[Number] reasons your [problem]" — list format, scannable

**Social proof hooks:**
- "[X] businesses already use [outcome]" — social validation
- "The results speak for themselves" — outcome focused

**Benefit-focused hooks:**
- "Get [specific outcome] without [common obstacle]" — direct value prop
- "The fastest way to [desired state]" — speed appeal

**Fear-of-missing-out hooks:**
- "Limited time [offer]" — scarcity
- "Only [X] spots left" — urgency

---

## Step 3: Generate Ad Variations
Produce 2–3 distinct creative variations, each with:
1. Hook/headline — what stops the scroll
2. Body copy — 1–2 short paragraphs, benefits-focused
3. CTA — button text or call-to-action
4. Visual direction — description for image generation

- Variation 1: Emotional/problem-focused
- Variation 2: Rational/benefit-focused
- Variation 3: Social proof or urgency-focused

---

## Step 4: Write Ad Copy — Rules
- First line must hook — grab attention in the first 10 words
- Keep it scannable — short sentences, white space, easy to read on mobile
- Focus on benefits, not features — "Save 2 hours a day" not "Includes automation tool"
- Match the audience's language — how they talk about the problem, not industry jargon
- One clear CTA — don't ask for multiple things

### Copy Length Guide
| Format | Length | Use For |
|---|---|---|
| Single image ad | 40–125 characters | Quick, punchy, high-impact |
| Carousel | Headline 40 chars, body 200 chars max | Storytelling, multiple points |
| Collection ad | Headline 40 chars, body 100 chars | Product showcase |

---

## Step 5: Generate Visual Concepts
For each variation, describe the visual in enough detail for an AI image generator or designer:
- Type: Carousel, single image, or video?
- Subject: [person using product / product close-up / illustration]
- Background: [solid color / lifestyle setting / gradient]
- Text overlay: "[short headline]"
- Mood/Tone: [professional, urgent, warm, minimalist]

To generate images with AI: use image-tools with a prompt based on the visual direction above.

---

## Step 6: Quality Check
Before finalizing, review each variation:
- Does the hook match the audience's pain point or desire?
- Is the CTA clear and singular?
- Is the copy scannable on mobile?
- Does the visual match the copy tone?
- Is the claim believable and backed up?

**Ad Relevance Diagnostics self-check (for reference):**
- Quality Ranking: Higher when creative is clear and relevant to audience
- Engagement Rate Ranking: Higher when the hook is strong and interactive
- Conversion Rate Ranking: Higher when the offer and CTA are aligned

These require 500+ impressions to appear in Meta. They are diagnostic signals, not guaranteed outcomes.

---

## Key Restrictions
- Do not make up fake statistics or testimonials
- Do not use misleading claims (Meta ad policies forbid this)
- Do not use competitor names in ads without permission
- Always ensure the ad creative matches the landing page it points to
