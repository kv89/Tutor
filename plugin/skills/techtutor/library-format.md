# Library format

The library stores concise, reusable concept references rather than conversation transcripts. Markdown files are the source of truth. The same content has two layouts; `TUTOR.md` says which one to use.

## Folder layout

Used when the tutor can read and write files. Paths are relative to the library folder.

| Path | Holds |
|---|---|
| `knowledge/index.md` | Index: one entry per concept |
| `knowledge/progress.md` | Progress table and the resume note |
| `knowledge/<category>/<id>.md` | One concept reference per file |
| `review/revision-queue.md` | Revision queue |

Create category directories only when their first concept is added. When creating a new folder library, start the three record files with the headings and empty tables shown in the single-file layout.

## Single-file layout

Used in Project mode and chat-only mode, where one file is easier to keep, download and replace. The file is named `tutor-library.md` and keeps these top-level sections in this order:

```markdown
# Tutor library

## Resume
Last session: YYYY-MM-DD — concept-id — one line on where the learner stopped.
Next: the next useful step.

## Index
### Software engineering
- `db-indexes` — Database indexes — index, B-tree, range scan

## Progress
| Concept | Status | Evidence or gap | Recurring misconception | Next review or exercise |
|---|---|---|---|---|

## Revision queue
| Concept | Why revisit | Recall or application prompt | Review timing |
|---|---|---|---|

## Concepts
### concept: db-indexes
(the concept reference, with its headings demoted by two levels)
```

Each concept is one `### concept: <id>` block. Its frontmatter fields become a short `id / category / status / last_reviewed` line under that heading. Links between concepts use the ID instead of a path. To move between layouts, map each block to `knowledge/<category>/<id>.md` and back without changing content.

## Resume note

Both layouts keep a two-line resume note: in the folder layout at the top of `knowledge/progress.md`, in the single-file layout under `## Resume`. Update it at the end of each meaningful session. A new conversation reads it first to continue where the learner stopped.

## Categories

- `ai-ml/` — AI, machine learning, statistics, neural networks, training, and evaluation.
- `llms/` — transformers, tokens, embeddings, inference, context, sampling, quantization, and model behavior.
- `rag/` — retrieval, vector search, grounding, and retrieval evaluation.
- `agents/` — AI agents, tools, MCP, orchestration, safety, and agent evaluation.
- `programming/` — languages, data structures, algorithms, debugging, and testing.
- `software-engineering/` — design, APIs, databases, reliability, security, and production practice.
- `system-design/` — distributed systems, architecture, scalability, and trade-offs.
- `cloud/` — cloud architecture, DevOps, deployment, and operations.
- `research/` — research methods and unfamiliar subjects that do not fit the categories above.

Choose the most specific useful category; do not create a second copy to cross-list a concept. Use a lowercase kebab-case filename, such as `transformer-attention.md`. Keep the title and a stable short ID in the document. Search the index and related files before creating one. Put aliases and related-topic links in the index instead of duplicating files.

## Concept reference format

Use relevant sections from this outline. Remove sections that do not help explain or revise the concept.

```markdown
---
id: short-stable-id
category: category/subcategory
status: introduced | practising | understood | demonstrated mastery
last_reviewed: YYYY-MM-DD
---

# Concept title

Prerequisites: [links or short list]
Related: [links]
Keywords: term, alias, related term

## Definition
## Why it exists
## Mental model
## How it works
## Example
## Uses and trade-offs
## Limitations and failure modes
## Misconceptions and corrections
## Comparisons
## Interview questions
## Quick review
## Active recall
### Questions
### Answers (peek after retrieval)
## Apply it
## Sources
## Learning evidence
- YYYY-MM-DD — Demonstrated / gap / correction / next review.
```

Do not fabricate dates, prerequisites, sources, evidence, or status. A date is the local date of an actual review or learning session. Store concise outcomes: learner evidence, unresolved gaps, important corrections, and a useful next exercise. Keep question answers in their own section so they can be hidden during recall.

## Index and progress maintenance

The index is the navigation map. Add one entry per concept with its stable ID, relative path (folder layout only), category, and useful search keywords; keep entries alphabetized within category. Progress is a compact cross-concept view of learning status, evidence, recurring misconceptions, gaps, and next review. Detailed explanations belong in the concept reference, not progress records.

Use only these states: **Introduced**, **Practising**, **Understood**, and **Demonstrated mastery**. See `TUTOR.md` for evidence thresholds. When evidence is absent, use Introduced or leave status unverified; do not promote status just because a session occurred.

After edits, verify links and read back changed files. If a write fails, report the limitation and offer the content instead of claiming it was saved.
