# Finish a container

Inputs: bootstrap facts and resolved bindings. Use
`references/shared/ledger.md`, `references/shared/cli.md`, and
`references/shared/pr-body.md`.

1. Require a clean container branch and a valid ledger. If the header records
   a PR, view it first and check head/base. If already merged, perform only
   bookkeeping recovery and return done. A closed, unmerged PR is CONFLICT.
2. Require no pending/running rows; execute them first. If autoMerge is true,
   every row must be done before opening the container PR. Report incomplete
   children as a halt to the parent, never merge a partial issue.
   A sprint may open with halted/blocked rows listed as not in this PR.
3. Fetch and merge the parent branch into the container. Abort on conflicts
   and report them. Do not force, rebase, or resolve conflicts automatically.
4. For a sprint (`autoMerge: false`), collect outstanding deferred checks from
   every current row plan and result, then run the complete configured suite,
   including E2E when the project has it, on the integrated branch before opening/refreshing its PR
   for human review. Run every deferred check outside that suite explicitly;
   keep uncovered deferrals visible and halt if any remain. For an issue
   container (`autoMerge: true`), reassess combined scope against its parent's
   current test plan, apply the higher default after expansion unless a revised
   plan is operator-approved, and run planned child merge checks, not an
   unconditional full suite.
   Use `references/shared/test.md` with fix no and apply
   `references/shared/subagents.md` return checks. Code repairs must be new
   issue work, never direct edits to a container branch.
5. Generate pr.md using the body reference, current ledger, child issue
   titles, releaseNotes settings, and archive links. Preserve previous audit
   additions; include each step's planned-versus-actual results, deferred
   checks and their final coverage, the exact gate result, and new
   regression-test coverage.
   Keep this local body ignored; push only product/OpenSpec changes if needed.
6. Open/update the PR with its own container issue, expected head and parent
   base, title-file, and body-file. Record the number/URL in the ledger header.
   Append the binding's container-pr-line once to the tracker and local mirror.
   Publish evidence when present via `references/shared/evidence.md`, then
   publish the updated container checkpoint. Do not commit bookkeeping.
7. Red gate: report TEST_FAILED with the open PR for failure inspection and
   stop; it is not ready for human merge review. autoMerge false with the
   complete green final gate: report the PR for human review and stop.
   Never merge MAIN_BRANCH.
8. For autoMerge true, recheck all rows done, planned gate green, expected PR head/base,
   and parent not MAIN_BRANCH. Squash with `dad pr merge`, preserving its
   returned PR and merge commit before any later bookkeeping. Honor refusals.
9. Move the container issue to done. Checkout the parent, fast-forward from
   origin, and confirm the merge commit is reachable. Delete the local child
   branch only when no newer unpublished work would be lost. Return done,
   PR, and the published all-done checkpoint URL as evidence for the parent row.

Any halt keeps the last completed token: merge-test after the suite passes,
merge after the squash. The issue-container caller preserves its work and
returns to the parent branch; the sprint caller reports in the conversation.
