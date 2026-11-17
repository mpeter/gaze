# Design

Remove the retired embedded command and its explicit ownership entry. Keep the
existing asset-driven walker rather than add a migration or deletion mechanism.
Refresh the repository copies and embedded testing reviewer and fix command
together; keep agent tool permissions and model settings unchanged.

## Already-installed files

Leave stale files untouched and unreported in both normal and force modes. This
matches the established sibling-project decision and the existing behavior for
assets no longer in the manifest. Deleting automatically risks destroying local
customizations; renaming or moving would also mutate files no longer owned by
the current asset set. Manual removal is available to project owners.

## Coverage strategy

Use `internal/scaffold/scaffold_test.go` and CLI initialization tests with real
temporary directories. Assert all seven exact paths, output hints, results,
stale-file bytes and absence from every result category and summary. Test normal
and force modes. Keep overwrite-on-diff coverage using the retained fix command.
The fixture for the obsolete filename is composed from neutral fragments so
tracked sources contain no retired tool name. Test the shipped markdown's
OpenSpec guidance and byte-for-byte parity with repository copies.

Constitution alignment is in the proposal; existing quality gates stay intact.
