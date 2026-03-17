# register-sentence Skill for French Learning Assistant

## Overview
Register custom English-to-French sentence pairs from your personal practice. Translates English sentences (with optional focus markers) to French and stores them in a versioned JSON file for review and study.

## Invocation
`register-sentence`

Then provide a newline-separated list of English sentences. Each sentence can include `*focus*` markers (in markdown) to highlight a specific word or phrase you want to emphasize.

### Example Input
```
I like to *eat* bread in the morning
She *speaks* French very well
We *go* to the market every Saturday
```

## Context from CLAUDE.md

- **Target language**: French (A1)
- **Instruction language**: English
- **Learner profile**: Native Slovak speaker, also fluent in English, Spanish, German. Travel/tourism/hobby goals. Prefers strict corrections.
- **Correction style**: Strict — flag all errors, always show correct form

---

## Workflow

### Step 1: Parse Input
- Extract newline-separated sentences from user input
- For each sentence, identify any `*focus*` markers
- Extract the focus word(s) (text between asterisks)
- Store the base sentence and focus separately

### Step 2: Translate to French
- Translate each English sentence to French
- Preserve the focus word placement in the French translation
- Apply markdown `*focus*` formatting to the corresponding French word
- Ensure translations are:
  - Grammatically correct (A1 level)
  - Natural and idiomatic
  - Suitable for travel/tourism/hobby contexts

### Step 3: Extract Focus Words
- From English: extract the text between `*` markers → `focus_word_en`
- From French translation: identify the corresponding French word(s) → `focus_word_fr`
- If no focus markers, both fields are `null`

### Step 4: Determine Version
- Check `data/custom-sentences/` for existing files matching pattern `custom-sentences-v*.json`
- Find the highest version number
- Increment by 1 for the new file

### Step 5: Generate JSON
- Save to `data/custom-sentences/custom-sentences-v[N].json` where [N] is the version number
- Include metadata: version, created_at (ISO 8601), sentence count
- Each sentence gets a sequential id within this file

### Step 6: Output Summary
Display a summary showing:
- Number of sentences registered
- The version file created (e.g., `custom-sentences-v1.json`)
- Preview of 2–3 examples
- Next steps (e.g., "Review in `data/custom-sentences-v1.json`, edit if needed, or register more with `register-sentence`")

---

## JSON Output Format

```json
{
  "version": 1,
  "created_at": "2026-03-17T10:30:00Z",
  "count": 3,
  "sentences": [
    {
      "id": 1,
      "english": "I like to *eat* bread in the morning",
      "french": "J'aime *manger* du pain le matin",
      "focus_word_en": "eat",
      "focus_word_fr": "manger",
      "topic": null,
      "level": null
    },
    {
      "id": 2,
      "english": "She *speaks* French very well",
      "french": "Elle *parle* très bien le français",
      "focus_word_en": "speaks",
      "focus_word_fr": "parle",
      "topic": null,
      "level": null
    },
    {
      "id": 3,
      "english": "We *go* to the market every Saturday",
      "french": "Nous *allons* au marché chaque samedi",
      "focus_word_en": "go",
      "focus_word_fr": "allons",
      "topic": null,
      "level": null
    }
  ]
}
```

### Field Descriptions
- **version**: File version number (increments with each call)
- **created_at**: ISO 8601 timestamp of file creation
- **count**: Total sentences in this file
- **sentences**: Array of sentence pairs
  - **id**: Sequential ID within this file
  - **english**: Original English sentence (with `*focus*` markers preserved)
  - **french**: French translation (with `*focus*` markers on corresponding words)
  - **focus_word_en**: Extracted focus word from English (null if no markers)
  - **focus_word_fr**: Extracted focus word from French (null if no markers)
  - **topic**: Optional field for user to add later (e.g., "food", "travel", "conversation")
  - **level**: Optional field for user to add later (e.g., "simple", "medium", "complex")

---

## Quality Checklist

Before outputting JSON, verify:
- ✓ All sentences are grammatically correct French
- ✓ Translations are natural, idiomatic A1 French
- ✓ Focus markers `*...*` are preserved in both English and French
- ✓ Corresponding words are correctly identified and marked
- ✓ All content is A1-level vocabulary and grammar
- ✓ Sentences fit travel/tourism/hobby contexts

---

## Error Handling

- **Empty input**: "No sentences provided. Please enter one or more English sentences separated by newlines."
- **Ambiguous translation**: If a sentence is unclear, ask for clarification before translating.
- **Multiple focus markers**: Handle multiple `*focus*` sections (e.g., `I *like* to *eat*`); extract as semicolon-separated list.
- **Out-of-scope**: If a sentence uses advanced grammar or vocabulary, gently suggest a simpler A1-equivalent.

---

## Example Invocations & Outputs

### Example 1: Three Sentences, One with Multiple Focus Words
**User**:
```
I *like* to eat
She speaks French
We *go* to the market
```

**Output**:
```
✅ Registered 3 sentences (version 1)
Saved to: data/custom-sentences/custom-sentences-v1.json

Preview:
• I *like* to eat → J'*aime* manger
• She speaks French → Elle parle français
• We *go* to the market → Nous *allons* au marché

Next: Review the file, add topics/levels if desired, or call `register-sentence` again.
```

---

## Integration with Other Skills

- **Sentence Renderer** can be extended to render custom-sentences JSON files
- **Vocab Drill** can reference focus words from registered sentences
- **Grammar Explainer** can use examples from registered sentences

---

## Notes

- The skill creates `data/custom-sentences/` if it doesn't exist
- Each call to `register-sentence` creates a new versioned file (doesn't append to old files)
- You can edit the JSON files manually later to add topics/levels or correct translations
- Focus words help you organize study priorities

---

**Status**: Skill ready for use.
**Language**: French A1
**Instruction language**: English