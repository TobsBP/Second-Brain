# LLM Wiki — Schema

This is Claude Code's operating guide for maintaining this Obsidian Vault as a personal knowledge wiki. Read this file at the start of every session.

## Directory layout

```
raw/
  journal/        ← immutable journal entries (markdown, one file per entry)
  books/          ← immutable book sources (one folder per book, one file per chapter)
  assets/         ← images downloaded from clipped articles
wiki/
  personal/       ← synthesized personal knowledge (goals, health, patterns, self-model)
  books/          ← book companion pages (characters, places, themes, plot threads)
  concepts/       ← cross-domain concepts that appear in both personal and book contexts
index.md          ← master catalog of all wiki pages (update on every ingest)
log.md            ← append-only activity log (update on every operation)
CLAUDE.md         ← this file
```

**Rule:** Never modify anything under `raw/`. Read from it; write only to `wiki/`, `index.md`, and `log.md`.

---

## Page formats

### Personal entity page (`wiki/personal/<slug>.md`)
Used for recurring entities in the user's life: goals, habits, health dimensions, relationships, psychological patterns.

```markdown
---
type: personal
tags: [goal|habit|health|relationship|psychology|pattern]
updated: YYYY-MM-DD
source_count: N
---

# <Entity Name>

One-paragraph current synthesis — what is known about this entity as of the latest ingest.

## History
Chronological list of key developments, with dates and source references.

## Patterns
Recurring themes or observations across multiple sources.

## Open questions
Things that remain unclear or worth tracking.

## Sources
- [[raw/journal/YYYY-MM-DD]] — one-line note on what this source contributed
```

### Book page (`wiki/books/<book-slug>/<page-slug>.md`)
Used for characters, places, themes, plot threads, symbols — one page per entity.

```markdown
---
type: book-entity
book: <Book Title>
tags: [character|place|theme|plot|symbol|event]
updated: YYYY-MM-DD
chapters_covered: N
---

# <Entity Name>

Current synthesis of everything known about this entity so far.

## Appearances
Chronological list of chapters where this entity appears, with brief notes.

## Connections
Links to other entities this one is meaningfully connected to: [[wiki/books/<book>/<other>]]

## Themes
Which broader themes this entity embodies or complicates.

## Open threads
Unresolved plot threads, unanswered questions, foreshadowing to watch for.
```

### Book overview page (`wiki/books/<book-slug>/overview.md`)
One per book. High-level summary, thesis, major themes, reading progress.

```markdown
---
type: book-overview
book: <Book Title>
author: <Author>
started: YYYY-MM-DD
chapters_read: N / TOTAL
tags: [fiction|nonfiction|...]
---

# <Book Title> — Overview

One-paragraph summary of what this book is about and the central thesis/story.

## Major themes
- [[wiki/books/<slug>/theme-name]] — one-line description

## Key characters / entities
- [[wiki/books/<slug>/character-name]] — one-line description

## Reading progress
| Chapter | Ingested | Notes |
|---------|----------|-------|
| 1 — Title | YYYY-MM-DD | brief note |

## Evolving thesis
How the central argument or story is building as more chapters are read.
```

### Concept page (`wiki/concepts/<slug>.md`)
For ideas that recur across personal life and books — e.g., "motivation", "identity", "loss", "routine".

```markdown
---
type: concept
updated: YYYY-MM-DD
appears_in: [personal, books/<book-slug>]
---

# <Concept Name>

Definition and current understanding of this concept.

## In personal context
How this concept shows up in the user's life and journals.

## In books
How this concept appears across books being tracked.

## Cross-references
Links to related concepts, personal pages, and book pages.
```

---

## Workflows

### Ingest: class notes

