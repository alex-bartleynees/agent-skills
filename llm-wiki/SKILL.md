---
name: wiki-llm
version: "1.0.0"
description: "Maintain a personal Obsidian knowledge base using the wiki-llm pattern — LLM writes, human reads. Use for ingesting sources, answering queries against the vault, linting for consistency, and processing inbox items."
---

# Wiki-LLM

An Obsidian vault maintained by an LLM agent. The agent writes and maintains
all content; the human reads, browses, and guides. The vault stays
human-readable markdown — valid Obsidian files at all times.

## Vault Location

The vault lives at `/home/netclaw/.netclaw/workspaces/obsidian-vault`. Set
this as your working directory before any operations.

Before starting work, read `AGENTS.md` in the vault root for the full schema.
Read `index.md` for the content catalog; read `log.md` for recent activity.

## Directory Structure (PARA Hybrid)

```
obsidian-vault/
├── 1-Active/       # Time-bound projects with deadlines
├── 2-Areas/        # Ongoing responsibilities (Work, Homelab, Coffee journal, etc.)
├── 3-Resources/    # Timeless reference material (AI, Kubernetes, NixOS, etc.)
├── 4-Archive/      # Completed projects
├── _System/        # Vault infrastructure (read-only to LLM writes)
├── _Inbox/         # Quick capture awaiting processing
├── assets/         # Images and attachments
├── AGENTS.md       # Full vault schema (read this first)
├── index.md        # Content catalog (LLM-maintained)
└── log.md          # Chronological activity log (LLM-maintained)
```

## Core Operations

### Ingest

When given a new source (article, note, idea):

1. Read the source completely.
2. Discuss key takeaways with the user — what's worth capturing.
3. Write a summary page in the appropriate directory (1-Active, 2-Areas, or 3-Resources).
4. Update related pages with cross-references to the new page.
5. Update `index.md` — add the new page with a one-line summary.
6. Append an entry to `log.md`.
7. Report what was touched.

### Query

When asked a question:

1. Read `index.md` first to find relevant pages.
2. Read those pages in full.
3. Synthesize an answer with `[[wiki links]]` as citations.
4. Offer to file the answer as a new wiki page if substantial.

### Lint / Health Check

When asked to lint:

1. Scan for contradictions between pages.
2. Find orphan pages (no inbound links).
3. Find important concepts lacking their own page.
4. Check for missing cross-references.
5. Report findings with specific `[[links]]`.
6. Offer to fix each issue.

### Inbox Processing

When asked to process the inbox:

1. Read all files in `_Inbox/`.
2. Classify each as: project task, area update, resource reference, or discard.
3. Move content to the right place, creating pages as needed.
4. Follow Ingest steps for new pages.

## Conventions

- **Frontmatter**: Every page gets YAML frontmatter with `tags`.
- **Wiki links**: Use `[[double-bracket links]]` for internal refs.
- **Page titles**: `# Page Title` as first heading after frontmatter. Filenames match titles.
- **Templates**: Use `_System/Templates/` as base for new pages.
- **One topic per page** — split long pages rather than letting them sprawl.
- **Never delete original content** — revise and expand.
- **Cross-reference aggressively** — every new page links to and is linked from related pages.
- **Read before you write** — always read related pages first.

## Markdown Conventions

Frontmatter on every page:

```yaml
---
tags:
  - project/dopamine-kick
  - architecture
  - dotnet
---
```

Wiki links: `[[1-Active/Dopamine-Kick-App/Backend Architecture]]` or `[[Backend Architecture]]`.

## Index and Log Formats

### index.md entries

```markdown
- [[Path/To/Page]] — one-line summary of content
```

### log.md entries

```markdown
## [YYYY-MM-DD] ingest | Source Title

- Created: [[New-Page]]
- Updated: [[Existing-Page]] (added cross-ref)
- Summary: one sentence

## [YYYY-MM-DD] query | Topic

- Filed result as: [[Page]] (or not filed)
```

## Common Pitfalls

- Don't invent new tags — check existing tags first.
- Don't write without reading related pages first.
- The `_System/` directory is read-only for LLM writes.
- Keep the index and log updated on every operation — they're the navigation system.
- The vault is synced via Syncthing; don't run git init or git operations unless asked.
