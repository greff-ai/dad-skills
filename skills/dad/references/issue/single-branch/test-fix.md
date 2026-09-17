# Verify a repair

Inputs: existing fix file, level unit/full, and change slug.
Stay in the same context. Do not commit, push, revert, or delegate.

1. Run the requested tier using `references/shared/test.md` with fix no.
   On green, append Resolution to the fix file with the verified result,
   commands, iteration count, and changed files; return pass.
2. On red, read the log and all prior attempts. Make one targeted repair,
   then log the failing test, hypothesis, edited files (or diagnostic-only),
   and pending result under Changes log.
3. Re-run the complete tier with fix no and record the result. Repeat up to
   20 repair iterations. The final repair must be tested before reporting it.
4. Stop early when context cannot fit another attempt or two consecutive
   attempts make no edits. Append Stuck with the current failure, log,
   attempts, touched files, and a concrete next step. Return
   `STUCK: <reason>; fix file <path>`. Preserve the work for the next run.

A missing command or unusable fix file propagates its configuration/MISSING
halt; it is not a test failure to repair.
