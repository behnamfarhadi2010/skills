---
name: Persian TTS Preprocessor
description: Use when preparing Persian / Farsi text for any TTS engine. Returns the same text with normalisation, half-spaces, punctuation, Ezafe marks, exception fixes, and optional spoken-style rewrite.
dependencies: []

category: productivity
subcategory: writing-assist
tags: [persian, farsi, tts, text-to-speech, preprocessing, normalization, ezafe, ssml]
author:
  name: Nima Aksoy
  url: https://nimaaksoy.com
  github: nimaaksoy
license: CC-BY-4.0
version: 0.1.0
created: 2026-05-17
updated: 2026-05-17
---

# Persian TTS Preprocessor

## Overview

Persian TTS engines (Azure, Google, MMS, XTTS, ElevenLabs) usually fail not because the model is bad, but because the **input text is ambiguous**. Persian doesn't write short vowels, often omits the ezafe (`ـِ`), uses elision in spoken form, and is sensitive to punctuation and syntax for prosody. Hand a raw paragraph to any engine and you'll get a stiff, book-like read with mispronunciations.

This skill takes a Persian text in, applies the minimum set of changes that make it read correctly, and returns the **same text** — same content, same paragraph structure, same line breaks — just TTS-ready.

## When to use this skill

- The user has Persian text and wants to feed it to a TTS engine (Azure, Google Gemini-TTS, MMS-TTS, XTTS, ElevenLabs, Coqui).
- The user is producing voiceovers, audiobooks, podcasts, IVR, or accessibility output in Persian.
- The user's previous TTS output was stiff, mispronounced, run-on, or read numbers as digits.
- The user has text that came from ASR / OCR / LLM and needs punctuation restored before TTS.

Do **not** use when:

- The user wants to translate Persian to another language — that's a translation skill.
- The user wants to write *new* Persian text (a poem, a song, marketing copy) — that's a writing skill.
- The text is non-Persian — apply a language-specific preprocessor.

## Tag legend

- `[Rule]` — always apply; safe in every register.
- `[Conditional]` — apply only when the input contains the trigger pattern.
- `[Register-dependent]` — apply only when the user asks for the *Fluent-formal* or *Spoken* register.
- `[Engine-specific]` — apply only when the target engine is known to need it.

## Instructions

### Step 0 — Pick the register

Ask the user **once**, then hold it for the whole text:

| Register | When to use | What changes |
|---|---|---|
| **Preserve** *(default)* | The user wants minimal interference — formal writing, news, audiobooks of literary work, legal/academic text | Only safe, mechanical fixes |
| **Fluent-formal** | The user wants formal but smooth reading — explainer videos, professional voiceover, e-learning narration | Above + selective Ezafe + number expansion + heavier punctuation repair |
| **Spoken** | The user wants natural conversational reading — chat assistants, social-media voiceover, drama | Above + colloquial rewrite (می‌خواهم → می‌خوام, این را → اینو) |

**If the register is unclear, default to Preserve and tell the user.** Don't aggressively rewrite by default — the user can always ask for Spoken.

### Step 1 — Character & whitespace normalisation `[Rule]`

Always safe. Apply to every input.

| Replace | With | Why |
|---|---|---|
| Arabic `ي` (U+064A) | Persian `ی` (U+06CC) | Many TTS engines mispronounce Arabic ي in Persian context |
| Arabic `ك` (U+0643) | Persian `ک` (U+06A9) | Same as above |
| Arabic digits `٠١٢٣...` | Persian digits `۰۱۲۳...` | Consistency; some engines tokenise differently |
| Tatweel `ـ` (U+0640) | (remove) | Decorative; tokenisers choke |
| Decorative diacritics (`ـٓ ـٔ` etc. on regular text) | (remove unless meaningful) | Causes noise |
| Multiple spaces | Single space | Tokeniser hygiene |
| Space *before* `،`, `؛`, `:`, `.`, `!`, `؟` | (remove) | Punctuation pattern |
| Missing space *after* punctuation (mid-text) | (add) | Same |
| Non-breaking space | Normal space | Sometimes mis-tokenised |
| Zero-width chars except ZWNJ (U+200C) | (remove) | Junk |

### Step 2 — Half-space (ZWNJ) restoration `[Rule]`

