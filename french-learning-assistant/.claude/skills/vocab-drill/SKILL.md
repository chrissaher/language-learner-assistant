# vocab-drill Skill for French Learning Assistant

## Overview
Extract vocabulary from topics or (future) textbooks, then generate comprehensive drills with enrichment data (gender, conjugations, related forms) and 40+ targeted exercises across 6 types. Helps learners master core vocabulary and understand relationships between words.

## Invocation

### Current (No Textbook)
- `vocab drill: topic [topic_name], [focus]`
- Example: `vocab drill: topic restaurant ordering, verbs`
- Example: `vocab drill: topic family, nouns`
- Example: `vocab drill: topic greetings, all`

### Future (When Textbook is Added)
- `vocab drill: chapter [N], [focus]`
- Example: `vocab drill: chapter 2, verbs`
- Example: `vocab drill: chapter 3, nouns`

---

## Workflow

### Step 1: Parse Input
- Extract topic (e.g., "restaurant ordering") or chapter number
- Extract focus (e.g., "verbs", "nouns", "adjectives", "all")
- If focus not provided, default to "all"

### Step 2: Compile Vocabulary List

#### Current Mode (No Textbook):
Use Claude's built-in knowledge of French A1 vocabulary for the specified topic.

Collect 15-25 words (mix of word types based on focus):
- If focus "verbs": 8-12 common verbs + 3-5 nouns/other
- If focus "nouns": 10-15 nouns + 2-3 verbs/adjectives
- If focus "adjectives": 8-12 adjectives + 3-5 nouns/verbs
- If focus "all": balanced mix of all types

Example for "restaurant ordering":
- Verbs: commander (to order), manger (to eat), boire (to drink), payer (to pay), vouloir (to want), avoir (to have)
- Nouns: menu, plat, vin, café, table, serveur, assiette, fourchette
- Adjectives: bon, chaud, froid, délicieux, épicé
- Other: s'il vous plaît, merci, l'addition (the bill), à point (medium), bien cuit (well-done)

### Step 3: Classify Each Word
Use French word-type rules:
- **Noun**: appears with article; can be singular/plural; has gender
- **Verb**: infinitive (-er, -ir, -re) or irregular (être, avoir, aller, etc.)
- **Adjective**: modifies noun; has gender/number agreement
- **Other**: adverb (-ment ending or fixed), preposition, pronoun, interjection, phrase

### Step 4: Generate Enrichment Data for Each Word

#### For Nouns:
- `article`: le, la, l', un, une, des (with full forms)
- `gender`: masculine (m) or feminine (f)
- `plural_form`: the plural spelling (usually adds -s)
- `pattern_hint`: rule if learnable (e.g., "-tion → always feminine")
- `pattern_reliability`: "high" / "moderate" / "memorize"
- `example_with_article`: full noun phrase with article and adjective (e.g., "un bon restaurant")

#### For Verbs:
- `infinitive`: base form (-er, -ir, -re)
- `conjugation_present`: present indicative for je/tu/il-elle/nous/vous (if regular)
- `passe_compose_example`: example sentence in passé composé
- `auxiliary`: "avoir" or "être"
- `is_irregular`: boolean
- `verb_note`: optional (reflexive, transitive, intransitive, etc.)
- `pronunciation_tip`: if relevant (e.g., "aller" sounds like "ah-lay")

#### For Adjectives:
- `masculine_form`: base form (e.g., "bon")
- `feminine_form`: with gender agreement (e.g., "bonne")
- `plural_forms`: masculine and feminine plural (e.g., "bons", "bonnes")
- `position`: "before" or "after" noun in French
- `antonym`: opposite word if relevant (e.g., "bon" ↔ "mauvais")
- `agreement_note`: how form changes for gender/number

#### For Other Words:
- `subtype`: "adverb" / "preposition" / "pronoun" / "interjection" / "phrase"
- `usage_note`: one sentence on when/how to use it

### Step 5: Generate 40+ Exercises
Distribute across 6 types, ~30% simple / 40% medium / 30% complex:

1. **fill_in_blank** (7-8 exercises): Complete a sentence with the correct form (gender, number, conjugation, etc.)
2. **multiple_choice** (6-8 exercises): Choose correct form from 3-4 options
3. **translation** (5-6 exercises): Translate English to French (or vice versa)
4. **error_correction** (5-6 exercises): Identify and fix the error (gender, conjugation, agreement, etc.)
5. **avoir_vs_etre** (5-6 exercises): Choose the correct passé composé auxiliary
6. **agreement_past_participle** (4-5 exercises): Correct past participle agreement with avoir

Every vocabulary word appears in at least 2-3 exercises.

### Step 6: Generate JSON
Save to `data/vocabulary/[topic_slug]_[focus].json` or `data/vocabulary/chapter_[N]_[focus].json`

Example: `data/vocabulary/restaurant_ordering_all.json`

