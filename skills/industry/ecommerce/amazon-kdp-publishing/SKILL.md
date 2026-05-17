---
name: Amazon KDP Publishing
description: Use when self-publishing a book on Amazon KDP. Covers format choice, technical specs, title/subtitle/description, keywords, categories, pricing/royalties, launch, ads, and review compliance.
dependencies: []

category: industry
subcategory: ecommerce
tags: [kdp, amazon, self-publishing, kindle, paperback, hardcover, book-marketing, publishing]
author:
  name: Nima Aksoy
  url: https://nimaaksoy.com
  github: nimaaksoy
license: CC-BY-4.0
version: 0.1.0
created: 2026-05-17
updated: 2026-05-17
---

# Amazon KDP Publishing

## Overview

End-to-end self-publishing through Amazon Kindle Direct Publishing — picking the right format, building a manuscript and cover that meet KDP specs exactly, writing metadata that converts, pricing for the actual royalty cliffs, and launching with ads + reviews without breaking Amazon's rules.

The skill is opinionated where the data is clear (price at $9.99 not $9.98 on US paperback; never put review requests in the description; use one wrap PDF for print covers) and lays out the decision when the answer depends on the book (genre, budget, marketplace).

## When to use this skill

- The user wants to publish a book on Amazon KDP — fiction, nonfiction, low-content, or print-only.
- The user has a manuscript and needs help formatting, packaging, pricing, or launching.
- The user wants to validate a niche or do competitive research before writing.
- The user is troubleshooting a KDP rejection (cover bleed, margin, font, ISBN).
- The user wants to set up an Amazon Ads stack for their book.
- The user is unsure about Kindle vs paperback vs hardcover trade-offs.

Do **not** use when:

- The user wants to write the book itself with deep craft guidance — that's a writing skill, not KDP-specific.
- The user wants a non-Amazon platform (IngramSpark, Draft2Digital, Smashwords) — KDP-specific rules don't transfer cleanly.
- The user wants academic, textbook, or magazine publishing — different workflows.

## Instructions

KDP success is a production line, not a single decision. Work in this order; each step gates the next.

### Step 1 — Validate the niche before writing the book

A great book in a muddled niche loses to a competent book in a clear niche. Use the market-research loop in `resources/market-research.md`:

1. Open the exact Amazon subcategory your book would live in.
2. Pull the top 20 books in that subcategory and record: title, subtitle, cover style, format mix, price, page count, rating, review count, series status.
3. Read 3-star reviews to find what readers wanted but didn't get — that's your opportunity map.
4. Read the "Look Inside" of the top 5 to see real competitive quality.
5. Score the niche on the 10-point scorecard (demand, fit, packaging clarity, competition shape, follow-on potential).

If the score is below 30/50, change the niche. Don't write into a dead shelf.

### Step 2 — Decide the format stack

For the first launch, default to **Amazon.com + Kindle + paperback**. Skip hardcover and Expanded Distribution until the page is converting.

Use `resources/technical-specs.md` for the side-by-side: accepted file formats, trim sizes, bleed rules, margin minimums, image DPI, color profiles, font minimums. The biggest format decisions that lock the rest of the workflow:

- **Bleed or no bleed?** If any interior page bleeds, the entire interior file must be set up with bleed (PDF only). For a 6×9 paperback with bleed, the page size becomes 6.125 × 9.25.
- **Trim size?** Default 6×9 for US paperback. Don't pick exotic trims unless the content demands it.
- **Color or black ink?** Color interiors hit 4×–10× the printing cost. Use only when essential.

### Step 3 — Format the manuscript to KDP specs the first time

Don't write in one format and reformat for KDP at the end. Pick the destination format up front:

- **Kindle**: write in DOCX or build in EPUB/KPF. Validate in **Kindle Previewer** before upload. Always.
- **Print interior**: if you need bleed, build PDF from the start. If not, DOCX is OK.
- **Print cover**: always a single wrap PDF (back + spine + front), built from KDP's template/calculator. 300 DPI, embedded fonts, flattened layers, 0.125" bleed on outer edges.

For full specs (margins by page count, trim ranges, font minimums, image DPI), see `resources/technical-specs.md`.

### Step 4 — Write the manuscript with structure that earns reviews

The book that gets bought needs metadata + cover; the book that earns reviews needs craft. See `resources/writing-and-editing.md` for:

- Fiction outline template (hook → inciting incident → midpoint shift → dark moment → resolution)
- Nonfiction outline template (promise → problem → framework → chapters → integration → next step)
- The editing ladder (structural self-edit → beta readers → developmental edit → line/copy edit → proofread)

