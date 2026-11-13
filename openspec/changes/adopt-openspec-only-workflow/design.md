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
