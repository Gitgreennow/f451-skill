# /commit-check — 30-Second Friction Before Every Commit

You wrote code. AI wrote code. Before it becomes permanent — can you explain what changed?

## Locale

Detect the user's language from their first message.
- Respond in the **same language the user writes in**.
- Technical terms always appear in English with a native-language translation in parentheses.
  Example (Korean): `commit` → `commit (코드 변경사항을 저장하는 것)`
  Example (Spanish): `commit` → `commit (guardar los cambios de código)`
- Code snippets, file paths, and variable names stay in English.
- If the user switches language mid-session, follow their switch.

## Philosophy

The most dangerous moment for cognitive debt is **commit time**.
That's when "I'll understand it later" becomes "I never understood it."

This skill adds **30 seconds of friction** — one question about what you're committing.
Skip is always available. But debt is tracked.

**Not a code review.** Not a test. Just: "Do you know what you're about to save?"

## User-invocable
Run when user types `/commit-check`.

Can also be wired as a pre-commit hook (see Setup section).

## Arguments
- `/commit-check` — check staged changes right now
- `/commit-check --auto` — register as pre-commit hook (asks on every commit)
- `/commit-check --off` — remove pre-commit hook
- `/commit-check --stats` — show skip/explain ratio history

## Instructions

### Step 0: Read the Diff

```bash
git diff --cached --stat
git diff --cached
```

Read the staged changes. Understand:
- Which files changed
- How many lines added/removed
- What the changes DO (not just what they ARE)

Do NOT show the full diff to the user. Summarize internally.

---

### Step 1: Summarize What Changed (5 seconds)

Show a quick visual summary:

```
📝 COMMIT CHECK
═══════════════
You're about to commit:

  Modified:  3 files
  Added:     47 lines
  Removed:   12 lines

  ┌─────────────────────────────────────┐
  │ app/features/auth/api/login.tsx     │  +23 lines  (new validation logic)
  │ app/core/lib/guards.server.ts       │  +15 lines  (added role check)
  │ app/routes.ts                       │   +9 lines  (new route)
  └─────────────────────────────────────┘
```

---

### Step 2: Ask ONE Question (20 seconds)

Pick the most important question based on what changed. Only ONE.

**Question types — pick the best fit:**

#### For new code (additions):
```
🚧 Quick check before commit:

   You added a role check in guards.server.ts.
   What happens if a user WITHOUT the right role hits this endpoint?

   A) They get a 403 Forbidden error
   B) They see the page but can't edit
   C) The app crashes
   D) Skip → commit anyway (debt +1)
```

#### For bug fixes (modifications):
```
🚧 Quick check before commit:

   You changed the validation in login.tsx.
   What was the bug you fixed — in one sentence?

   → Your answer: _______________
   → or D) Skip → commit anyway (debt +1)
```

#### For refactors (same behavior, different structure):
```
🚧 Quick check before commit:

   You moved code from login.tsx to guards.server.ts.
   Why is the new location better?

   A) It's reusable — other routes can use the same guard
   B) It's faster — server-side is quicker
   C) No real reason, just cleaner
   D) Skip → commit anyway (debt +1)
```

#### For AI-generated code (large additions the user didn't write):
```
🚧 Quick check before commit:

   48 new lines in auth/api/login.tsx — looks AI-generated.
   Pick the ONE line you understand best and explain what it does:

   Line 12:  const hashedPassword = await bcrypt.hash(password, 12);
   Line 23:  if (!user) throw new Response("Not found", { status: 404 });
   Line 31:  const session = await createUserSession(user.id);

   Which line, and what does it do?
   → Your answer: _______________
   → or D) Skip → commit anyway (debt +1)
```

#### For config/boilerplate (low cognitive value):
```
🚧 Quick check before commit:

   Looks like config/boilerplate changes (routes, imports, types).
   Auto-pass — no explanation needed for this one. ✅

   Ownership: not counted (boilerplate is fine to delegate)
```

