# grammar-explainer Skill for French Learning Assistant

## Overview
Generate a full grammar lesson: explanation, conjugation/declension tables, 10+ examples, pronunciation tips, transfer notes, and 12+ exercises (4 core types) with a solution key. Helps learners understand the "why" behind French grammar and provides practice to cement understanding.

## Invocation
- `grammar: [topic]`
- `grammar: [topic], focus: [specific aspect]`

**Examples**:
- `grammar: present tense` — present indicative conjugation
- `grammar: passé composé` — compound past, avoir/être choice, past participle agreement
- `grammar: gender and articles` — gender assignment, article use (le/la/les, un/une/des)
- `grammar: word order` — French SVO order, adjective placement, inversion in questions
- `grammar: imperatives` — commands (tu, nous, vous forms)
- `grammar: relative pronouns, focus: qui vs que` — qui (subject) vs que (object)

---

## Workflow

### Step 1: Parse Input
- Extract topic from user invocation
- Extract optional focus from comma-separated detail
- Normalize topic: remove accents, convert to ASCII, lowercase
  - Example: "Passé Composé" → `passe_compose`
  - Example: "Genre et Articles" → `genre_et_articles`

### Step 2: Generate Comprehensive Explanation
Create the following sections (all in English):

#### 2.1 "What is it?"
Definition in French grammar terms. Example:
> "The present indicative (présent) is the base, here-and-now tense in French. It expresses actions and states that are happening now, happen regularly, or are generally true."

#### 2.2 "When to use it"
Context and conditions specific to French. Example:
> "Use present indicative for:
> - Actions happening right now: 'Je parle' (I am speaking)
> - Habitual/repeated actions: 'Je parle français chaque jour' (I speak French every day)
> - General truths: 'La terre tourne' (The Earth spins)
> - Near future: 'Je vais demain' (I'm going tomorrow)"

#### 2.3 "How to form it"
Step-by-step construction for regular and irregular verbs. Example for present tense:
> "Regular -er verbs (parler, manger, danser, etc.):
> - Remove -er: parl-
> - Add personal ending: -e, -es, -e, -ons, -ez, -ent
> - je parle, tu parles, il/elle parle, nous parlons, vous parlez, ils/elles parlent"

If applicable, include a conjugation table (see Step 3 below).

#### 2.4 "Common Exceptions"
Irregular forms at A1 level. Example for present tense:
> "Common irregular verbs:
> - être (to be): je suis, tu es, il/elle est, nous sommes, vous êtes, ils/elles sont
> - avoir (to have): j'ai, tu as, il/elle a, nous avons, vous avez, ils/elles ont
> - aller (to go): je vais, tu vas, il/elle va, nous allons, vous allez, ils/elles vont
> - faire (to do/make): je fais, tu fais, il/elle fait, nous faisons, vous faites, ils/elles font"

#### 2.5 Transfer Notes
Include notes for the learner's language pair (from CLAUDE.md):
- **Slovak → French**: "Slovak uses aspect (perfective/imperfective); French doesn't. French tenses (présent vs. imparfait vs. passé composé) mark time, not completion."
- **English → French**: "English present tense doesn't conjugate ('I/you/he speaks'); French does (je/tu/il parle). This is a major difference."
- **Spanish → French** (optional): "Spanish uses preterite (hablé) for single past events; French uses passé composé (j'ai parlé). The usage is similar, but forms differ."
- **German → French** (optional): "German has complex sentence structure with verbs at clause end; French word order is simpler and more like English."

#### 2.6 Pronunciation Tips
For grammar topics where pronunciation is relevant. Example:
> "In present tense -er verbs, the endings are mostly silent:
> - je parle, tu parles, il parle sound identical (parl)
> - The difference is only in nous (nous parlons, -ons added) and vous (vous parlez, -ez added)
> - Nasal vowels in parlons and parlez are critical to hear and produce"

#### 2.7 "Key Takeaway"
One bold sentence with the single most critical point. Example:
> **In French, verb conjugation changes with the subject (je/tu/il/nous/vous/ils), and you must learn these patterns because they're used constantly.**

### Step 3: Generate Conjugation/Declension Table (if applicable)
For verb or adjective topics, include an actual table showing the forms. Example:

| Person | Regular -er (parler) | Regular -ir (finir) | avoir | être | aller |
|---|---|---|---|---|---|
| je | parle | finis | ai | suis | vais |
| tu | parles | finis | as | es | vas |
| il/elle | parle | finit | a | est | va |
| nous | parlons | finissons | avons | sommes | allons |
| vous | parlez | finissez | avez | êtes | allez |
| ils/elles | parlent | finissent | ont | sont | vont |

### Step 4: Generate 10+ Examples
- **3 simple** (A1 vocabulary, present tense, straightforward context)
- **3 medium** (A1 vocabulary, present + mixed constructions, slightly complex context)
- **4+ complex** (A1 vocabulary, varied tenses if relevant, richer context, shows edge cases)

Each example:
- French sentence in larger text
- English translation
- Grammar note (1 line) explaining the construction

Example:
```
Je suis étudiant.
→ I am a student.
Note: Subject pronoun "je" + verb "suis" (être in present); no article needed before profession/status
```

### Step 5: Generate 12+ Exercises
Distribute across 4 core types:

1. **fill_in_blank** (4-5 exercises): Complete a sentence with the correct form
   - Prompt: "Complete the sentence with the correct form of [verb/adjective/etc.]"
   - Solution: the correct form
   - Explanation: why this form is correct

2. **multiple_choice** (3-4 exercises): Choose the correct form from 3-4 options
   - Prompt: sentence with blank
   - Options: a) form1, b) form2, c) form3, d) form4
   - Solution: correct letter
   - Explanation: why this option is correct; why others are wrong

