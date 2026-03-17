---
name: create-assistant
description: >
  Use this skill when the user writes "create assistant" or "new language assistant".
  Walks through a guided Q&A to collect the learner's profile (10 questions), generates a personalized
  {language}-learning-assistant/ subfolder with CLAUDE.md, four skill definition files (sentence-builder,
  sentence-renderer, grammar-explainer, vocab-drill), README.md, index.html, styles.css, and .claude/settings.json.
  Updates the platform root CLAUDE.md registry table and index.html to include the new assistant.
---

## Allowed Tools
* Bash: check if subfolder exists, create directories, get current date/time
* Read: read root CLAUDE.md and index.html to update them; read styles.css to copy
* Write: create all files in the new subfolder and update root files

---

## Workflow Overview

This skill collects learner profile information in two rounds (6 core + 3-4 follow-ups), displays a confirmation table, then generates a complete, language-adapted language learning assistant subfolder.

The key generalization mechanism: Claude synthesizes the target language's grammar system from its knowledge, then writes language-specific skill specifications and example content directly into the generated files. No hardcoded per-language branches — instead, each skill file is customized based on the target language's linguistic properties.

---

## Step 1 — Greeting & Introduction

Display a warm, friendly greeting:

```
## 🚀 Create a New Language Learning Assistant

I'll help you set up a personalized language learning assistant. This takes just a few minutes.

I'll ask you 10 questions about your learning goals, language, and preferences.
Then I'll generate a complete, ready-to-use assistant folder with four Claude Code skills,
a personalized learning plan, and everything configured for your specific language pair.

Let's get started! 👇
```

Proceed immediately to Step 2 without waiting.

---

## Step 2 — Round 1: Core Profile Questions (all 6 together)

Display the 6 core questions as a numbered list and ask the user to answer all of them before proceeding:

```
**Round 1 — Your Learning Profile**

Please answer these 6 questions. You can use abbreviations and write naturally.

1. **What language are you learning?**
   (e.g., French, Spanish, Japanese, Portuguese, Mandarin, etc.)

2. **What is your current level in that language?**
   (e.g., A1, A2, B1, beginner, intermediate, false beginner, self-taught for 6 months, etc.)

3. **What is your native language?**
   (e.g., English, Spanish, French, Korean, Arabic, etc.)

4. **Do you speak any other languages fluently?**
   (e.g., "English and Portuguese" or "just English" or "no")

5. **What is your main goal for learning this language?**
   (e.g., travel and tourism, business, living abroad, academic study, hobby, moving closer to family, etc.)

6. **Are you using a specific textbook or curriculum?**
   (e.g., "Momente A1 (Hueber)", "Assimil", "Duolingo + YouTube", "no textbook — free practice", etc.)
```

Wait for all 6 answers before proceeding to Step 3. Do not continue until you have answers to each question.

---

## Step 3 — Round 2: Follow-Up Questions (3-4 conditional follow-ups)

Based on the answers, ask 3-4 conditional follow-up questions:

```
**Round 2 — Customization**

A few more details to personalize your assistant:

7. **What language should instructions and explanations be in?**
   (Default: usually your native language, but you can choose any language you're fluent in)

8. **What correction style do you prefer?**
   - **Strict** — Flag every error (spelling, grammar, gender, case, word order), always show the correct form
   - **Balanced** — Flag major errors; mention minor ones when relevant
   - **Conversational** — Focus on comprehension, don't interrupt the learning flow

9. [IF textbook was mentioned in Q6]
   **Describe your textbook structure.**
   (e.g., "24 chapters called 'Lektion', each with vocabulary, grammar, and dialogue sections"
   or "PDF glossaries for chapters 1-12 and 13-24"
   or "online platform with units and levels")

   [IF no textbook in Q6]
   **What topics or themes should the practice content focus on?**
   (e.g., "travel, dining, shopping; conversation with native speakers; business communication", etc.)

10. **Any special notes about your learning?** *(optional)*
   (e.g., "I have ADHD and prefer shorter lessons", "focus on Paris dialect",
   "avoid formal subjunctive until B1", "I love cooking", etc.)
```

Wait for answers before proceeding to Step 4.

---

## Step 4 — Profile Confirmation

Display a clear confirmation table with all collected information:

```
## 📋 Learner Profile — Please Confirm

| Field | Your Answer |
|-------|-------------|
| **Target language** | {language} |
| **Current level** | {level} |
| **Native language** | {native_language} |
| **Other fluent languages** | {other_languages or "None"} |
| **Learning goal** | {goal} |
| **Textbook/curriculum** | {textbook or "No textbook — free practice"} |
| **Instruction language** | {instruction_language} |
| **Correction style** | {strict/balanced/conversational} |
| **Textbook details** OR **Content focus** | {details or focus} |
| **Special notes** | {notes or "None"} |

---

## Subfolder Details

A new folder will be created at: **`{language_slug}-learning-assistant/`**

**Is this correct?** (yes / no / [corrections])

If the user says "no" or provides corrections, loop back to the relevant questions and update the table until they confirm. Do not proceed until confirmed.
```

