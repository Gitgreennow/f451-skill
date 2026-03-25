# /explain — Understand Any Code in 60 Seconds

Point at any file or function. Get the "why" before the "how."
No jargon walls. No 50-line explanations. Just: problem → picture → code → your turn.

## Locale

Detect the user's language from their first message.
- Respond in the **same language the user writes in**.
- Technical terms always appear in English with a native-language translation in parentheses.
  Example (Korean): `loader` → `loader (페이지 열 때 서버에서 실행되는 함수)`
  Example (Spanish): `loader` → `loader (función que se ejecuta en el servidor al abrir la página)`
- Code snippets, file paths, and variable names stay in English.
- Diagram labels: bilingual — English term + native translation.
- If the user switches language mid-session, follow their switch.

## Philosophy

Vibe coders don't need a lecture. They need:
1. **Why does this exist?** (the problem it solves)
2. **What does it look like?** (a visual mental model)
3. **How does the code do it?** (3-5 key lines, translated)
4. **Can I explain it back?** (ownership check)

60 seconds. If it takes longer, the explanation is bad, not the learner.

## User-invocable
Run when user types `/explain`.

## Arguments
- `/explain <file-path>` — explain a specific file
- `/explain <file-path>:<function-name>` — explain a specific function
- `/explain <concept>` — explain a concept found in the codebase (e.g., "RAG", "RLS", "loader")
- `/explain --visual` — extra visual aids (ASCII diagrams, flow charts)
- `/explain --deep` — follow-up deep dive after the 60-second version

## Instructions

### Step 0: Read the Target

Read the file/function the user pointed at. Also read:
- Imports (what does this depend on?)
- Callers (what calls this? Use Grep to find references)
- Schema/types (what data shape does it work with?)

Build an internal understanding. Do NOT dump this on the learner.

---

### Step 1: The Problem (10 seconds)

**Start with what breaks if this code didn't exist.**

Format:
```
❓ WITHOUT THIS CODE:
   [One sentence describing what would fail or be impossible]

   Real example:
   "[Concrete scenario a user would experience]"
```

Example:
```
❓ WITHOUT THIS CODE:
   Users couldn't ask questions about their uploaded documents.

   Real example:
   "You upload a 200-page PDF about machine learning.
    You want to ask 'What is backpropagation?'
    Without this file — no answer. Just a silent PDF."
```

**Rules:**
- One sentence max for the problem
- The "real example" must be a user-facing scenario, not a technical one
- No code yet. No jargon yet.

---

### Step 2: The Picture (15 seconds)

**Show a visual mental model.** Vibe coders think in pictures, not abstractions.

Pick the best format for this code:

#### Option A: Flow diagram (for data pipelines, request handlers)
```
📊 WHAT HAPPENS:

   User question
       │
       ▼
   ┌─────────┐    "Turn the question
   │ Embed   │     into numbers"
   └────┬────┘     (vector embedding)
        │
        ▼
   ┌─────────┐    "Find the most
   │ Search  │     similar chunks"
   └────┬────┘     (cosine similarity)
        │
        ▼
   ┌─────────┐    "Ask AI with
   │ Generate│     those chunks as context"
   └────┬────┘     (RAG = Retrieval-Augmented Generation)
        │
        ▼
   AI answer with sources
```

#### Option B: Before/After (for transformations, formatters)
```
📊 WHAT IT DOES:

   BEFORE                          AFTER
   ┌─────────────────┐            ┌──────────────────────┐
   │ Raw PDF file     │    ──▶    │ Title: "ML Basics"   │
   │ (binary blob)    │           │ Pages: 47            │
   │                  │           │ TOC: [Ch1, Ch2, Ch3] │
   │                  │           │ Author: "Kim et al." │
   └─────────────────┘            └──────────────────────┘

   Plain English:
   "Takes a messy file → extracts the useful metadata"
```

#### Option C: Layer cake (for middleware, auth, guards)
```
📊 WHERE IT SITS:

   Browser request
       │
       ▼
   ┌─────────────────┐
   │  Route handler   │  ← "Which URL was requested?"
   ├─────────────────┤
   │  Auth guard  ◀───┤  ← "Is this user logged in?" ⭐ THIS FILE
   ├─────────────────┤
   │  Access check    │  ← "Can they see this collection?"
   ├─────────────────┤
   │  Business logic  │  ← "Do the actual work"
   └─────────────────┘
```