### Step 7: Output Summary
```
✅ Generated vocabulary drill: "Restaurant Ordering" (all word types)
15 words total: 6 verbs, 5 nouns, 2 adjectives, 2 other
40+ exercises across 6 types

Saved to: data/vocabulary/restaurant_ordering_all.json

Next: Run `render sentences` (coming soon), or create more drills.
```

---

## JSON Output Format

```json
{
  "topic": "restaurant ordering",
  "focus": "all",
  "topic_slug": "restaurant_ordering",
  "word_count": 15,
  "generated_at": "2026-03-17T15:00:00Z",
  "vocabulary": [
    {
      "id": 1,
      "word": "menu",
      "word_type": "noun",
      "article": "le",
      "gender": "m",
      "plural_form": "menus",
      "pattern_hint": "Masculine noun, regular plural (adds -s)",
      "pattern_reliability": "high",
      "english": "menu",
      "example_with_article": "le menu français"
    },
    {
      "id": 2,
      "word": "commander",
      "word_type": "verb",
      "infinitive": "commander",
      "conjugation_present": {
        "je": "commande",
        "tu": "commandes",
        "il_elle": "commande",
        "nous": "commandons",
        "vous": "commandez",
        "ils_elles": "commandent"
      },
      "passe_compose_example": "J'ai commandé un café.",
      "auxiliary": "avoir",
      "is_irregular": false,
      "english": "to order",
      "pronunciation_tip": "Stress on first syllable: COM-ahn-day"
    },
    {
      "id": 3,
      "word": "bon",
      "word_type": "adjective",
      "masculine_form": "bon",
      "feminine_form": "bonne",
      "plural_forms": {
        "masculine": "bons",
        "feminine": "bonnes"
      },
      "position": "before",
      "antonym": "mauvais",
      "agreement_note": "Doubles final consonant in feminine (bon → bonne); masculine singular before vowel is 'bon' → 'bel' (e.g., 'un bon vin' but 'un bel ami')",
      "english": "good"
    },
    {
      "id": 4,
      "word": "s'il vous plaît",
      "word_type": "other",
      "subtype": "phrase",
      "usage_note": "Polite request phrase; used when asking for something (formal 'vous' form). Informal: 's'il te plaît'",
      "english": "please"
    }
  ],
  "exercises": [
    {
      "id": 1,
      "type": "fill_in_blank",
      "level": "simple",
      "prompt": "Complete: 'Je veux un ___ (menu).'",
      "solution": "menu",
      "explanation": "Masculine noun 'menu' in indefinite article 'un'; no gender agreement with adjective yet in this simple context"
    },
    {
      "id": 2,
      "type": "multiple_choice",
      "level": "simple",
      "prompt": "Choose: 'La ___ est bonne.' (nourriture, food)",
      "options": ["a) nourriture", "b) nourritures", "c) nouriture", "d) nourritures"],
      "solution": "a",
      "explanation": "Feminine singular 'nourriture' with singular adjective 'bonne'. Spelled correctly (not 'nouriture'). Plural isn't used here."
    },
    {
      "id": 3,
      "type": "translation",
      "level": "medium",
      "prompt": "Translate: 'I would like to order a good coffee, please.'",
      "solution": "Je voudrais commander un bon café, s'il vous plaît.",
      "explanation": "Conditional 'je voudrais' (I would like); 'bon' (good) before masculine noun 'café'; 'un' (indefinite article); 's'il vous plaît' (please, formal)"
    },
    {
      "id": 4,
      "type": "error_correction",
      "level": "simple",
      "prompt": "Fix: 'Je commande un menu bons.'",
      "solution": "Je commande un menu bon. / Je commande de bons menus.",
      "explanation": "'menu' is singular, so 'bons' (plural) is wrong. Either use singular 'bon' or plural 'menus' with 'bons'."
    },
    {
      "id": 5,
      "type": "avoir_vs_etre",
      "level": "medium",
      "prompt": "Choose: 'J'___ commandé un café.' (avoir or être?)",
      "solution": "ai",
      "explanation": "'Commander' uses 'avoir' auxiliary: j'ai commandé. 'Être' is only for reflexive verbs and specific motion verbs (aller, venir, arriver, etc.)"
    },
    {
      "id": 6,
      "type": "agreement_past_participle",
      "level": "complex",
      "prompt": "Correct: 'Les filles, je les ai commandé des crèmes glacées.'",
      "solution": "Les filles, je les ai commandées des crèmes glacées.",
      "explanation": "'Commandées' (past participle) agrees with the direct object pronoun 'les' (feminine plural girls). With avoir, agreement happens when direct object precedes the verb."
    }
  ]
}
```

---

## Topic-Specific Vocabulary (Examples for A1)

