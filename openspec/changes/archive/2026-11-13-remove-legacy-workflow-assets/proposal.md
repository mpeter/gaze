# Remove retired workflow assets

## Why

Fresh `gaze init` deployments still point users to the retired requirements
pipeline. Shipped guidance must agree with the single OpenSpec authority.

## What Changes

- Stop deploying the retired testability command; seven assets remain.
- Detect only active OpenSpec changes in no-argument `/gaze fix` mode.
- Point the testing reviewer at OpenSpec artifacts and governance.
- Replace the closing hint with `/gaze` and `/gaze-fix`.
- Leave already-installed retired files untouched and unreported, even with
  `--force`; initialization never deletes user files.

## Capabilities

### Modified Capabilities

- `scaffold-command-directory`: asset manifest, ownership and workflow guidance.

## Impact

Breaking removal of a shipped command. Analysis, Go APIs and JSON schemas do not
change. Add an Unreleased changelog entry, but do not bump the product version.
The constitution requires a MAJOR bump for breaking public APIs or analysis
behavior; release classification of the command removal remains a maintainer
decision at release time. Website documentation tracking is due before merge,
not this local-only handoff.

## Constitution Alignment

- Accuracy: PASS; analysis is unchanged and manifest checks are exact.
- Minimal Assumptions: PASS; preserve existing user files without deleting them.
- Actionable Output: PASS; hints and reviewer guidance name supported commands.
- Testability: PASS; temporary-directory tests cover fresh, repeat and force
  modes, stale content and result/output reporting, plus embedded-copy parity.