---

## Step 5 — Internal Language Property Inference (not displayed to user)

Before creating any files, Claude should internally reason through the target language's properties. **This reasoning is not shown to the user, but it informs the generated files.**

Claude synthesizes:

### 5.1 Writing System & Slug Normalization
- **Latin alphabet languages** (English, French, Spanish, Portuguese, German): replace accented characters with ASCII equivalents (e.g., French: `é→e, à→a, ç→c, ù→u`)
- **Cyrillic** (Russian, Bulgarian, Serbian): transliterate to Latin (e.g., `й→y, ё→yo`)
- **Non-Latin scripts** (Japanese, Arabic, Korean, Mandarin): romanize or transliterate
  - Japanese: `Hiragana/Kanji → Romaji` (e.g., `だいがく (daigaku) → daigaku`)
  - Arabic: `Arabic script → transliteration` (e.g., `كتاب (kitaab) → kitaab`)
  - Mandarin: `Characters → Pinyin` (e.g., `学习 (xuéxí) → xuex` simplified)
  - Korean: `Hangul → Romanization` (e.g., `학습 (hak-seup) → hakseup`)

### 5.2 Grammar System Properties
For the target language, determine:
- **Number of genders** (none, 1, 2, 3, or more) and their names (masculine/feminine/neuter, masc/fem/neut, etc.)
- **Case system** (none, 2, 4, 6, etc.) and their names (nominative, accusative, dative, genitive, locative, ablative, etc.)
- **Article system**: definite (the), indefinite (a/an), partitive, etc. and their forms
- **Key tenses** for A1 level: present tense name, past tense name(s), future tense (if relevant)
- **Verb properties**: persons/pronouns distinguished in conjugation (e.g., ich/du/er/wir/ihr/Sie for German, je/tu/il/nous/vous/ils for French)
- **Mood system** (indicative, subjunctive, conditional, etc.)
- **Aspect** (for languages like Russian, Mandarin, Navajo)
- **Politeness levels** (for languages like Japanese, Korean)
- **Tone system** (for Mandarin, Cantonese, Vietnamese, Thai)

### 5.3 Verb Conjugation Model for Skill Specs
- **For verbs in sentence-builder**: what persons should be covered? (e.g., French: je/tu/il-elle/nous/vous/ils-elles; Japanese: casual/polite/negative forms)
- **For verbs in vocab-drill enrichment**: what tense forms to include? (French: présent + passé composé + auxiliary avoir/être; Spanish: presente + pretérito + imperfecto; Japanese: masu-form + te-form + dictionary form)

### 5.4 Noun Enrichment Fields for Vocab Drill
Determine what metadata is valuable for nouns:
- **French/Spanish/Portuguese**: `article`, `gender`, `plural_form`, `pattern_hint` (e.g., "-tion → feminine in French")
- **Russian/Polish/Czech**: `gender`, `case_forms` (genitive singular at minimum), `declension_class`
- **Japanese**: `kanji`, `hiragana`, `romaji`, `counter_word`, `politeness_level`
- **Mandarin**: `simplified_characters`, `traditional_characters`, `pinyin`, `tone_marks`, `classifier/measure_word`
- **German** (already known): `article`, `gender`, `plural_marker`, `plural_full`
- **Finnish/Hungarian/Turkish**: `case_forms` (genitive, inessive, adessive, elative, illative, etc.)

### 5.5 Language-Specific Exercise Types
Replace the German-specific `plural_formation` and `haben_vs_sein` with target-language equivalents:

| Language | Exercise Type 1 | Exercise Type 2 | Notes |
|----------|-----------------|-----------------|-------|
| **French** | `avoir_vs_etre` (Passé Composé auxiliary) | `agreement_past_participle` (gender/number agreement with avoir) | |
| **Spanish** | `ser_vs_estar` (copula distinction) | `preterite_vs_imperfect` (tense selection) | |
| **Portuguese** | `ser_vs_estar` | `formal_vs_informal` (você vs. tu) | |
| **Italian** | `avere_vs_essere` | `agreement_past_participle` | |
| **Russian** | `aspect_selection` (perfective vs. imperfective) | `case_ending` (noun/adjective agreement) | High morphology |
| **Polish/Czech** | `case_ending` | `aspect_selection` | |
| **German** | `plural_formation` | `haben_vs_sein` | (baseline) |
| **Japanese** | `verb_te_form` (te-form morphology) | `honorific_form` (keigo politeness) | |
| **Korean** | `formal_vs_informal` (-습니다 vs. -어/아) | `subject_particle` (이/가 vs. 는/은) | |
| **Mandarin** | `measure_word_selection` (量词 selection) | `aspect_marker` (了, 着, 过 use) | No verb conjugation |
| **Arabic** | `gender_agreement` (noun+adjective) | `dual_form` (for languages with dual) | |

