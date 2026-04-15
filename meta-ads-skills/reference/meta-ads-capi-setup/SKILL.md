---
name: meta-ads-capi-setup
description: "[Didoo AI] Step-by-step guide for setting up Meta Conversions API (CAPI) — server-side event tracking that improves conversion measurement accuracy and reduces CPA discrepancy vs pixel-only tracking. Use when setting up tracking for a new account or when CPA shown in Meta Ads Manager is significantly higher than actual leads/sales."
---

# Meta Ads CAPI Setup Guide

## Required Credentials
| Credential | Where to Get | Used For |
|-----------|-------------|---------|
| META_ACCESS_TOKEN | Meta Developer Console → Graph API Explorer → Generate Token | All Meta Marketing API calls |
| META_PIXEL_ID | Meta Events Manager → select your Pixel → copy Pixel ID | Identifying which pixel to configure |

> **Token note:** The System User access token (generated in Business Settings) is the primary credential for CAPI API calls. You still need a registered Meta App (App ID) to set up the CAPI integration in Events Manager — the App ID is required when creating the System User and configuring token permissions. Make sure the token includes these scopes: `ads_management`, `business_management`, `pages_read_engagement`.

---

## When to Use
When setting up tracking for a new account, or when CPA shown in Meta Ads Manager is significantly higher than actual leads/sales (indicating a tracking gap). CAPI is especially critical for lead generation campaigns where offline conversions are the true business outcome.

---

## What CAPI Does (vs Pixel Only)
- **Pixel only**: Browser-based. Tracks users who click your ad and then take action on your website. Misses users who convert on different devices, after clearing cookies, or without clicking.
- **CAPI**: Server-to-server. Sends conversion events directly from your server to Meta. More accurate, more reliable, harder to block.

Together: Pixel + CAPI gives Meta the most complete picture → better optimization → lower CPA over time.

If you're not using CAPI, you're likely overpaying for every lead. The gap can be 20–50% depending on industry and browser mix.

---

## Prerequisites
Before setting up CAPI, you need:
1. Meta Pixel installed on your website
2. A Meta App registered at developers.facebook.com (App ID required to configure CAPI in Events Manager)
3. A server environment that can send HTTP requests (or a partner integration like Shopify, WooCommerce, Zapier, or Google Tag Manager Server-side)
4. Access to your Meta Business Manager with pixel-level permissions

---

## Step 1: Choose Your Integration Method

### Recommended for Most SMBs — Native Platform Integration
Use your e-commerce platform's built-in Meta integration. This is the easiest path:

| Platform | How to Connect |
|----------|---------------|
| Shopify | Shopify Admin → Sales Channels → Meta |
| WooCommerce | Plugins → Meta for WooCommerce |
| Other platforms | Check Meta's partner directory at meta.com/partners |

**Steps (native integration):**
1. Go to Meta Events Manager → select your Pixel
2. Click "About CAPI" → "Get Started"
3. Select your platform from the partner list
4. Follow the on-screen prompts to authorize the connection
5. Select which events to send (see Step 3 below)

---

### Advanced — Manual Integration Options
Only use these if you are a developer or using a platform without native Meta integration.

| Method | Best For | Complexity |
|--------|----------|------------|
| Google Tag Manager Server-Side | Teams already using GTM | Medium |
| Zapier / Make.com | No-code teams, non-standard tech stacks | Medium |
| Meta Conversions API via Meta Business Manager | Simple server events without developer resources | Low |
| Direct API Integration | Developers building custom infrastructure | High |

For Direct API: use your META_ACCESS_TOKEN to send events to `https://graph.facebook.com/v21.0/{pixel_id}/events`.

---

## Step 2: Generate a System User Access Token
1. Go to Meta Business Manager → Business Settings → System Users
2. Create a new system user with "Analytics" or "Ads" permissions
3. Generate an access token with: `ads_management`, `business_management`, `pages_read_engagement`
4. Save this token securely — you'll need it for all API calls

