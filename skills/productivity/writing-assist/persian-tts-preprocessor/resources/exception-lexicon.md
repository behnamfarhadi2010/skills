# Exception lexicon — words where standard letter-to-sound fails

These are not rules. They're memorised exceptions where Persian's standard orthography doesn't produce the correct pronunciation. Hand them to the engine as-is and you get a wrong reading.

The skill handles these by either marking them in the text or warning the user that the engine may need help.

---

## 1. واو معدوله (silent `و`) — the `خوا-` family

Persian words written with `خوا` historically had a long `o-â` sound. Modern Tehran-standard Persian dropped the `o`, but the spelling kept the `و`. The `و` is silent.

| Word | Wrong reading | Correct |
|---|---|---|
| خواهر | xa-vâ-har | khâ-har |
| خواب | xa-vâb | khâb |
| خواندن | xa-vân-dan | khân-dan |
| خواستن | xa-vâs-tan | khâs-tan |
| خواهان | xa-vâ-hân | khâ-hân |
| خواهش | xa-vâ-hesh | khâ-hesh |
| خواب‌آلود | xa-vâb-âlud | khâb-âlud |
| خوابیدن | xa-vâ-bi-dan | khâ-bi-dan |
| خواستار | xa-vâs-târ | khâs-târ |
| خواندنی | xa-vân-da-ni | khân-da-ni |
| خوانده | xa-vân-de | khân-de |
| خوانندگان | xa-vâ-nan-de-gân | khâ-nan-de-gân |
| خواستن | xa-vâs-tan | khâs-tan |
| خوارزم (proper) | xa-vâ-razm | khâ-razm |

**Rule:** any word starting with `خوا` is in the silent-`و` family. The `و` is not pronounced.

**Engine behaviour:**
- Some Persian-trained TTS engines (ParsVoice, ManaTTS) handle this correctly.
- General multilingual engines (Azure, Google) often get this wrong.

**Fix for engines that don't support custom lexicon:**
- The skill flags the word but can't directly fix it in plain text (you can't remove the `و` without changing spelling).
- If the engine consistently mispronounces, switch to phonetic notation (some engines accept `[xahar]` or IPA in tags).

**Don't apply outside the `خوا-` family.** `خوب` (`khub`) is NOT in this class — the `و` here is the `u` sound, pronounced.

---

## 2. Words with rare Arabic spellings

| Word | Note |
|---|---|
| الله (`allâh`) | Standard religious. Always preserve as-is. |
| الرحمن (`ar-rahmân`) | Religious. Preserve. |
| الرحیم (`ar-rahim`) | Religious. Preserve. |
| لـ (in fixed expressions like `لاحول`) | Religious / archaic. Leave alone. |
| صلوة, زکوة | Archaic Arabic spellings of `salât` and `zakât`. Persian usually writes صلات, زکات. If you see these in text, leave alone unless you're sure of the modern intent. |
| ـٰ (dagger alif, e.g. `الله` , `هذا`) | Quranic / classical. Preserve. |

Don't try to modernise religious or classical Arabic terms. They're written in fixed forms.

---

## 3. Common foreign loans — leave as-is

Persian has absorbed many foreign words with phonetic-Persian spellings that engines usually handle correctly:

| Word | Reading |
|---|---|
| کامپیوتر | computer |
| تلویزیون | television |
| اینترنت | internet |
| موبایل | mobile |
| رادیو | radio |
| موسیقی | musiqi (Arabic) |
| اتوبوس | otobus |
| اتومبیل | otomobil |
| سینما | sinemâ |
| تئاتر | te'âtr |

**Don't try to fix these.** Most engines pronounce them correctly. Modifying the spelling can break the pronunciation.

---

## 4. Proper-name phantom-ezafe risk

Two-syllable Persian names ending in `-i`, `-â`, `-u`, or `-a` are commonly mispronounced by TTS engines that insert a phantom ezafe inside:

| Name | Wrong reading | Correct |
|---|---|---|
| نازلی | nâz-EH-li | nâz-li |
| سارا | sâ-RAH | SÂ-râ |
| لیلا | ley-LAH | LEY-lâ |
| ندا | ne-DAH | ne-DÂ |
| رضا | re-ZÂ-a | re-ZÂ |
| علی | a-LI | a-LI (usually correct, but watch in long names) |
| نیما | ni-MAH | nimâ |
| سینا | si-NAH | si-nâ |
| رویا | ru-YAH | ru-yâ |
| شیما | shi-MAH | shi-mâ |
| نگار | ne-GÂR-r | ne-GÂR |
| ساغر | sâ-GAR | sâ-ghar |

