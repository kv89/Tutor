# Personal Technical Tutor

An AI assistant becomes a patient technical tutor for AI/ML, LLMs, agents, programming, debugging, software engineering, system design, cloud and research. It gauges what you know, guides you with graduated hints, repairs misconceptions, and gives the full answer when you ask. After a meaningful lesson it saves a concept note and your progress as plain Markdown, so later sessions can continue and quiz you from it.

It works in coding agents (Claude Code, Codex CLI, OpenCode), in Claude.ai and Claude Desktop, and in ChatGPT Projects. Running it needs no packages, services or database. Your library is always plain Markdown.

## Start a session

Type `/techtutor` at the start of any chat, optionally followed by what you want, such as `/techtutor teach me database indexes`. The tutor finds your library, tells you where you left off and stays in tutor mode until you ask it to stop. Skill names must be lowercase, so the command is `/techtutor`.

| App | How `/techtutor` works |
|---|---|
| Claude Code, Claude.ai, Claude Desktop, Claude mobile, Cowork | A real command once the plugin is installed (Options A–C) |
| ChatGPT | An instruction inside your Tutor Project (Option D). Outside that Project it does nothing |
| Other coding agents | A skill command, where the agent supports them (Option E) |

## Choose how to use it

| You use | Install | Where notes are saved |
|---|---|---|
| Claude Code, Codex or OpenCode in this folder | [Clone and run](#option-a-clone-and-run-in-this-folder) | `knowledge/` and `review/` in this folder, automatically |
| Claude Code anywhere | [Claude Code plugin](#option-b-claude-code-plugin) | A folder you pick, automatically |
| Claude.ai, Claude Desktop, Cowork | [Claude app plugin](#option-c-claudeai-claude-desktop-and-cowork) | A connected folder, or `tutor-library.md` in a Claude Project |
| ChatGPT | [ChatGPT Project](#option-d-chatgpt-project) | `tutor-library.md` in the ChatGPT Project |
| Other coding agents (Cursor, Windsurf, Cline and others) | [Skills CLI](#option-e-other-coding-agents) | A folder you pick, automatically |

## Option A: Clone and run in this folder

1. Get the project and open a terminal in it:

```sh
git clone https://github.com/kv89/Tutor
cd Tutor
```

2. Start one tool from this folder:

```sh
claude      # or: codex, opencode
```

3. Check it loaded the rules. Paste: `Before teaching, tell me which project instruction files you loaded and whether you will read TUTOR.md for a tutoring request. Do not edit files.` It should name `AGENTS.md` (Claude Code: `CLAUDE.md`) and say it will read `TUTOR.md`.

4. Say what you want, for example `Teach me database indexes.` You do not need to restate the rules.

To test the full loop (hints, full answer, saving, retrieval), run the five smoke tests in [evaluations/test-scenarios.md](evaluations/test-scenarios.md#smoke-tests).

| Tool | How it finds the rules |
|---|---|
| Claude Code | `CLAUDE.md` imports `AGENTS.md`. The skill `/techtutor` is found through `.claude/skills/`, a link to `plugin/skills/techtutor/`. |
| Codex CLI | Loads `AGENTS.md`, which routes to `TUTOR.md`. |
| OpenCode | Loads `AGENTS.md`, and discovers the skill in `.claude/skills/`. |

You need one of these tools installed and signed in, and write access to this folder so notes can be saved. Setup guides: [Claude Code](https://code.claude.com/docs/en/setup), [Codex CLI](https://developers.openai.com/codex/cli), [OpenCode](https://opencode.ai/docs/). Always start the tool from this folder, because project instructions load from the current directory.

## Option B: Claude Code plugin

```sh
claude plugin marketplace add kv89/Tutor
claude plugin install techtutor@kv89-tutor
```

Or inside a session: `/plugin install techtutor --marketplace kv89/Tutor`.

On the first lesson the tutor asks where to keep your library, then creates `knowledge/` and `review/` there. To use this repository's library, start Claude Code in this folder.

## Option C: Claude.ai, Claude Desktop and Cowork

1. In Claude.ai or the desktop app, open **Customize → Plugins → Add → Add marketplace** and enter `kv89/Tutor`.
2. Add **Technical Tutor**, and turn on **Code execution and file creation**.
3. Choose where notes are saved:
   - **Folder (Claude Desktop or Cowork):** connect a folder, for example by opening it in Cowork or enabling a filesystem extension for it. Notes then save there automatically.
   - **Project (web, desktop or mobile):** create a Project named `Tutor` and chat inside it. On the first lesson the tutor offers to start `tutor-library.md`. Add that file to the Project's files. Each new chat in the Project reads it and continues from its resume note.
4. At the end of a lesson, look for the save message:
   - If the tutor edited the Project file, open it in Project files to confirm the change.
   - If it says **"Not saved yet"**, download the file it gives you and replace `tutor-library.md` in Project files.

## Option D: ChatGPT Project

ChatGPT does not install this plugin, so you add the rules as Project files:

1. Create a ChatGPT Project named `Tutor`.
2. Upload [`TUTOR.md`](plugin/skills/techtutor/TUTOR.md) and [`library-format.md`](plugin/skills/techtutor/library-format.md) to the Project's files.
3. In the Project instructions, paste: `When I type /techtutor, or ask for tutoring, follow TUTOR.md and library-format.md in this Project's files, starting with "Starting a session". My library is tutor-library.md in this Project's files; use Project mode.`
4. Start each chat in the Project with `/techtutor`.
5. After the first lesson, add the `tutor-library.md` it gives you to the Project files. Replace it whenever the tutor says "Not saved yet".

To use the tutor outside the Project, make a Custom GPT named TechTutor with the two files in its Knowledge and the same instruction, then call it with `@TechTutor` in any chat. A Custom GPT can't keep `tutor-library.md` up to date in its Knowledge, so the Project is better for continuing between sessions.

## Option E: Other coding agents

```sh
npx skills add kv89/Tutor -g
```

This uses the open-source [skills CLI](https://vercel.com/changelog/introducing-skills-the-open-agent-skills-ecosystem). Add `-a <agent>` to pick one agent. As with Option B, the tutor asks where to keep your library.

## Where notes are saved

The tutor picks a storage mode once per conversation. The full rules are in [TUTOR.md](plugin/skills/techtutor/TUTOR.md#where-the-library-lives).

| Mode | When | What you do |
|---|---|---|
| Folder | It can read and write files | Nothing. It saves and reads back automatically |
| Project | A Project holds `tutor-library.md` | Replace the file when it says "Not saved yet" |
| Chat-only | No files at all | Keep the `tutor-library.md` it gives you, ideally in a Project |

Both layouts hold the same content and are described in [library-format.md](plugin/skills/techtutor/library-format.md). App memory, when available, holds at most a one-line bookmark. Your notes always live in the Markdown library.

## Example prompts

- `/techtutor`
- `/techtutor teach me self-attention in transformers.`
- `Continue where I left off.`
- `Help me debug this function. Hints first: [paste code]`
- `Show the complete solution and explain each step: [paste problem]`
- `Review database indexes from my notes. Quiz me first, hide answers until I reply.`
- `Prepare a 20-minute interview review of [topics]. Use my weak areas and give me one novel problem.`

## How it works

1. **Assess** what you know from the conversation and your saved progress.
2. **Guide** with a small hint, then stronger hints, worked examples and prerequisites as needed.
3. **Diagnose** misconceptions: name the faulty model, explain why it fails, try another explanation.
4. **Check** with your own explanation or a new problem. Recognition alone is not mastery.
5. **Save** meaningful lessons: search for an existing note, update or create one, update the index, progress and resume note, then read the files back.

It will not make you guess forever. Ask for the answer and you get it with reasoning.

The full rules, including the four learning states (Introduced, Practising, Understood, Demonstrated mastery), are in [TUTOR.md](plugin/skills/techtutor/TUTOR.md). That file is the single source of truth.

## Files

| Path | Purpose |
|---|---|
| [plugin/skills/techtutor/TUTOR.md](plugin/skills/techtutor/TUTOR.md) | Teaching, logging and storage rules. Edit here to change behavior. |
| [plugin/skills/techtutor/library-format.md](plugin/skills/techtutor/library-format.md) | Folder and single-file layouts, categories, concept-note format. |
| [plugin/skills/techtutor/SKILL.md](plugin/skills/techtutor/SKILL.md) | Skill entry point. Routes to the two files above. |
| [plugin/.claude-plugin/plugin.json](plugin/.claude-plugin/plugin.json) | Plugin manifest. |
| [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json) | Marketplace listing used by Options B and C. |
| `.claude/skills/techtutor` | Link to the skill folder, for Claude Code and OpenCode in this folder. |
| [AGENTS.md](AGENTS.md) | Project instructions read by Codex and OpenCode. Routes to `TUTOR.md`. |
| [CLAUDE.md](CLAUDE.md) | Claude Code adapter that imports `AGENTS.md`. |
| [knowledge/index.md](knowledge/index.md) | Concept map with keywords. One entry per concept. |
| [knowledge/progress.md](knowledge/progress.md) | Resume note, plus status, evidence, gaps and misconceptions per concept. |
| `knowledge/<category>/<concept>.md` | Concept notes, created as you learn. |
| [review/revision-queue.md](review/revision-queue.md) | Concepts worth revisiting. See also [review/README.md](review/README.md). |
| [evaluations/test-scenarios.md](evaluations/test-scenarios.md) | Manual test scenarios and recorded results. |

Your concept notes, index, progress and revision queue are personal learning data. Keep them out of any public copy of this repository. Plugins and skills load only from `plugin/`, and your notes are never part of it.

## Status

Verified in Claude Code on 2026-10-25: guided hints, a full answer on request, saving a note, updating it without a duplicate, and quizzing from the saved note in a fresh session. Details are in [evaluations/test-scenarios.md](evaluations/test-scenarios.md).

Verified on 2026-10-27 from a local copy: the plugin passes `claude plugin validate`, installs from a local marketplace, and loads one skill. In headless Claude Code with writes blocked, it did not claim a save and asked where to keep the library. In a folder holding only `tutor-library.md`, it resumed from the resume note.

Not yet verified:
- Installing from GitHub (Options B, C and E). These need the changes pushed to `kv89/Tutor`.
- Option C in Claude.ai and Claude Desktop, including whether Claude can edit Project files directly. Users report mixed results, so the tutor reads back or hands you the file.
- Option D in ChatGPT, and Option E in any agent.
- Codex CLI and OpenCode have not been launched with this project.
- Misconception repair, transfer checks, mastery gating and read-only behavior have no recorded results.
- Behavior with many concepts, such as index growth and duplicate detection, is untested. A large `tutor-library.md` may exceed what a Project reads at once.

Known limits:
- There is no scheduler. Revision happens when you ask, using the revision queue.
- Logging and status decisions rely on the agent following `TUTOR.md`. Nothing validates them, so check the files.
- In Project mode without direct file editing, you replace `tutor-library.md` by hand after each lesson.

## Troubleshooting

- **Acts like a generic chatbot:** check you started the tool from this folder (`pwd`). In Claude Code run `/context` and look for `CLAUDE.md`. Otherwise ask: `Read TUTOR.md and follow it for this session.`
- **Note not saved:** check the tool has write access, then look at the files. Trust the files, not the agent's claim. If writing is blocked it should give you the note text instead.
- **New chat doesn't remember the last lesson:** in a Project, check `tutor-library.md` is in Project files and is the latest version. Then ask: `Continue where I left off.`
- **Duplicate notes:** compare them, keep one, merge anything useful, fix the index, then delete the other.
- **Skill not found:** check `plugin/skills/techtutor/SKILL.md` exists. In this folder, check the `.claude/skills/techtutor` link resolves (`ls .claude/skills/techtutor/`). Codex uses `AGENTS.md` instead.
- **Stop a lesson:** say so, for example `Stop tutoring. Answer directly and don't save notes.`
