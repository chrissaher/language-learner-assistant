# Multi-Language Learning Platform

A personalized language learning platform built with Claude Code. Create custom learning assistants for any language with AI-generated sentence practice, grammar lessons, and vocabulary drills.

## 🌍 Available Assistants

### [French Learning Assistant](https://chrissaher.github.io/language-learner-assistant/french-learning-assistant/) — A1 Level
**Learner Profile:** Slovak native (fluent in English, Spanish, German)
**Focus:** Travel, tourism, and hobby
**Features:** Custom sentence pairs, interactive drills, dark mode support

- 📖 [View French Lessons](https://chrissaher.github.io/language-learner-assistant/french-learning-assistant/)
- 🗂️ [GitHub Folder](./french-learning-assistant/)

---

### [German Learning Assistant](./german-learning-assistant/) — A1 Level
*GitHub Pages coming soon*

---

## 🎯 What This Is

This is a **framework for creating personalized language learning tools**. Each subfolder is a completely self-contained learning assistant with:

- **Sentence Builder** — generates 10 example sentences per word with grammar notes
- **Grammar Explainer** — full lessons with 10+ examples and 12+ exercises
- **Vocabulary Drill** — targeted exercises with 40+ questions per topic
- **Custom Sentences** — your own English-French pairs, rendered as interactive HTML
- **Dark Mode** — automatically follows your OS preference
- **Mobile-Friendly** — all content works on phones and tablets

## 📁 Project Structure

```
language-learner-assistant/          ← You are here
  README.md                          ← This file (project hub)
  CLAUDE.md                          ← Platform guide & architecture
  index.html                         ← Visual hub (planned)
  french-learning-assistant/         ← Personalized A1 French assistant
    CLAUDE.md                        ← Learner profile & skill specs
    README.md                        ← Quickstart guide
    index.html                       ← Master index of lessons
    data/
      custom-sentences-html/         ← Your custom sentence drills
      sentences-html/                ← Generated sentences
      grammar-html/                  ← Generated grammar lessons
      vocabulary-html/               ← Generated vocabulary drills
    .claude/skills/                  ← Skill definitions
  german-learning-assistant/         ← Another A1 German assistant
    [same structure]
```

## 🚀 How to Use

### For Students
1. **Pick a language** — click one of the assistants above
2. **Open the index page** — it links to all your lessons
3. **Click a lesson** — each one is a standalone HTML file
4. **Toggle sentences** — click to reveal French translations
5. **Study offline** — save HTML files, no internet needed

### For Developers / Customization
1. **Clone this repo**
2. **Open any subfolder in Claude Code** (e.g., `french-learning-assistant/`)
3. **Use the skills** to generate more content:
   - `sentence builder: [word]`
   - `grammar: [topic]`
   - `vocab drill: topic [name]`
   - `render custom sentences`
4. **Update CLAUDE.md** if your goals change
5. **Push to GitHub** and enable GitHub Pages for the subfolder

## 📚 Each Assistant Includes

- **CLAUDE.md** — your personalized context and learner profile
- **README.md** — quick-start guide
- **index.html** — master hub with links to all lessons
- **styles.css** — shared stylesheet (light/dark mode)
- **data/** — JSON sources + rendered HTML
- **.claude/skills/** — skill definitions for content generation

## 🛠️ Built With

- **Claude Code** — AI-powered content generation
- **HTML + CSS** — lightweight, portable, offline-friendly
- **JavaScript** — interactive toggles, accordion dropdowns
- **No build tools** — just open index.html in a browser

## 💡 Key Features

✨ **Personalized by default**
- Each assistant is customized to your native language, goals, and learning style

🌙 **Dark mode**
- Automatically follows your OS preference

📱 **Mobile-first design**
- All content works great on phones

🎯 **Focus on production**
- Every generated lesson is styled and ready to study

🔄 **Interactive learning**
- Toggle buttons, collapsible sections, vocabulary tables

📖 **Transfer notes**
- Parallels and false friends between your native language and target language

## 🔗 Links

- **Platform guide:** [CLAUDE.md](./CLAUDE.md)
- **French A1 assistant:** [french-learning-assistant/](./french-learning-assistant/)
- **German A1 assistant:** [german-learning-assistant/](./german-learning-assistant/)

## 📝 License

Personal learning project. Feel free to fork and adapt for your own languages and learning goals.

---

**Platform Status:** Active
**Last Updated:** 2026-03-17
**Next Step:** Pick a language assistant above and start learning!