If budget is tight, spend on **cover + copy edit/proof + market fit** first. A gorgeous cover can't rescue a broken book; a strong book with bad packaging dies unseen.

### Step 5 — Build the cover

Two covers, two different files:

- **Kindle marketing cover**: 2560 × 1600 px JPEG, RGB, 300 DPI, under 5 MB. If the cover is mostly light, add a thin border so it doesn't disappear on Amazon's white background.
- **Print cover**: one wrap PDF using KDP's calculator/template. 300 DPI assets, CMYK images, embedded fonts, flattened transparencies. Spine text only if 79+ pages. Keep important content ≥ 0.25" from the trim edge (paperback) or ≥ 0.635" from the edge (hardcover).

Commercial cover rules that matter more than the technicals:

- **One focal idea**, not five.
- **Title readable at thumbnail size** (test it at 200 px wide on your phone).
- **One genre signal** — the reader should know the shelf in one second.
- **Title and author exactly match the metadata.**

### Step 6 — Write the metadata for clicks and compliance

Metadata is where many self-publishers quietly lose sales. Strict KDP rules, all enforced. See `resources/metadata.md` for the full ruleset.

| Field | Rule | Action |
|---|---|---|
| Title | Must match the cover exactly. No promo claims ("free", "bestselling"), rank claims, trademarks, HTML, junk formatting. | Lead with the main promise/hook. |
| Subtitle | Optional. Title + subtitle under 200 characters. Same rules as title. | Sharpen audience, outcome, or subgenre. |
| Description | Up to 4,000 chars. Basic HTML allowed (b, i, u, ul, ol, li, h4-h6, br, p). HTML counts toward limit. **No URLs, no testimonials, no review requests, no promo language, no time-sensitive info, no keyword stuffing.** | Write for scanning: hook → promise → proof → CTA. |
| Keywords | Up to 7 phrases. Don't repeat title/contributor/categories. No subjective ("best ever"), time-sensitive ("new"), unowned brands, or Amazon program names (Kindle Unlimited, KDP Select). | Use reader-language phrases — topic + setting + trope + problem + audience. |
| Categories | Up to 3 Amazon categories. Differ by marketplace and format. Up to 72 hrs to apply. | 1 broad shelf + 1 mid shelf + 1 specific shelf. |
| Series/cross-format | Title must match across ebook/paperback/hardcover to share a detail page. | Identical naming everywhere. |

**Description HTML scaffold:**

```html
<b>One-line hook in bold.</b><br><br>
A short paragraph that names the situation, the promise, or the question.<br><br>
<h5>Inside this book:</h5>
<ul>
  <li>Bullet 1 — concrete benefit</li>
  <li>Bullet 2 — concrete benefit</li>
  <li>Bullet 3 — concrete benefit</li>
</ul>
<p>A one-line close. No review requests. No URLs.</p>
```

### Step 7 — Pick ISBN and imprint

- **Free KDP ISBN**: imprint shows as *Independently published*. Locked to KDP — can't use elsewhere.
- **Own ISBN** (Bowker in US, Nielsen in UK, etc.): imprint shows whatever you registered. Reusable across platforms.

If you intend to publish only on KDP, the free option is fine. If you want a polished imprint or plan to use IngramSpark/D2D later, buy your own. ISBN agencies are strict — KDP checks book details and imprint match the ISBN registry exactly (capitalisation, spacing). Imprint field max: 100 characters.

eBooks and low-content books are the only formats where ISBN is optional.

### Step 8 — Price with the calculator, not by guess

The royalty math has hard cliffs. See `resources/pricing.md` for full scenarios.

**Kindle:**
- 35% royalty: $0.99–$200 (some thresholds by file size).
- 70% royalty: $2.99–$9.99 in eligible territories. Formula: `0.70 × (list price − VAT − delivery cost)`. Delivery cost on Amazon.com = $0.15/MB.
- In Brazil, Japan, Mexico, India: 70% requires KDP Select enrollment.
- Public-domain books are not 70%-eligible without substantial original contribution.

**Paperback (Amazon.com):**
- `$9.99 and above` → 60% royalty rate
- `$9.98 and below` → 50% royalty rate
- **Always price at $9.99 not $9.98.** The royalty rate jumps at the threshold — $9.99 typically nets you ~$1 more per copy on the same printing cost.
- Formula: `(royalty rate × list price) − printing cost = royalty per copy`.
- Printing cost (US, black ink, regular trim, 300pp): `$1.00 + (300 × $0.012) = $4.60`. So a $9.99 paperback at 60% nets `(0.60 × 9.99) − 4.60 = $1.39`.

