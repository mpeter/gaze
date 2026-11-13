# Scaffold command deployment

## Purpose

Define the files and ownership boundaries used by `gaze init` when deploying
OpenCode quality-reporting assets. This baseline consolidates the shipped
manifest and the command-directory migration at revision `13bee279`.

## Requirements

### Requirement: Scaffold output directory for command files

`gaze init` MUST write command files to `.opencode/commands/` (plural)
instead of `.opencode/command/` (singular).

#### Scenario: Fresh project initialization

- **GIVEN** a Go project with no `.opencode/` directory
- **WHEN** the user runs `gaze init`
- **THEN** command files MUST be created under `.opencode/commands/`
  including `gaze.md`, `gaze-fix.md` and the retired review command

#### Scenario: Re-initialization with existing files at old path

- **GIVEN** a Go project with command files at `.opencode/command/gaze.md`
  from a previous `gaze init` run
- **WHEN** the user runs `gaze init`
- **THEN** new command files MUST be created under `.opencode/commands/`
- **AND** the old files at `.opencode/command/` MUST NOT be modified or
  removed (migration is handled by `uf init`, not `gaze init`)

### Requirement: Tool-owned file identification

The `isToolOwned` function MUST recognize tool-owned command files under the
`commands/` prefix (plural). References, the retired review command,
`commands/gaze-fix.md` and `agents/gaze-test-generator.md` are tool-owned;
the reporter, testing reviewer and `commands/gaze.md` are user-owned.

#### Scenario: Tool-owned command file detection

- **GIVEN** the relative path of the retired review command under `commands/`
- **WHEN** `isToolOwned` is called with this path
- **THEN** it MUST return `true`

#### Scenario: User-owned command file detection

- **GIVEN** a relative path `"commands/gaze.md"`
- **WHEN** `isToolOwned` is called with this path
- **THEN** it MUST return `false`

### Requirement: Internal cross-references

Scaffolded command files that reference other command files MUST use
the `.opencode/commands/` path (plural). No-argument `/gaze fix` MUST
detect the previous numbered workflow or an active OpenSpec change.
The testing reviewer MUST read the constitution and relevant planning artifacts.

#### Scenario: gaze-fix.md cross-references

- **GIVEN** the embedded `gaze-fix.md` asset
- **WHEN** it references the previous implementation command
- **THEN** the path MUST be under `.opencode/commands/`
- **AND** the OpenSpec apply reference MUST be
  `.opencode/commands/opsx-apply.md`

### Requirement: Embedded asset source-of-truth test

`TestEmbeddedAssetsMatchSource` MUST verify byte-identical content between
embedded assets and live `.opencode/` files using the `commands/` directory.

#### Scenario: Embedded-to-live file comparison

- **GIVEN** embedded assets under `internal/scaffold/assets/commands/`
- **WHEN** `TestEmbeddedAssetsMatchSource` runs
- **THEN** each embedded command file MUST match its counterpart at
  `.opencode/commands/<filename>`

### Requirement: Shipped asset manifest

`gaze init` MUST deploy eight files: three agents (`gaze-reporter.md`,
`gaze-test-generator.md`, `reviewer-testing.md`), three commands (`gaze.md`,
`gaze-fix.md`, the retired review command), and two references
(`doc-scoring-model.md`, `example-report.md`). Its closing hint MUST mention
quality reports and the retired testability command.

#### Scenario: Exact manifest

- **GIVEN** a fresh project
- **WHEN** `gaze init` runs
- **THEN** exactly those eight assets are created and reported