#### Option D: Actor diagram (for multi-component interactions)
```
📊 WHO TALKS TO WHOM:

   ┌────────┐         ┌──────────┐         ┌────────┐
   │ Client │ ──req──▶│  Server  │ ──sql──▶│   DB   │
   │(React) │◀──sse── │(Action)  │◀──rows──│(Supa)  │
   └────────┘         └──────────┘         └────────┘
                           │
                           │ ──prompt──▶ ┌────────┐
                           │ ◀──stream── │  AI    │
                                         │(Gemini)│
                                         └────────┘
```

**Rules:**
- Always include plain-English labels on every box/arrow
- Technical terms get a parenthetical translation: `(vector embedding)` → `"Turn text into numbers"`
- Mark the target file/function with ⭐ THIS FILE
- Max 15 lines for the diagram

---

### Step 3: The Code (20 seconds)

**Show the 3-5 most important lines.** Side-by-side with plain English.

Format:
```
📄 [file path] (lines X-Y)

┌──────────────────────────────────┬──────────────────────────────┐
│ Code                             │ Plain English                │
├──────────────────────────────────┼──────────────────────────────┤
│ const embedding = await model    │ Take the user's question     │
│   .embedContent(query);          │ and turn it into numbers     │
│                                  │ (a "vector")                 │
├──────────────────────────────────┼──────────────────────────────┤
│ const results = await db         │ Find the 5 closest matches   │
│   .select()                      │ in the database              │
│   .orderBy(cosineDistance)       │ (closest = most relevant)    │
│   .limit(5);                     │                              │
├──────────────────────────────────┼──────────────────────────────┤
│ const answer = await llm         │ Give those 5 chunks to AI    │
│   .generate({ context: results,  │ and ask it to answer         │
│     question: query });          │ using ONLY that context       │
└──────────────────────────────────┴──────────────────────────────┘
```

**Rules:**
- Read the ACTUAL file. Show REAL code. Never fabricate.
- Max 5 code blocks in the table
- Every code block gets a plain-English translation
- Technical terms get inline translation on first use:
  - `cosineDistance` → `(closest = most relevant)`
  - `embedding` → `(a "vector" — text turned into numbers)`
  - `RLS` → `(Row-Level Security — database rules for who sees what)`
  - `loader` → `(runs on the server when you visit a page)`
  - `action` → `(runs on the server when you submit a form)`
- If a line is just boilerplate (imports, types), skip it

---

### Step 4: Your Turn (15 seconds)

**One question. Low barrier. Choice-first.**

Pick the best format:

#### For "what does it do" understanding:
```
🎓 QUICK CHECK:

   If a user asks "What is backpropagation?" and you have 200 documents,
   what does this code do FIRST?

   A) Send all 200 docs to AI
   B) Turn the question into numbers and find similar chunks
   C) Search by keyword like Ctrl+F
```

#### For "why this way" understanding:
```
🎓 QUICK CHECK:

   Why doesn't this code just send ALL the documents to AI?

   A) AI has a token limit — can't read everything
   B) It would be too slow
   C) Both — plus it would cost a fortune
```

#### For "where does it fit" understanding:
```
🎓 QUICK CHECK:

   If you removed this auth guard, what would happen?

   A) The page would break
   B) Anyone could see any collection, even private ones
   C) Nothing — it's just extra safety
```

**After they answer:**
- Correct → "Exactly. [One sentence reinforcing WHY]"
- Wrong → "Close! The answer is [X] because [one sentence]. The key thing: [insight]."

**Then offer next steps:**
```
Want to go deeper?
  A) Show me what calls this function (trace upstream)
  B) Show me what this function calls (trace downstream)
  C) I'm good — got it 👍
```

---

### --deep mode: Follow-up Deep Dive

If the user picks A or B, or uses `--deep`:

1. **Trace upstream or downstream** — show the call chain as a visual:
```
📊 CALL CHAIN (upstream — who triggers this?):

   User clicks "Send" in chat
       │
       ▼
   chat-panel.tsx → handleSubmit()
       │
       ▼
   fetcher.submit() → POST /api/chat-stream
       │
       ▼
   chat-stream.tsx → action()     ⭐ THIS FILE
       │
       ▼
   generate.node.ts → generate()
```

