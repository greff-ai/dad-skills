# Plan a repair

Inputs: readable failure log or verifier findings, change slug, gate
test/verify/merge, failing tier, and level unit/full.
Run in the current worker context; no delegation or commits here.

1. Require concrete failure context and the active change directory.
   Read previous fix logs so an unsuccessful approach is not repeated.
2. Create the next unused `fix/NN-<short-name>.md` under that change.
   Include Problem (failure, root-cause hypothesis, gate, evidence, whether
   intended behavior/spec boundaries would change), Plan, and Tasks.
   Never overwrite a prior fix.
3. Implement the scoped repair and mark tasks as completed. Respect the
   caller's allowed paths. A merge-gate repair that needs changed intent
   returns a major failure before implementing that expansion.
4. Follow `references/issue/single-branch/test-fix.md` in this context with
   the fix file and level.
5. Return pass with fix path and edited files, or the exact halt. Leave all
   edits and logs intact. The owning worker commits; the leaf reconciles
   proposal/design/specs/tasks afterward.
