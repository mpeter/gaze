# Changelog

## Unreleased

### Breaking Changes

- `gaze init` no longer ships the retired standalone testability command; it
  deploys seven supported assets. Existing retired files remain untouched and
  unreported, including with `--force`; project owners can remove them manually.
- No-argument `/gaze fix` selects only active OpenSpec changes. Testing reviewer
  guidance now uses OpenSpec governance and change artifacts. Initialization
  hints point to `/gaze` quality reports and `/gaze-fix` test generation.
