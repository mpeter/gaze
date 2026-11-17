# Adopt one requirements workflow

## Why

The repository contains two requirements pipelines. OpenSpec will be the only
active authority, with historical strategic specifications retained as archives.
The owner authorized this migration and explicitly requires local commits only.

## What Changes

- Move the constitution to `openspec/constitution.md` and update its references.
- Archive the numbered strategic specifications with provenance and unchanged
  checkbox state; unresolved work remains visibly unverified.
- Retire the previous commands, scripts, templates and workflow skill.
- Reword retained historical references neutrally without erasing their meaning.
- Repair existing OpenSpec artifact structure without changing requirements.

Shipped assets are handled separately by `remove-legacy-workflow-assets`.

## Capabilities

### New Capabilities

- `repository-workflow`: one requirements authority and traceable history.

## Impact

Repository guidance, governance location and historical planning artifacts.
No analysis behavior or version changes.

## Constitution Alignment

- Accuracy: PASS; no analysis changes and historical completion is not inferred.
- Minimal Assumptions: PASS; preserve unknown task state explicitly.
- Actionable Output: PASS; all active instructions point at one workflow.
- Testability: PASS; validate artifacts and run repository gates after integration.
