## ADDED Requirements

### Requirement: Single requirements authority

The repository MUST use OpenSpec as its only active requirements pipeline.
Governance MUST reside at `openspec/constitution.md` and active work MUST use
`openspec/changes/<change>/` artifacts.

#### Scenario: Starting a change

- **GIVEN** a change requiring a planning record
- **WHEN** a contributor follows repository guidance
- **THEN** the guidance identifies OpenSpec, not a competing numbered pipeline

### Requirement: Historical task integrity

Archived strategic specifications MUST retain their content and task state,
except neutral terminology, corrected paths and appended migration evidence.
An unchecked task MUST remain unchecked unless independently proven complete.

#### Scenario: Unknown completion

- **GIVEN** an unchecked historical task without task-level completion evidence
- **WHEN** the strategic specification is archived
- **THEN** it remains unchecked and is marked unverified