### Restaurant Ordering
- **Verbs**: commander, manger, boire, payer, avoir, vouloir, prendre, goûter
- **Nouns**: menu, plat, vin, café, table, serveur, assiette, fourchette, couteau, verre, l'addition
- **Adjectives**: bon, chaud, froid, délicieux, épicé, cher, sucré, salé
- **Other**: s'il vous plaît, merci, l'eau (water), avec (with), sans (without), à point (medium), bien cuit (well-done)

### Family
- **Nouns**: père, mère, frère, sœur, grand-mère, grand-père, tante, oncle, cousin, cousine, mari, femme, enfant, fils, fille
- **Adjectives**: jeune, vieux, aîné, cadet
- **Verbs**: avoir, être, aimer, vivre
- **Other**: dans, avec, mon/ma, mon/mes

### Greetings
- **Interjections/Phrases**: bonjour, bonsoir, bonne nuit, au revoir, salut, comment allez-vous, ça va, je vais bien, enchanté(e)
- **Verbs**: aller, être, s'appeler
- **Other**: et vous, merci

---

## Language-Specific Notes

### Gender Assignment Patterns
French nouns don't have clear rules, but some patterns help:
- **Feminine patterns**: -e (e.g., table, maison), -tion (e.g., nation), -ée (e.g., journée), -ure (e.g., nature), -ité (e.g., liberté), -ice (e.g., directrice)
- **Masculine patterns**: -ment (e.g., moment), -eau (e.g., bureau), -et (e.g., paquet), -er (e.g., mer — wait, exceptions!), -age (e.g., voyage)
- **No clear pattern**: learn with article!

### Verb Conjugation at A1
- Regular -er verbs (largest group): parler, manger, danser, etc. → je parle, tu parles, il parle, nous parlons, vous parlez, ils parlent
- Regular -ir verbs: finir, partir, sentir, etc. → je finis, tu finis, il finit, nous finissons, vous finissez, ils finissent
- Regular -re verbs: vendre, prendre, etc. → je vends, tu vends, il vend, nous vendons, vous vendez, ils vendent
- Irregular verbs at A1: être, avoir, aller, faire, pouvoir, vouloir, devoir, venir, savoir, tenir, etc. (memorize these!)

### Adjective Placement
- **Before noun**: beau, bon, petit, grand, jeune, vieux, joli, premier, dernier, nouveau, mauvais, meilleur
- **After noun**: most other adjectives (color, nationality, shape, etc.)
- **Both**: some adjectives change meaning depending on position (ancien, pauvre, propre, simple, certaine)

---

## Slug Normalization (for Filenames)

Convert topic name to lowercase filename:
- Remove accents: é→e, è→e, ê→e, à→a, ç→c, ù→u, etc.
- Replace spaces with underscores
- Remove special characters

Examples:
- "Restaurant Ordering" → `restaurant_ordering`
- "Famille (Family)" → `famille`
- "Salutations" → `salutations`
- "Chapitre 1" → `chapter_1` (for future textbook mode)

---

## Quality Checklist

- ✓ Vocabulary list compiled correctly (15-25 words, balanced by word type based on focus)
- ✓ All words are A1-appropriate
- ✓ All enrichment data is accurate (gender, conjugations, forms)
- ✓ 40+ exercises: ~7-8 per type (fill_in_blank, multiple_choice, translation, error_correction, avoir_vs_etre, agreement_past_participle)
- ✓ ~30% simple, ~40% medium, ~30% complex exercises
- ✓ Every vocabulary word appears in at least 2-3 exercises
- ✓ All French text is grammatically correct (strict error-checking)
- ✓ Solution key provided with explanations
- ✓ avoir_vs_etre exercises test auxiliary choice for passé composé
- ✓ agreement_past_participle exercises test gender/number agreement with avoir

---

## Error Handling

- **Unknown topic**: Ask for clarification. "Which topic would you like to focus on? (e.g., 'restaurant ordering', 'family', 'shopping', 'travel', 'greetings')"
- **Invalid chapter number** (future mode): "Chapter [N] is not found. Could you provide the chapter name or verify the number?"
- **Ambiguous focus**: Assume "all" if not specified. Or ask: "Which word types? (verbs / nouns / adjectives / all)"

---

## Integration with Other Skills

- **Sentence Builder**: May use vocabulary words from here as examples
- **Grammar Explainer**: May reference grammar patterns in vocabulary enrichment
- Users can link to vocabulary drills from `index.html`

---

## Future Enhancement: Textbook Mode

When the user adds a textbook:
1. Update CLAUDE.md with: PDF path, chapter structure (chapter names, number of vocabulary words per chapter), vocabulary list format
2. In this skill, switch invocation from `topic` to `chapter [N]`
3. Extract vocabulary from PDF or from Claude's knowledge of that specific textbook
4. Generate same 40+ exercises

---

**Status**: Skill ready for use (topic-based mode).
**Language**: French A1
**Instruction language**: English
**Correction style**: Strict (flag all errors, show correct form)
