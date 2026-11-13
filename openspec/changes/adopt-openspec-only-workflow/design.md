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
