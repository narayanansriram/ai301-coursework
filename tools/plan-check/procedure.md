# Procedure: how this skill grades a plan package

## Read order

1. Read the repo-facts block first (or, live, the repo's CONTRIBUTING/
   AI-policy docs and bug-report template): it sets what the Comms
   check's AI-policy half requires before anything else is graded.
2. Read the issue and thread highlights (or, live, the issue thread)
   next, and note down any maintainer-stated cause, fix location,
   testing request, or rejected approach verbatim. This is the
   "explicit direction" the Comms check's thread-aware half will check
   the plan comment against.
3. Read the repro-evidence block (or, live, the student's posted repro
   comment / the house repro pack) next, and note every step, control
   run, and artifact result as a plain fact ("step 3: exit 0 with a 5s
   delay when X is disabled"). This is the ground truth the Diagnosis
   and Test-Plan checks are read against, so read it before the plan
   itself to avoid anchoring on the plan's framing of its own evidence.
4. Read the candidate plan last, in full, before grading anything.
5. Read the candidate plan comment last of all.

## Evidence gathering

- **Diagnosis-Grounded**: pull the plan's stated cause as one sentence.
  Walk every fact noted in step 3 above and mark each as
  "supports," "contradicts," or "silent on" the stated cause. Also note
  whether the diagnosis cites a thread comment (step 2) as its basis,
  and whether that comment's claim is independently confirmed by any
  step-3 fact or only asserted.
- **Scope-Bounded**: list every distinct change the plan describes as a
  short phrase each. Mark each phrase as "required by the diagnosis" or
  "not required by the diagnosis." Note any explicit not-in-scope line.
- **Executable**: list every file/function/location the plan names.
  List every step in its approach and mark each as "an edit" or "a
  research task."
- **Test-Plan-Decisive**: pull the plan's test plan as one sentence.
  Compare it against the repro-evidence facts noted in step 3: does it
  name a specific step/outcome from that evidence, or a generic
  assurance?
- **Honesty**: pull any risk/unknown/open-question language verbatim.
  Separately, scan the plan's other claims for confidence the step-3
  facts do not support (an unmeasured performance claim, an approach
  picked without comparison).
- **Comms-Thread-Aware**: compare the plan comment against the
  maintainer-direction notes from step 2 (does it engage them?), and
  against the repo-facts AI-policy line from step 1 (disclosure rule,
  human-voice rule, or none — see evidence guide) to decide what the
  comment needs to contain.

## Check execution

Grade in this order: Diagnosis-Grounded, Scope-Bounded, Executable,
Test-Plan-Decisive, Honesty, Comms-Thread-Aware. Diagnosis-Grounded goes
first because a wrong cause makes Scope and Executable moot in
substance (a bounded, executable plan for the wrong fix is still not
ready) — grade them anyway on their own terms, but note in the summary
when a later check's pass is contingent on a diagnosis that already
failed.

For each check, decide strictly from the gathered evidence above: if
the gathered facts give a clean pass or fail per the rubric's pass
condition, grade it that way and quote the deciding fact. If the
gathered facts leave real ambiguity (the repro evidence is silent on
the claimed mechanism and the plan does not flag it as unverified;
the thread has no maintainer direction to be silent about, so treat the
thread-aware half as passing by default), grade `unclear` and say what
would have resolved it. Do not re-read the whole package for each
check; the read order and evidence-gathering steps above front-load
everything each check needs.

## Verdict assembly

Apply the rubric's verdict rule exactly: `accept` only if every
`required` check (Diagnosis-Grounded, Scope-Bounded, Executable,
Test-Plan-Decisive, Comms-Thread-Aware) graded `pass`. Any `required`
check graded `fail` or `unclear` forces `reject`. Honesty (`preferred`)
is reported in the summary and JSON but never changes the verdict. In
the JSON output's `evidence` field for each check, quote the specific
fact from evidence-gathering that decided it (a repro step, a scope
line, a thread comment, the AI-policy line) — never a restatement of
the pass condition itself.
