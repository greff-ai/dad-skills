# Start a dad workflow

1. Locate the installed `dad` skill from the host's skill metadata. A wrapper
   uses its sibling `dad/`; an explicit `DAD_SKILL_DIR` may supply another
   location. Require `scripts/dad.mjs` and this reference. If unavailable,
   report `MISSING: dad payload; install dad alongside its wrappers`.
   Never assume a host-specific configuration directory.
2. Read configuration and capabilities using `references/shared/cli.md`.
   The explained `git.mainBranch` setting supplies `dadDir` as `{WORK_DIR}`
   and `value` as `{MAIN_BRANCH}`. Preserve `DAD_DIR` for nested config.
3. Record the repository root, current branch, and
   `git status --porcelain --untracked-files=all`. Issue drafting may start
   with a dirty tree; branch workflows require a clean tree.
4. Read only the selected workflow's settings through `dad config get`.
   Required value missing: `NOT_CONFIGURED: <path>; configure it in
   {WORK_DIR}/settings.json`. Optional value missing: record unset.
   Use column aliases and configured types/prefixes, never tracker labels.
5. Keep these facts for the flow and delegates: payload path, repo root,
   WORK_DIR, MAIN_BRANCH, DAD_DIR if set, capabilities, loaded settings, branch,
   and cleanliness. Delegates inherit them and read only missing settings.

Setup belongs to `dad init`; a flow never installs tools, repairs credentials,
or rewrites settings to bypass an error. Missing configuration or tracker IDs
should point the user to `references/shared/setup.md` for an explicit setup
request, not silently run it as recovery.

## Errors

A failed command returns `error.{code,name,message,details}`. Unless the flow
explicitly handles it, stop with `<NAME>: <message>`. Do not add retries around
the CLI's bounded retry policy; rate limits stop the affected operation.

Flow reasons use `QUESTION`, `DIRTY_TREE`, `WRONG_BRANCH`, `STUCK`,
`MISSING`, `TEST_FAILED`, `VERIFY_FAILED`, `CONFLICT`, `OUT_OF_SCOPE`,
or `FAILURE`. Include the concrete reason and recovery action. A child returns
the reason to its container, which records it and continues independent rows.
Conflicts with a ledger are reported, never silently repaired by guessing.