### 5.6 Transfer Note Structure
Based on the learner's L1 and any L2:
- Always: `{L1} → {target language}` transfer notes (highlighting cognates, false friends, structural parallels)
- If L2 provided: `{L2} → {target language}` transfer notes
- Specific examples from Claude's knowledge of typical mistakes/parallels for this language pair

For example:
- **Spanish → French**: Spanish has 2 genders; French has 2 but with fewer -o/-a endings; both have Romance grammar (verb conjugation, articles) so many parallels
- **English → German**: English lost most cases; German kept 4; both have V2 word order in German but English is SVO
- **English → Japanese**: Completely different: no gender, agglutinative, formal/casual registers, particles instead of cases, VSO-like word order
- **Spanish → Japanese**: Similar: formal/informal registers, topic prominence, but completely different writing systems and grammar

### 5.7 HTML Language Tag (BCP 47)
Determine the correct `lang` attribute:
- French: `fr`, `fr-CA` (Canadian), `fr-CH` (Swiss)
- Spanish: `es`, `es-ES` (European), `es-MX` (Mexican)
- German: `de`, `de-DE`, `de-AT`, `de-CH`
- Japanese: `ja`
- Mandarin: `zh-Hans` (simplified), `zh-Hant` (traditional)
- Korean: `ko`
- Portuguese: `pt`, `pt-BR` (Brazilian), `pt-PT` (European)
- Russian: `ru`
- Arabic: `ar` or `ar-EG` (Egyptian), `ar-SA` (Saudi), etc.
- Italian: `it`, `it-IT`

---

## Step 6 — Create Folder Structure

Use Bash to create all necessary directories. This can be done in a single command with multiple `-p` flags:

```bash
mkdir -p {language_slug}-learning-assistant/.claude/skills/sentence-builder
mkdir -p {language_slug}-learning-assistant/.claude/skills/sentence-renderer
mkdir -p {language_slug}-learning-assistant/.claude/skills/grammar-explainer
mkdir -p {language_slug}-learning-assistant/.claude/skills/vocab-drill
mkdir -p {language_slug}-learning-assistant/data/sentences
mkdir -p {language_slug}-learning-assistant/data/sentences-html
mkdir -p {language_slug}-learning-assistant/data/grammar
mkdir -p {language_slug}-learning-assistant/data/grammar-html
mkdir -p {language_slug}-learning-assistant/data/vocabulary
mkdir -p {language_slug}-learning-assistant/data/vocabulary-html
mkdir -p {language_slug}-learning-assistant/resources
```

Before creating, check if the folder already exists:
```bash
if [ -d "{language_slug}-learning-assistant" ]; then echo "EXISTS"; fi
```

If it exists, warn the user:
> "A `{language_slug}-learning-assistant/` folder already exists. Do you want to overwrite it (warning: existing files will be replaced), create a different name, or cancel?"

Only proceed if the user confirms.

---

## Step 7 — Generate Personalized CLAUDE.md (CRITICAL)

This is the most important file. Generate a complete, language-adapted CLAUDE.md for the subfolder. The file must include:

### 7.1 Header & Learner Profile
```markdown
# {Language} Learning Assistant — Architecture & Learner Profile

> Generated by the multi-language platform `create-assistant` skill on {today's date}.
> This file is the single source of truth for all content generation in this assistant.

## Learner Profile

| Field | Value |
|-------|-------|
| **Native language** | {native_language} {if other_languages: (also fluent in {other_languages})} |
| **Instruction language** | {instruction_language} |
| **Current level** | {level} (started {today}) |
| **Target level** | {suggested target or "to be determined"} |
| **Goal** | {goal} |
| **Textbook** | {textbook or "Free practice — focus on {focus}"} |
| **Correction style** | {strict/balanced/conversational} — {brief explanation} |
| **Special notes** | {special_notes or "None"} |
```

### 7.2 Guidelines for All Skills
Copy the "Guidelines for All Skills" section from the platform CLAUDE.md (generic part), but adapt:
- Replace "Language transfer notes" item with actual language names: `{L1} → {target}` and `{L2} → {target}` (if applicable)
- Keep "Level-appropriate content", "Instruction language", "Textbook alignment", "Correction style", "Realistic context" as-is

### 7.3 Project Structure
Standard project structure (copy from platform CLAUDE.md, no changes).