**Rules:**
- ONLY ONE question per commit
- Always include Skip option (D)
- Skip is NEVER blocked — the commit always goes through
- Config/boilerplate gets auto-pass (don't waste their time)
- AI-generated code gets the "pick ONE line" format (lowest barrier)

---

### Step 3: Evaluate & Record (5 seconds)

**If they answered:**

Correct answer:
```
✅ Nice — you know what you're committing. Ownership +1.

Committing...
```

Wrong answer:
```
❌ Close! Actually: [one sentence correction].
   The key thing: [insight].

   Committing anyway — but consider reviewing this file later.
   Ownership: recorded as "partial" (0.5).
```

Free-text answer (bug fix, explanation):
- If it shows understanding → ✅ Ownership +1
- If it's vague ("I fixed the bug") → push once: "Which bug? One sentence."
- If still vague after push → record as partial (0.5), commit anyway

**If they skipped:**
```
⏭️ Skipped. Debt +1 recorded.

   This session: 3 explained, 1 skipped
   Ownership: 75% 🟢

Committing...
```

---

### Step 4: Update Debt Ledger

```bash
mkdir -p .gstack/cognitive-debt
```

Append to `.gstack/cognitive-debt/commit-log.jsonl`:
```json
{"date":"2026-03-25T14:30:00Z","commit":"abc1234","files":3,"lines_added":47,"question_type":"new_code","result":"explained","ownership":1.0}
```

Result values:
- `"explained"` — correct answer (1.0)
- `"partial"` — wrong but tried (0.5)
- `"skipped"` — hit D (0.0)
- `"auto_pass"` — boilerplate, no question asked (null — not counted)

---

### --stats: Show History

```
COMMIT CHECK HISTORY
════════════════════
Last 7 days:

  Date        Commits   Explained   Skipped   Ownership
  ──────────  ────────  ─────────   ───────   ─────────
  2026-03-25  5         4           1         80% 🟢
  2026-03-24  8         5           3         63% 🟡
  2026-03-23  3         3           0         100% 🟢
  2026-03-22  6         2           4         33% 🔴

  Weekly avg: 69% 🟡
  Trend: → STABLE

  🟡 Tip: You skipped 4 commits on March 22.
     Run /explain on those files to catch up.
```

---

### --auto: Pre-Commit Hook Setup

```bash
# Create pre-commit hook that reminds to run /commit-check
cat > .git/hooks/pre-commit << 'HOOKEOF'
#!/bin/sh
echo ""
echo "🚧 Run /commit-check before committing!"
echo "   (or commit with --no-verify to skip)"
echo ""
HOOKEOF
chmod +x .git/hooks/pre-commit
echo "✅ Pre-commit reminder installed. Run /commit-check --off to remove."
```

**Note:** This is a REMINDER hook, not a blocking hook. It prints a message
but doesn't prevent the commit. The actual /commit-check runs in CC.

### --off: Remove Hook

```bash
rm -f .git/hooks/pre-commit
echo "✅ Pre-commit reminder removed."
```

---

## Smart Question Selection

Priority order for picking which question to ask:

1. **AI-generated code** (large additions) → "Pick ONE line and explain it"
2. **Security-related changes** (auth, guards, RLS) → "What happens if this fails?"
3. **Bug fixes** → "What was the bug?"
4. **New features** → "What does this add for the user?"
5. **Refactors** → "Why is the new way better?"
6. **Config/boilerplate** → Auto-pass

**Detection heuristics:**
- AI-generated: 30+ new lines in a single file, no corresponding test changes
- Security: file path contains `auth`, `guard`, `security`, `rls`, `policy`, `permission`
- Bug fix: commit message contains `fix`, `bug`, `patch`, `hotfix`
- Config: file is `*.config.*`, `*.json`, `routes.ts`, `schema.ts` (structure only, no logic)

---

## Visual Feedback

After answering, show a mini progress bar for the current session:

```
Session ownership: ████████░░ 80% (4/5 explained)
```

If ownership drops below 40% in a session:
```
⚠️ You've skipped 4 out of 6 commits this session.
   You're building something you can't explain.
   Consider running /explain on the files you skipped.
```

---

## Core Rules

### Always
- ✅ ONE question per commit — respect their time
- ✅ Skip is always available — never block a commit
- ✅ Auto-pass boilerplate — don't waste friction on config
- ✅ Track everything — even skips are data
- ✅ Show session progress — make ownership visible
- ✅ Translate jargon — same rules as /explain

### Never
- ❌ Don't block the commit — friction, not a gate
- ❌ Don't ask more than one question — 30 seconds max
- ❌ Don't shame for skipping — record it, don't judge
- ❌ Don't ask about imports/types/formatting — zero learning value
- ❌ Don't turn this into a code review — /review exists for that

## Session Flow

```
/commit-check
    │
    ├─ Step 0: Read staged diff (internal)
    ├─ Step 1: Visual summary (5s)
    ├─ Step 2: ONE question + skip option (20s)
    ├─ Step 3: Evaluate + feedback (5s)
    └─ Step 4: Update debt ledger

Total: ~30 seconds
```

## Disclaimer

This skill tracks self-reported understanding, not actual code quality.
A correct answer doesn't guarantee correct code. Use /review for code review
and /qa for testing. This is a learning tool, not a quality gate.
