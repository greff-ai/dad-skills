# PR bodies

Every PR uses `dad pr open` with its own issue key; the CLI emits the tracker
linking form. Use title-file and body-file in `references/shared/cli.md`.
Never interpolate titles or prose into shell code.

Links to committed OpenSpec/code files use the forge's absolute blob/tree URLs
pinned to a published commit containing the target. Relative paths do not
resolve as repository links there; child branch names may be deleted. Keep
directory identifiers repo-relative. Use the same absolute links in local
mirrors. For merged children, pin links to their reachable merge commit.
Ignored sprint files have no blob URLs: use issue/checkpoint URLs and uploaded
evidence URLs instead. Never commit local reports merely to make them linkable.

Every PR description has `## Test results`: tested revision/environment,
commands and pass/fail/skip counts, limitations, and specific added regression
tests with what each proves. A passing total alone is insufficient when new
tests were added. Use `references/shared/evidence.md` when review evidence was
captured; with none, publish only the summary. Do not generate a PDF.
Child PRs compare actual results with the current step test plan and list
deferred checks separately. Sprint PRs list each deferred check and the final
suite or explicit final check that covered it; none may remain uncovered.

## Leaf

Write to `{ISSUE_DIR}/pr.md`:

- `## Summary`: 2-5 sentences describing the result.
- `## Changes`: short behavior/file bullets.
- `## Openspec change`: change name, without links to movable active artifacts.
- `Issue directory:` with the local working directory, not a clickable artifact.

At merge, append `## Archived openspec change` with commit-pinned links to
proposal.md, design.md, and tasks.md. A failed merge appends `## Merge issues`
with the failure and attempted repairs. Preserve these sections on retries.

## Container

Write to `<container-dir>/pr.md`:

- `## Summary`: 2-5 sentences.
- `## Issues`: table `Issue | Title | Type | PR | Archived change`, with
  merged children. Lead with user-visible changes. Include internal items but
  mark their type internal when `issueTypes.<type>.releaseNotes` is false.
  A nested container links its published checkpoint and its leaves' archives.
- `## Not in this PR`: unfinished children and halt reasons.
- `## Merge instructions`, only when the binding requests it: ask the human
  to use a merge commit so issue commits survive on the main branch.
- `Suite:` tiers, result, and failing summary if red.
- `Container directory:` local working path, not a clickable artifact.

Reopening an existing head with `dad pr open` updates its title/body.
Regenerate only an open PR, preserving prior audit additions from the local
mirror and remote body, including uploaded evidence URLs. After publishing
attachments, refresh the local body from the remote before any later update;
never replace uploaded URLs with local paths or discard earlier evidence.
Append merge-time history; never rewrite a merged PR.
A base mismatch is CONFLICT, not permission to retarget.
