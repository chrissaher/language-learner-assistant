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

- **Custom sentence pairs** — your own English-French sentences, rendered as interactive accordion cards with toggle translations
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

### Content Generation
| Invocation | What it does |
|---|---|
| `sentence builder: [word]` | Generates 10 A1 sentences and saves JSON |
| `grammar: [topic]` | Generates a full grammar lesson (explanation + exercises) |
| `vocab drill: topic [name], [focus]` | Generates vocabulary drills with 40+ exercises |
| `register-sentence` | Register your own English-French sentence pairs |

### Content Rendering
| Invocation | What it does |
|---|---|
| `render sentences` | Renders sentence builder JSONs to HTML |
| `render custom sentences` | Renders your custom sentences to interactive HTML |

## Getting Started

### Quick Start
1. **Open `index.html`** — browse all your lessons in one place
2. **Click a lesson** — each is a standalone, styled HTML page
3. **Toggle sentences** — click cards to reveal French translations

### Create New Content
1. **Read CLAUDE.md** — it contains your personalized learning profile and detailed skill specifications

2. **Register custom sentences** (your own pairs):
   - `register-sentence` — add your English-French sentence pairs
   - `render custom sentences` — render them to interactive HTML

3. **Try other skills** (choose one):
   - `sentence builder: manger` (to eat)
   - `sentence builder: maison` (house)
   - `grammar: present tense`

4. **Build a few lessons**, then run:
   - `render sentences` — converts your sentence JSONs to styled HTML

5. **For vocabulary drills**, specify a topic:
   - `vocab drill: topic restaurant ordering, all`
   - `vocab drill: topic family, nouns`
   - `vocab drill: topic greetings, all`

6. **View everything** — run `render custom sentences` and open `index.html` to see all your work

## CLAUDE.md Reference

Your personalized CLAUDE.md is the source of truth for all content generation. It includes:
- Your learner profile (Slovak native, fluent in English/Spanish/German)
- Detailed skill specifications (language-adapted for French A1)
- Language transfer notes (Slovak ↔ French, English ↔ French, Spanish ↔ French, German ↔ French)
- Learning tips specific to French

If your goals change or you progress to a new level, update CLAUDE.md and the skills will adapt.

## 🌐 GitHub Pages Setup

To make this assistant available online as a GitHub Page:

1. **Push to GitHub** — add this folder to your repository
2. **Enable GitHub Pages** in your repo settings:
   - Go to Settings → Pages
   - Source: main branch (or your branch)
   - Choose folder: `french-learning-assistant/`
3. **Access online** — your assistant is available at:
   ```
   https://chrissaher.github.io/language-learner-assistant/french-learning-assistant/
   ```

## 📦 Portable & Self-Contained

This entire folder works standalone:
- Clone or download just this folder
- Open `index.html` in any browser
- Works offline — no internet needed after download
- No build tools or dependencies

---

**Generated:** 2026-03-17
**Status:** Active, all skills ready to use
**Next step:** `register-sentence` to add your pairs, or `sentence builder: [word]` to generate more content