**Hardcover:**
- Same 50%/60% threshold as paperback.
- Printing cost (US, black ink, 300pp): `$5.65 + (300 × $0.012) = $9.25`. So $24.99 at 60% nets `(0.60 × 24.99) − 9.25 = $5.74`.

**Always run KDP's Printing Cost & Royalty Calculator before locking price.** Guessing here costs real money.

### Step 9 — Preview, proof, then publish

- **Kindle**: validate in Kindle Previewer on phone view, tablet view, and e-reader view.
- **Print**: order a physical proof copy. Check contrast, spine fit, barcode safety, interior margins, paper weight, cover finish (matte vs gloss).

What's editable after launch (no new edition):
- Description
- Keywords
- Categories
- Territories
- Pricing

What's locked after launch (requires new edition):
- Title
- Subtitle
- Edition number
- Primary author
- Most print physical details (trim, paper, ink, page count)

Get the locked fields right the first time. Plan to iterate on the editable fields based on launch data.

### Step 10 — Launch with a stack, not a tweet

See `resources/launch-ads-reviews.md` for the full plan. Sequence:

1. **Author Central** — set up your author page first. This is free real estate.
2. **Sponsored Products** — start with one automatic campaign and one manual keyword campaign. Low daily budget ($5–$10) until you see what converts.
3. **KDP Select free promotion** (only if you enrolled exclusive to Kindle) — use as a launch boost.
4. **External traffic** only after the Amazon page converts. Outside traffic hitting a weak page wastes the click.

**Reviews — the hard rules.** Amazon prohibits incentivised reviews, friend/family reviews, asking for positive reviews specifically, refunds for reviews, and putting review requests in the KDP description. The safe ask is: *"If you found this helpful, an honest review would mean a lot."* ARC readers get the same neutral ask, never *"please leave a positive review."*

### Step 11 — Iterate on the dashboard, not your gut

Track these metrics weekly:

| Area | Metric | Where |
|---|---|---|
| Demand | Units sold per format | KDP Reports |
| KU (if enrolled) | KENP pages read | KDP Reports |
| Money | Royalty per format and marketplace | KDP Reports |
| Ads | Clicks, spend, CPC, ACOS, ROAS | Amazon Ads console |
| Conversion | Rating + review count + 3-star themes | Amazon product page |
| Trend | Rank trajectory in category | Amazon product page |

Operating heuristic:
- **Cheap clicks, weak sales** → fix the page (cover, description, pricing).
- **Expensive clicks, strong conversion** → fix targeting and bids.
- **Sales but no reviews** → strengthen the post-read ask.

## Examples

### Example 1 — fiction launch

**Input:** *"Coastal mystery novel, 75k words, first-time self-publisher. Walk me through the launch."*

**Skill response:**

1. Confirm the niche scorecard (Mystery/Thriller > Cosy + small-town + female-led is healthy; pure literary is harder).
2. Format stack: Kindle + 6×9 paperback. Skip hardcover for first release.
3. Title/subtitle template: `[Evocative Title]: A [Setting] Mystery About [Hook]`.
4. Cover: thumbnail-readable, one focal image (lighthouse / boat / cliff), title in serif, author at bottom. Test at 200 px wide.
5. Description: hook + situation + stakes + bullet of what to expect + neutral close.
6. Keywords: `coastal mystery; missing girl mystery; small town suspense; atmospheric thriller; lighthouse novel; family secrets fiction; stormy island mystery`.
7. Categories: 1 broad (Literary Fiction), 1 mid (Mystery/Thriller > Women Sleuths), 1 specific (Mystery/Thriller > Amateur Sleuths).
8. Pricing: Kindle $4.99 (70% window, $3.39 royalty per sale). Paperback $14.99 (60% royalty, ~$4.39 net on a 300pp interior).
9. Launch: Author Central + 1 auto + 1 manual SP campaign at $7/day each. Recruit 10–20 ARC readers via newsletter/social.

(Full sample metadata in `resources/sample-assets.md`.)

### Example 2 — nonfiction quick decision

**Input:** *"What's the right paperback price for a 220-page business book? US only."*

**Skill response:**

The 60% royalty rate kicks in at $9.99 on Amazon.com.

Printing cost: `$1.00 + (220 × $0.012) = $3.64`.