### 7.4 Skill Specifications — The Four Skills (LANGUAGE-ADAPTED)

#### 7.4.1 Sentence Builder

Adapt from German spec:

**Purpose**: Generate N example sentences for a given word, grouped by difficulty, with translations and grammar notes.

**Invocation**: `sentence builder: [word]` or `sentence builder: [word] ([type])`

**Key Parameters (adapted for target language):**
- **Sentence count**: 10 total (3 simple, 3 medium, 4 complex)
- **Grammar coverage**:

  *For verbs in {target language}*:
  - Cover at least [appropriate number and names of persons]: [list them]
    - Example for French: "cover at least 5 different persons: je, tu, il/elle, nous, vous, ils/elles"
    - Example for Japanese: "cover formal (polite -masu) and casual (dictionary form); include negative forms"
  - Use [appropriate tenses for A1]:
    - Example for French: "present indicative (présent) for most; passé composé for complex group (at least 1 sentence)"
    - Example for Spanish: "presente for most; at least 1 pretérito indefinido sentence in complex group"
    - Example for Japanese: "masu-form for most; include te-form and negative forms; avoid past tense for A1"
    - Example for Mandarin: "present-tense sentences (no conjugation); include 了 aspect marker in some sentences; no past tense yet"

  *For nouns in {target language}*:
  - Cover variety of [relevant features]:
    - Example for French: "singular and plural forms; use both definite (le/la/les) and indefinite (un/une/des) articles; show gender agreement with adjectives where relevant"
    - Example for Japanese: "include appropriate counters (個, つ); use with particles は/を/に as context allows"
    - Example for Mandarin: "use appropriate measure words (个, 本, 只, etc.); show both simplified and traditional if relevant"

  *For adjectives in {target language}*:
  - Example for German: "predicate and attributive positions; show agreement in attributive position"
  - Example for French: "predicate position (c'est + adjective) and attributive position; show gender/number agreement"
  - Example for Japanese: "i-adjectives and na-adjectives; show attributive and predicate forms"

- **Vocabulary scope**: all {level}-appropriate (A1 = ~1200–1500 words for most languages)
- **Context**: {goal}-relevant wherever natural, but allow other realistic contexts

**JSON Schema** (universal across all skills):
```json
{
  "word": "<the word as user typed it>",
  "base_form": "<dictionary form>",
  "word_type": "<noun|verb|adjective|adverb|other>",
  "generated_at": "<ISO 8601 datetime>",
  "sentences": [
    {
      "id": 1,
      "target": "<{language} sentence>",
      "translation": "<{instruction_language} translation>",
      "level": "<simple|medium|complex>",
      "grammar_note": "<1-line grammar note>"
    }
  ]
}
```

**Transfer Notes** (filled in with actual language pair):
- **{native_language} → {target_language}**:
  - [Specific parallel or false friend Claude knows; 2-3 bullet points]
  - Example for Spanish → French: "Both have Romance grammar (verb conjugation, gendered nouns) but different conjugation patterns. Spanish 'ser/estar' distinction is very different from French ('être' with different uses). Watch out: Spanish 'actual' ≠ French 'actuel'."

- **{other_language} → {target_language}** (if applicable):
  - [Specific parallel or false friend; 2-3 bullet points]

#### 7.4.2 Sentence Renderer

**Purpose**: Convert unrendered sentence JSON files to styled HTML pages; skip already-rendered files.

**Invocation**: `render sentences`

**Output**: HTML saved to `data/sentences-html/{stem}.html` for each unrendered JSON

**Key Parameters** (language-adapted):
- HTML `lang` attribute: `<html lang="{lang_code}">` (where {lang_code} is the BCP 47 tag: `fr`, `es`, `de`, `ja`, `zh-Hans`, etc.)
- Footer text: `Generated by {Language} Learning Assistant · {level}`
- Page `<title>`: `Sentences: {word} ({word_type}) — {level}`
- Section headers use level badges: "Simple (3)", "Medium (3)", "Complex (4)"

**JSON field access**: read `target` field (not `german`)

#### 7.4.3 Grammar Explainer

**Purpose**: Generate a full grammar lesson: explanation + 10+ examples + 12+ exercises (4 types) + solution key.

**Invocation**: `grammar: [topic]` or `grammar: [topic], focus: [specifics]`

**Slug Normalization** (target-language specific):
- Example for French: `é→e, à→a, è→e, ê→e, ç→c, ù→u`
- Example for Spanish: `á→a, é→e, í→i, ó→o, ú→u, ü→u, ñ→n`
- Example for Russian: `й→y, ё→yo, щ→shch, ж→zh, ч→ch` (transliterate to Latin)
- Example for Japanese: `hiragana/kanji → romaji` (e.g., `だいがく → daigaku`)
- Example for Mandarin: `characters → pinyin without tones` (e.g., `学习 → xuex`)

