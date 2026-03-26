# F451 Skills — Fight Cognitive Debt While Coding

> AI makes you productive. But are you learning?

Claude Code skills that embed learning friction into your coding workflow.
Built on [F451](https://f451.tech) — the AI-era learning infrastructure platform.

## Why This Exists

AI writes your code. You accept it. Ship it. It works. But when it breaks — you have no idea why.

This is **cognitive debt**: the gap between what you build and what you understand.

- Shen and Tamkin (2026): **17% skill degradation** in developers using AI without cognitive engagement
- Kosmyna et al.: **weakened brain connectivity** (EEG) when using LLMs passively

F451 Skills fight this by embedding learning into your existing coding workflow — not a separate app, not a course, not a study session.
## Quick Start

```bash
# Install all skills
git clone https://github.com/Gitgreennow/f451-skill.git /tmp/f451-skills
cp -r /tmp/f451-skills/skills/* ~/.claude/skills/
```

Then type `/f451`, `/explain`, `/commit-check`, or `/cognitive-debt` in Claude Code.

## The Skill Collection

```
LEARN (invoke when you want to learn)
  /f451           Learn any codebase
  /explain        Understand any file (60s)

FRICTION (at decision points)
  /commit-check   Before every commit (30s)
  /plan-check     Before approving a plan (coming soon)

TRACK (visibility)
  /cognitive-debt  Ownership score and trends
```

### /f451 — Learn Any Codebase by Explaining It

Full codebase learning with the Feynman Technique. You explain, AI corrects, you explain again.

| Command | What it does |
|---------|-------------|
| `/f451` | Learn the whole codebase |
| `/f451 --module auth` | Focus on one module |
| `/f451 --depth shallow` | Architecture overview (5 min) |
| `/f451 --depth deep` | Code-level deep dive (Bloom 1-6) |
| `/f451 --resume` | Continue where you left off |

3-stage flow: **Overview** (what does it do?) → **Essence** (why does this code exist?) → **Detail** (trace the data flow yourself).

Socratic hint ladder when stuck: choices → fill-in → reveal + explain back.
### /explain — Understand Any Code in 60 Seconds

Point at any file or function. Get: problem → picture → code → quiz.

| Command | What it does |
|---------|-------------|
| `/explain <file>` | Explain a file |
| `/explain <file>:<function>` | Explain a function |
| `/explain "RAG"` | Explain a concept |
| `/explain --deep` | Follow-up deep dive |
| `/explain --visual` | Extra ASCII diagrams |

Every explanation: **what breaks without it** → **visual diagram** → **3-5 lines of code translated** → **one quiz question**.

### /commit-check — 30-Second Friction Before Every Commit

One question about your diff. Skip always available. Debt tracked.

| Command | What it does |
|---------|-------------|
| `/commit-check` | Check staged changes now |
| `/commit-check --auto` | Install pre-commit reminder |
| `/commit-check --stats` | Show skip/explain history |
| `/commit-check --off` | Remove pre-commit hook |

Smart question selection: AI-generated code gets "pick ONE line and explain it." Security changes get "what happens if this fails?" Config/boilerplate gets auto-pass.

### /cognitive-debt — Track Your Cognitive Ownership

See how much you actually thought vs delegated to AI.

| Command | What it does |
|---------|-------------|
| `/cognitive-debt` | Current session report |
| `/cognitive-debt --start` | Begin tracking |
| `/cognitive-debt --history` | Trend over time |

Ownership thresholds: 🟢 70%+ healthy | 🟡 40-69% leaning on AI | 🔴 <40% accumulating debt.

### /plan-check — Friction at Design Approval *(coming soon)*

Before approving any plan: "What is the key decision? Why this approach?"

## How It Fits Together

```
You start coding
  |
  +-- Want to understand the codebase? --> /f451
  +-- Confused by one file? ------------> /explain
  +-- Ready to commit? -----------------> /commit-check (30s)
  +-- Approving a design? --------------> /plan-check (1min)
  +-- End of session --------------------> /cognitive-debt
```

## Locale

All skills auto-detect your language and respond in it. Technical terms stay in English with a native translation to prevent mistranslation:

```
embedding (text turned into numbers for comparison)
loader (function that runs on the server when you visit a page)
RLS (Row-Level Security — database rules for who sees what)
```

## Design Principles

1. **Friction, not a gate** — skip is always available, but debt is tracked
2. **Explain it or you do not know it** — Feynman technique at every stage
3. **Track ownership, not productivity** — Bloom levels + cognitive debt score
4. **60 seconds or less** — respect developer time
5. **AI assists, you think** — conceptual inquiry over code delegation

## Contributing

PRs welcome. Each skill is a single `SKILL.md` file in `skills/<name>/`.

## Inspired By

- [codebase-to-course](https://github.com/zarazhangrui/codebase-to-course) — Visual codebase course generation
- [F451](https://f451.tech) — AI-era learning infrastructure platform

## License

MIT
