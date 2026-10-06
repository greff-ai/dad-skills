# Initialize a project

Run only for an explicit setup request. Ordinary workflows report missing
prerequisites; they do not install tools or reconfigure a project mid-run.

1. Identify the target repository, preserve its existing edits, and locate
   the installed dad payload through host metadata, sibling skills, or
   DAD_SKILL_DIR. Require the bundled CLI. Use the host's supported skill
   installer for a missing payload; install all nine skills together.
2. Check Node, Git, the authenticated forge CLI, and OpenSpec. Initialize
   OpenSpec for the user's host when requested. Install explore, propose,
   apply, verify, and archive; the core profile may omit verify. Use a local
   profile override instead of changing global configuration. DAD_SKILLS_DIR
   can identify a custom/global OpenSpec skills directory.
3. Identify the GitHub issue repository and project owner/number. Cloning
   creates no project: select an existing one or create a dedicated one
   within the requested setup scope; ask when ownership or permission is
   unclear. For Linear, identify the team instead and require an exported
   LINEAR_API_KEY; the forge and its login stay GitHub.
4. Run init using `references/shared/cli.md`. Inspect every check and warning,
   including a failure report's error.details. A fresh repository or Linear
   team may lack the four configured type labels; a fresh project may lack
   the configured status options. dad reports but never creates labels:
   treat missing ones as unfinished setup before filing. Match settings to
   intended existing resources, or provision authorized ones. Do not rename
   shared board fields without permission. Rerun init after corrections;
   never hand-edit its generated ID cache.
5. Configure the repository's actual test commands in its settings. A
   greenfield project's first tracked task may create the declared scripts
   before its merge gate. Never fabricate a passing result for a missing
   command; report unavailable/skipped tiers explicitly.
6. Account for all generated setup files, including OpenSpec directory
   markers; init maintains cache and sprint/ rules in the dad .gitignore.
   Existing tracked sprint records need a separately approved migration after
   their checkpoints/evidence are preserved remotely; never delete them during
   setup. Commit/push only approved initialization files before starting
   branch workflows. Preserve unrelated user edits and stop for a safe clean
   baseline rather than discarding them.
7. Report the configured project, capabilities, remaining prerequisites, and
   readiness for the requested workflow. Setup files no sprint or issue.
