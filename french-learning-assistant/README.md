# French Learning Assistant

A personal A1-level French learning tool built with Claude Code.
Generates sentence practice, grammar lessons, and vocabulary drills — rendered as styled HTML pages with dark mode support.

## Learner Profile

| Field | Value |
|-------|-------|
| Native language | Slovak |
| Also fluent in | English, Spanish, German |
| Instruction language | English |
| Current level | A1 (beginner) |
| Goal | Travel, tourism, and hobby |
| Study method | Custom sentence pairs (English ↔ French, translate and repeat); will add textbook later |
| Correction style | Strict (flag all errors with correct forms) |

## What's Inside

- **Sentence practice** — 10 example sentences per word, grouped by difficulty (simple / medium / complex), with English translations and grammar notes
- **Grammar lessons** — full explanations, 10+ examples, 12+ exercises across four types, transfer notes for Slovak/English/Spanish/German speakers, and a solution key
- **Vocabulary drills** — words from topics you choose (or textbooks later) with enrichment (conjugations, gender, forms, etc.) and 40+ targeted exercises
- **Dark mode** — automatically follows your OS preference via a single shared `styles.css`

## Project Structure

```
french-learning-assistant/
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
| `sentence builder: [word]` | Generates 10 A1 sentences and saves JSON |
| `render sentences` | Renders unrendered JSON files to HTML |
| `grammar: [topic]` | Generates a full grammar lesson (explanation + exercises) |
| `vocab drill: topic [name], [focus]` | Generates vocabulary drills with 40+ exercises |

## Getting Started

1. **Read CLAUDE.md** — it contains your personalized learning profile and detailed skill specifications

2. **Try your first skill** (choose one):
   - `sentence builder: manger` (to eat)
   - `sentence builder: maison` (house)
   - `grammar: present tense`

3. **Build a few lessons**, then run:
   - `render sentences` — converts your sentence JSONs to styled HTML

4. **For vocabulary drills**, specify a topic:
   - `vocab drill: topic restaurant ordering, all`
   - `vocab drill: topic family, nouns`
   - `vocab drill: topic greetings, all`

5. **Share or fork** — each subfolder is completely self-contained and portable

## CLAUDE.md Reference

Your personalized CLAUDE.md is the source of truth for all content generation. It includes:
- Your learner profile (Slovak native, fluent in English/Spanish/German)
- Detailed skill specifications (language-adapted for French A1)
- Language transfer notes (Slovak ↔ French, English ↔ French, Spanish ↔ French, German ↔ French)
- Learning tips specific to French

If your goals change or you progress to a new level, update CLAUDE.md and the skills will adapt.

---

**Generated:** 2026-03-17
**Status:** Active, all skills ready to use
**Next step:** `sentence builder: [word]` or `grammar: [topic]`