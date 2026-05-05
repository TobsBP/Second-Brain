# LLM Wiki — Personal Knowledge Vault

An Obsidian vault managed by Claude Code. Raw notes come in; structured, interlinked wiki pages come out.

## What it does

Claude Code reads raw inputs (class notes, journal entries, book chapters, job documents) and synthesizes them into a navigable wiki under `wiki/`. It tracks sources, maintains a master index, and keeps an append-only activity log.

## Directory layout

```
raw/
  journal/        ← daily journal entries (immutable)
  books/          ← book sources, one folder per book (immutable)
  assets/         ← images from clipped articles
  job/            ← living work docs: task boards, team notes, standups
wiki/
  personal/       ← synthesized personal knowledge (goals, health, self-model, relationships)
  books/          ← book companion pages (characters, places, themes, plot threads)
  subjects/       ← university subject notes (one folder per course code)
  concepts/       ← cross-domain ideas that appear in multiple contexts
index.md          ← master catalog of all wiki pages
log.md            ← append-only activity log
CLAUDE.md         ← operating guide for Claude Code
```

`raw/` is mostly immutable — Claude only writes to `raw/job/`. All synthesis goes to `wiki/`, `index.md`, and `log.md`.

## How to use it

Open this folder in Obsidian for navigation. Use Claude Code (via the CLI or VS Code extension) to ingest new content and run queries.

### Ingest a class note

Drop a note file into `raw/subjects/<CODE>/` then tell Claude:

```
ingest S03 aula 5
```

Claude will extract key concepts, update the subject overview page, create or update concept pages, and log the operation.

Subject codes: `C24` (Inteligência Artificial), `C09` (Computação Gráfica), `S03` (Arquitetura de Software), `S02` (Banco de Dados 2), `S07` (Qualidade e Gerência de Config).

### Ingest a journal entry

Drop a file into `raw/journal/` (named `YYYY-MM-DD.md`) then:

```
ingest journal 2026-05-05
```

Claude will discuss key takeaways with you before writing, then update or create pages under `wiki/personal/`.

### Ingest a book chapter

Drop content into `raw/books/<book-folder>/` then:

```
ingest Modern Software Engineer chapter 3
```

Claude will discuss the chapter, update the book overview, and maintain per-entity pages (characters, themes, etc.) under `wiki/books/<book-slug>/`.

### Query the wiki

Just ask a question in natural language. Claude reads `index.md` first to find relevant pages, then synthesizes an answer with citations. If the answer is substantive, it will offer to save it as a new wiki page.

### Health check

```
lint
```

Claude scans for contradictions, stale pages, orphan pages, broken index links, and topics thin on sources, then asks which to act on.

## Wiki page types

| Type | Location | Purpose |
|------|----------|---------|
| `personal` | `wiki/personal/` | Goals, habits, health, relationships, patterns |
| `book-entity` | `wiki/books/<slug>/` | Characters, places, themes, plot threads |
| `book-overview` | `wiki/books/<slug>/overview.md` | One per book — summary, progress, themes |
| `concept` | `wiki/concepts/` | Ideas that recur across personal life and books |
| Subject overview | `wiki/subjects/<CODE>/` | Course overview + topics covered |
| Subject concept | `wiki/subjects/<CODE>/<slug>.md` | Per-concept deep-dives |

## Conventions

- File names: `kebab-case.md`
- Internal links: Obsidian wiki-link format `[[path/to/page]]`
- Dates: ISO 8601 (`YYYY-MM-DD`)
- Every page has YAML frontmatter with `type`, `updated`, and relevant tags
- Every new page must be linked from at least one other page (no orphans)
- Each entity page opens with a single synthesis paragraph — rewritten on every update, never appended to
- All wiki output is in English

## Key files

- `index.md` — start here when navigating; lists every wiki page with a one-line description
- `log.md` — append-only record of every ingest operation; grep it to see what has been processed
- `CLAUDE.md` — full operating guide used by Claude Code to drive all workflows