**Explanation Sections** (language-adapted):
- "What is it?" — definition in target language's grammar taxonomy
- "When to use it" — rules and conditions (specific to {language})
- "How to form it" — construction steps or conjugation/declension tables
- "Common exceptions" — irregular forms at {level}
- "{native_language} transfer note" — parallel or false friend
- "{other_language} transfer note" — if applicable
- "Key Takeaway" — one bold sentence with the single most critical point

**Examples**: 10+ total, grouped by level (3 simple / 3 medium / 4+ complex)
- Grammar notes reference {target_language}-specific grammar terms (not German terms like "Perfekt")

**Exercises**: 12+ total, distributed across 4 core types + language-specific types
- Core 4 types: `fill_in_blank`, `multiple_choice`, `translation`, `error_correction`
- Complexity: ~1/3 simple, ~1/3 medium, ~1/3 complex

**Transfer note section names** (adapted):
- `{native_language} Transfer Notes` and `{other_language} Transfer Notes` (not hardcoded "Spanish" / "English")

**JSON Schema** (same as German CLAUDE.md but with `target` field instead of `german`):
```json
{
  "topic": "<topic as user typed>",
  "focus": "<focus or null>",
  "topic_slug": "<lowercase, spaces→underscores, special chars→ASCII>",
  "generated_at": "<ISO 8601>",
  "explanation": {
    "sections": [
      {"title": "<section>", "content": "<full text>"}
    ],
    "transfer_notes": ["<bullet>", "<bullet>"],
    "key_takeaway": "<sentence>"
  },
  "examples": [
    {
      "id": 1,
      "target": "<{language} sentence>",
      "translation": "<{instruction_language} translation>",
      "level": "simple|medium|complex",
      "grammar_note": "<1-line>"
    }
  ],
  "exercises": [
    {
      "id": 1,
      "type": "fill_in_blank|multiple_choice|translation|error_correction",
      "level": "simple|medium|complex",
      "prompt": "<exercise text>",
      "options": ["a) ...", "b) ...", "c) ..."],  // only for multiple_choice
      "solution": "<correct answer>",
      "explanation": "<why this is correct>"
    }
  ]
}
```

#### 7.4.4 Vocabulary Drill

**Purpose**: Extract vocabulary from textbook or generate from topic; provide enrichment and 40+ exercises.

**Invocation**:
- If textbook: `vocab drill: {chapter_term} [N], [focus]`
  - Example for French: `vocab drill: unité [5], nouns`
  - Example for Spanish: `vocab drill: capítulo [3], verbs`
- If no textbook: `vocab drill: topic [topic_name], [focus]`
  - Example: `vocab drill: topic greetings, all`

**Source**:
- If textbook provided: [describe the PDF(s) or resources the learner will provide]
  - Example: "For chapters 1-12, use Alter Ego+ glossary PDF; for 13-24, use the separate Part 2 glossary. Classification: nouns have article (le/la/les/un/une), verbs are infinitive, adjectives are base form, other words are fixed particles."
  - Example: "No PDF available; use built-in knowledge of {textbook_name} for {language} A1 vocabulary. Focus on the topics in chapter {chapter_number}."
- If no textbook: "Use Claude's knowledge of {language} A1 vocabulary for the topic: {topic}."

**Word Type Classification** (target-language specific):
- Example for French:
  - Noun: starts with article (le/la/les, un/une, du/de la)
  - Verb: infinitive ending -er/-ir/-re
  - Adjective: base form (no article, not a verb)
  - Other: adverbs (adverbs in -ment), prepositions, etc.
- Example for Japanese:
  - Noun: can appear with particles (は、を、に)
  - Verb: infinitive form (dictionary form ending in -u, -iru, -eru)
  - Adjective: i-adjective (ends in -い) or na-adjective (base + な)
  - Other: particles, adverbs

**Enrichment Fields** (target-language specific):

*For {language} nouns*:
- Example for French:
  - `article`: le, la, les, un, une, etc.
  - `gender`: masc or fem
  - `plural_form`: plural spelling
  - `pattern_hint`: rule (e.g., "-tion → always feminine", "-ment → always masculine")
  - `pattern_reliability`: high / moderate / memorize
- Example for Japanese:
  - `kanji`: if applicable
  - `hiragana`: hiragana reading
  - `romaji`: Latin romanization
  - `counter_word`: appropriate counter (e.g., 個, つ, 本, 枚)
  - `politeness_note`: register (formal/casual) if relevant
- Example for Mandarin:
  - `simplified_characters`: 汉字
  - `pinyin`: pīnyīn with tones (tonal marks)
  - `measure_word`: appropriate classifier (个, 本, 只, etc.)

