# Per-step test plans

Use this policy for each container-plan row. The container issue's
`## Test plans` section is durable. Draft each plan before its branch starts.
A needs-issue row may use its title and branch pattern; append its key and
exact branch before dispatch.
Keep revisions append-only by row; the latest operator-approved revision wins.
Handoffs and resumes read the issue and checkpoint. Do not infer an override
from a request to work faster.

## Default child gates

Classify expected scope from the files and behavior the step can affect. Mixed
scope takes the highest applicable risk. Reassess against the actual diff and
test coverage before child merge.

| Risk | Child merge checks |
| --- | --- |
| Documentation/specification only | Relevant documentation, schema, and OpenSpec validation. Do not require application typecheck, lint, unit tests, or E2E when executable behavior cannot change. Code-generation inputs, runtime configuration, and test fixtures do not automatically qualify. |
| Copy/layout | Configured typecheck, lint, and unit tests; selected browser tests for affected pages and interactions; appropriate visual/manual evidence including relevant responsive views. The entire E2E suite is not the default child gate. |
| Risky or unmapped | The configured full suite: typecheck, lint, unit tests, and E2E when the project has it. Use for authentication, authorization, data integrity, backend behavior, dependencies, infrastructure, or impact not confidently mapped to focused checks. Missing or unusable focused coverage also selects the full suite. |

Map commands and selections to actual project coverage. Name exact commands
or tests when known; do not invent a browser suite or count an unrelated
command as E2E. Missing required coverage or command is NOT_CONFIGURED.
No E2E suite: record not applicable and run configured checks, with no
deferral. Existing E2E suite without a command: NOT_CONFIGURED.

## Plan and revisions

Record this block in the container issue before the child branch begins. A
separate artifact needs a durable URL in the plan.

```markdown
### Test plan: <step title or issue key>
Step/issue: <planned title and key when assigned>
Child branch: <intended branch pattern; exact branch before dispatch>
Expected scope: <files/behavior>
Risk: documentation/specification only | copy/layout | risky or unmapped
Rationale: <why this category and coverage fit>
During implementation: <checks and exact commands/selections when known>
Before child merge: <required checks and exact commands/selections>
Visual/manual evidence: <captures, views and interactions, or none with reason>
Intentionally omitted: <check and why, or none>
Deferred to final sprint: <exact checks, or none>
Child merge allowed when: <passing checks, evidence, and scope conditions>
Source: default
```

The operator may explicitly revise one step, including deferring selected
checks or its full child suite to the integrated sprint. Append a revision to
that step's plan with `Source: operator-approved`, the operator's direction and
rationale, the exact checks still required on the child, the exact checks
deferred to the sprint branch, and the accepted tradeoff. A revision must not
waive the final sprint gate. Publish the revision in the container issue and
checkpoint before the child uses it. If an instruction is ambiguous, retain
the current gate and ask; silence and speed requests are not approval.
An approved zero-check child plan records all deferrals and still requires
matching scope and evidence.

If the actual diff exceeds documented scope, stop using the old gate. Apply
the higher default category and update the plan before merging, unless the
operator explicitly approves a revised plan for the expanded scope. Re-run
required checks on the changed code and synced base. Preserve prior failure
handling and repair limits.

## Results and final sprint gate

Keep planned checks separate from results. At completion record the tested
revision/environment, each check as ran/passed, ran/failed, or deferred,
evidence URLs, and new required checks. Child merge requires passing current
checks, published evidence, matching scope, and explicit deferrals. Failures
follow the existing repair flow; deferral is never a pass.

Before opening or refreshing the sprint PR for human review, run the complete
configured suite, including E2E when available, on that branch. Run deferred checks outside
that suite there; list outstanding deferrals until covered. Missing commands,
failures, or unpublished evidence block review. A red PR is for failure
inspection only. Never auto-merge MAIN_BRANCH.

## Examples

Default documentation: for prose/specs with no executable effect, run document
checks and `openspec validate --all --strict`. Omit application checks on this
child; run the final suite.

Default copy/layout step: run configured typecheck, lint, unit, and selected
affected-page browser checks; attach relevant desktop/mobile captures. Defer
the full E2E suite to the final sprint gate.

Operator-approved deferral: for low-risk copy, run child typecheck and a named
page browser test; defer lint, unit, and E2E to the sprint. Record rationale,
selections, and delayed-defect risk. Backend changes trigger the risky default
until a new plan is approved.