### Fix techniques

Pick one based on the situation:

1. **Wrap in Persian quotation marks**: `«نازلی»` — many engines treat quoted spans as proper nouns and resist ezafe insertion.
2. **Use the possessive form**: `نازلیِ من` (literally "my Nazli"). The explicit ezafe locks the rhythm and prevents Suno from inventing one.
3. **Anchor in a full clause**: `اسمت نازلیه` instead of bare `نازلی، بیا`. The surrounding context gives the engine enough cue.
4. **Spell out in transliteration alongside**: `نازلی (Nazli)` — last resort; visually noisy.

### Three+ syllable names are usually safer

| Name | Usually correct |
|---|---|
| محمدرضا | mohammad-rezâ |
| علیرضا | ali-rezâ |
| فریماه | fari-mâh |
| پرستو | pa-ras-tu |
| نگارین | ne-gâ-rin |

Don't apply the fix techniques to these unless you observe a real problem.

### City and place names

| Name | Note |
|---|---|
| تهران | Usually correct |
| اصفهان | Sometimes mispronounced (es-fa-hân vs es-fâ-hân) |
| قم | Usually correct |
| مشهد | Usually correct |
| تبریز | Usually correct |
| شیراز | Usually correct |
| یزد | Usually correct |
| بوشهر | Usually correct |

Most major city names are common enough that engines handle them. Smaller / less-known cities may need wrapping.

---

## 5. Honorifics and titles

| Title | Reading |
|---|---|
| دکتر | dok-tor |
| مهندس | mo-han-des |
| استاد | os-tâd |
| آقای | â-ghâ-ye |
| خانم | khâ-nom |
| سرکار خانم | sar-kâr khâ-nom |
| جناب | je-nâb |
| حاج | hâj |
| سید | sey-yed |
| سیده | sey-ye-de |

Engines usually handle these. The combination of title + name (`دکترِ علی`) sometimes drops the ezafe — see `ezafe.md`.

---

## 6. Abbreviations to expand

| Abbreviation | Expansion |
|---|---|
| م.ا (Mr.) | "آقایِ" |
| خ. (Mrs.) | "خانمِ" |
| دکتر = د. | "دکتر" (don't use abbreviation form) |
| م.ا.ا (= معاون اول) | "معاونِ اول" |
| ج.ا.ا (= جمهوری اسلامی ایران) | "جمهوریِ اسلامیِ ایران" |
| س.ل.ل (= ساعت / ل/ل) | usually skip — too rare |
| ه.ش (Solar Hijri) | "هجری شمسی" |
| ه.ق (Lunar Hijri) | "هجری قمری" |
| ه.م (= هجری میلادی) | "هجری میلادی" or "میلادی" |
| ع. (after a name = peace be upon him) | "علیه‌السلام" |
| ص. (after Muhammad) | "صلی‌الله‌علیه‌و‌آله" — but most TTS engines don't render this well; consider expanding or skipping |

---

## 7. Common mispronunciation patterns (engine-dependent)

These are not always wrong, but worth flagging:

| Word | Sometimes wrong | Correct |
|---|---|---|
| ای (vocative) | uy (closed) | ey (open) |
| است | as-t | ast (one syllable) |
| می‌تواند | mi-tu-vâ-nad | mi-tavâ-nad |
| پنج (5) | pen-j | panj (one syllable) |
| تخت | tax-t | takht (one syllable) |
| سخت | sax-t | sakht (one syllable) |

The fix is usually engine-dependent. For Azure / Google / ElevenLabs, the standard Persian-script form should work. For MMS or older models, you may need to add hint markers (some engines accept `[say: takht]` or similar).

---

## 8. What this skill should NOT try to fix

Don't try to fix every Persian word's pronunciation. Most are correct on most engines.

**Out of scope:**
- General mispronunciations that vary by engine
- Regional dialect pronunciations
- Old-style poetic readings (`میم` for `می‌ام`, etc.)
- Highly specialised vocabulary (medical, legal, scientific terminology)
- Foreign words that the engine handles correctly

**In scope for this lexicon:**
- The `خوا-` family (high-frequency, always wrong without intervention)
- Proper-name phantom-ezafe (common failure mode with a known fix)
- Religious / classical Arabic phrases (preserve as-is)
- Common abbreviations (expand)

If the user reports a specific word being mispronounced and it's not in this list, add it to a project-specific lexicon. Don't bloat this skill's exception list with every observed mispronunciation.
