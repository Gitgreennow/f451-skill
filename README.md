# /f451 — Learn Any Codebase by Explaining It

A Claude Code skill that teaches you codebases through **active recall**, not passive reading.

> "If you can't explain it simply, you don't understand it well enough." — Richard Feynman

## The Problem

AI reads code for you → you think you understand → you don't → **cognitive debt** accumulates.

## The Solution

Instead of explaining code TO you, this skill makes you explain code BACK:

```
Explore code → Trace data flow → Explain it (Feynman) → Quiz (Bloom 1-6) → Repeat
```

| Traditional | /f451 |
|---|---|
| AI explains code to you | **You** explain code to AI |
| Read and forget | Feynman test: explain or retry |
| No progress tracking | Bloom's Taxonomy levels 1→6 |
| Static output | Interactive Socratic dialogue |
| One-time use | Session resume + context assets |

## Install

```bash
# Global install
mkdir -p ~/.claude/skills && cd ~/.claude/skills
git clone https://github.com/Gitgreennow/f451.git

# Or per-project
mkdir -p .claude/skills && cd .claude/skills
git clone https://github.com/Gitgreennow/f451.git
```

## Usage

```
/f451                      # Full learning (recommended)
/f451 --module auth        # Specific module only
/f451 --depth shallow      # Architecture overview (Bloom 1-2)
/f451 --depth deep         # Code-level deep dive (Bloom 1-6)
/f451 --resume             # Continue previous session
/f451 --publish            # Export context assets
```

## How It Works

### 5 Phases

1. **Recon** — AI builds internal mental model (you don't see this)
2. **Curriculum** — 4-8 modules following your user journey
3. **Interleaved Learning** — Mix of Explore / Trace / Feynman / Quiz / Challenge
4. **Bloom Tracking** — Visual progress with levels and freshness
5. **Context Assets** — Export for others to learn from

### Learning Blocks

- 📂 **Explore** — "Open this file. What do you think it does?"
- 🔍 **Trace** — "Follow the data from A → B → C"
- 🎓 **Feynman** — "Explain this back to me. No jargon."
- 📝 **Quiz** — Bloom level 1→6 questions
- 🛠 **Challenge** — Actually modify the code (optional)

### Socratic Hint Ladder

When stuck, you get graduated hints — never the answer immediately:

1. Direction → 2. Narrow scope → 3. Multiple choice → 4. Partial reveal → 5. Full reveal + "now explain WHY"

### Session Progress

```
LEARNING PROGRESS
═════════════════
Module                  Bloom   Status      Freshness
──────────────────────  ────    ────        ──────
1. Project Structure     Lv.3   █████░░░    🟢 Just now
2. Data Flow             Lv.2   ████░░░░    🟡 Yesterday
3. Auth System           —      ░░░░░░░░    ⬜ Not started

Progress: 28% | Avg Bloom: 2.5 | Feynman Pass: 67%
```

## Context Assets (--publish)

Export reusable learning materials:

- `context-assets/architecture.md` — Architecture guide
- `context-assets/concepts.json` — Concept map with relationships
- `context-assets/learning-path.md` — Module order + questions
- `context-assets/feynman-prompts.md` — Feynman prompts per concept
- `context-assets/quiz-bank.json` — Bloom 1-6 quiz bank

Upload to [F451](https://f451.dev) as context assets for others.

## Philosophy

- **Cognitive engagement over delegation** — You think, AI guides
- **Feynman technique** — Explain it = know it
- **Bloom's Taxonomy** — Track depth, not exposure
- **Interleaved practice** — Mix block types for retention
- **Context assets** — Learning creates value for others

## Works With

Any codebase: TypeScript, Python, Go, Rust, Ruby, Java, PHP, .NET — auto-detected.

## License

MIT
