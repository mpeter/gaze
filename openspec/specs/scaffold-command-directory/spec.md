# Scaffold command deployment

## Purpose

Define the files and ownership boundaries used by `gaze init` when deploying
OpenCode quality-reporting assets. This baseline consolidates the shipped
manifest and the command-directory migration at revision `13bee279`.

## Requirements

### Requirement: Scaffold output directory for command files

`gaze init` MUST write command files to `.opencode/commands/` (plural)
instead of `.opencode/command/` (singular). Only supported quality commands
MUST be deployed; the retired review command MUST NOT be created.

#### Scenario: Fresh project initialization

- **GIVEN** a Go project with no `.opencode/` directory
- **WHEN** the user runs `gaze init`
- **THEN** command files MUST be created under `.opencode/commands/`
  including only `gaze.md` and `gaze-fix.md`

#### Scenario: Re-initialization with existing files at old path

- **GIVEN** a Go project with command files at `.opencode/command/gaze.md`
  from a previous `gaze init` run
- **WHEN** the user runs `gaze init`
- **THEN** new command files MUST be created under `.opencode/commands/`
- **AND** the old files at `.opencode/command/` MUST NOT be modified or
  removed (migration is handled by `uf init`, not `gaze init`)

### Requirement: Tool-owned file identification

The `isToolOwned` function MUST recognize tool-owned command files under the
`commands/` prefix (plural). References, `commands/gaze-fix.md` and
`agents/gaze-test-generator.md` are tool-owned; the reporter, testing reviewer
and `commands/gaze.md` are user-owned. The retired review command MUST NOT
remain in the explicit ownership list.

#### Scenario: Tool-owned command file detection

- **GIVEN** the relative path `"commands/gaze-fix.md"`
- **WHEN** `isToolOwned` is called with this path
- **THEN** it MUST return `true`

#### Scenario: User-owned command file detection

- **GIVEN** a relative path `"commands/gaze.md"`
- **WHEN** `isToolOwned` is called with this path
- **THEN** it MUST return `false`

### Requirement: Internal cross-references

Scaffolded command files that reference other command files MUST use
the `.opencode/commands/` path (plural). No-argument `/gaze fix` MUST detect
only active OpenSpec changes, excluding `archive/`, and MUST NOT invoke the
previous numbered workflow. If several changes are active, it MUST ask which
change to use. If none is active, it MUST offer OpenSpec implementation or
batch remediation. The testing reviewer MUST read `openspec/constitution.md`
and the relevant change's proposal, design, tasks and capability specifications.

#### Scenario: gaze-fix.md cross-references

- **GIVEN** the embedded `gaze-fix.md` asset
- **WHEN** it references the implementation command
- **THEN** the path MUST be `.opencode/commands/opsx-apply.md`
- **AND** no previous workflow implementation command is referenced

#### Scenario: Multiple active changes

- **GIVEN** several active OpenSpec changes with tasks
- **WHEN** `/gaze fix` runs without arguments
- **THEN** the user is asked which change to implement

#### Scenario: No active change

- **GIVEN** no active OpenSpec change
- **WHEN** `/gaze fix` runs without arguments
- **THEN** the command offers OpenSpec implementation or batch test generation

### Requirement: Embedded asset source-of-truth test

`TestEmbeddedAssetsMatchSource` MUST verify byte-identical content between
embedded assets and live `.opencode/` files using the `commands/` directory.

#### Scenario: Embedded-to-live file comparison

- **GIVEN** embedded assets under `internal/scaffold/assets/commands/`
- **WHEN** `TestEmbeddedAssetsMatchSource` runs
- **THEN** each embedded command file MUST match its counterpart at
  `.opencode/commands/<filename>`

### Requirement: Shipped asset manifest

`gaze init` MUST deploy exactly seven files: three agents (`gaze-reporter.md`,
`gaze-test-generator.md`, `reviewer-testing.md`), two commands (`gaze.md`,
`gaze-fix.md`), and two references (`doc-scoring-model.md`, `example-report.md`).
Its closing hint MUST mention `/gaze` quality reports and `/gaze-fix` test
generation, not the retired testability command.

#### Scenario: Exact manifest

- **GIVEN** a fresh project
- **WHEN** `gaze init` runs
- **THEN** exactly those seven assets are created and reported

### Requirement: Preserve stale installed assets

`gaze init` MUST leave files outside the current embedded manifest untouched
and unreported, including already-installed copies of the retired review
command. This MUST hold both without and with `--force`.

#### Scenario: Normal initialization with a stale command

- **GIVEN** an existing retired command with locally customized bytes
- **WHEN** `gaze init` runs without `--force`
- **THEN** those bytes remain unchanged
- **AND** the stale file is absent from every result list and summary

#### Scenario: Forced initialization with a stale command

- **GIVEN** an existing retired command with locally customized bytes
- **WHEN** `gaze init --force` runs
- **THEN** those bytes remain unchanged
- **AND** the stale file is absent from every result list and summary
