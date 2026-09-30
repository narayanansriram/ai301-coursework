# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in eval mode, the repro report's opening environment line
(usually the first line or a labelled "Environment:" section), read
against the issue context's stated version/OS and the repo-facts block's
latest-release line. In live mode, the same spot in the student's draft
repro report, read against the issue thread on GitHub.

What good looks like: OS named, the tool's own version named, and any
runtime/library version the bug plausibly depends on (interpreter,
dependency version) also named. If the tested version is not the
version the issue was filed against, the report says so in its own
words rather than leaving the reader to diff two numbers.

## Steps

Where it lives: the repro report's numbered steps or command block, in
eval mode; the same section of the student's draft in live mode.

What makes them followable: every input a stranger would need (config
file contents, input file contents, exact command/flags) is shown
inline, not referenced from a location only the author can reach. A
step that names a private repo, an unshared config, or "our internal
setup" fails this even if the rest reads well. The steps must also hit
the specific trigger the issue names (the right flag, the right input
shape) — running an adjacent variant is a Steps-Followable or
Behavior-Matches-Issue problem depending on what the resulting artifact
shows.

## Behavior shown

Where it lives: the artifact block(s) in the repro report (command
output, log excerpt, screenshot description), read against the "Issue"
section's described behavior (error text, exit code, crash vs.
graceful error, visual symptom).

What it means to show the issue's behavior rather than an adjacent one:
the artifact's own text/exit code matches the behavior class the issue
reports, not a different failure mode that a confident narration
papers over. A graceful validation error is not a crash; a compile
error is not a runtime panic; a "the tool ran fine" log is not "the
tool hung." Read the artifact itself first, then check whether the
report's narration matches what it actually shows.

## Honesty

Where it lives: the report's concluding language ("Expected"/"Actual",
or an "Analysis" section), read against the artifacts a few lines
above it, and against any environment delta already noted.

How to tell honest from overclaiming: an honest report's conclusion is
exactly as strong as its evidence — including "I could not reproduce
this, here is what I tried and what differed" as a fully valid,
passing outcome. An overclaiming report asserts a root cause, a
guarantee, or a completed verification ("I verified this race
condition," "guaranteed reproducible") with no transcript or artifact
showing that specific thing, or draws a stronger conclusion than the
shown artifact supports (e.g., claiming a version-gated behavior
"affects the current release" from a test run on an old version).

## Comms

Where it lives: the candidate claim comment, read against the issue
it names and, for disclosure, against the repo-facts block's stated
contribution/AI policy line.

What specific-and-honest looks like: the claim comment names the
concrete behavior or file it is about and states a next investigative
step, never a promised fix, a promised timeline, or a self-assignment
with no content. Boilerplate ("assign me," "+1 can confirm," "same as
above") fails regardless of the repro report's quality. Separately,
when the repo-facts block states an AI-disclosure requirement, check
both comments for an explicit disclosure of AI assistance; its absence
fails the AI-Disclosure check even when everything else about the
package is strong.