3. **translation** (2-3 exercises): Translate English to French (or vice versa)
   - Prompt (in English): "Translate to French: [sentence]"
   - Solution: the French translation
   - Explanation: grammar rule used, key vocabulary

4. **error_correction** (3-4 exercises): Identify and fix the error
   - Prompt (in French): sentence with error
   - Solution: corrected sentence
   - Explanation: what was wrong, what rule applies

**Complexity distribution**: roughly 1/3 simple, 1/3 medium, 1/3 complex across all exercises

**Content**: all exercises relate to the grammar topic; use A1 vocabulary and everyday-life context

### Step 6: Generate Solution Key
Compile all exercises with:
- Exercise number
- Correct answer/solution
- Explanation (1-2 sentences on why this is correct)

---

## JSON Output Format

```json
{
  "topic": "present tense",
  "focus": null,
  "topic_slug": "present_tense",
  "generated_at": "2026-03-17T14:00:00Z",
  "explanation": {
    "sections": [
      {
        "title": "What is it?",
        "content": "The present indicative (présent) is the base, here-and-now tense..."
      },
      {
        "title": "When to use it",
        "content": "Use present indicative for..."
      },
      {
        "title": "How to form it",
        "content": "Regular -er verbs (parler, manger, etc.)..."
      },
      {
        "title": "Common Exceptions",
        "content": "être, avoir, aller, faire..."
      },
      {
        "title": "Conjugation Table",
        "content": "| Person | -er | -ir | avoir | être | aller | ..."
      }
    ],
    "transfer_notes": [
      "Slovak → French: Slovak uses aspect (perfective/imperfective); French doesn't. French tenses mark time, not completion.",
      "English → French: English present doesn't conjugate; French does. 'I/you/he speaks' vs. 'je/tu/il parle'."
    ],
    "pronunciation_tips": [
      "In present -er verbs, the endings are mostly silent: je parle, tu parles, il parle sound identical.",
      "The difference is heard in nous (parlons) and vous (parlez) with nasal vowels."
    ],
    "key_takeaway": "In French, verb conjugation changes with the subject, and you must learn these patterns because they're used constantly."
  },
  "examples": [
    {
      "id": 1,
      "target": "Je suis étudiant.",
      "translation": "I am a student.",
      "level": "simple",
      "grammar_note": "Subject pronoun + verb 'suis' (être); no article before profession"
    }
  ],
  "exercises": [
    {
      "id": 1,
      "type": "fill_in_blank",
      "level": "simple",
      "prompt": "Complete: 'Je ___ français.' (parler in present)",
      "solution": "parle",
      "explanation": "Regular -er verb 'parler'; 1st person singular present = je parle"
    },
    {
      "id": 2,
      "type": "multiple_choice",
      "level": "simple",
      "prompt": "Which is correct? 'Nous ___ au cinéma.'",
      "options": ["a) allons", "b) allez", "c) aller", "d) vais"],
      "solution": "a",
      "explanation": "'allons' is the nous form of 'aller' in present. 'allez' is vous (formal), 'aller' is infinitive, 'vais' is je."
    },
    {
      "id": 3,
      "type": "translation",
      "level": "medium",
      "prompt": "Translate to French: 'They (feminine) are happy.'",
      "solution": "Elles sont heureuses.",
      "explanation": "Feminine plural subject pronoun 'elles' + verb 'sont' (être); adjective 'heureuses' agrees (feminine plural)"
    },
    {
      "id": 4,
      "type": "error_correction",
      "level": "medium",
      "prompt": "Fix: 'Je suis allé au marché. Elle suis allée aussi.'",
      "solution": "Je suis allé au marché. Elle est allée aussi.",
      "explanation": "'Elle' requires 'est' (3rd person singular of être), not 'suis' (1st person). Passé composé subject-verb agreement."
    }
  ]
}
```

