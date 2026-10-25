# Manual acceptance scenarios

Run these manually in Codex, Claude Code, and OpenCode. Use disposable prompt examples; do not alter the learner's progress records during evaluation unless the learner requests a real lesson. For write scenarios, use an identified sample concept and inspect the resulting files. Record observed results, not assumptions.

## Teaching behavior

1. **Graduated hints:** Ask for help solving a small programming problem without requesting its answer. The tutor should infer current understanding, start with a useful hint, and escalate only as needed.
2. **Misconception repair:** State a persistent misconception. The tutor should identify the faulty model, explain its limit, use a materially different strategy, and check whether the correction helped.
3. **Answer disclosure:** Explicitly request the full answer. The tutor should provide it with reasoning instead of continuing to withhold it.
4. **AI depth:** Ask how transformer attention works and when it is useful. Check for an accurate mechanism, practical example, limitations, and separation of facts from assumptions.
5. **Engineering parity:** Ask about a conventional engineering topic such as database indexes or API idempotency. Check for comparable rigor, including trade-offs and failure cases.
6. **Novel transfer:** After an explanation, ask the learner to solve or explain a new related case. The response should count as evidence, not mere exposure.

## Knowledge library

7. **Create after teaching:** Teach a substantial concept. Check that a categorized reference, index entry, and progress record are created with useful content.
8. **Update without duplication:** Continue a concept already in the library. Check that its reference is updated without a duplicate file or index entry.
9. **Navigation integrity:** Inspect links after a create or update. Check that paths exist, categories fit, and each concept appears once in the index.
10. **Quick revision:** Request interview or exam revision. Check that only relevant references and progress evidence are retrieved, not transcripts or the entire library.
11. **Mastery evidence:** Discuss a concept without attempting novel application. Check that status remains Introduced, Practising, Understood, or unverified as warranted; it must not become Demonstrated mastery without independent transfer evidence.
12. **Learning gaps:** Record a missed prerequisite or recurring misconception, then ask a related follow-up. Check that the record informs a useful explanation or exercise.
13. **Unavailable writes:** In a read-only environment, request a meaningful lesson. Check that the tutor states the write limitation and provides proposed note content without claiming an update.

## Tool loading and portability

14. **Instruction and skill discovery:** In Codex, confirm project `AGENTS.md` is loaded and directs tutoring to `TUTOR.md`. In Claude Code and OpenCode, confirm the `technical-tutor` skill is discoverable. Activate tutoring and confirm `TUTOR.md` and only relevant knowledge files are read. Verify the Claude `CLAUDE.md` import and each tool's behavior in its running environment.
15. **No infrastructure:** Confirm the workflows use local Markdown and available file tools, without packages, MCP, a database, or external services.

## Smoke tests

Quick end-to-end check of teaching and saving. Run in one tool, in order, from this folder. Use a copy of the project to keep real progress clean. Inspect the files on disk; a claim of success is not evidence.

| ID | Prompt | Expect |
|---|---|---|
| S1 | `Help me reason about why a database index can speed up a lookup. Don't give me the full explanation yet.` | A diagnostic or first hint, no full explanation. |
| S2 | `Now give me the complete explanation of how a database index speeds up lookup, including the trade-offs.` | A full, reasoned answer. Note, index and progress updated; status stays Introduced. |
| S3 | `Teach me database indexes, starting with what I know. Use hints and examples as useful, then save a concise concept reference and my demonstrated understanding or gaps.` | One categorized note, one index entry, one progress row. |
| S4 | `Continue our database-index lesson. Add one useful comparison with a full table scan and update the existing note; do not create another concept file.` | Existing note updated; no second file; one index entry. |
| S5 | In a new session: `Use the saved database-index reference to quiz me with three active-recall questions. Don't show the answers until I try.` | Questions come from the saved note; answers withheld. |

## Results

Record observed results only. Blank means not run.

| Test | Tool | Date | Result | Notes |
|---|---|---|---|---|
| S1 | Claude Code (headless) | 2026-10-25 | Pass | Sorted-list diagnostic, full explanation withheld. |
| S2 | Claude Code (headless) | 2026-10-25 | Pass | Full answer; arithmetic correct; Postgres collation claim flagged unverified; note and progress updated; status Introduced. |
| S3 | Claude Code (manual, project owner) | 2026-10-25 | Pass | Note created at `knowledge/software-engineering/database-indexes.md`. Not independently observed. |
| S4 | Claude Code (headless) | 2026-10-25 | Pass | One concept file, one index entry; Comparisons section added; break-even figure checked by hand. |
| S5 | Claude Code (headless) | 2026-10-25 | Pass | Three questions from the note, answers withheld. |
| Scenarios 1–15 | Codex | | | |
| Scenarios 1–15 | OpenCode | | | |
| Scenarios 2, 6, 11, 12, 13 | Claude Code | | | Learner-answer scenarios; not run. |
