# Run a single-branch issue

Called by `references/container/execute.md`. Inputs: bootstrap facts, issue key,
branch, ISSUE_DIR, container branch, container's parent branch, and on retry
the halt, last completed sub-step, and known PR. Complete this file and
`references/issue/single-branch/merge.md` in the same context.

Use `references/shared/cli.md` for commands,
`references/shared/subagents.md` for workers, and
`references/shared/ledger.md` for checkpoints and the final report.
Publish each completed checkpoint and halt to this issue. Restore from its
latest matching comment on retry; a local mirror is never required on origin.

## Start or resume

1. Require a clean tree on the container branch, which must not be MAIN_BRANCH.
   Read required settings paths.openspec and the step's published test plan
   using `references/shared/test-plan.md`. Require the commands/coverage named
   for this child's checks before work; the sprint's full tier is checked by
   its final gate. Preserve explicit operator-approved deferrals on resume.
2. Parse the supplied branch and view the issue. Its key (case-insensitive)
   and type must match. For a fresh row require an open issue not already done.
   The ledger owns sprint membership; a differing tracker sprint is a warning.
3. Before checkout or PR creation, inspect any recorded PR or one recorded in
   ISSUE_DIR/pr.md. A merged PR takes the bookkeeping recovery in
   `references/shared/ledger.md`, even if the recorded token predates merge.
   A merge token without a verifiable PR is CONFLICT.
4. Fetch origin. Require the container branch and running parent checkpoint
   to be published; refresh ISSUE_DIR/issue.md locally. Fast-forward a behind local container;
   ahead/diverged is CONFLICT for this child.
5. Fresh row: refuse an existing local/remote child branch or colliding active/
   archived change. Create the child from origin's container branch with
   `git switch --no-track -c <child> origin/<container>`. Publish explicitly
   with `git push -u origin <child>`; never inherit the parent's upstream.
   Retry: checkout the existing child, fast-forward from origin if behind;
   divergence halts. A missing branch is creatable only before the branch
   checkpoint. Check the checkpoint evidence before skipping any completed work.
6. Move the issue to active and append its working branch once to the tracker
   and local mirror. Publish the branch checkpoint; no mirror-only commit.

## Implement

Continue after the last completed checkpoint; a retry must not propose or
apply from scratch when their evidence exists.

1. Propose in a worker with ISSUE_DIR/issue.md as seed and the parsed slug as
   change name. Follow `references/shared/openspec-flow.md`; unanswered
   questions halt. Check branch/cleanliness and committed artifacts.
2. Apply in a separate worker. Check that all change tasks are complete.
3. Test in a worker using `references/shared/test.md`, tier
   planned-implementation, fix yes. Record results against the current plan.
4. Reconcile the actual implementation and fix records into the change's
   proposal/design/specs/tasks. Commit any corrections.
5. Verify in a read-only worker. On critical findings run
   `references/issue/single-branch/fix.md` in a worker with gate verify,
   level unit, and the findings. Reconcile, then verify again. At most three
   repair rounds; report warnings and stop with VERIFY_FAILED if still red.
6. Append deviations from the original issue to both issue surfaces (none is
   valid), preserving earlier history. Write the PR body using
   `references/shared/pr-body.md`, retaining prior audit sections.
7. Commit and push the child. A failed push leaves the commit intact and
   reports the branch as local-only. Open/update its PR against the container
   using title-file/body-file and the issue key. Store `PR: <number> <url>`
   locally and in the issue checkpoint before merging, without a bookkeeping commit.
8. Publish captured evidence via `references/shared/evidence.md`; preserve
   its remote URLs in the local PR mirror and issue checkpoint. Pending or
   failed publication halts here. With no captures, no attachment is required. Move the
   issue to review; PR checkpoint complete. Follow
   `references/issue/single-branch/merge.md` now.

## Halt

After creating/checking out the child, commit only this flow's work in progress
on that branch, excluding ignored sprint records; push best-effort and return
to the container branch. Preserve
all work and report a failed push as a warning. Never stash, reset, or discard
files to pass a return check. If the tree cannot safely be made clean, report
DIRTY_TREE and let orchestration stop.

Return the shared ledger report with the last completed checkpoint, PR,
archive, warnings, and exact halt. A merged child uses bookkeeping recovery;
it must never recreate a deleted branch.
