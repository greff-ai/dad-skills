# Container ledger

`<container-dir>/tasks.md` is a local working copy; issue checkpoints are the
durable resume point. Keep its format and row identity stable across runs.
Paths identify local files, not repository links. Existing ledgers
using an em dash as the title separator remain valid.

```markdown
# Ledger — <title>

container: <type> <key> — <title> (<url>)
bindings: <reference path>
branch: <container branch>
parent: <parent branch>
directory: <container-dir>
created: <YYYY-MM-DD>
pr: none
updated: <YYYY-MM-DD>

## Rows

- [ ] 1. <key> — <title>
  type: <type>
  size: single-branch
  branch: <branch>
  dir: <container-dir>/issues/<branch with / replaced by ->
  depends on: none
  state: pending
  sub-step: none
  pr: none
  archived: none
  halt: none
  warnings: none
```

All keys are required. Header identity must match the active bindings.
PR values are `<number> <url>` or none. Preserve older `? <url>` recovery
records; use the URL to view that PR when needed. Never infer completion from
an archive path alone: it may have arrived in an unrelated merge.

- Append rows, numbered from 1; never remove or renumber them.
- State is pending, running, done, halted, or `blocked by <keys>`.
  Only done is checked. Unknown states or invalid size halt the affected row
  with CONFLICT and are not dispatched until repaired.
- Size is always single-branch or multi-branch. A binding that disallows
  containers accepts only single-branch children.
- Dependencies name earlier row keys. Explicit `none` means independent;
  only an omitted plan entry uses the binding default. Reject missing,
  self, or forward references and cycles.
- A retry preserves sub-step, PR, archive, warnings, and halt. Blocking a
  previously halted row does not erase its resume evidence.
- Publish changed state before dispatch using the checkpoint protocol below.
  Sprint ledgers, mirrors, logs, and evidence stay ignored; never force-add them.
  Product code and OpenSpec artifacts still use scoped commits and pushes.

## Publish and restore

Post a `## dad checkpoint` comment on the container issue containing the entire
current ledger in a fenced Markdown block. Use `dad issue comment` with a body
file via `references/shared/cli.md`. Skip an identical latest checkpoint.
Record the returned comment URL locally. Failure preserves local state and
stops dispatch or merge; never treat an unpublished checkpoint as durable.

Each child similarly checkpoints its own issue after every completed token and
on halt: issue, branch, parent, directory, change slug, sub-step, PR, archive,
warnings, halt, test/evidence URLs, and significant fix findings/decisions.
Preserve prior comments. Before retrying a failed write, read comments and reuse
an identical checkpoint already posted.

On resume, read the container and child issues with comments. Restore missing
local files from the latest matching checkpoints and current issue/PR bodies;
an older parent row may be advanced only with verified child checkpoint evidence.
Validate issue/branch/parent identity and chronology; conflicting records halt
CONFLICT. No published checkpoint means re-plan only for genuinely fresh work,
not existing branches. Never reset a lost ledger to pending or rely on ignored
files arriving in a checkout/merge. Container completion is proven by its remote
all-done checkpoint, child PRs, and reachable merge commits, not a local path.

## Checkpoints

Leaf tokens: branch, propose, apply, test, reconcile, verify, pr, merge-test,
archive, merge, done. Container-child tokens: plan, execute, merge-test, merge,
done. None means no completed checkpoint.

Leaf evidence: branch exists; propose has committed proposal/design/tasks/specs;
apply and later have completed tasks; archive has exactly one matching archive
and no active change; merge has a confirmed merged PR with the expected head
and base. A checkpoint is the last completed step, not the step that failed.
Resume after it, verifying its evidence first. Re-run tests after code or base
changes; do not treat an old test checkpoint as proof for a new tree.

## Child report

```text
issue: <key> <url>
branch: <child> <- <container>
pr: <number> <url> | none
archived: <archive path, container checkpoint comment URL, or none>
sub-step: <last completed token or none>
warnings: <details or none>
files written: <paths or none>
result: pass | fail
halt: <CODE>: <reason, only on failure; last line>
```

A child returns on the container branch with a clean tree.
Success requires done plus confirmed merge evidence; a failed report updates
the row and leaves it unchecked. Test and verification details may precede
this block.

## Interrupted merge recovery

For running rows or a retry with merge evidence, find the PR from the row,
the child's issue checkpoint, pr.md, the nested ledger header, or the issue's
Merged in PR / Issue PR line.
Use `dad pr view`; confirm head, base, merged state, and a mergeCommit
reachable from origin's container branch before marking done.
A leaf also needs its unique archive on that branch; a container needs its
published nested checkpoint with all children done.

Finish missing issue bookkeeping and the done-column move, then record done.
No branch recreation and no new PR. If a known PR is closed without merging,
halt CONFLICT. If proof is absent, record an interrupted halt and resume the
child using existing checkpoint evidence; never guess that it merged.