2. **Show the connecting code** at each step (same side-by-side format)
3. **Ask a Bloom Lv.3+ question:**
```
🎓 DEEPER CHECK:

   You want to add a "summarize this collection" feature.
   Looking at this call chain, which files would you need to change?

   Your answer → ___
```

---

### --visual mode: Extra Visual Aids

If the user uses `--visual`, add one or more of:

#### Analogy box
```
💡 ANALOGY:

   Vector search is like a librarian who READ every book.

   You: "I need something about neural networks"
   Librarian: "Ah, pages 42-47 of THIS book are closest
               to what you're asking about."

   The "reading" happened beforehand (embedding).
   The "finding" happens instantly (similarity search).
```

#### Size/scale context
```
📏 SCALE:

   This function handles:
   ├─ ~500 requests/day in production
   ├─ Each request searches ~10,000 vector chunks
   ├─ Response time: ~2 seconds
   └─ Cost: ~$0.01 per query (embedding + AI)
```

#### Diff view (for understanding changes)
```
📝 WHAT CHANGED:

   BEFORE (without this code):
   ┌─────────────────────────────┐
   │ User asks question          │
   │ → AI answers from memory    │
   │ → Often wrong/hallucinated  │
   └─────────────────────────────┘

   AFTER (with this code):
   ┌─────────────────────────────┐
   │ User asks question          │
   │ → Find relevant chunks      │
   │ → AI answers FROM chunks    │
   │ → Accurate + has sources    │
   └─────────────────────────────┘
```

---

## Jargon Translation Guide

When you encounter these terms, ALWAYS provide inline translation on first use:

| Term | Translation |
|------|------------|
| `loader` | (runs on the server when you visit this page) |
| `action` | (runs on the server when you submit a form) |
| `middleware` | (code that runs BEFORE your main code — like a security checkpoint) |
| `RLS` | (Row-Level Security — the database decides who can see what) |
| `embedding` / `vector` | (text turned into numbers so computers can compare meaning) |
| `cosine similarity` | (measuring how "close" two pieces of text are in meaning) |
| `RAG` | (Retrieval-Augmented Generation — find relevant info first, then ask AI) |
| `SSE` / `streaming` | (sending the answer word-by-word instead of all at once) |
| `ORM` | (talks to the database using code instead of raw SQL) |
| `schema` | (the blueprint — what columns a database table has) |
| `migration` | (a script that changes the database structure) |
| `hook` (React) | (a function that gives your component superpowers — state, effects, etc.) |
| `fetcher` | (React Router's way of sending data to the server without reloading) |
| `guard` | (a checkpoint that blocks you if you're not authorized) |
| `seed` | (fake data to test with — like filling a store with mannequins before opening) |

---

## Core Rules

### Always
- ✅ Problem first — "what breaks without this?" before any code
- ✅ Visual before text — diagram/picture before code block
- ✅ Real code only — read the actual file, cite line numbers
- ✅ Translate on first use — every technical term gets a parenthetical
- ✅ End with a question — ownership check, choice-first (A/B/C)
- ✅ 60 seconds max for the basic explanation — respect their time
- ✅ Mark the target with ⭐ in diagrams

### Never
- ❌ Don't dump the full file — max 5 code blocks
- ❌ Don't explain imports — skip boilerplate
- ❌ Don't use jargon without translation — EVER
- ❌ Don't give 10-line explanations — 1-2 sentences per concept
- ❌ Don't skip the visual — every explanation needs a picture

## Session Flow

```
/explain <target>
    │
    ├─ Step 0: Read target + context (internal)
    ├─ Step 1: "Without this code..." (10s)
    ├─ Step 2: Visual diagram (15s)
    ├─ Step 3: Core code side-by-side (20s)
    ├─ Step 4: Quick check question (15s)
    │
    └─ Want more?
         ├─ A) Trace upstream → --deep
         ├─ B) Trace downstream → --deep
         └─ C) Got it 👍
```

## Disclaimer

This skill provides AI-generated code explanations for learning purposes.
Explanations may oversimplify complex systems. When accuracy matters,
verify with source code, tests, and documentation.
