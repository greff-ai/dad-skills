# Merge a leaf into its container

Continue the leaf flow with the same arguments and report contract from
`references/issue/single-branch/pr.md`. Commands are in
`references/shared/cli.md`. The base is the supplied container, never
MAIN_BRANCH. Require a clean tree; preserve merge and archive evidence on retry.

1. Read the known PR before mutating it. Require expected head/base.
   Merged: finish `references/shared/ledger.md` bookkeeping recovery and
   return done. Closed without merge: CONFLICT. If no PR is recorded, recover
   it from ISSUE_DIR/pr.md; open only when no prior PR is known and the
   published child still exists.
2. Fetch origin. Check for upstream commits incorrectly carried by the child:
   intersect `git rev-list origin/<container>..HEAD` with commits reachable
   from origin's MAIN_BRANCH or container-parent branch but not
   origin/<container>. A nonempty intersection is CONFLICT. This guard checks
   unexpected upstream code; it does not claim to prove historical branch origin.
3. Merge origin/<container> into the child. On conflict, abort this merge,
   report the paths, and halt for manual resolution. Do not rebase, force-push,
   or auto-resolve conflicts. Confirm the base is now an ancestor.
4. Run the full suite in a worker with gate merge and fix yes. Allow at most
   three rounds, reading earlier fix logs before another attempt.
   Minor in-scope failures may use the fix chain. A change to intended behavior
   or design is a major failure: append Merge issues, move the card to active,
   and halt. Never silently expand the issue to make tests pass.
5. After repairs, reconcile artifacts and verify again before proceeding.
   Refresh the open PR's test summary and changed evidence after the final
   suite, preserving existing attachment URLs per the shared evidence guide.
   Record merge-test only for a passing full suite on the current code/base.
   On retry, rerun whenever the base/code changed or the prior tested tree is
   uncertain.
6. Archive the change on this branch, in this context, through
   `references/shared/openspec-flow.md`. An already archived change must
   have exactly one matching archive and no active directory. Do not archive
   through incomplete-task or sync warnings.
7. Commit/push the archive first; use that full commit ID to form absolute
   proposal/design/tasks URLs as specified in `references/shared/pr-body.md`.
   Append them once to ISSUE_DIR/pr.md, ISSUE_DIR/issue.md, the PR body, and
   issue body. Read both remote bodies to avoid duplicating history. Local
   link mirrors remain ignored. Publish the archive checkpoint only
   after all four agree; on retry reuse valid published links already present.
8. Re-read the PR; require the same head/base. Squash with `dad pr merge`.
   Honor branch protection failures verbatim. If it already merged, recover
   bookkeeping. Otherwise retain its merge commit and mark checkpoint merge.
9. Append `Merged in PR <url>` to the tracker issue and move it to done.
   Checkout the container and fast-forward from origin. Verify the merge
   commit is reachable there. Append the merged-PR line to the local issue
   mirror if absent, then publish the done checkpoint without a bookkeeping commit.
10. Remove the local child branch only after its merged PR and clean return
    are confirmed; do not force-delete a branch with newer unpublished commits.
    Failure to remove a safely retained branch is a warning. Report done,
    the PR, merge commit, archive, test evidence, and any warnings.

## Failed merge

Append a concise Merge issues section to the local PR file and remote PR
without replacing prior sections; move the issue back to active.
Preserve local reports and publish the failed checkpoint. Commit only product
and OpenSpec work on the child branch, then return clean
to the container with the last completed checkpoint. A failure after the
squash keeps checkpoint merge and the PR number, even if later bookkeeping
fails. Never reopen or recreate that PR.
