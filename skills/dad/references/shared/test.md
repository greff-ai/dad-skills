# Run tests

Inputs: tier `planned-implementation`, `planned-merge`, `unit`, or `full`,
the current per-step plan when planned, change slug, originating gate
`test|verify|merge`, and `fix: yes|no` (default yes).
Run in the current context; dispatch nothing.

1. Read settings with `references/shared/cli.md`. For a planned tier, read
   `references/shared/test-plan.md` and the latest approved plan from the
   container issue/checkpoint; use only that tier's required checks. For unit,
   `commands.test` is required; for full, inspect project coverage and require
   `commands.e2e` only when an E2E suite exists. Record no E2E suite as not
   applicable, not deferred. Configured typecheck/lint precede test.
   Report an absent required command or unusable selected coverage as
   NOT_CONFIGURED. Do not substitute an unrelated command.
2. Run selected commands verbatim from the repository root in plan order,
   stopping at the first failure. For full, run typecheck, lint, test, then
   e2e when available. Capture stdout, stderr, exit code, and elapsed time in a unique
   temporary log. Collect selected browser and visual/manual evidence when
   the plan requires it; use `references/shared/evidence.md`.
3. Return commands, pass/fail/skip counts, tested revision/environment, and
   specific added tests with the behavior each proves. If review evidence was
   captured, return its paths and captions via `references/shared/evidence.md`;
   ordinary test output alone does not require attachments.
   For a planned tier, compare every check with the current plan and mark
   ran/passed, ran/failed, or deferred; never count deferral as pass.
   Green: return pass, checks run/skipped, and log path. With `fix: no`, red:
   return fail, failing tier, summary, and log; make no repairs.
4. With `fix: yes`, pass the failure, gate, tier, and slug to
   `references/issue/single-branch/fix.md` in this context. Propagate its halt;
   on success rerun the entire requested tier with `fix: no`.

This reference does not commit or push. Its worker commits scoped repairs
before returning as required by `references/shared/subagents.md`.