*For {language} verbs*:
- Example for French:
  - `infinitive`: -er/-ir/-re
  - `conjugation_present`: present indicative (je, tu, il/elle, nous, vous, ils/elles)
  - `passe_compose`: example past form with auxiliary
  - `auxiliary`: avoir or être
  - `is_irregular`: boolean
  - `verb_note`: optional (separable, reflexive, etc.)
- Example for Japanese:
  - `dictionary_form`: dictionary form (infinitive)
  - `masu_form`: polite form (e.g., 学ぶ → 学びます)
  - `te_form`: te-form (conjunctive)
  - `negative_form`: negative form
  - `is_irregular`: boolean
  - `verb_note`: transitive/intransitive, etc.
- Example for Mandarin:
  - `base_form`: character form
  - `pinyin`: pinyin with tones
  - `aspect_marker_examples`: examples of 了/着/过 usage
  - `is_irregular`: boolean (rare in Mandarin)

*For {language} adjectives*:
- Example for French: `antonym`, `gender_forms`, `agreement_note`
- Example for Japanese: `antonym`, `type` (i-adjective vs. na-adjective)

*For {language} others*:
- `subtype`: adverb, preposition, pronoun, conjunction, interjection, phrase
- `usage_note`: one sentence on when to use it

**Language-Specific Exercise Types** (replaces `plural_formation` and `haben_vs_sein`):
- Example for French:
  - `avoir_vs_etre` (Passé Composé auxiliary selection)
  - `agreement_past_participle` (gender/number agreement with avoir)
- Example for Spanish:
  - `ser_vs_estar` (copula distinction)
  - `preterite_vs_imperfect` (tense selection)
- Example for Japanese:
  - `verb_te_form` (te-form morphology)
  - `formal_vs_informal` (keigo politeness levels)
- Example for Mandarin:
  - `measure_word_selection` (choosing correct classifier)
  - `aspect_marker` (using 了, 着, 过 correctly)

**Exercises**: 40+ total, distributed across 6 types:
1. fill_in_blank
2. multiple_choice
3. translation
4. error_correction
5. {language_specific_1} (e.g., avoir_vs_etre for French)
6. {language_specific_2} (e.g., agreement_past_participle for French)

Complexity: ~30% simple, ~40% medium, ~30% complex. Every vocabulary word appears in at least one exercise.

---

## Step 8 — Generate 4 Adapted Skill Definition Files

Generate all four skill SKILL.md files (`.claude/skills/{name}/SKILL.md`). Each follows the same step structure as the platform reference skills but with all language-specific content replaced.

**sentence-builder/SKILL.md**: Use the platform's sentence-builder as template, but:
- Change `"german"` → `"target"`
- Change `"english"` → `"translation"`
- Replace all German grammar terminology with {target_language} terminology
- Include language-appropriate examples in the target language
- Replace "Spanish transfer note" / "English transfer note" with actual language names

**sentence-renderer/SKILL.md**: Minimal changes:
- Change `"german"` → `"target"` field access
- Change `<html lang="de">` → `<html lang="{lang_code}">`
- Change footer/title strings to use {language} and {level}

**grammar-explainer/SKILL.md**: Language-adapted:
- Replace slug normalization rule with target-language rules
- Replace "Spanish transfer note" / "English transfer note" section names with actual language names
- Replace Präsens/Perfekt complexity descriptors with target-language tense names
- All examples in the target language

**vocab-drill/SKILL.md**: Most heavily language-adapted:
- Replace `vocab drill: lektion [N]` with `vocab drill: {chapter_term} [N]` (e.g., `unité`, `capítulo`)
- Replace PDF resource reference with actual paths or "use built-in knowledge"
- Replace word-type classification rules with target-language rules
- Replace noun/verb enrichment fields with target-language fields
- Replace `plural_formation` and `haben_vs_sein` with language-specific exercise types
- All examples in the target language

---

## Step 9 — Copy styles.css

Read `styles.css` from the platform root directory and write verbatim to `{language_slug}-learning-assistant/styles.css`.

No modifications needed — the stylesheet is language-neutral and works for all languages.

---

## Step 10 — Generate index.html (Starter)

Write `{language_slug}-learning-assistant/index.html` with a starter structure:

