# Publish test evidence

Always summarize tests in the PR description per `references/shared/pr-body.md`.
Attach screenshots only when captured as review evidence. Do not generate a
PDF. Routine logs, checkpoints, measurements, and counts belong in the test
summary; they do not require image attachments.

## Prepare

Keep originals and generated files under the ignored issue/container directory.
Review for secrets and unrelated personal data before publishing. Use existing
image tools; do not add application dependencies just to prepare evidence.

Select useful screenshots and write a short caption for each: the visible state
and what it demonstrates. Distinguish appearance from behavior proved by a
named test; screenshots alone do not prove interactions. Keep overall results,
limitations, and specific added tests with what they prove in the PR summary.

Downsample copies, not originals: preserve aspect ratio, never upscale,
and aim for at most 1600 pixels on the longest edge. Compress while keeping
text readable and remove redundant captures. Inspect the final images for
legibility, clipping, and matching captions. Report an
unavoidable size/quality tradeoff rather than degrading the proof.

## Publish

Open/update the PR through dad first. Use its explicit repository and PR number,
not whichever branch happens to be checked out. Upload only new/changed evidence;
reuse confirmed URLs on retry and preserve earlier test history.

Use the native attachment contract in `references/shared/cli.md` for the
downsampled copies. Require attachment-capable GitHub CLI (>=2.99) and write
access. Missing support or failed publication halts before the PR checkpoint
or merge; preserve local files and report the prerequisite, never commit them
as a workaround.
Even a failed upload command may have published some attachments: read back the
PR before retrying, retain successful URLs, and retry only missing files.
Verify remote attachment links, then arrange each uploaded image with its
visible caption under `## Evidence` using the shared PR-body workflow. Alt text
alone is not a caption. Preserve the rest of the remote body and earlier URLs;
refresh the local mirror and publish evidence URLs and test summary in the
issue checkpoint. Containers link child PR evidence and attach only their own
new captures. No ignored local path is a review link.
