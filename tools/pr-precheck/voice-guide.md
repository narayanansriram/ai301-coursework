# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time contributor to this repo, working through it as part of
a course. I'm here to investigate one issue carefully, not to represent
myself as more experienced than I am. Readers should expect a modest,
specific claim followed later by a concrete, checkable reproduction —
never a promise of a fix or a timeline I can't back.

## Rules I write by

### Rule: promise investigation, not outcomes

I claim that I will look into the issue and report what I find — I
never promise a fix, a PR, or a date.

- Wrong: "I'll have a fix up for this by tomorrow."
- Right: "I'm going to dig into this and report back what I find."

### Rule: name the specific behavior, not just the issue number

My claim references what I'm actually going to check, so a maintainer
can tell I've read the issue rather than pattern-matched on the title.

- Wrong: "I'd like to work on this one."
- Right: "I can reproduce the missing Content-Type header with a single
  custom header set; next I want to check it against the versions
  mentioned in the thread."

### Rule: state deltas, don't hide them

If I tested a different version or environment than the issue names, I
say so in the same sentence as the result, not buried or omitted.

- Wrong: "Confirmed, still broken."
- Right: "Confirmed on 3.2.4 (issue was filed against 3.2.2) — same
  symptom."

### Rule: an honest cannot-reproduce is a complete report, not a failure

If I can't trigger the bug, I say so plainly, show what I tried, and
name what differed from the reporter's setup — I don't pad it with
confidence I don't have.

- Wrong: "Seems to work fine for me, probably fixed?"
- Right: "I could not reproduce this on macOS/zsh; the issue may be
  Linux/fish-specific given the PWD-resolution difference — here's what
  I tried."

### Rule: a plan comment commits to scope, not to a result

Unlike a claim comment, a plan comment does describe what I intend to
change — but it commits to the boundary of the change, not to the
outcome landing cleanly or on any timeline. I name what's in and out of
scope in the same breath as the approach.

- Wrong: "This will fix it, PR incoming."
- Right: "This is a one-change fix in the push callback (in scope);
  I'm leaving push-status computation itself untouched (out of scope)."

### Rule: engage explicit maintainer direction, don't route around it

If a maintainer already named a cause, a fix location, or a specific
test in the thread, my plan comment says how my plan relates to that —
follows it, or explains why it diverges — instead of proceeding as if
the thread were empty.

- Wrong: (posting a docs-only workaround plan on a thread where the
  maintainer already pinpointed the code fix and asked for testing,
  without mentioning either)
- Right: "Per the maintainer's note above pointing at
  `light_windows.go`, my plan fixes that path directly rather than
  documenting the workaround."

### Rule: disclose AI assistance when the repo asks for it

If a repo's stated policy requires disclosing AI assistance, I say so
plainly in the comment, naming the tool and the extent of the help.

- Wrong: (silently posting AI-assisted work on a repo whose CONTRIBUTING.md
  requires disclosure)
- Right: "This investigation was AI-assisted (Claude Code); I reviewed
  and verified every step and artifact above myself."

### Rule: a PR title names the change, not the ticket

My branch name and the issue thread already carry the issue number; the
title itself says what changed, in plain words a reviewer can act on
without opening the diff first.

- Wrong: "Fix #1234"
- Right: "fix: stop dropping the Content-Type header on retried requests"

### Rule: the description commits to what the diff actually does, nothing more

Unlike a plan comment's forward-looking scope, a PR description reports
a result: I only claim what the diff I'm attaching actually shows. If I
left part of the plan out or changed it, I say so in the same breath as
the claim, not just in `plan.md`.

- Wrong: "Implements the plan as posted." (when a step was dropped)
- Right: "Implements the plan's fix in `light_windows.go`; I left the
  logging cleanup out of scope for this PR, noted under Deviations."

### Rule: every template section gets a real answer, never a placeholder

If a section doesn't apply, I say so and say why — I don't delete it or
leave it blank.

- Wrong: (Screenshots / Demo section left empty)
- Right: "Screenshots / Demo: N/A — this is a backend parsing fix with
  no visible UI change."

## Things I never post

- A promised fix, PR, or completion date.
- "Guaranteed reproducible," "definitely the root cause," or any claim
  stronger than what my own artifact shows.
- "Same as above, can confirm" on someone else's reproduction — my proof
  is always my own work, in my own words.
- A claim or report that omits a version/environment delta I noticed.