```html
<!DOCTYPE html>
<html lang="{lang_code}">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{Language} Learning Assistant — {level}</title>
  <link rel="stylesheet" href="../../styles.css">
</head>
<body>
<div class="container">
  <header>
    <h1>{Language} Learning Assistant</h1>
    <p class="meta">{level} Level · {textbook or "Free Practice"} · {goal}</p>
  </header>

  <section id="grammar">
    <h2><span class="section-badge badge-grammar">Grammar</span>Grammar Lessons</h2>
    <ul class="file-list">
      <li><em>No grammar lessons yet. Use `grammar: [topic]` to create one.</em></li>
    </ul>
  </section>

  <section id="sentences">
    <h2><span class="section-badge badge-sentences">Sentences</span>Sentence Practice</h2>
    <ul class="file-list">
      <li><em>No sentence drills yet. Use `sentence builder: [word]` to create one.</em></li>
    </ul>
  </section>

  <section id="vocabulary">
    <h2><span class="section-badge badge-vocabulary">Vocabulary</span>Vocabulary Drills</h2>
    <ul class="file-list">
      <li><em>No vocabulary drills yet. Use `vocab drill: {chapter_term} [N]` to create one.</em></li>
    </ul>
  </section>

  <footer>{Language} Learning Assistant · {level} · Generated {today}</footer>
</div>
</body>
</html>
```

---

## Step 11 — Generate README.md

Write `{language_slug}-learning-assistant/README.md`:

```markdown
# {Language} Learning Assistant

A personal {level}-level {language} learning tool built with Claude Code.
Generates sentence practice, grammar lessons, and vocabulary drills — rendered as styled HTML pages with dark mode support.

## Learner Profile

| Field | Value |
|-------|-------|
| Native language | {native_language} |
| Instruction language | {instruction_language} |
| Current level | {level} |
| Goal | {goal} |
| Textbook | {textbook or "Free practice"} |
| Correction style | {correction_style} |

## What's Inside

- **Sentence practice** — 10 example sentences per word, grouped by difficulty (simple / medium / complex), with {instruction_language} translations and grammar notes
- **Grammar lessons** — full explanations, 10+ examples, 12+ exercises across four types, and a solution key
- **Vocabulary drills** — words from your textbook/topic with enrichment (conjugations, declensions, gender, etc.) and 40+ targeted exercises
- **Dark mode** — automatically follows your OS preference via a single shared `styles.css`

## Project Structure

```
{language_slug}-learning-assistant/
  CLAUDE.md                 ← Your personalized context (read this first!)
  README.md                 ← This file
  index.html                ← Master index of all lessons (auto-updated)
  styles.css                ← Shared stylesheet (light + dark mode)
  data/
    sentences/              ← JSON source files
    sentences-html/         ← Rendered sentence pages
    grammar/                ← JSON source files
    grammar-html/           ← Rendered grammar pages
    vocabulary/             ← JSON source files
    vocabulary-html/        ← Rendered vocabulary pages
  resources/                ← Your textbook PDFs, glossaries, etc.
  .claude/skills/           ← Claude Code skill definitions
```

## Skills (Claude Code)

Invoke these skills by typing in Claude Code from this folder:

| Invocation | What it does |
|---|---|
| `sentence builder: [word]` | Generates 10 {level} sentences and saves JSON |
| `render sentences` | Renders unrendered JSON files to HTML |
| `grammar: [topic]` | Generates a full grammar lesson (explanation + exercises) |
| `vocab drill: {chapter_term} [N], [focus]` | Extracts vocabulary and generates exercises |

## Getting Started

1. **Read CLAUDE.md** — it contains your personalized learning profile and detailed skill specifications

2. **Try your first skill** (choose one):
   - `sentence builder: [word in {language}]`
   - `grammar: [grammar topic in English, e.g., "present tense" or "word order"]`

3. **Build a few lessons**, then run:
   - `render sentences` — converts your sentence JSONs to styled HTML

4. **For vocabulary drills** (if you have a textbook):
   - Put your textbook glossary PDF in `resources/`
   - Update the vocab-drill spec in CLAUDE.md with the PDF path
   - Run `vocab drill: {chapter_term} [N], [focus]`

5. **Share or fork** — each subfolder is completely self-contained and portable

## CLAUDE.md Reference

Your personalized CLAUDE.md is the source of truth for all content generation. It includes:
- Your learner profile
- Detailed skill specifications (language-adapted for {language})
- Design patterns and guidelines
- Language transfer notes ({native_language} ↔ {language}, {other_language} ↔ {language})

If your goals change or you progress to a new level, update CLAUDE.md and the skills will adapt.

---

**Generated:** {today}
**Status:** Active, all skills ready to use
```

---

## Step 12 — Generate .claude/settings.json

Write `{language_slug}-learning-assistant/.claude/settings.json`:

```json
{
  "allowedTools": [
    "Bash(mkdir -p data/*; ls data/)",
    "Write(data/**)",
    "Write(./*.html)",
    "Read(data/**)",
    "Read(resources/**)",
    "Read(CLAUDE.md)",
    "Read(README.md)"
  ]
}
```

This permits the skills to create directories, write JSON and HTML files, and read resources as needed.

---

## Step 13 — Update Platform Root Files