ZWNJ (`U+200C`, نیم‌فاصله) joins morphologically related parts as one token. Critical for correct tokenisation and Ezafe handling.

Apply these patterns (regex-friendly):

| Pattern | Replace with | Examples |
|---|---|---|
| `می ` + verb stem | `می‌` + verb | `می روم` → `می‌روم` |
| `نمی ` + verb stem | `نمی‌` + verb | `نمی توانم` → `نمی‌توانم` |
| `بی ` + word (when prefix, not preposition) | `بی‌` + word | `بی هوش` → `بی‌هوش` |
| noun + ` ها` (plural) | noun + `‌ها` | `کتاب ها` → `کتاب‌ها` |
| noun + ` های` (plural+Ezafe) | noun + `‌های` | `کتاب های من` → `کتاب‌های من` |
| adj + ` تر` / ` ترین` | adj + `‌تر` / `‌ترین` | `بزرگ تر` → `بزرگ‌تر`, `بهترین` ← already joined |
| word + ` ام/ات/اش/امان/اتان/اشان` (clitic possessive) | word + `‌ام` etc. | `کتاب ام` → `کتاب‌ام` |
| word + ` ای` (indefinite/vocative `ای`) | word + `‌ای` | `مردی` ← already joined |

See `resources/half-space.md` for the full pattern list and edge cases.

### Step 3 — Punctuation restoration `[Conditional]`

Trigger: text appears to come from ASR / OCR / LLM (long run-on sentences, missing `،` and `.`, or no `؟` on obvious questions).

Apply:

| Action | Why |
|---|---|
| Add `،` between clauses joined by `و`, `اما`, `ولی`, `چون`, `اگر`, `که` (when grammatically a clause boundary) | Mid-sentence breath, prevents run-on prosody |
| Add `.` at sentence ends | Engine resets pitch |
| Add `؟` after interrogatives (`آیا`, `چرا`, `چطور`, `کی`, `کجا`, `چی`, `چه`, `کدام`) or rising-intonation lines | Engine produces question contour, not statement |
| Add `!` for clear exclamations (interjections, commands) | Sparingly — overuse can cause shouted/distorted output on some engines |
| Convert Latin `?` → Persian `؟` | Persian-script consistency |
| Convert Latin `,` → Persian `،` in Persian-only sentences | Same |
| Convert Latin `;` → Persian `؛` in Persian-only sentences | Same |
| Keep paragraph breaks as-is | Don't merge paragraphs |

If the original text already has good punctuation, **don't change it.** This step is only for repair.

### Step 4 — Number / date / time / URL / unit expansion `[Conditional]`

Trigger: input contains digits, dates, times, URLs, units, currency, or symbols.

Apply expansion table:

