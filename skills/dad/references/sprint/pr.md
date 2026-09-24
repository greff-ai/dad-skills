# Run a sprint to an open PR

Input: sprint key or current sprint branch.
Start with `references/shared/bootstrap.md`, then resolve
`references/sprint/bindings.md`. Use `references/shared/cli.md`.

1. Require a clean tree and the resolved sprint. Check required commands for
   each child against its plan; require the full commands, including available E2E, at the
   final integrated gate. If its recorded PR is already merged, report it and stop.
2. Use `dad sprint ensure-branch` with the recorded name. It leaves HEAD
   unchanged; checkout the returned branch afterward and fast-forward from
   origin. Reject a branch mismatch or divergence.
3. Create SPRINT_DIR, mirror the issue body and comments to issue.md, and append
   the working branch once to the tracker and mirror. Preserve local audit
   additions. Restore checkpoint state per `references/shared/ledger.md`;
   keep sprint records local and publish their checkpoints to the issue.
4. Run `references/container/plan.md` with the sprint bindings.
5. Run `references/container/execute.md`. Ordinary child halts are recorded
   and independent work continues. Unsafe branch/tree returns stop the run.
6. Run `references/container/merge.md`. It runs the complete integrated suite
   and any deferred checks outside it before opening or refreshing the sprint
   PR for review, and reports incomplete children. autoMerge is false.
7. Report sprint PR URL, merged/halted/blocked issues, and suite result.
   The human merges the sprint PR with a merge commit.

On rerun, retain ledger identities, add new scoped issues, retry unfinished
rows, and update the same open PR. Never create another PR for a merged head.