1. User drops a note file into `raw/subjects/<CODE>/` and says "ingest [subject] aula [N]".
2. Read the note fully.
3. Extract: key concepts, definitions, examples, formulas, and anything flagged as important.
4. Update the subject's overview page (`wiki/subjects/<CODE>/overview.md`): add the class to the log, update topics covered.
5. For each key concept: create or update a concept page in `wiki/subjects/<CODE>/<concept-slug>.md`.
6. Generate a **"Key questions to review"** section in the overview — questions the student should be able to answer based on what was covered.
7. Check if the topic connects to other subjects or books already in the wiki — add cross-references.
8. Update `index.md`.
9. Append to `log.md`: `## [YYYY-MM-DD] ingest | subject | <CODE> aula <N> — <topic>`

Subject codes:
- C24 → Inteligência Artificial
- C09 → Computação Gráfica
- S03 → Arquitetura de Software
- S02 → Banco de Dados 2
- S07 → Qualidade, Gerência de Config e Evolução de Software

### Ingest: journal entry

1. User drops a new file into `raw/journal/` and says "ingest [filename]".
2. Read the entry fully.
3. **Discuss** key takeaways with the user before writing anything — ask if anything should be emphasized or de-emphasized.
4. Write a summary page if the entry is long enough to warrant one (otherwise just update entity pages).
5. Identify all personal entities mentioned (goals, habits, health, relationships, feelings, patterns).
6. For each entity: update or create the page in `wiki/personal/`. Add a source entry, update History, revise the synthesis paragraph if the new data changes it.
7. Check if any concepts in `wiki/concepts/` are relevant — update them.
8. Update `index.md`.
9. Append to `log.md`: `## [YYYY-MM-DD] ingest | journal | <filename>`

### Ingest: book chapter

1. User drops content into `raw/books/<book>/` and says "ingest [book] chapter [N]".
2. Read the chapter.
3. **Discuss** key events, characters, and themes with the user.
4. Update the book's overview page (or create it if first chapter).
5. For each character, place, or theme that appears: update or create the page in `wiki/books/<book-slug>/`.
6. Flag any contradictions with earlier chapters (e.g., a character behaving inconsistently with their established traits).
7. Check if any personal concepts in `wiki/concepts/` resonate — note the connection.
8. Update `index.md`.
9. Append to `log.md`: `## [YYYY-MM-DD] ingest | book | <Book Title> ch.<N>`

### Query

1. Read `index.md` first to find relevant pages.
2. Read those pages.
3. Synthesize an answer with citations to wiki pages (e.g., `[[wiki/personal/goals]]`).
4. If the answer is substantive (a comparison, analysis, or new insight), ask the user: "Should I file this as a wiki page?" If yes, create it under the most appropriate category.

### Lint

Run when the user asks for a health check. Check for:
- **Contradictions**: pages that claim conflicting things
- **Stale pages**: personal pages not updated in 30+ days that may be outdated
- **Orphans**: wiki pages with no inbound links from other wiki pages
- **Missing pages**: concepts mentioned inline but lacking their own page
- **Dead ends**: pages in `index.md` that no longer exist
- **Data gaps**: topics thin on sources that a web search could fill

Report findings as a list. Ask the user which to act on.

---

## Conventions

- **File naming**: `kebab-case.md` for all wiki files.
- **Internal links**: always use Obsidian wiki-link format `[[path/to/page]]`, relative to vault root.
- **Dates**: ISO 8601 (`YYYY-MM-DD`) everywhere.
- **Frontmatter**: always include `type`, `updated`, and relevant tags. Use YAML.
- **No orphans**: every new wiki page must be linked from at least one other page (usually the book overview or the index).
- **One synthesis paragraph**: every entity page opens with a current synthesis — the single most accurate, up-to-date summary. Rewrite it on every update; don't append.
- **Source references**: always cite which raw file contributed which claim, so claims can be traced.

---

## Index format

`index.md` is organized by category. Each entry: `- [[path/to/page]] — one-line description`. Update it on every ingest.

## Log format

`log.md` is append-only. Each entry starts with `## [YYYY-MM-DD] <operation> | <domain> | <detail>`. Never edit past entries.