---

## Language-Specific Topics & Focus Areas

### Common A1 Topics:
- **Verbs & Tenses**: Present indicative, Passé composé, Immediate future (aller + infinitive), Imperative
- **Nouns & Articles**: Indefinite articles (un/une/des), Definite articles (le/la/les), Gender, Plural forms, Partitive (du/de la)
- **Adjectives**: Agreement (gender/number), Placement (before vs. after noun), Common adjectives
- **Pronouns**: Subject pronouns (je/tu/il/nous/vous/ils), Direct object pronouns (le/la/les), Indirect object pronouns (lui/leur)
- **Prepositions**: à, de, en, pour, par, avec, sans, sur, sous, etc.
- **Word Order**: SVO basic order, Inversion in questions (est-ce que vs. inversion), Adjective placement
- **Negation**: ne...pas, ne...jamais, ne...rien, ne...plus

---

## Slug Normalization (for Filenames)

Convert topic name to lowercase filename:
- Remove accents: é→e, è→e, ê→e, à→a, ç→c, ù→u, etc.
- Replace spaces with underscores
- Remove special characters

Examples:
- "Passé Composé" → `passe_compose`
- "Genre et Articles" → `genre_et_articles`
- "Pronoms: Lui vs. Leur" → `pronoms_lui_vs_leur`

---

## Quality Checklist

- ✓ Explanation covers "What", "When", "How", "Exceptions", "Transfer Notes", "Pronunciation", "Key Takeaway"
- ✓ Conjugation/Declension table (if applicable) is correct and complete
- ✓ 10+ examples: 3 simple, 3 medium, 4+ complex, all with English translations and grammar notes
- ✓ 12+ exercises: distributed across 4 types, ~1/3 each difficulty level
- ✓ Solution key provided with explanations
- ✓ All French text is grammatically correct (strict error-checking)
- ✓ All exercises are A1-appropriate vocabulary
- ✓ Transfer notes highlight actual paralells or false friends for Slovak/English/Spanish/German speakers
- ✓ Pronunciation tips included where relevant
- ✓ Language-appropriate terminology (French grammar terms, not German)

---

## Error Handling

- **Unknown topic**: Ask for clarification. "I'm not familiar with this French grammar topic. Could you be more specific? (e.g., 'present tense', 'passé composé', 'adjective agreement')"
- **Too advanced for A1**: Suggest an A1-appropriate alternative. "This topic is typically B1+. For A1, I recommend learning [related, simpler topic] first."
- **Ambiguous focus**: Ask for clarification. "Which aspect of [topic] would you like to focus on? (e.g., conjugation, agreement, usage)"

---

## Integration with Other Skills

- **Sentence Builder**: May reference grammar lessons to explain constructions
- **Vocab Drill**: May generate exercises based on grammar rules taught here
- Users can link to grammar HTML pages from `index.html`

---

**Status**: Skill ready for use.
**Language**: French A1
**Instruction language**: English
**Correction style**: Strict (flag all errors, show correct form)