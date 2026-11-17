# Implementation Plan: GazeCRAP Data in Report Pipeline

**Branch**: `022-report-gazecrap-pipeline` | **Date**: 2026-03-17 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/openspec/changes/archive/2026-03-17-report-gazecrap-pipeline/spec.md`

## Summary

Wire the `ContractCoverageFunc` callback into the `gaze report` pipeline so that GazeCRAP scores, quadrant distribution, and GazeCRAPload appear in CI reports. The `gaze crap` subcommand already computes this data, but the `gaze report` internal pipeline (in `internal/aireport`) skips it entirely — `runCRAPStep` constructs `crap.Options` without setting `ContractCoverageFunc`, resulting in null GazeCRAP fields and a silently-passing `--max-gaze-crapload` threshold.

The fix extracts `buildContractCoverageFunc` from `cmd/gaze/main.go` to `internal/crap/contract.go`, then calls it in `runProductionPipeline` before the CRAP step, passing the resulting callback to `runCRAPStep`.

## Technical Context

**Language/Version**: Go 1.25+  
**Primary Dependencies**: `golang.org/x/tools` (SSA builder), Cobra (CLI)  
**Storage**: N/A (no persistence changes)  
**Testing**: Standard library `testing` package only. `-race -count=1` required.  
**Target Platform**: darwin/linux (amd64/arm64)  
**Project Type**: Single CLI binary  
**Performance Goals**: Report pipeline completes within existing CI timeout (20m). Quality analysis runs twice (once for callback, once for quality JSON); ~2x quality runtime but quality is < 30% of total.  
**Constraints**: No new external dependencies. No new CLI flags. Existing `--coverprofile` behavior preserved.  
**Scale/Scope**: Affects 4 files in `internal/aireport`, 2 files in `internal/crap`, 1 file in `cmd/gaze`. ~150 lines of code movement + ~50 lines of new plumbing.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

### I. Accuracy — PASS

The change ensures `gaze report` produces the same GazeCRAP scores as `gaze crap` (SC-002 requires exact match). The `ContractCoverageFunc` callback uses identical logic — extracted from `cmd/gaze/main.go` to `internal/crap/contract.go` with no behavioral changes. No false positives or false negatives are introduced.

### II. Minimal Assumptions — PASS

No new assumptions about host projects. The contract coverage callback is best-effort (returns nil if quality analysis fails for all packages), preserving the existing graceful degradation. No new CLI flags or configuration required.

### III. Actionable Output — PASS

The change adds GazeCRAP data (quadrant distribution, per-function GazeCRAP scores, GazeCRAPload count, fix strategies informed by Q3 classification) to the report payload. This makes the AI-formatted report more actionable — it can now distinguish between "needs tests" (line coverage gap) and "needs assertions" (contract coverage gap).

### IV. Testability — PASS

Coverage strategy defined in research.md R6:
- Unit: extracted `BuildContractCoverageFunc` tested with fixture packages
- Unit: `pipeline_internal_test.go` mocks updated for new `crapStep` signature
- Integration: `TestRunReport_JSONFormat_ValidOutput` asserts `gaze_crapload` presence
- Existing `buildContractCoverageFunc` tests in `cmd/gaze/main_test.go` adapted for new location

No untestable code introduced. All new functions accept explicit parameters (no global state).

### Post-Design Re-check — PASS

Design decisions (R1-R6 in research.md) maintain all four principles. The quality analysis redundancy (R5) is a performance trade-off, not a correctness issue, and can be optimized in a future spec.

## Project Structure

### Documentation (this feature)

```text
openspec/changes/archive/2026-03-17-report-gazecrap-pipeline/
├── plan.md              # This file
├── research.md          # Phase 0 output — 6 research decisions
├── data-model.md        # Phase 1 output — entity modifications
├── quickstart.md        # Phase 1 output — before/after examples
├── checklists/
│   └── requirements.md  # Spec quality checklist (all pass)
└── tasks.md             # Phase 2 output (created by retired task-generation command)
```

### Source Code (repository root)

```text
internal/
  crap/
    contract.go          # NEW — extracted BuildContractCoverageFunc
    contract_test.go     # NEW — unit tests for BuildContractCoverageFunc
    analyze.go           # UNCHANGED (Options struct already has ContractCoverageFunc)
  aireport/
    runner.go            # MODIFIED — runProductionPipeline calls BuildContractCoverageFunc
    runner_steps.go      # MODIFIED — runCRAPStep accepts ContractCoverageFunc parameter
    pipeline_internal_test.go  # MODIFIED — updated mocks for new crapStep signature

cmd/gaze/
  main.go               # MODIFIED — buildContractCoverageFunc replaced with call to crap.BuildContractCoverageFunc
  main_test.go           # MODIFIED — tests updated to use new function location
```

**Structure Decision**: No new packages. The extracted function moves to `internal/crap/contract.go` because its return type (`ContractCoverageInfo`) is defined in that package. The `internal/aireport` package imports `internal/crap` (existing dependency).

## Complexity Tracking

No constitution violations to justify. All gates pass.
