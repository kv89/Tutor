# Personal Technical Tutor

A coding agent (Claude Code, Codex CLI or OpenCode) becomes a patient technical tutor for AI/ML, LLMs, agents, programming, debugging, software engineering, system design, cloud and research. It gauges what you know, guides you with graduated hints, repairs misconceptions, and gives the full answer when you ask. After a meaningful lesson it saves a concept note and your progress as plain Markdown, so later sessions can quiz you from it.

No packages, services or database are needed. Everything is local Markdown.

## Quick start

Open a terminal in this folder and start one tool:

```sh
cd "/path/to/Tutor"
claude      # or: codex, opencode
```

Then say what you want, for example `Teach me database indexes.` You do not need to restate the rules.

| Tool | How it finds the rules |
|---|---|
| Claude Code | `CLAUDE.md` imports `AGENTS.md`. The skill `/technical-tutor` is in `.claude/skills/`. |
| Codex CLI | Loads `AGENTS.md`. It has no skill here, so `AGENTS.md` alone routes to `TUTOR.md`. |
| OpenCode | Loads `AGENTS.md`, and discovers the skill in `.claude/skills/`. |

You need one of these tools installed and signed in, and write access to this folder so notes can be saved. Setup guides: [Claude Code](https://code.claude.com/docs/en/setup), [Codex CLI](https://developers.openai.com/codex/cli), [OpenCode](https://opencode.ai/docs/).

Always start the tool from this folder, because project instructions load from the current directory.

## Example prompts

- `Teach me self-attention in transformers.`
- `Help me debug this function. Hints first: [paste code]`
- `Show the complete solution and explain each step: [paste problem]`
- `Review database indexes from my notes. Quiz me first, hide answers until I reply.`
- `Prepare a 20-minute interview review of [topics]. Use my weak areas and give me one novel problem.`

## How it works

1. **Assess** what you know from the conversation and your saved progress.
2. **Guide** with a small hint, then stronger hints, worked examples and prerequisites as needed.
3. **Diagnose** misconceptions: name the faulty model, explain why it fails, try another explanation.
4. **Check** with your own explanation or a new problem. Recognition alone is not mastery.
5. **Save** meaningful lessons: search for an existing note, update or create one, update the index and progress, then read the files back.

It will not make you guess forever. Ask for the answer and you get it with reasoning.

The full rules, including the four learning states (Introduced, Practising, Understood, Demonstrated mastery), are in [TUTOR.md](TUTOR.md). That file is the single source of truth.

## Files

| Path | Purpose |
|---|---|
| [TUTOR.md](TUTOR.md) | Teaching and logging rules. Edit here to change behavior. |
| [AGENTS.md](AGENTS.md) | Project instructions read by Codex and OpenCode. Routes to `TUTOR.md`. |
| [CLAUDE.md](CLAUDE.md) | Claude Code adapter that imports `AGENTS.md`. |
| [.claude/skills/technical-tutor/SKILL.md](.claude/skills/technical-tutor/SKILL.md) | On-demand tutor skill. |
| [knowledge/README.md](knowledge/README.md) | Categories, file naming, concept-note format. |
| [knowledge/index.md](knowledge/index.md) | Concept map with keywords. One entry per concept. |
| [knowledge/progress.md](knowledge/progress.md) | Status, evidence, gaps and misconceptions per concept. |
| `knowledge/<category>/<concept>.md` | Concept notes, created as you learn. |
| [review/revision-queue.md](review/revision-queue.md) | Concepts worth revisiting. See also [review/README.md](review/README.md). |
| [evaluations/test-scenarios.md](evaluations/test-scenarios.md) | Manual test scenarios and recorded results. |

Your concept notes, index, progress and revision queue are personal learning data. Keep them out of any public copy of this repository.

## Status

Verified in Claude Code on 2026-10-25: guided hints, a full answer on request, saving a note, updating it without a duplicate, and quizzing from the saved note in a fresh session. Details are in [evaluations/test-scenarios.md](evaluations/test-scenarios.md).

Not yet verified:
- Codex CLI and OpenCode have not been launched with this project.
- Misconception repair, transfer checks, mastery gating and read-only behavior have no recorded results.
- Behavior with many concepts, such as index growth and duplicate detection, is untested.

Known limits:
- There is no scheduler. Revision happens when you ask, using the revision queue.
- Logging and status decisions rely on the agent following `TUTOR.md`. Nothing validates them, so check the files.

## Troubleshooting

- **Acts like a generic chatbot:** check you started the tool from this folder (`pwd`). In Claude Code run `/context` and look for `CLAUDE.md`. Otherwise ask: `Read TUTOR.md and follow it for this session.`
- **Note not saved:** check the tool has write access, then look at the files. Trust the filesystem, not the agent's claim. If writing is blocked it should give you the note text instead.
- **Duplicate notes:** compare them, keep one, merge anything useful, fix `knowledge/index.md`, then delete the other.
- **Skill not found:** check `.claude/skills/technical-tutor/SKILL.md` exists. Codex has no skill by design.
- **Stop a lesson:** say so, for example `Stop tutoring. Answer directly and don't save notes.`
