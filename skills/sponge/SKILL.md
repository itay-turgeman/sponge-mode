---
name: sponge
description: |
  Sponge Mode is a structured multi-phase intake and synthesis workflow for when the user wants to dump a large, messy, heterogeneous pile of information — images, CSVs, PDFs, screenshots, notes, specs, links, voice transcripts, code snippets, or any mix — and then interactively query, mold, synthesize, and extract structured outputs from it. 

  Trigger this skill ANY time you see:
  - User says "sponge mode", "absorb this", "here's a brain dump", "take all of this in", "I want to dump a bunch of stuff"
  - User uploads multiple files of mixed types and says something like "let's work with all of this"
  - User pastes a wall of raw, unorganized info and says "I want to make sense of this" or "help me organize/query/understand this"
  - User says "I'm going to upload a bunch of things, then I'll ask you about it"
  - Any signal of "I have a lot of raw material and I want to work with it" regardless of phrasing
---

# Sponge Mode

Two phases: **Absorb first, mold later.**

Argument: $ARGUMENTS

---

## Phase 1: ABSORB

**Entry:** Announce sponge mode is active. Keep it loose and high-energy — the user is in collection mode.

> "Sponge Mode active. Send everything — images, CSVs, notes, PDFs, screenshots, voice dumps, links, code, specs, whatever. I'll absorb, catalog, and hold it all. When you're ready, say 'done' or start asking questions."

**Accept everything without complaint:**
- Images, CSVs, PDFs, plain text, code, voice transcripts, URLs, JSON/YAML, mixed in one message — all fine

**For each piece, silently micro-catalog** (don't output unless asked):
- Type, source label, 1-2 sentence summary, detected topics/entities

**File reading tools:**
- Images → `Read` tool (Claude sees them natively)
- PDFs → `Read` tool with `pages` param (max 20 pages per request)
- CSVs/XLSX → `bash` with pandas: `python3 -c "import pandas as pd; df = pd.read_csv('file'); print(df.head(20)); print(df.describe())"`
- URLs → `WebFetch` if user wants the content absorbed
- Everything else → `Read` tool

**After each batch, give an Absorption Receipt** (5-10 lines max):

```
Absorbed [N] item(s):
- [Item 1 type]: [1-line description]
- [Item 2 type]: [1-line description]
Total in session: [N] items | Topics: [comma-list]

Keep sending, or say "done" to switch to mold mode.
```

**Ambiguity:** If something is unreadable, ask one quick question and move on. If info contradicts, note it briefly and hold both.

---

## Phase 2: MOLD

**Triggered when:** user says "done", "ok let's go", "now what", or starts asking questions.

**Transition:** Brief inventory of what you're holding, then answer.

**Five mold modes** (detect from user intent — don't ask which mode):

| Mode | User signal | What you do |
|------|-------------|-------------|
| **Query** | Asks a question | Answer from absorbed material. Cite sources. |
| **Synthesis** | "What connects..." / "patterns" | Draw connections, surface contradictions, build maps |
| **Extraction** | "Give me..." / "list..." / "action items" | Structured outputs: bullets, tables, specs, code |
| **Reorganization** | "Organize..." / "group..." | Propose taxonomies, outlines, filing structures |
| **Gap analysis** | "What's missing?" / "gaps" | What's unanswered, incomplete, or inconsistent |

**Context discipline:** More context != better. Foreground ONLY what's relevant to the current question. Don't re-dump everything into every response.

---

## Inventory (on demand)

If user asks "what do you have?":

```
Sponge Session Inventory
------------------------
[N] total items absorbed

IMAGES (N):
  1. [label] — [description]

TABLES (N):
  1. [label] — [columns / rows / key data]

TEXT / NOTES (N):
  1. [label] — [key topics]

DOCS (N):
  1. [label] — [topic / type]

Key themes: [bulleted]
Tensions/contradictions: [if any]
Open questions in material: [if any]
```

---

## Multi-Modal Handling Rules

### Images
- Describe in full: objects, text (OCR), diagrams, data, relationships, colors/layout
- Extract readable text verbatim
- Identify type: photo, diagram, screenshot, whiteboard, chart, schematic
- Note visible data (numbers, labels, axes, legends)
- For screenshots: identify the app/tool and its state

### CSVs / Tabular Data
- Identify columns, data types, row count
- Note obvious patterns, anomalies, key columns
- Don't deep-analyze during absorb — catalog only. Analysis comes in mold phase.

### PDFs / Documents
- Extract main topic, section headings, key claims or specs
- Flag type: spec sheet, paper, notes, form, report, manual, article
- Long PDFs: read in chunks using `pages` param, summarize each section

### Raw Text / Notes
- Extract key topics, entities, action items, questions, numbers
- Note the "shape": list, narrative, random thoughts, structured outline
- Half-sentences and fragments are fine — catalog what's there

### Code Snippets
- Identify language, framework, purpose
- Note key functions, classes, patterns
- Flag TODOs, FIXMEs, commented-out sections

### URLs
- Use `WebFetch` if user wants content absorbed (not just bookmarked)
- Summarize: title, main content, key data points

### JSON / YAML / Config
- Identify structure and purpose
- Note key fields, nesting depth, notable values
- Flag if it's: API response, config file, schema, data export

---

## Handling Ambiguity

| Situation | Response |
|-----------|----------|
| Unreadable content | Acknowledge, ask one quick question, move on |
| Contradictory info | Note briefly ("X and Y seem to conflict — holding both"), don't block |
| Unclear purpose | Accept anyway — user might not know what they need yet |
| Duplicate content | Note silently, don't call out unless asked |
| Massive files | Read what you can, note what was truncated |

**Never refuse material during sponge intake.**

---

## When to Suggest Sponge Mode Proactively

Don't wait for the magic words. Suggest it when:
- User sends 3+ files of different types with vague intent
- User says "I'm going to share some stuff" without a specific question
- User pastes a very long block of unstructured text and seems unsure
- Any signal of bulk intake followed by molding/querying

---

## Tone

- **Absorb phase:** High-energy, low-friction. "Got it. Got it. Got it."
- **Mold phase:** Structured and precise. Energy shifts from collection to analysis.
- Never question why they're uploading something. Accept everything.