| Pattern | Read as |
|---|---|
| `۱۲۳۴` (cardinal) | `هزار و دویست و سی و چهار` |
| `۲۰۲۶/۰۵/۱۷` or `1405/02/27` (Persian date) | `بیست و هفتم اردیبهشتِ هزار و چهارصد و پنج` |
| `2026-05-17` (Gregorian) | `هفدهم می دو هزار و بیست و شش` |
| `۱۰:۳۰` (time) | `ساعتِ ده و نیم` or `ده و سی دقیقه` |
| `۲۵٪` | `بیست و پنج درصد` |
| `$25` / `۲۵$` | `بیست و پنج دلار` |
| `https://example.com` | `سایتِ اگزمپل دات کام` (or skip the URL entirely if it's not meant to be read) |
| `2.5kg` | `دو و نیم کیلوگرم` |
| `№3` / `#3` | `شمارهٔ سه` |
| `1st`, `2nd`, `3rd` | `اولین`, `دومین`, `سومین` |
| `&` | `و` |
| `@username` | `اَت یوزرنیم` (or omit if not meant to be read) |
| `*`, `_`, `~`, ``` ` ``` (Markdown) | (remove — they're formatting, not content) |

Keep ordinals, fractions, and large numbers in **spelled-out Persian**, not digits.

For currency: name the currency. For units: name the unit (don't use abbreviations like `kg`).

### Step 5 — Ezafe marking — selective, not global `[Conditional]` `[Register-dependent]`

The ezafe (`ـِ`) is the unstressed `-e` that links noun phrases: `کتابِ من` (my book). In normal Persian, ezafe is rarely written; the reader infers it. TTS engines guess wrong on ambiguous noun groups.

**Selective rule** (Preserve register):

- Mark ezafe only on **noun-noun** or **noun-adjective** groups where the engine is likely to drop it.
- Mark on the **first ezafe** of a chain, then let context handle the rest.
- Do NOT mark ezafe globally — over-marked text reads like a textbook.

| Pattern | Mark |
|---|---|
| `noun + space + adjective` (clearly attributive) | `nounِ adjective` — *e.g.* `کتاب خوب` → `کتابِ خوب` |
| `noun + space + noun` (clearly possessive) | `nounِ noun` — *e.g.* `صدای تو` (already explicit via ـی) → leave |
| Noun ending in `ه` + space + word | `noun‌ی + word` — *e.g.* `خانه بزرگ` → `خانه‌ی بزرگ` |
| Noun ending in long vowel `ا/و/ی` + space + word | `noun + ی + word` — *e.g.* `پای من` ← already explicit |
| Names (proper nouns) + space + descriptor | `نامِ کامل` — only mark when ambiguous |

**Fluent-formal register**: mark ezafe more aggressively, including in compound technical phrases.

**Spoken register**: mark ezafe in the *spoken-form* rewrite (`خونه‌ی بزرگ`, `اسمِ تو`).

See `resources/ezafe.md` for the full decision tree and edge cases.

### Step 6 — Exception lexicon `[Rule]`

A small list of words where the standard letter-to-sound mapping fails. These are not rules — they're memorised exceptions.

| Pattern | Class | Correct reading |
|---|---|---|
| `خوا` (`خواهر`, `خواب`, `خواندن`, `خواستن`, `خواهان`, `خواهش`, `خواب‌آلود`...) | واو معدوله — silent `و` | `khâhar`, `khâb`, `khândan`, `khâstan` |
| `الله` | religious | `allâh` |
| `صلوة` / `زکوة` (rare) | historical | engine-specific |
| Common foreign loans (`کامپیوتر`, `تلویزیون`, `اینترنت`) | retain phonetic | usually safe |
| Proper-name patterns: short single-vowel names (`نازلی`, `سارا`, `لیلا`, `ندا`) | phantom-ezafe risk | wrap in `« »` quotes or use possessive form (`نازلیِ من`) to lock |

The skill should NOT try to fix every possible Persian historical spelling. Only the `خوا-` family is high-frequency enough to handle proactively. Everything else, leave alone — the engine usually handles it.

See `resources/exception-lexicon.md` for the extended list.

### Step 7 — Spoken-style rewrite `[Register-dependent]`

Apply **only** if register = *Spoken*. Otherwise skip.

Conversion table (the safe core):

| Formal | Spoken |
|---|---|
| می‌روم | میرم |
| می‌رود | میره |
| می‌رویم | میریم |
| می‌خواهم | می‌خوام |
| نمی‌خواهم | نمی‌خوام |
| می‌خواهی | می‌خوای |
| می‌خواهد | می‌خواد |
| می‌گویم | میگم |
| می‌گوید | میگه |
| است | -ـه (suffixed: `خوبه` for `خوب است`) / hast |
| این است | اینه |
| آن | اون |
| آنها | اونا |
| این را | اینو |
| را (after consonant) | -و / رو |
| خانه | خونه |
| نان | نون |
| می‌دانم | میدونم |
| نمی‌دانم | نمیدونم |
| می‌توانم | می‌تونم |
| یک | یه |
| دیگر | دیگه |
| اکنون | الان |
| همین‌طور | همینطور |

**Do not** rewrite proper names, technical terms, religious phrases, fixed expressions, or quoted material.

**Do not** mix registers — once a word is rewritten to spoken, all instances of that word in the text should match.

See `resources/spoken-rewrite.md` for the extended table and "don't rewrite" list.

### Step 8 — Preserve the structure on output

Return the text with the **same paragraph structure, line breaks, and overall layout** as the input. Only the content of each line changes.

| Preserve | Reason |
|---|---|
| Paragraph breaks | TTS engines reset prosody at paragraph boundaries |
| Line breaks within stanzas (poetry, lyrics) | Sometimes structurally meaningful |
| Headings and section markers | User may chunk by section |
| List bullets / numbering | User's formatting choice |
| Quoted passages | Keep quote marks as-is |
| Markdown bold/italic | Remove if asked, but flag — TTS doesn't render markdown |

Add a short note at the end summarising what changed:

```
Changes applied:
- Register: <preserve / fluent-formal / spoken>
- Character normalisation: <count> replacements
- Half-spaces restored: <count>
- Punctuation: <count> additions
- Numbers/dates/symbols expanded: <count>
- Ezafe marks added: <count>
- Exception fixes (واو معدوله, names): <count>
- Spoken rewrites: <count> (Spoken register only)
- Structure: paragraph breaks preserved
```

### Step 9 — Engine-specific adapter (optional) `[Engine-specific]`

If the user names the target engine, also add the engine-specific layer at the end:

| Engine | Add |
|---|---|
| **Azure Speech (fa-IR)** | Add SSML `<break>` tags at paragraph breaks (`<break time="500ms"/>`). Note: Azure does NOT support phoneme / custom lexicon for fa-IR voices, so all pronunciation fixes must be in the text. |
| **Google Gemini-TTS (fa-IR)** | Add SSML `<break>` similarly. Currently Preview — expect some variance. |
| **MMS-TTS Persian** | **Strip all punctuation** before sending. MMS removes it anyway. Instead, **split into short sentences** and synthesise separately with controlled silence between (~ 400ms). |
| **XTTS-v2 / ElevenLabs** | XTTS-v2 doesn't officially support fa — use only fine-tuned community checkpoints. ElevenLabs handles Persian; punctuation works as expected. |
| **Unknown engine** | Skip this step. Deliver the text without engine-specific markup. |

See `resources/engine-adapters.md` for the full per-engine notes.

## Examples

### Example 1 — minimal (Preserve register)

**Input:**

```
این کتاب خوب رو که دیروز خریدم همون کتابیه که بهت گفته بودم
```

**Output (Preserve):**

```
این کتابِ خوب رو که دیروز خریدم، همون کتابیه که بهت گفته بودم.
```

**Changes:**
- Added ezafe on `کتابِ خوب` (was missing).
- Added `،` before `همون` (clause boundary).
- Added `.` at sentence end.
- (No character normalisation needed — input was clean.)

### Example 2 — full pipeline (Fluent-formal register)

**Input:**

```
سال 1405/02/01 ساعت 10:30 جلسه با مدير شركت داريم نميتونم بيام
```

**Output (Fluent-formal):**

```
سالِ یکم اردیبهشتِ هزار و چهارصد و پنج، ساعتِ ده و نیم، جلسه با مدیرِ شرکت داریم. نمی‌تونم بیام.
```

**Changes:**
- Arabic `ي`/`ك` → Persian `ی`/`ک` (4 replacements).
- Date `1405/02/01` → `یکم اردیبهشتِ هزار و چهارصد و پنج`.
- Time `10:30` → `ساعتِ ده و نیم`.
- Ezafe on `سالِ`, `ساعتِ`, `مدیرِ شرکت`.
- Half-space restored on `نمی‌تونم`.
- Added `،` between clauses.
- Added `.` to split the run-on into two sentences.

### Example 3 — Spoken rewrite

**Input:**

```
من می‌خواهم این کتاب را به تو بدهم چون آن را دوست داری
```

**Output (Spoken):**

```
من می‌خوام این کتابو بهت بدم، چون اونو دوست داری.
```

**Changes:**
- `می‌خواهم` → `می‌خوام` (spoken).
- `این کتاب را` → `این کتابو` (spoken; را → -و).
- `به تو بدهم` → `بهت بدم` (spoken contractions).
- `آن را` → `اونو` (spoken).
- Added `،` before `چون` and `.` at end.

### Example 4 — engine-specific (MMS-TTS adapter)

**Input + register chosen + engine = MMS-TTS:**

```
اگر دیر رسیدی، شروع کن. ولی عجله نکن.
```

**Output for MMS:**

```
[Chunk 1, send to MMS, insert 400ms silence after]
اگر دیر رسیدی شروع کن

[Chunk 2, send to MMS, insert 400ms silence after]
ولی عجله نکن
```

MMS strips punctuation, so the text is delivered as two separate synthesis calls with explicit silence between.

### Example 5 — when to refuse a change

**Input:**

```
داستان «خانهٔ مادربزرگ» نوشتهٔ صادق هدایت — یکی از مهم‌ترین آثار ادبیات معاصر
```

**Output (Preserve, do NOT rewrite):**

```
داستانِ «خانهٔ مادربزرگ» نوشته‌ی صادق هدایت — یکی از مهم‌ترین آثارِ ادبیاتِ معاصر.
```

**Changes:**
- Ezafe on `داستانِ`, `آثارِ`, `ادبیاتِ`.
- `نوشتهٔ` → `نوشته‌ی` (preferred form for tokenisation).
- Added `.` at sentence end.

**Do NOT** rewrite:
- The book title `خانهٔ مادربزرگ` (quoted — preserve).
- The author name `صادق هدایت`.
- The literary register — this is formal writing about literature, even in Spoken mode the title and author would stay formal.

## Resources

- `resources/half-space.md` — full ZWNJ pattern list, edge cases, and false-positive guards.
- `resources/punctuation.md` — Persian punctuation conventions, repair heuristics for ASR/OCR/LLM-sourced text, when to add vs leave.
- `resources/ezafe.md` — when to mark Ezafe per register, the decision tree, and the post-`ه` special case.
- `resources/numbers-and-symbols.md` — number expansion (Persian + Arabic + Roman digits), dates (Gregorian + Persian + Hijri), times, currencies, units, URLs, common symbols.
- `resources/exception-lexicon.md` — the `خوا-` family, religious terms, common foreign loans, proper-name handling for phantom-ezafe risk.
- `resources/spoken-rewrite.md` — extended formal-to-colloquial table, register transitions, "do not rewrite" list (proper names, technical terms, religious phrases, fixed expressions, quotes).
- `resources/engine-adapters.md` — per-engine quirks: Azure (no custom lexicon), Google Gemini (SSML break support), MMS (strips punctuation), XTTS / ElevenLabs / Coqui notes, SSML break duration recommendations.
- `resources/test-cases.md` — the 12-slice test suite (Ezafe, post-`ه` Ezafe, homograph, واو معدوله, half-space verbs, suffixes, yes/no questions, clause boundaries, numbers/dates, mild colloquial, clitics, long text). Use to verify the preprocessor's output on each engine.

## Notes & limitations

- **Persian TTS is front-end-limited, not model-limited.** The preprocessor matters more than the engine choice. Even a great model produces stiff output on raw text; a moderate model produces natural output on preprocessed text.
- **Preserve mode is the default.** Don't aggressively rewrite to Spoken unless the user explicitly asks. Rewriting formal text into colloquial form can break tone (academic, legal, literary, religious).
- **Ezafe marking is selective, not global.** Marking every possible ezafe makes the output read like a school textbook. Only mark where the engine is likely to drop it.
- **Engine support for Persian varies wildly.** Azure has two voices but no custom lexicon. MMS strips punctuation. XTTS-v2 doesn't officially support Persian. ElevenLabs and Google Gemini-TTS are currently the safer English-speaker-friendly defaults for high-quality Persian; specialised Persian-trained models (ManaTTS, ParsVoice fine-tunes) are stronger for native quality but require infrastructure. See `resources/engine-adapters.md`.
- **Some Persian sounds remain AI-hard even with clean text.** `ع`, `ح`, `ق`, `ء` are inconsistently rendered. Proper names ending in `-li`, `-ra`, `-ma` (نازلی, سارا, نیما) often get a phantom ezafe inserted (`nâz-LI` → `nâz-EH-li`). The skill flags these but the engine may still mispronounce. Re-roll 2–3 times and pick.
- **Regional dialects (Khorasani, Lori, Kurdish, Bandari) are out of scope.** Default register is Tehran-standard Persian (`fa-IR` معیار). If the user wants regional flavour, write the معیار form and use post-processing in the audio.
- **Don't try to mark every Ezafe perfectly.** Mark the high-risk ones, let context handle the rest. The goal is "good enough that TTS reads naturally", not "every linguistic relationship encoded".
- **The skill output should be reversible.** A user should be able to compare input vs output and understand every change. If a change isn't explainable in one line, it's probably wrong.

## Changelog

- `0.1.0` — initial version. Distilled from a comprehensive research brief on Persian TTS preprocessing covering phonetic basics, normalisation, half-space restoration, punctuation repair, Ezafe marking, exception lexicon, spoken-style rewrite, engine-specific adapters, and a 12-slice evaluation framework.
