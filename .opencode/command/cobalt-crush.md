---
description: >
  Invoke the Cobalt-Crush developer persona for implementation tasks.
  With arguments: delegates to the cobalt-crush-dev agent. Without
  arguments: selects an active OpenSpec change and runs /opsx-apply.
---
<!-- scaffolded by uf vdev -->
<!-- scaffolded by uf vdev -->
<!-- scaffolded by uf vdev -->
<!-- scaffolded by uf vv0.6.1 -->

# Command: /cobalt-crush

## Description

Invoke the Cobalt-Crush developer persona to implement code with
clean code principles, convention pack adherence, and feedback loop
integration.

## User Input

```text
$ARGUMENTS
```

## Usage

```
/cobalt-crush [task description or instructions]
/cobalt-crush                    # select active OpenSpec change and implement
```

### Examples

```
/cobalt-crush implement T015 from tasks.md
/cobalt-crush fix the failing test in internal/metrics/store_test.go
/cobalt-crush refactor runMetrics to reduce complexity
/cobalt-crush                    # detects active OpenSpec context
```

## Instructions

### When arguments are provided

Delegate to the `cobalt-crush-dev` agent using the Task tool with
`subagent_type: "cobalt-crush-dev"`. Pass the user's arguments as
the task prompt. The agent will:

1. Read AGENTS.md, constitution, convention packs, and active specs
2. Apply its engineering philosophy (clean code, SOLID, TDD awareness)
3. Load convention packs from `.opencode/uf/packs/`
4. Check `.uf/artifacts/` for Gaze/Divisor feedback
5. Execute the requested task following project conventions
6. Document design decisions in code comments

### When no arguments are provided

Select the active OpenSpec change and delegate implementation
to the `cobalt-crush-dev` agent:

1. **Check for an OpenSpec active change**: Look for directories
   under `openspec/changes/` that contain a `tasks.md` file,
   excluding `archive/`. If several exist, use the branch-matching
   change or ask the user to select one; never choose arbitrarily.
   **Validate branch**: Run `git rev-parse --abbrev-ref HEAD`.
   The current branch must be `opsx/<change-name>` where
   `<change-name>` matches the detected change directory name.
   If not on the correct branch, **STOP** with error:
   > "OpenSpec change `<name>` detected but you are on branch
   > `<current-branch>`. Run: `git checkout opsx/<name>`"

   If on the correct branch, read the full contents of
   `.opencode/command/opsx-apply.md` and delegate it to the
   `cobalt-crush-dev` agent via the Task tool — pass the command
   file's instructions as the agent's prompt so the agent
   executes the apply workflow.

2. **If no change is detected**: Stop and recommend `/opsx-propose`
   to create an OpenSpec change before implementation.

## Branch Safety Guardrails

**CRITICAL**: Before switching branches or suggesting a branch
switch for any reason:

1. Run `git status --short` to check for uncommitted changes.
2. If uncommitted changes exist, **STOP** and warn the user.
   Show the list of uncommitted files and ask for confirmation
   before proceeding.
3. Never silently switch branches with a dirty working tree --
   uncommitted changes may follow to the wrong branch or be
   lost entirely.
4. When implementation is complete, all changes MUST be
   committed and pushed on the current branch before suggesting
   any branch switch, PR creation, or archiving.
