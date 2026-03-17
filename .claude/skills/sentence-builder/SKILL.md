# sentence-builder Skill for French Learning Assistant

## Overview
Generate 10 example sentences for a given French word, grouped by difficulty (simple / medium / complex), with English translations and grammar notes. Helps learners see words in context and understand conjugations, gender agreement, and usage patterns.

## Invocation
- `sentence builder: [word]`
- `sentence builder: [word] (noun)` / `(verb)` / `(adjective)` — optional type hint for disambiguation

## Context from CLAUDE.md

- **Target language**: French (A1)
- **Instruction language**: English
- **Learner profile**: Native Slovak speaker, also fluent in English, Spanish, German. Travel/tourism/hobby goals. Prefers strict corrections and pronunciation tips.
- **Content scope**: Everyday life (greetings, meals, travel, shopping, directions, family, hobbies)
- **Correction style**: Strict — flag all errors, always show correct form

---

## Workflow

### Step 1: Parse Input
- Extract the word from the user's invocation
- Determine word type: noun, verb, adjective, adverb, or other (or accept user's type hint)
- Look up the word in French A1 vocabulary; if not found, ask the user to clarify or provide context

### Step 2: Determine Dictionary Form & Metadata
- **Nouns**: find the singular form and gender (m/f)
- **Verbs**: find the infinitive
- **Adjectives**: find the masculine singular form

### Step 3: Generate 10 Sentences
- **3 simple sentences** (A1, present tense, singular subject, high-frequency context)
- **3 medium sentences** (A1, present tense + 1 example in passé composé, plural or mixed person, moderate context)
- **4 complex sentences** (A1, include passé composé with correct auxiliary, mixed tenses, richer context)

#### 3a. For Verbs:
- **Simple**: Cover je, tu, il/elle in present indicative (infinitive present tense)
  - Example for "parler": "Je parle français", "Tu parles avec moi?", "Il parle doucement"
- **Medium**: Add nous/vous in present; introduce infinitive or passé composé (just 1 example)
  - Example: "Nous parlons français à la maison", "Vous parlez anglais?", "J'ai parlé avec ma mère"
- **Complex**: Include passé composé with correct avoir/être auxiliary; show varied contexts
  - Example: "Elle a parlé au téléphone pendant une heure", "Ils ont parlé de leurs vacances"

**Verb conjugation model for A1**:
- For regular verbs (-er, -ir, -re): show standard conjugation patterns
- For irregular verbs (être, avoir, aller, faire, venir, pouvoir, vouloir, devoir, savoir): show the actual conjugation
- Auxiliary choice (avoir vs. être): most verbs use avoir; use être for reflexive verbs and certain verbs of motion (aller, venir, arriver, partir, monter, descendre, sortir, rester, tomber, naître, mourir)

#### 3b. For Nouns:
- **Simple**: Show singular with definite and indefinite articles
  - Example for "maison": "La maison est grande", "Je veux une maison", "La maison rouge"
- **Medium**: Show plural; introduce gender agreement with adjectives
  - Example: "Les maisons sont petites", "Des maisons modernes", "Une maison blanche vs. un jardin blanc"
- **Complex**: Show varied articles (le/la/les, un/une/des, du/de la); gender agreement in context
  - Example: "Du lait pour le café", "De la eau n'existe pas (c'est 'de l'eau')", "Les femmes et les hommes parlent"

#### 3c. For Adjectives:
- **Simple**: Show predicate position (c'est + adjective)
  - Example for "grand": "C'est grand", "C'est très grand", "C'est un grand garçon"
- **Medium**: Show attributive position (adjective + noun or noun + adjective); introduce gender/number agreement
  - Example: "Un grand garçon", "Une grande fille", "De grands garçons"
- **Complex**: Show varied positions (some adjectives go before, some after noun) and full agreement
  - Example: "Un bel homme" (before), "Un homme heureux" (after), "Des belles maisons" (varies)

### Step 4: Add Grammar Notes
For each sentence, add a 1-line grammar note explaining:
- **Verbs**: the tense, person, and auxiliary (if passé composé), conjugation pattern
  - Example: "Past participle 'parlé' doesn't agree with subject (avoir auxiliary)"
- **Nouns**: the gender, number, article choice
  - Example: "Feminine singular with indefinite article (une)"
- **Adjectives**: gender/number agreement, position, special forms
  - Example: "Masculine plural agreement with 'garçons'; 'grand' → 'grands'"

### Step 5: Generate JSON
- Save JSON to `data/sentences/[word_stem].json` (stem = lowercase, accents removed, no spaces)
  - Example: "parler" → `parler.json`, "l'eau" → `leau.json`, "être" → `etre.json`

### Step 6: Output Summary
Display a summary to the user:
```
✅ Generated 10 sentences for "word" ([type])
Saved to: data/sentences/[word_stem].json

**Simple (3)** — present tense, common contexts
**Medium (3)** — present + passé composé, mixed persons
**Complex (4)** — passé composé, richer contexts

Next: Run `render sentences` to convert to HTML, or create more drills.
```

---

## JSON Output Format

```json
{
  "word": "parler",
  "base_form": "parler",
  "word_type": "verb",
  "generated_at": "2026-03-17T10:30:00Z",
  "sentences": [
    {
      "id": 1,
      "target": "Je parle français.",
      "translation": "I speak French.",
      "level": "simple",
      "grammar_note": "Present indicative, 1st person singular (je + -e ending for regular -er verbs)"
    },
    {
      "id": 2,
      "target": "Tu parles avec moi?",
      "translation": "Are you speaking with me?",
      "level": "simple",
      "grammar_note": "Present indicative, 2nd person singular (tu + -es ending)"
    },
    {
      "id": 3,
      "target": "Il parle doucement.",
      "translation": "He speaks slowly.",
      "level": "simple",
      "grammar_note": "Present indicative, 3rd person singular (il + -e ending, silent)"
    },
    {
      "id": 4,
      "target": "Nous parlons français à la maison.",
      "translation": "We speak French at home.",
      "level": "medium",
      "grammar_note": "Present indicative, 1st person plural (nous + -ons ending)"
    },
    {
      "id": 5,
      "target": "Vous parlez anglais?",
      "translation": "Do you (formal) speak English?",
      "level": "medium",
      "grammar_note": "Present indicative, 2nd person plural/formal (vous + -ez ending)"
    },
    {
      "id": 6,
      "target": "J'ai parlé avec ma mère.",
      "translation": "I spoke with my mother.",
      "level": "medium",
      "grammar_note": "Passé composé: auxiliary avoir (present) + past participle parlé (ends in -é for -er verbs)"
    },
    {
      "id": 7,
      "target": "Elle a parlé au téléphone pendant une heure.",
      "translation": "She spoke on the phone for an hour.",
      "level": "complex",
      "grammar_note": "Passé composé, 3rd person singular; duration phrase (pendant)"
    },
    {
      "id": 8,
      "target": "Ils ont parlé de leurs vacances.",
      "translation": "They spoke about their vacations.",
      "level": "complex",
      "grammar_note": "Passé composé, 3rd person plural (ils + ont); preposition 'de' + definite article 'leurs'"
    },
    {
      "id": 9,
      "target": "Tu parles trop!",
      "translation": "You talk too much!",
      "level": "medium",
      "grammar_note": "Present indicative imperative-style; 'trop' (too much) is an adverb"
    },
    {
      "id": 10,
      "target": "Parler est facile pour toi.",
      "translation": "Speaking is easy for you.",
      "level": "complex",
      "grammar_note": "Infinitive as subject of sentence (treated as masculine singular noun); 'pour + toi' = for you"
    }
  ]
}
```

---

## Language Transfer Notes (from CLAUDE.md)

Include these in your reasoning and prompts if relevant:

### Slovak → French
- Slovak has 6 cases and aspect; French has **neither**
- French gender (2) is simpler than German (3) but similar to Spanish
- **Pronunciation**: French nasal vowels (on, an, in, un) and silent letters are critical; these don't exist in Slovak
- **False friend**: French "j'ai" (I have) sounds like English "j'ay" or Slovak would hear "žaj" — practice this

### English → French
- English has no verb conjugation (except I/he); French conjugates for each person
- English and French share Latinate cognates (biology, action, etc.) but pronunciation differs
- **False friend**: English "actual" ≠ French "actuel" (means "present" or "current")
- **Word order**: mostly SVO (like English), but prepositions differ (e.g., "at the table" = "à la table", not "on the table")

### Spanish → French
- Both Romance languages with gender (2) and conjugation
- **Difference**: Spanish uses pretérito indefinido for completed past (ej. "Hablé"); French uses passé composé ("J'ai parlé")
- **Difference**: Spanish present tense differs from French (e.g., Spanish "hablo" but French "je parle")
- **Similarity**: Both have tu/vous registers for politeness

### German → French
- Both have articles and noun gender
- **Difference**: German has 4 cases; French has **zero cases**
- **Difference**: German conjugates adjectives; French does less
- **Similarity**: Both capitalize nouns (French doesn't; German does)

---

## Quality Checklist

Before outputting JSON, verify:
- ✓ 10 sentences total (3 simple, 3 medium, 4 complex)
- ✓ Verbs: cover multiple persons (at least je, tu, il/elle, nous, vous); include ≥1 passé composé
- ✓ Nouns: show singular/plural, definite/indefinite articles, gender agreement
- ✓ Adjectives: show predicate and attributive positions, gender/number agreement
- ✓ All sentences are grammatically correct (strict error-checking)
- ✓ All sentences are in everyday-life context (travel, meals, family, shopping, hobbies)
- ✓ Grammar notes are accurate and explain the construction
- ✓ Translations are natural and idiomatic English
- ✓ All A1 vocabulary (no subjunctive, complex conditional, or advanced tenses)

---

## Pronunciation Guidance (for Grammar Notes)

When the word has tricky French pronunciation, include a note:
- Nasal vowels: "on", "an", "in", "un" (sound like nose, not English)
- Silent letters: final -t, -s, -d, -e, -ent are usually silent
- Liaison: connect certain word endings to vowel-starting next words (e.g., "Les_amis")
- H aspiration: some h's are "aspiré" (block liaison, like "le huit" not "l'huit")

---

## Error Handling

- **Unknown word**: Ask the user for context or clarification. "I'm not familiar with this word. Could you provide an example sentence or clarify the context?"
- **Ambiguous word** (e.g., "parler" could be noun or verb): Ask the user for type hint or use context. Default to most common word type.
- **Misspelling**: Suggest the correct French spelling and confirm before proceeding.
- **Out-of-scope** (e.g., very advanced or technical word): Politely suggest an A1-equivalent word instead.

---

## Example Invocations & Outputs

### Example 1: Verb
**User**: `sentence builder: manger`
**Output**: JSON with 10 sentences about "manger" (to eat), covering je/tu/il/nous/vous in present, plus passé composé examples

### Example 2: Noun
**User**: `sentence builder: maison`
**Output**: JSON with 10 sentences about "maison" (house, feminine), showing singular/plural, la/une/des forms, gender agreement with adjectives

### Example 3: Adjective
**User**: `sentence builder: beau (adjective)`
**Output**: JSON with 10 sentences about "beau" (beautiful, masculine), showing "un beau garçon", "une belle fille", predicate and attributive positions

---

## Integration with Other Skills

- **Sentence Renderer** reads this JSON and converts to styled HTML
- **Vocab Drill** may use sentences from here as exercise examples
- **Grammar Explainer** may cross-reference sentences generated here to illustrate a grammar rule

---

**Status**: Skill ready for use.
**Language**: French A1
**Instruction language**: English