| Price | Rate | Royalty per copy |
|---|---|---|
| $9.98 | 50% | `(0.50 × 9.98) − 3.64 = $1.35` |
| **$9.99** | **60%** | `(0.60 × 9.99) − 3.64 = $2.35` |
| $14.99 | 60% | `(0.60 × 14.99) − 3.64 = $5.35` |
| $19.99 | 60% | `(0.60 × 19.99) − 3.64 = $8.35` |

For a 220-page business nonfiction book, $14.99 is the sweet spot — buyers expect business books in the $12–18 range, you clear the 60% threshold, and you net ~$5.35 per copy. **Never $9.98** — you lose ~$1 vs $9.99 for one cent in shelf price.

### Example 3 — rejection troubleshoot

**Input:** *"KDP rejected my paperback cover. What do I check?"*

**Skill response:**

Five most common rejection reasons, in order:

1. **Bleed missing or wrong size** — need 0.125" beyond trim on outer edges if any page bleeds. Cover PDF needs the full wrap.
2. **Spine text on a <79-page book** — KDP rejects spine text under 79 pages. Move text off the spine.
3. **Margins too tight** — at least 0.25" outside margin (or 0.375" with bleed). Gutter grows with page count: 0.375" for 24–150 pages, 0.5" for 151–300, 0.625" for 301–500.
4. **Image under 300 DPI** — re-export assets at 300 DPI or higher.
5. **Embedded color profile or unflattened layers** — flatten everything, embed fonts, strip color profiles (KDP removes them anyway).

If none of those, paste the rejection email — KDP's wording usually names the file area precisely.

## Resources

- `resources/technical-specs.md` — full spec tables for Kindle / paperback / hardcover: file formats, trim sizes, bleed rules, margin minimums by page count, font sizes, image DPI, color profiles, file size limits.
- `resources/metadata.md` — title/subtitle rules, description HTML scaffold + forbidden content, keyword strategy, category selection, ISBN & imprint decision, BISAC vs Amazon categories.
- `resources/pricing.md` — Kindle 35%/70% rules and territories, paperback/hardcover 50%/60% threshold, printing cost formulas, royalty scenarios, KDP calculator pointer.
- `resources/market-research.md` — the niche validation loop, competitive analysis template, 10-point niche scorecard.
- `resources/writing-and-editing.md` — fiction outline template, nonfiction outline template, editing ladder, where to spend a tight budget.
- `resources/launch-ads-reviews.md` — Amazon Ads starter stack (Sponsored Products auto + manual + product targeting), review compliance rules, external marketing principles, metrics to watch.
- `resources/sample-assets.md` — sample fiction metadata (cosy mystery), sample nonfiction metadata (founder productivity), sample HTML descriptions, sample keyword sets.
- `resources/checklist.md` — pre-launch publishing checklist (market fit / manuscript / files / metadata / rights / pricing / launch assets / proof / post-launch dashboard).

## Notes & limitations

- **KDP rules change.** Royalty thresholds, accepted file formats, and category trees shift periodically. When the numbers matter (pricing decision, format choice), check the official KDP help page for the latest figure rather than trusting this skill's snapshot. The current numbers in `pricing.md` were sourced in May 2026.
- **One marketplace, one format at first.** Trying to launch Kindle + paperback + hardcover + audiobook + ten marketplaces simultaneously muddies the signal. Default sequence: Amazon.com + Kindle + paperback. Add formats and markets after the page converts.
- **No review hacks.** Amazon enforces review compliance aggressively, and a ban can take down your whole account, not just one book. Stick to honest-review asks. Never refund-for-review. Never friends/family. Never offer bonuses for reviews.
- **The cover is the click.** The description converts the click into a sale. The book itself earns the second sale (and the review). All three matter; weak cover kills the funnel before anything else can.
- **Public-domain books aren't 70% eligible** for Kindle royalty unless they contain substantial original contribution (translation, commentary, annotation, adaptation).
- **Title, subtitle, and edition are locked after publish** in most cases. Description, keywords, categories, and pricing are not. Get the locked fields right the first time.
- **Amazon platform features outside scope.** This skill doesn't cover audiobook (ACX), KDP Print Expanded Distribution accounting in depth, foreign-currency tax nuances, or Author Central biography optimisation tactics. Those are adjacent skills.

## Changelog

- `0.1.0` — initial version. Distilled from a comprehensive KDP research brief covering technical specs, metadata, pricing, workflow, market research, writing craft, launch, ads, reviews, legal basics, and sample assets. Eight resource files cover the depth; SKILL.md is the 11-step workflow.
