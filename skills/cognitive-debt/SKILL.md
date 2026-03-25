# /cognitive-debt — Track Your Cognitive Ownership

Measure how much you're actually thinking vs how much AI is doing for you.

## Locale

Detect the user's language from their first message.
- Respond in the **same language the user writes in**.
- Technical terms always appear in English with a native-language translation in parentheses.
  Example (Korean): `ownership score` → `ownership score (내가 직접 생각한 비율)`
  Example (Spanish): `ownership score` → `ownership score (proporción de pensamiento propio)`
- Code snippets, file paths, and variable names stay in English.
- If the user switches language mid-session, follow their switch.

## Philosophy

AI makes you productive, but are you learning? This skill tracks the ratio across your entire CC session:
- **What you decided** — code you wrote with intent, explanations you gave, choices you made
- **What AI decided** — code AI generated, answers AI gave, suggestions you accepted without questioning
- **What you verified** — tests you ran, manual checks, code you reviewed before accepting

The ratio is your **cognitive ownership score**. High = you're learning. Low = you're accumulating debt.

## User-invocable
Run when user types `/cognitive-debt`.

## Arguments
- `/cognitive-debt` — show current session report
- `/cognitive-debt --start` — begin tracking a new session
- `/cognitive-debt --history` — show trend across saved sessions

## Instructions

### When invoked with --start or no args (first time)

Start tracking the current coding session. From this point, silently classify every interaction:

**Classification:**

| Category | What counts | Weight |
|----------|------------|--------|
| `you_decided` | User wrote code, explained a concept, made an architectural choice, debugged independently | 1.0 |
| `you_chose` | User picked from options AI presented, directed the approach | 0.75 |
| `you_verified` | User ran tests, reviewed AI output, caught an error, asked "why?" | 0.5 |
| `ai_assisted` | User asked for help, AI gave hints, user still did the work | 0.25 |
| `ai_decided` | AI wrote code user accepted without modification, AI made choices user didn't question | 0.0 |

**Ownership Score Formula:**
```
score = weighted_sum / total_interactions
```

### When invoked with no args (during active session)

Show the current report:

```
COGNITIVE OWNERSHIP REPORT
══════════════════════════
Session: 2026-03-25 (45 min active)
Project: f451

  You decided:      12  (35%)  ██████████████░░░░░░
  You chose:         8  (23%)  █████████░░░░░░░░░░░
  You verified:      5  (15%)  ██████░░░░░░░░░░░░░░
  AI assisted:       6  (17%)  ███████░░░░░░░░░░░░░
  AI decided:        3  ( 9%)  ████░░░░░░░░░░░░░░░░

  Ownership Score: 71% 🟢

BREAKDOWN BY AREA
─────────────────
  Architecture decisions:  90% 🟢  (you drove the design)
  Feature code:            65% 🟡  (AI wrote most, you reviewed)
  Bug fixes:               80% 🟢  (you debugged, AI helped)
  Tests:                   40% 🔴  (AI generated, you didn't verify)
  Config/boilerplate:      10% ⚪  (expected — let AI handle this)

DEBT ALERTS
───────────
  🔴 Tests: You accepted 3 AI-generated tests without running them.
     → Run them now, or explain what each test verifies.
  🟡 Feature code: 4 blocks accepted without modification.
     → Review the last commit. Can you explain each change?
```

**Thresholds:**
- 🟢 70%+ — Healthy learning. You're driving.
- 🟡 40-69% — Leaning on AI. Try explaining before asking.
- 🔴 <40% — Accumulating debt. Pause and review what you accepted.
- ⚪ Boilerplate/config under 30% is fine — that's what AI is for.

### Debt Alerts

Flag specific patterns that indicate cognitive debt:

1. **Accepted without reading** — AI generated 20+ lines, user said "looks good" instantly
2. **No tests run** — Code was written but never tested
3. **Copy-paste chain** — 3+ AI suggestions accepted in a row without modification
4. **Architecture by AI** — AI chose the approach, user didn't question alternatives
5. **Never asked why** — User accepted explanations without probing deeper

For each alert, suggest a specific action:
- "Review the last 3 commits. Can you explain each one?"
- "Run the tests before continuing."
- "Before accepting, try writing it yourself first."

### When invoked with --history

```bash
ls .gstack/cognitive-debt/*.json 2>/dev/null | sort -r | head -10
```

Show trend:

```
COGNITIVE OWNERSHIP TREND
═════════════════════════
Date        Project     Score   Time    You/AI ratio
──────────  ──────────  ─────   ─────   ────────────
2026-03-25  f451        71% 🟢  45min   25/3
2026-03-24  f451        58% 🟡  1h20m   18/12
2026-03-22  f451        82% 🟢  30min   20/2
2026-03-20  other-proj  45% 🟡  2h      15/18

Trend: → STABLE (avg 64% over 4 sessions)
Insight: Longer sessions tend to drop in ownership.
         Consider breaks every 45 min.
```

### Session Save

Save session data on every report:

```bash
mkdir -p .gstack/cognitive-debt
```

Write to `.gstack/cognitive-debt/{date}-{HHMMSS}.json`:
```json
{
  "date": "2026-03-25T14:30:00Z",
  "project": "f451",
  "duration_minutes": 45,
  "interactions": {
    "you_decided": 12,
    "you_chose": 8,
    "you_verified": 5,
    "ai_assisted": 6,
    "ai_decided": 3
  },
  "ownership_score": 0.71,
  "areas": {
    "architecture": 0.90,
    "feature_code": 0.65,
    "bug_fixes": 0.80,
    "tests": 0.40,
    "config": 0.10
  },
  "alerts": [
    {"type": "no_tests_run", "details": "3 AI-generated tests not executed"},
    {"type": "accepted_without_mod", "count": 4}
  ]
}
```

## Integration with /f451

When `/f451` learning sessions are active, cognitive debt tracking is automatic.
The `/f451` skill reports ownership per learning module.
This skill (`/cognitive-debt`) tracks ownership across ALL coding activities — not just learning.

## Core Rules

- ✅ Track silently — don't interrupt the user's flow with constant reports
- ✅ Be honest — if the user isn't thinking, say so clearly
- ✅ Suggest specific actions — not "think more" but "review this specific commit"
- ✅ Distinguish boilerplate from decisions — config/setup at low ownership is fine
- ✅ Show trends — one session means nothing, patterns matter
- ❌ Don't shame — frame as awareness, not judgment
- ❌ Don't block — alerts are suggestions, not gates
- ❌ Don't count trivial interactions — "yes", "ok", "next" aren't decisions

## Disclaimer

This is an approximate measure based on interaction patterns, not a precise cognitive assessment.
The score reflects conversational dynamics, not actual brain activity.
Use as a self-awareness tool, not a grade.
