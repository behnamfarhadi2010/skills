---
name: Google Ads Campaign Builder
description: Use when the user asks to create, audit, or optimize a Google Ads campaign, ad group, keyword list, or ad copy. Produces structured campaign specs ready to implement.

dependencies: []

category: marketing
subcategory: paid-advertising
tags: [google-ads, ppc, sem, ad-copy, keywords, campaign-setup, cpc, conversion]
author:
  name: Behnam Farhadi
  url:
  github:
license: CC-BY-4.0
version: 0.1.0
created: 2026-05-26
updated: 2026-05-26
---

# Google Ads Campaign Builder

## Overview

Guides the model through building or auditing a Google Ads campaign from scratch — from campaign type selection and keyword research to ad group structure, ad copy, and bid strategy. Built for digital marketers who want structured, implementation-ready output rather than generic advice.

## When to use this skill

- The user asks to "set up a Google Ads campaign", "write ad copy for Google Ads", or "audit my campaign".
- The user describes a product/service and asks how to advertise it on Google.
- The user wants help with keyword match types, negative keywords, or Quality Score improvement.
- The user asks about bid strategies (Target CPA, ROAS, Maximize Conversions, etc.).
- The user wants to structure ad groups or build a Search/Shopping/Display/Performance Max campaign.

Do **not** use this skill when:
- The user is asking about Meta Ads, LinkedIn Ads, or other platforms — use a platform-specific skill.
- The user only wants a general explanation of how Google Ads works — answer directly without the full framework.
- The user is asking about GA4 analytics or Tag Manager setup — those are separate workflows.

## Instructions

Work through these steps in order. Skip only if the user has already provided the information.

### 1. Gather campaign context

Ask (or extract from the user's message):
- **Goal**: Leads, sales, traffic, brand awareness, app installs?
- **Product/Service**: What is being advertised? Include URL if available.
- **Target audience**: Location, language, demographics, device preference.
- **Budget**: Daily or monthly budget in CAD/USD.
- **Landing page**: Existing URL or does one need to be drafted?

### 2. Recommend campaign type

Based on the goal, recommend the right campaign type using `resources/campaign-types.md`:
- **Search** — for intent-driven leads/sales
- **Shopping** — for e-commerce product listings
- **Display** — for retargeting or brand awareness
- **Performance Max** — when the user wants Google to optimize across all channels
- **Video (YouTube)** — for top-of-funnel brand awareness

State your reasoning in one sentence before naming the type.

### 3. Build keyword strategy

Produce a keyword list with three tiers:
- **Exact match** `[keyword]` — high intent, tightest control
- **Phrase match** `"keyword"` — moderate reach
- **Broad match** `keyword` — discovery, use sparingly

Always include a **Negative Keywords** section. Minimum 10 negatives relevant to the product.

Group keywords by theme — each theme becomes one ad group.

### 4. Structure ad groups

For each keyword theme:
- Name the ad group (e.g. `[Brand] - Product Type - Intent`)
- List 5–15 keywords
- Note the landing page URL for that group

### 5. Write ad copy

For each ad group, produce at least **one Responsive Search Ad (RSA)**:
- **Headlines**: 10–15 options (max 30 characters each). Pin Headline 1 = primary keyword, Headline 2 = value prop, Headline 3 = CTA.
- **Descriptions**: 4 options (max 90 characters each). Focus on benefits, not features.
- **Display URL path**: `/category/keyword-slug`

Use `resources/ad-copy-template.md` for the output format.

### 6. Recommend bid strategy

Match the bid strategy to the goal:
- New account / no conversion data → **Maximize Clicks** (to gather data)
- Has conversion data (30+ conversions/month) → **Target CPA** or **Maximize Conversions**
- E-commerce with ROAS target → **Target ROAS**
- Brand awareness → **Target Impression Share**

State the recommended strategy and the condition for switching.

### 7. Flag Quality Score risks

Before handing off, check for and flag:
- Keyword-to-ad copy mismatch (low Expected CTR)
- Ad copy not matching landing page headline (low Landing Page Experience)
- Single-keyword ad groups (SKAGs) — note that Google's RSA model has reduced their benefit
- Missing ad extensions (Sitelinks, Callouts, Call, Structured Snippets — all should be added)

### 8. Produce the final output

Deliver a structured brief with these sections:
1. Campaign Settings (type, goal, location, language, budget, bid strategy)
2. Ad Groups (one block per group: name, keywords, negatives, landing page)
3. Ad Copy (RSA headlines + descriptions per ad group)
4. Extensions checklist
5. Recommended next steps (conversion tracking, A/B test plan, review timeline)

## Examples

### Example 1 — Search campaign for a local e-commerce store

**Input:**
> "I run heritageshops.ca selling Newfoundland gifts online. I want to get more sales with Google Ads. Budget is $1,500/month CAD."

**Expected behaviour:**

1. Identify goal: Sales / e-commerce conversions.
2. Recommend **Shopping + Search** combo (Shopping for product-level intent, Search for branded/gift queries).
3. Build keyword themes: `Newfoundland gifts`, `NL souvenirs online`, `buy Newfoundland crafts`, `[brand] Heritage Shops`.
4. Write RSAs for at least 2 ad groups.
5. Recommend **Maximize Conversions** (if conversion tracking is set up) or **Maximize Clicks** to start.
6. Flag: Add Sitelink extensions for product categories; ensure landing pages match ad group themes.

### Example 2 — Auditing an underperforming campaign

**Input:**
> "My Google Ads campaign has a 2% CTR but a 0.8% conversion rate. CPC is $4.50 and I'm spending $800/month with no clear ROI."

**Expected behaviour:**

1. Diagnose likely issues: Low conversion rate despite decent CTR → landing page or offer mismatch.
2. Ask for: campaign type, keywords, landing page URL, conversion action definition.
3. Check keyword match types — overly broad match inflating spend.
4. Review ad copy for CTA alignment with landing page.
5. Recommend: pause low-Quality Score keywords, test a dedicated landing page, switch to Target CPA once 30+ conversions are recorded.
6. Produce a prioritized 3-step action plan.

## Resources

- `resources/campaign-types.md` — reference table of Google Ads campaign types, goals they suit, and when to avoid them.
- `resources/ad-copy-template.md` — blank RSA template with character-count helpers for headlines and descriptions.

## Notes & limitations

- This skill covers **Search, Shopping, Display, and Performance Max** campaigns. YouTube/Video campaigns are supported at a high level only.
- Google Ads UI changes frequently. Bid strategy names and settings paths may shift — always verify in the current Google Ads interface.
- Keyword recommendations are strategic, not based on live search volume data. Pair this skill with Google Keyword Planner or a keyword research tool for volume validation.
- Conversion tracking setup (Google Tag, GA4 import, phone call tracking) is out of scope — treat it as a prerequisite and flag if missing.
- This skill does not have access to the user's Google Ads account. All output is a planning brief for manual or API implementation.

## Changelog

- `0.1.0` — initial version.