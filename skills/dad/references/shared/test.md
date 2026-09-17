# Run tests

Inputs: tier `unit` or `full`, change slug, originating gate
`test|verify|merge`, and `fix: yes|no` (default yes).
Run in the current context; dispatch nothing.

1. Read settings with `references/shared/cli.md`: `commands.typecheck` and
   `commands.lint` are optional; `commands.test` is required; `commands.e2e`
   is required for full. Report an absent required command as NOT_CONFIGURED.
2. Run configured commands verbatim from the repository root, in that order,
   stopping at the first failure. Full adds e2e after test. Capture stdout,
   stderr, exit code, and elapsed time in a unique temporary log.
3. Return commands, pass/fail/skip counts, tested revision/environment, and
   specific added tests with the behavior each proves. If review evidence was
   captured, return its paths and captions via `references/shared/evidence.md`;
   ordinary test output alone does not require attachments.
   Green: return pass, tiers run/skipped, and log path. With `fix: no`, red:
   return fail, failing tier, summary, and log; make no repairs.
4. With `fix: yes`, pass the failure, gate, tier, and slug to
   `references/issue/single-branch/fix.md` in this context. Propagate its halt;
   on success rerun the entire requested tier with `fix: no`.

This reference does not commit or push. Its worker commits scoped repairs
before returning as required by `references/shared/subagents.md`.
