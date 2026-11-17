# Design

OpenSpec owns current requirements. Historical strategic specifications move to
dated archive directories, retaining original prose except neutral terminology
and updated paths. Each archive receives a migration evidence section naming the
source revision, original feature identifier and any matching merged upstream PR.
No old task is newly marked complete; unchecked tasks gain an unverified note.

Constitution alignment is assessed in the proposal. The governance relocation
clarifies the selected workflow without weakening its four principles, review
gates or release rule. The owner overrides remote-push requirements for this task.

Verification: compare historical checkbox sequences and retained documents with
the base revision, validate OpenSpec, inspect tracked names and content, and run
Go build/vet/race tests and available CI lint/quality gates at committed HEAD.

## Initial inventory

At `13bee279`, 175 tracked files belonged to the previous workflow paths or
shipped command; 432 content hits were spread across command/agent guidance,
150 strategic documents, embedded assets, OpenSpec archives, docs and config.
The fork has zero PRs; evidence was collected from 178 upstream PRs instead.
Local branches were `main` and this task branch. Remote numbered branches
`035-fix-ambiguous-classification` and `036-fix-missing-effects` correspond to
merged upstream PRs #82 and #83, respectively. These implement product features,
not workflow-only work; they are unchanged. No fetched branch is exclusively
the retired workflow's implementation. Upstream PR #93 (`038-platform-scaffold`)
is closed and unmerged; #318 (`chore/spec040-retrospective-learnings`) is open
and belongs to newer upstream work outside this fork's base. No remote state
was deleted or modified.

Workflow-only upstream records include merged PR #52 (spec-first guardrails)
and merged PR #288 (historical artifact immutability). Neither has a fetched
branch here. The open retrospective PR #318 is reported only, not modified.

The already-completed command-directory migration was archived using
`--skip-specs`: upstream PR #127 proves it shipped, and its 24 pre-existing
completed tasks are unchanged. Reapplying its historical deltas after the
product retirement would replace newer scenarios; skipping spec updates
preserves the current canonical capability. No `--no-validate` flag was used;
repository-wide validation remains required.

## Historical integrity check

All 150 numbered documents moved into 24 dated archives; the 127 existing
archive documents remain in place. Their 1,336 checked and 35 unchecked boxes
retain exact per-file state and order. Every unchecked item is explicitly
unverified; no historical box was newly checked. The open items are in the
classification-document consolidation (9), project documentation (8), coverage
reason guidance (14), target warning guidance (2), and the two CI report
archives (1 each). Original requirement/scenario and governance tokens and
document line counts are retained. Forty-nine separate migration records carry
source revision, original dates, PR evidence and limits of that evidence.