---

## Step 3: Configure Which Events to Send
1. Go to Meta Events Manager → select your Pixel
2. Click "About CAPI" → "Get Started"
3. Select your integration method
4. Configure which events to send:

| Event | Recommended For |
|-------|-----------------|
| PageView | All campaigns — helps Meta build audience |
| ViewContent | E-commerce product pages |
| AddToCart | E-commerce |
| Purchase | E-commerce (critical — this is your conversion event) |
| Lead | Lead generation (critical — matches your optimization goal) |
| CompleteRegistration | Webinar, courses, freemium signups |
| Contact | Local service businesses |

---

## Step 4: Test Your CAPI Connection
In Meta Events Manager:
1. Select your pixel → "Test Events" tab
2. Trigger the events on your website (submit a form, complete a purchase, etc.)
3. Check that events appear in the test panel within seconds

**What to verify:**
- Events are arriving with Deduping source confirmed (CAPI and Pixel aren't double-counted)
- Event IDs are being passed (for deduplication)
- Parameter completeness — more parameters = better optimization

**Event parameters that improve CAPI quality:**
- event_id — unique ID per event, enables deduplication
- user_data — email, phone, country (hashed with SHA-256)
- custom_data — value, currency, content_name, content_category

---

## Step 5: Verify CAPI Is Working Correctly
In Meta Events Manager, check:
1. Event Quality Score — should be "Active" with green status
2. Matching Quality — how many user parameters Meta can match to real users
3. No significant gap between pixel events and CAPI events

**Matching Quality benchmarks:**
- 7+ parameters with good hash quality → Excellent
- 5–6 parameters → Good
- < 5 parameters → Poor — optimize your event setup

---

## Step 6: Standard Events vs. Custom Events

### Standard Events (Preferred)
Meta's predefined event names: Lead, Purchase, PageView, ViewContent, AddToCart, InitiateCheckout, CompleteRegistration. Better optimization because Meta knows exactly what the event means.

### Custom Events
Use only when no standard event fits. Define custom name and parameters yourself. Required for: offline sales, phone calls, specific business workflows.

---

## CAPI Common Issues and Fixes
| Issue | Likely Cause | Fix |
|-------|--------------|-----|
| CAPI events not appearing in Meta | Token expired or permissions wrong | Regenerate access token with correct permissions |
| Huge gap between pixel and CAPI volumes | CAPI not connected properly | Check Events Manager → CAPI status |
| CPA artificially high in Meta | CAPI not sending offline conversions (lead gen) | Verify Lead event is firing server-side |
| Events marked as "Unmatched" | User data not being passed correctly | Ensure email/phone is hashed before sending |
| Duplicate events (double counting) | Both pixel and CAPI firing without deduplication | Pass event_id in both pixel and CAPI calls |

---

## Lead Generation — CAPI Is Especially Critical
For lead gen campaigns, the conversion happens offline — someone fills out a form, you call them, they sign up. Meta pixel can't see this.

**What CAPI must send for lead gen:**
1. Lead event when form is submitted (server-side, not just pixel)
2. Lead event when you mark a lead as qualified in your CRM (offline → Meta)
3. Revenue data if possible (even approximate) — helps Meta optimize for quality

**CRM Integrations that support CAPI for leads:** HubSpot, Salesforce, Zapier, Make.com.

If you're not sending offline lead data back to Meta: Meta is optimizing for form submissions only. You're paying for leads that never convert.

---

## Event Deduplication — Preventing Double Counting
When both Pixel and CAPI send the same event (same event_id), Meta deduplicates automatically. Requirements:
- Same event_id in both pixel and CAPI call
- Same event_name
- Events fired within the same 48-hour window
- Same user data (at least one: email, phone, client_ip, client_user_agent)

---

## Security Notes
- Access tokens for CAPI should be treated like passwords — store in environment variables, never in code repositories
- Use HTTPS for all CAPI calls
- Hash PII (email, phone) using SHA-256 before sending to Meta
