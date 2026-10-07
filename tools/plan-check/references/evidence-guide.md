# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the plan's own "Cause"/"Diagnosis"/
"Problem statement" section or opening lines, read against every step,
control run, and artifact in the `## Repro evidence` block, and against
any `## Thread highlights` line the diagnosis cites by name. In live
mode: the student's `plan.md` diagnosis, read against the reproduction
evidence in their own posted repro comment on the issue (or the house
repro pack), and against the live issue thread for any maintainer
comment the diagnosis leans on.

What good looks like: the diagnosis names a mechanism, and that
mechanism is either (a) directly demonstrated by a repro step — a
control run that isolates it, an error message that names it, a timing
comparison that implicates it — or (b) explicitly marked "unverified" /
"assumption" / "if X turns out to be the case" by the plan itself. A
diagnosis fails this test the moment a repro step's own result cuts
against it: e.g. the repro shows the delay persists with the mechanism
the diagnosis blames entirely removed from the loop, or a control run
succeeds under conditions the diagnosis says should fail. A diagnosis
that borrows a thread comment's claimed root cause verbatim, without
the repro evidence independently confirming it, is not grounded — it is
grounded gossip.

## Scope

Where it lives: the plan's "Scope"/"In scope"/"Not in scope" lines, and,
absent those, its "Changes"/"Proposed changes"/"Files and areas" list
read on its own terms.

What good looks like: the count and shape of changes matches what the
diagnosis requires — one fix, one place it lives (occasionally two or
three call sites sharing one root cause). A drive-by rewrite looks like:
a numbered change list where item 1 is the actual fix and items 2
through N are upgrades, abstractions, CI additions, or "since we're in
here" work the diagnosis never asked for. A stated not-in-scope line
that correctly excludes adjacent work is a strong positive signal but is
not required if the changes list is already narrow.

## Executability

Where it lives: the plan's "Approach"/"Changes"/numbered-steps section.

What good looks like: at least one real file path, module, or named
function, plus a stated mechanism ("add X to Y", "call Z before W
registers its own handlers", "drain pending query responses before
pane input is wired") rather than research tasks ("profile it",
"figure out where undo state lives", "dig into the editor code"). A
plan that promises to go find the cause and the fix later, rather than
naming both now, is not executable — it is a promise to write a plan.
Naming a module/path and a mechanism the author has already traced
(e.g. "visible in debug output"), while leaving the exact function name
to be pinned once inside the file during the PR itself, still passes —
that is an ordinary implementation detail, not an unresolved location.

## Test plan

Where it lives: the plan's "Test plan" section, read against the exact
steps and artifacts in `## Repro evidence`.

What good looks like: the test plan re-runs (or clearly maps onto) the
repro's own steps and states the specific after-state that would confirm
the fix — a color that must flip without leaving a view, an exit code,
a timing bound, a specific line of output. A vague test plan says
"confirm it works," "make sure nothing regresses," or "should feel
fast" without tying that to any observable, repro-traceable outcome; it
counts as a fail even when everything else about the plan is strong,
because nobody reading it could tell whether the fix worked.

## Honesty

Where it lives: any "Risk"/"Unknowns"/"Open questions" section, and,
absent one, the confidence level of claims elsewhere in the plan that
the repro evidence does not fully settle (e.g. an untested performance
claim, an approach chosen over an alternative without measuring both).

What good looks like: a plan that still has a genuine unknown (cost not
yet measured, an edge case not yet tried) says so in those terms, rather
than asserting it will "obviously" work. A plan with no material
unknowns left needs no risk section to pass this check.

## Comms

Where it lives: the plan comment, read against two sources — the
`## Thread highlights` block (for maintainer/collaborator direction
already given: a diagnosed cause, a named fix location, a testing
request, an approach already rejected) and the `## Repo facts` block's
stated contribution policy and AI-use policy. In live mode: the live
issue thread and the repo's actual CONTRIBUTING/AI-policy docs per
`scope.md`'s house rules.

What good looks like, thread-aware half: when a maintainer has already
named a cause or fix location or asked for specific testing in the
thread, the plan comment engages that — builds on it, or explicitly
argues against it — rather than proceeding as if the thread were empty.
A docs-only workaround plan posted on a thread where the maintainer
already pinpointed the code-level bug and asked for testing is the
textbook failure here, even if the docs-only plan is itself well
written.

What good looks like, AI-policy half: read the repo-facts AI-policy
line for which of two distinct rules it states. A **disclosure rule**
("AI usage must be disclosed, stating the tool and extent") is only
satisfied by an explicit disclosure sentence in the plan or comment; a
**human-voice rule** ("comments must be written by a human in their own
words," no mention of a disclosure statement) is satisfied by the
comment simply reading as specific and human-voiced, with no disclosure
sentence required. No stated policy passes by default. Do not apply a
disclosure requirement to a repo that only states a human-voice rule,
and do not wave away a repo that explicitly demands disclosure just
because the comment sounds human.
