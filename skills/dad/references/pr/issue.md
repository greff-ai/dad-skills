# Run one issue to a main-branch PR

Input: one non-sprint issue key. Run in the coordinating conversation after
`references/shared/bootstrap.md`; commands are in `references/shared/cli.md`.

1. View the issue with comments. Require it open, not type sprint, with no
   parent; a sub-issue is OUT_OF_SCOPE naming its parent. Restore its latest
   checkpoint per `references/shared/ledger.md`; a parent other than
   MAIN_BRANCH is CONFLICT naming that container. With no checkpoint, ask
   before branching if the issue has a tracker sprint value.
2. A recorded merged PR: append `Merged in PR <url>` once, move the issue to
   done, report, and stop. Closed unmerged is CONFLICT. Never recreate a
   branch or PR.
3. Size comes from the issue's recommendation, else single-branch with a
   warning. Fresh work picks a three-word kebab slug and uses `dad branch name`;
   an existing branch without a matching checkpoint is CONFLICT. ISSUE_DIR is
   WORK_DIR/sprint/issues/ plus the branch with slashes replaced by hyphens.
4. Before branching, append this issue's test plan once using
   `references/shared/test-plan.md`, keeping operator-approved revisions. Its
   final gate is the complete configured suite, including E2E when available;
   deferrals move only to that gate.
5. Single-branch: run `references/issue/single-branch/pr.md` with base
   MAIN_BRANCH, then Review below. Multi-branch: run
   `references/issue/multi-branch/bindings.md` with its main-parented bindings.

Reruns resume after the last verified checkpoint, reusing branch and PR.

## Review

Stay in the leaf context. Require a clean issue branch and its open PR with
expected head and base MAIN_BRANCH.

1. Fetch and merge origin's MAIN_BRANCH into the branch. On conflict abort,
   report the paths, and halt.
2. Reassess scope against the plan. Run tier full with gate merge and fix yes
   via `references/shared/test.md`, plus deferred checks outside it; at most
   three rounds. Changed intent is a major failure: append Merge issues, move
   the issue to active, and halt. Reconcile and verify repairs per
   `references/shared/openspec-flow.md`. Record merge-test only when green.
   Still red: TEST_FAILED with the open PR for inspection.
3. Archive per `references/shared/openspec-flow.md`; commit and push. Append
   commit-pinned archive links once to both local mirrors and both remote
   bodies, reusing valid links on retry. Record archive.
4. Refresh the PR's test results and evidence via
   `references/shared/pr-body.md` and `references/shared/evidence.md`. Move
   the issue to review, publish the review checkpoint, and report the PR.
   A human merges it; never merge MAIN_BRANCH.
