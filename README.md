# Sponge Mode

A Claude Code plugin for bulk intake and synthesis. Dump a messy pile of mixed-format material — images, CSVs, PDFs, screenshots, notes, code, links — then query, mold, and extract structured outputs from it.

## Install

```bash
claude plugin install --github itay-turgeman/sponge-mode
```

## Usage

Type `/sponge` in any Claude Code session, or just start dumping material and say "sponge mode" — Claude will activate automatically.

## How It Works

Two phases:

### Phase 1: Absorb

Send everything. Images, CSVs, PDFs, plain text, code snippets, URLs, JSON, voice transcripts — all at once, mixed formats, no structure needed. Sponge catalogs each piece silently and gives you a short receipt after each batch.

```
Absorbed 4 item(s):
- Image: Architecture diagram showing service mesh
- CSV: Q1 revenue data (12 rows, 6 columns)
- PDF: API specification v2.3
- Notes: Brain dump on migration strategy
Total in session: 4 items | Topics: architecture, revenue, API, migration

Keep sending, or say "done" to switch to mold mode.
```

### Phase 2: Mold

Say "done" or start asking questions. Sponge detects what you need:

| Mode | You say | What happens |
|------|---------|-------------|
| **Query** | "What does the CSV say about X?" | Answers from your material, cites sources |
| **Synthesis** | "How do these connect?" | Draws patterns across items, surfaces contradictions |
| **Extraction** | "Give me action items" | Structured outputs: bullets, tables, specs, code |
| **Reorganization** | "Organize this into categories" | Proposes taxonomies, outlines, filing structures |
| **Gap analysis** | "What's missing?" | Surfaces holes, inconsistencies, open questions |

## Supported Formats

- Images (screenshots, photos, diagrams, whiteboards, sketches)
- CSVs / spreadsheets
- PDFs (specs, papers, reports, manuals)
- Plain text / notes / brain dumps
- Code snippets (any language)
- URLs (fetched and summarized)
- JSON / YAML / config files
- Voice transcripts
- Any mix of the above in a single message

## Trigger Phrases

Sponge Mode activates automatically when you say things like:

- "sponge mode"
- "absorb this"
- "here's a brain dump"
- "take all of this in"
- "I want to dump a bunch of stuff"
- "let's work with all of this"

Or just invoke it directly with `/sponge`.

## License

MIT