### 13.1 Update CLAUDE.md (root)

Read `/Users/csaravia/Documents/personal/language-learner-assistant/CLAUDE.md` (the platform root CLAUDE.md). Find the "Current Assistants" table. Append a new row:

```
| {Language} | {level} | `{language_slug}-learning-assistant/` | {today} |
```

Write the file back.

### 13.2 Update index.html (root)

Read `/Users/csaravia/Documents/personal/language-learner-assistant/index.html`. Find the `<ul class="file-list">` inside the `#assistants` section. Prepend (at the top of the list) a new entry:

```html
<li><a href="{language_slug}-learning-assistant/index.html">
  <span class="label">{Language} Learning Assistant</span>
  <span class="meta-inline">{level} · {native_language} → {language}</span>
  <span class="date">{today}</span>
</a></li>
```

Write the file back.

---

## Step 14 — Display Success Summary

Display a warm, encouraging summary:

```
## ✅ Assistant Created Successfully!

**{Language} Learning Assistant** is ready to use at:
📁 `{language_slug}-learning-assistant/`

---

### What Was Created:

- ✓ Personalized `CLAUDE.md` with your learner profile and full skill specs
- ✓ `sentence-builder/SKILL.md` — tailored for {language} grammar
- ✓ `sentence-renderer/SKILL.md` — converts JSON to styled HTML
- ✓ `grammar-explainer/SKILL.md` — generates {language} grammar lessons
- ✓ `vocab-drill/SKILL.md` — extracts and enriches {language} vocabulary
- ✓ `styles.css` — light + dark mode stylesheet
- ✓ `README.md` — quick-start documentation
- ✓ `index.html` — starter page (auto-updates as you add content)
- ✓ `data/` folders — ready to store sentences, grammar lessons, vocabulary
- ✓ Platform root updated with your new assistant in the registry

---

### Next Steps:

1. **Open the assistant folder** in Claude Code:
   Open `{language_slug}-learning-assistant/` as your working directory

2. **Read your personalized CLAUDE.md** — it contains everything you need to know about your specific setup

3. **Create your first skill**. Try one of these:
   - `sentence builder: {example_word_in_language}`
   - `grammar: {example_grammar_topic}`

4. {If textbook: "**Add your textbook glossary** to `resources/` and update the vocab-drill spec in CLAUDE.md with the PDF path"}
   {If no textbook: "**Focus on topics** you're interested in — the skills will use Claude's knowledge"}

5. **Generate HTML pages**:
   - After creating a few sentences, run `render sentences` to convert them to styled HTML
   - Your `index.html` auto-updates with links to all content

---

### Pro Tips:

- Your **CLAUDE.md** is the single source of truth — all skills read it for your profile and preferences
- The **skill files** are language-adapted — they contain {language}-specific grammar terminology, not German examples
- Your assistant is **completely portable** — you can clone the folder and use it anywhere
- **Update CLAUDE.md** whenever your goals change or you advance a level — skills adapt automatically
- All generated **HTML pages work offline** and support dark mode

---

**Congratulations! You're all set.** Happy learning! 🎉
```

---

## Edge Cases & Error Handling

### If the subfolder already exists:
Display:
> "A folder `{language_slug}-learning-assistant/` already exists. Options:
> 1. **Overwrite** — replace existing files (warning: you'll lose any unsaved changes)
> 2. **Different name** — create with a modified name (e.g., `{language_slug}-learning-assistant-2/`)
> 3. **Cancel** — don't create anything
>
> Choose: (1 / 2 / 3 / custom name)"

### If the user provides a non-Latin script language:
- Ensure the subfolder name is ASCII: `japanese-learning-assistant/`, `arabic-learning-assistant/`, not `日本語-learning-assistant/` or `العربية-learning-assistant/`
- Use the correct BCP 47 `lang` attribute: `ja`, `ar`, `zh-Hans`, etc.
- Explain in the CLAUDE.md: "All filenames and folder names are in ASCII for compatibility. The HTML pages use the correct language code (`{lang_code}`) so the browser renders your language correctly."

### If the user says no textbook:
- The vocab-drill spec changes to `vocab drill: topic [topic_name], [focus]` (topic-based instead of chapter-based)
- Explain in the CLAUDE.md: "No PDF glossary provided. The vocab-drill skill uses Claude's built-in knowledge of {language} A1 vocabulary. Specify a topic (e.g., `vocab drill: topic restaurant ordering, verbs`) and the skill generates exercises for that topic."

### If the user mentions a less-common language:
- Generate the CLAUDE.md and skills anyway, using Claude's knowledge
- Add a note in the generated CLAUDE.md: "⚠️ This language is less commonly taught. Some grammar details may be approximate and should be verified against your textbook or native speakers. Please report any issues to improve accuracy."