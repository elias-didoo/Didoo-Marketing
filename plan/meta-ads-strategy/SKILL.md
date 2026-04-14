---
name: meta-ads-strategy
description: "[Didoo AI] Defines campaign strategy for Meta Ads — sets objective, targeting, structure, budget, and bidding approach. Use as the first step before launching any new Meta Ads campaign."
---

# Meta Ads Strategy Planning

## When to Use
Before launching any Meta Ads campaign — to define what you're trying to achieve, who you're targeting, and how to structure the campaign for success. Use as the first step in a new campaign workflow.

---

## Step 1: Understand the Product and Goal
Ask the user:
- What are you advertising? (Product, service, or brand)
- What's the offer? (What does someone get when they click?)
- What's the call-to-action? (Sign up, buy now, download, book a call?)

Get the product URL if available.

---

## Step 2: Define Campaign Objective
Ask: "What does success look like for this campaign?"

Match to Meta's campaign objectives:
- Leads → Lead generation campaign
- Sales / purchases → Conversions campaign
- Traffic / clicks → Link clicks campaign
- Brand awareness → Awareness campaign
- App installs → App engagement campaign

---

## Step 3: Define Target Audience
Ask about:
- Location: Which countries/regions?
- Age range: Who buys this?
- Gender: Relevant or not?
- Interests: What topics does the audience care about?
- Behaviors: Any relevant behaviors (e.g., online shoppers, business owners)?
- Custom audiences: Do they have an existing customer list to target?

Be specific — vague audiences ("everyone") waste budget. Push for specificity even if it means starting narrow.

---

## Step 4: Define Campaign Structure
Decide:
- CBO (Campaign Budget Optimization) vs ABO (Adset Budget Optimization)

**CBO vs ABO — Decision Table:**
| Scenario | Structure | Bidding strategy |
|---|---|---|
| Testing a new offer | CBO | Lowest cost |
| Same offer, multiple audiences | CBO | Lowest cost |
| Need to control CPL / have a specific CPA target | ABO | Cost per result goal (Cost cap) |
| Multiple geo, budget allocations differ by location | ABO | Cost per result goal (Cost cap) |
| Scaling a proven winner | ABO | Target cost |
| Always-on with stable performance | CBO | Lowest cost |

**When to use CBO:**
- You want Meta to find the best-performing audience automatically
- You have 2–3 test adsets and want to optimize across them

**When to use ABO:**
- You have specific budget allocations per audience or geo
- You need to control cost per result tightly
- You want to run clean A/B tests with equal budget per segment

**Number of adsets and ads:**
- Start with 2–3 adsets, not 20. Each needs enough data to learn.
- 2–4 ad variations per adset to start, not 10.

---

## Step 5: Define Budget and Bidding

**Budget:**
- What's the daily or lifetime budget?
- Is this a test (smaller budget) or scaling (ready to increase)?
- Rule of thumb: need ~50 results per week per adset to exit learning phase

**Bidding strategy:**
- Lowest cost (default): Meta finds the cheapest results — good for volume
- Cost per result goal (Cost cap): Cap the cost per result — good for controlling CPL/CPA
- Bid cap: Set a max bid — gives more control but may limit delivery
- Target cost: Maintain stable cost — good for scaling

---

## Step 6: Competitive Positioning
If relevant (for DTC, SaaS, or brands entering new markets):
- Who are the main competitors in the ad auction?
- What's the typical CPM and CPL in this category?
- Any seasonal or market timing factors to consider?

Use web search to research benchmarks if user doesn't know.

---

## Step 7: Document the Strategy
Output a clear strategy brief with:
- Product, Campaign Objective, Offer, CTA
- Target Audience (location, age/gender, interests, exclusions)
- Campaign Structure (number of adsets, budget type, daily budget)
- Bidding Strategy
- Timeline and decision point

---

## Key Rules
- Always start with clear success metrics: what CPL/CPA is good for this business?
- Structure for learning — give Meta enough data to optimize
- Don't over-segment audiences — too many small adsets = not enough data per adset
- Budget must be realistic — $5/day won't get meaningful learning data
