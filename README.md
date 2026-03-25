# /f451 — Learn Any Codebase by Explaining It

> "If you can't explain it simply, you don't understand it." — Richard Feynman

A Claude Code skill that teaches you any codebase through the **Feynman Technique** — you explain, AI corrects, you explain again. No passive reading. No cognitive debt.

## The Problem

AI gives you answers, but you don't actually learn. You ask ChatGPT how some code works, get a perfect explanation, nod along, and forget it in 10 minutes. Your ability to think independently erodes — that's **cognitive debt**.

## The Solution

`/f451` flips the script: instead of explaining code TO you, it makes you explain it BACK.

**3-Stage Learning Flow:**

```
Stage 1: "What is this?"     → Overview, no code (Bloom 1-2)
Stage 2: "Why this way?"     → Problem first, then code (Bloom 2-4)
Stage 3: "Can I do it?"      → Deep dive + challenges (Bloom 4-6)
```

## What Makes This Different

| Traditional | /f451 |
|---|---|
| AI explains code to you | **You** explain code to AI |
| Read and forget | Feynman test: explain or retry |
| No progress tracking | Bloom 1→6 + cognitive ownership score |
| Static output | Interactive, problem-first Socratic dialogue |
| One-time use | Session resume + exportable context assets |

## Install

```bash
# Global (recommended)
git clone https://github.com/Gitgreennow/f451-skill.git ~/.claude/skills/f451

# Per-project (vendored)
git clone https://github.com/Gitgreennow/f451-skill.git .claude/skills/f451
rm -rf .claude/skills/f451/.git
```

## Usage

Open any project in Claude Code and type:

```
/f451                    # Full codebase learning
/f451 --module auth      # Focus on one module
/f451 --depth shallow    # Architecture only (5 min)
/f451 --depth deep       # Code-level deep dive
/f451 --resume           # Continue where you left off
/f451 --publish          # Export shareable learning materials
```

## How It Works

### Stage 1: Overview (no code, 5 min)
```
"This project solves [X] by [Y]."

┌──────────┐   ┌──────────┐   ┌──────────┐
│  INPUT   │──▶│ PROCESS  │──▶│  OUTPUT  │
└──────────┘   └──────────┘   └──────────┘

🎓 "Explain this project to a friend. No code words."
```

### Stage 2: Essence (problem → code)
```
❓ Without vector search, how do you find info in 100 documents?
   A) Read all of them    B) Ctrl+F    C) Something smarter?

📄 Here's how the code solves it:
┌─────────────────────────────┬──────────────────────┐
│ Code                        │ What it does         │
├─────────────────────────────┼──────────────────────┤
│ model.embedContent(text)    │ Turn text → numbers  │
│ db.insert(vectors)          │ Save in database     │
└─────────────────────────────┴──────────────────────┘

🎓 "Explain WHY this layer exists, not WHAT it does."
```

### Stage 3: Detail (trace + build)
```
🔍 Follow the chain:
  handleSubmit() → fetcher.submit() → ??? → generate.node.ts

🛠 Challenge: Add a new API endpoint. Try first, hints if stuck.

🎓 "Explain this module to a junior dev. 5 sentences."
```

### Cognitive Ownership Report
```
COGNITIVE OWNERSHIP REPORT
══════════════════════════
Interactions:          12
├─ Explained by you:    7  (58%)  ← Your thinking
├─ Chose correctly:     3  (25%)  ← Guided choice
├─ Needed hints:        1  ( 8%)  ← AI-assisted
└─ Was told answer:     1  ( 8%)  ← Cognitive debt

Ownership Score: 83% 🟢
```

Thresholds: 🟢 70%+ healthy | 🟡 40-69% leaning on AI | 🔴 <40% accumulating debt

### 3-Step Hint Ladder

When you're stuck:

1. **Choices** — "A) Server or B) Local?" (low barrier)
2. **Fill-in + hint** — "The function starts with `handle___`"
3. **Reveal + Feynman** — "It's [X]. Now explain WHY."

Even at step 3, you still have to explain it back.

## Context Assets (--publish)

After learning, export reusable materials:

- `architecture.md` — Architecture guide with learning path
- `concepts.json` — Concept map with relationships
- `learning-path.md` — Module order + key questions
- `feynman-prompts.md` — Feynman check prompts per concept
- `quiz-bank.json` — Bloom 1-6 quiz bank with answers

Upload these to [F451](https://f451.io) as context assets for others to learn from.

## Philosophy

Built on [F451](https://f451.io)'s learning infrastructure principles:

- **Problem first, code second** — Understand WHY before HOW
- **Cognitive engagement > convenience** — You do the thinking, AI guides
- **Track what you truly know** — Bloom levels + cognitive ownership
- **Your learning creates value** — Context assets are shareable

### Academic Foundation
- Shen & Tamkin (2026) — AI use causes 17% skill degradation without cognitive engagement
- Kosmyna et al. — EEG shows weakened brain connectivity with LLM dependence

## Inspired By

- [codebase-to-course](https://github.com/zarazhangrui/codebase-to-course) — Visual overview approach
- [F451](https://f451.io) — AI-era learning infrastructure platform

## Works With

Any codebase: TypeScript, Python, Go, Rust, Ruby, Java, PHP, .NET — auto-detected.

## License

MIT
