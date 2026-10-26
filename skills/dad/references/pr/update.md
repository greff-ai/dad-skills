# Address PR feedback

Input: instructions or review threads, plus an optional sprint or issue key;
otherwise use the current branch. Start with `references/shared/bootstrap.md`.

A sprint key or branch follows `references/sprint/update.md`. An issue whose
latest checkpoint has parent MAIN_BRANCH and an open PR from its branch to
MAIN_BRANCH takes the direct update below. Otherwise halt WRONG_BRANCH,
naming any sprint branch its checkpoint records.

## Direct update

1. Require a clean tree; checkout and fast-forward the issue branch. Require
   its recorded PR open, unmerged, with expected head and base.
2. Use supplied instructions, else `dad pr threads`, keeping thread IDs.
   Split requests. One beyond the issue's intent is OUT_OF_SCOPE: suggest a
   new issue and leave its thread open. On retry, reuse commits the issue
   checkpoint records for a request or thread URL.
3. Show thread-derived changes mapped to threads and wait for approval;
   supplied instructions are approval. Note threads to leave open.
4. Implement in a worker per `references/shared/subagents.md`, committing on
   this branch. When required behavior changes, update current specs under
   paths.openspec and validate them; never edit the archive.
5. Rerun Review steps 1-2 of `references/pr/issue.md`, without archiving.
6. Push. Append `## Review updates` to both PR bodies, refresh test results,
   and publish an issue checkpoint with request/thread URLs and commits.
7. Reply to each addressed thread with its commit link using a reply file,
   skipping an identical reply, then resolve it unless asked to leave it open.
   Red or halted work leaves threads open. Report commits, PR, and results.
