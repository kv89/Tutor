# Project progress

## 2026-10-27 — Packaged as an installable plugin and skill; storage modes added
**Done (verified):**
- Skill folder that works on its own: `plugin/skills/technical-tutor/` holds `SKILL.md`, `TUTOR.md` (moved from the root) and `library-format.md` (moved from `knowledge/README.md`). `.claude/skills/technical-tutor` is now a symlink to it.
- Install files: `.claude-plugin/marketplace.json` (marketplace `kv89-tutor`, source `./plugin`) and `plugin/.claude-plugin/plugin.json` (`technical-tutor` 0.1.0).
- `TUTOR.md` has a new "Where the library lives" section (folder, desktop without file access, Project, chat-only), a memory-as-bookmark rule, and reworded file-safety rules. `library-format.md` adds the single-file `tutor-library.md` layout and the resume note.
- README rewritten with install options A–E and a storage-mode table. Scenarios 16–18 added to `evaluations/test-scenarios.md`.
- Checks run: `claude plugin validate .` and `./plugin` pass. A mutation check (`..` in source) made validation fail, then it passed after revert. A local-marketplace install into a temp `CLAUDE_CONFIG_DIR` loaded 1 skill. Three headless tests passed: writes blocked → no false save claim; resume from `tutor-library.md`; loader check in the repo finds the new `TUTOR.md` path.

**Next:** the owner pushes, then tests Option C in Claude.ai (does Claude edit Project files directly or hand over the file?), Option D in ChatGPT, and `npx skills add kv89/Tutor` (blocked here by the permission classifier, not run).

**Open questions:**
- Add a LICENSE? Only Anthropic's plugin directory requires one.
- Does a headless or installed run prompt to read the skill's own files outside the working folder? In the test, reads were denied until `Read` was allowed.

**Later the same day — renamed to `/techtutor`:** the plugin, skill and folder are now `techtutor` (`plugin/skills/techtutor/`, install id `techtutor@kv89-tutor`, symlink `.claude/skills/techtutor`). Skill names must be lowercase, so the command is `/techtutor`. `TUTOR.md` has a new "Starting a session" section. README has a "Start a session" table, and the ChatGPT Project instruction now handles `/techtutor`. Verified: both validations pass, a temp-config install loads skill `techtutor`, and headless `/techtutor` resumed from `tutor-library.md`. No mutation check on the start-up instructions, because that is prompt behaviour and has no code test.
- Owner says `/caveman` worked in ChatGPT web and mobile. That is unconfirmed as a real install: the word alone may explain it. Test with `what caveman intensity levels do you have?`

**Uncommitted:** all of the above, plus this file.

## 2026-10-25 — Initial release, Claude Code verified, Codex test started
**Done (verified):**
- Repo pushed to https://github.com/kv89/Tutor (`main` at `3d34f8c`, in sync with `origin/main`). 12 tracked files, none personal (checked with `git ls-files`).
- Smoke tests S1, S2, S4, S5 passed in headless Claude Code on a temp copy. S3 passed in the owner's manual run. Results are in `evaluations/test-scenarios.md`.
- README rewritten (84 lines) with clone and setup-check steps. Status definitions now live only in `TUTOR.md`.
- Personal learning data stays local: `.gitignore` excludes `knowledge/*/`, `.serena/`, `.claude/settings.json`. The three record files are `skip-worktree` locally. Backup of real data is in `../Tutor-local-backup/`.

**In progress:** Codex test. Loader check passed (it applied `AGENTS.md`, planned to read `TUTOR.md`). The "help me reason" prompt FAILED the intended behaviour: it gave the full explanation at once and saved a note without a diagnostic. This run was made in the real project folder, not a clone.

**Next:** restore the real library from `../Tutor-local-backup/` (remove `knowledge/software-engineering/payment-api-idempotency.md` and the `payment-idempotency` lines in `knowledge/index.md` and `knowledge/progress.md`). Then run the eval plan: Phase 0 spike (1 case, 3 tools, clean-config isolation), Phase 1 red test for the S1 regression, then harden `TUTOR.md`.

**Open questions / decisions for me:**
- Eval judge model (Claude only, or one per tool family)? Runs per case, N (default 3)?
- Confirm the Codex-test files in the real library should be removed.
- Packaging as an installable plugin and a Claude Desktop chat setup are parked, not planned.

**Gotchas:**
- The real library now differs from the backup because of the Codex test (two files changed, one file added). I verified this with `cmp` and `diff`, and have not restored it.
- Opening Codex or Claude in `~/Documents/Projects/Tutor` writes to the real library. Use a fresh clone in `/tmp` for tests.
- Global Claude settings load the caveman and Serena hooks, which may skew eval output.
- Pulling changes to the three `skip-worktree` files needs `git update-index --no-skip-worktree` first.
- Committed git author is `kv89 <kalaskarvishald@gmail.com>`, confirmed by the owner as correct.
- OpenCode has not been tested. Claude Desktop chat steps were researched from web sources and not tested.

**Uncommitted:** `docs/progress.md` (this file, new).
