# gaze init

Scaffold OpenCode agent and command files into the current directory. After running this command, you can use the `/gaze` command in OpenCode to generate AI-powered quality reports.

## Synopsis

```
gaze init [flags]
```

## Arguments

This command takes no positional arguments. It always operates on the current working directory.

## Flags

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--force` | `bool` | `false` | Overwrite existing files. Without this flag, existing user-owned files are skipped. |

## Scaffolded Files

The `init` command creates exactly seven files:

```
.opencode/
├── agents/
│   ├── gaze-reporter.md          # Quality reporting agent (user-owned)
│   ├── gaze-test-generator.md    # Test generation agent (tool-owned)
│   └── reviewer-testing.md       # Testability reviewer (user-owned)
├── commands/
│   ├── gaze.md                   # /gaze command definition
│   └── gaze-fix.md               # Test generation and OpenSpec implementation
└── references/
    ├── example-report.md         # Canonical example output for the reporter
    └── doc-scoring-model.md      # Document-enhanced classification scoring model
```

### File Ownership Model

Gaze uses a mixed ownership model for scaffolded files:

- **User-owned files** (`agents/gaze-reporter.md`, `agents/reviewer-testing.md`, `commands/gaze.md`): Created once, never overwritten on subsequent `gaze init` runs (unless `--force` is used). Users can customize these files.
- **Tool-owned files** (`references/`, `agents/gaze-test-generator.md`, `commands/gaze-fix.md`): Updated automatically on `gaze init` if the content has changed. These files are maintained by Gaze and should not be manually edited.

Files outside this seven-file manifest, including previously installed retired
commands, are never deleted, modified or reported by `gaze init`, even with
`--force`. Project owners can remove stale files manually.

## Configuration Interaction

This command does not read `.gaze.yaml`.

## Examples

### Initialize OpenCode integration

```bash
cd /path/to/your/go/project
gaze init
```

```
Gaze OpenCode integration initialized:
  created: .opencode/agents/gaze-reporter.md
  created: .opencode/agents/gaze-test-generator.md
  created: .opencode/agents/reviewer-testing.md
  created: .opencode/commands/gaze-fix.md
  created: .opencode/commands/gaze.md
  created: .opencode/references/doc-scoring-model.md
  created: .opencode/references/example-report.md

Run /gaze for quality reports and /gaze-fix for test generation.
```

### Re-initialize (update tool-owned files)

```bash
gaze init
```

On subsequent runs, user-owned files are skipped and tool-owned files are updated if their content has changed.

### Force overwrite all files

```bash
gaze init --force
```

Overwrites all seven current assets, including user-customized agent prompts
and commands. Files outside the manifest remain untouched.

## See Also

- [OpenCode Integration](../../guides/opencode-integration.md) — how to use `/gaze` after initialization
- [`gaze report`](report.md) — the CLI equivalent of the `/gaze` OpenCode command
