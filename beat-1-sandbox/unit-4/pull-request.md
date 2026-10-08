# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/103

**Branch**

fix/62-redis-health-url

## Eval iterations

**Run history**

Two full runs:

1. First full run: 14/20 agreement (`categories: clear-accept 1/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`) — below the 18/20 bar. The `Standards-Met` check required a literal `Closes #<n>` issue reference, but most practice submissions reference their issue inline (e.g. `(#4208)`, `per the plan posted on the issue`) rather than using that exact phrasing, so `Standards-Met` failed almost every `clear-accept` package for a phrasing reason, not a real standards gap.
2. Final full run (committed): **18/20 agreement — bar: PASS** (`categories: clear-accept 5/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`), matching the agreement line in the committed `eval-run.txt`.

Between the two full runs, I re-graded only the affected packages with `--only pkg-01,pkg-02,pkg-05,pkg-08,pkg-11,pkg-13,pkg-19,pkg-20` (8/8 agreed on that partial run) before spending the second full run, per the retry guidance.

**Package analysis**

`pkg-13` — my rubric's verdict: `reject` (gold: `accept`). This is a `clear-accept` package (a Windows `cmd.exe` metacharacter-escaping fix with an honestly disclosed deferral of percent-sign expansion). My `Evidence-Observable` check failed it both times I ran it, while five other `clear-accept` packages with a similar shape passed. The candidate PR's test evidence shows the caret-escaping fix working for the reported URL case but, by my check's reading, doesn't show an equally concrete before/after for the deferred `%VAR%` case it explicitly declines to handle — my check's pass condition ties the evidence to "the plan's named repro behavior," and I read the stated deferral as leaving that behavior only partially demonstrated, where the gold label treats an honestly-disclosed, out-of-scope limitation as not needing its own repro proof. This is one of the four packages the assignment calls out as genuinely arguable, and it's the shape I'd watch if I revised this check again: making "Evidence-Observable" read a disclosed deferral in `Honest-About-Gaps` terms rather than treating it as missing evidence for the main fix.

**Check rationale**

`Standards-Met`, as it now reads in `rubric.md`:

> Pass if every section or item the repo's own template actually asks for (whatever it calls them — Description, Problem/Solution, a requirements checklist, etc.) has real, non-placeholder content; the PR clearly identifies the issue it addresses by number, in whatever form the PR uses (an explicit `Closes #<n>`, a parenthetical `(#<n>)`, or a sentence naming "issue #<n>" — any unambiguous reference counts, there is no required phrasing unless the repo's own template mandates one); and — if the repo states an AI-use disclosure requirement — the description contains a specific, human-voiced disclosure of how AI was used (not a generic template sentence). [...] Live mode exception: Path Review's own template requires the literal `Closes #<n>` form, so grade that half strictly there.

It reads this way because my first full run's failing `clear-accept` packages (`pkg-02`, `pkg-05`, `pkg-08`, `pkg-11`, `pkg-13`, `pkg-19`) all failed on this check, and when I read their actual descriptions, none of them used the literal `Closes #<n>` phrasing my original check demanded — they referenced their issue inline, in whatever convention that repo's own template or culture used. I had written the check against Path Review's own template shape and wrongly generalized it as a universal requirement. I rejected the single hardcoded phrasing in favor of "any unambiguous reference," while keeping the strict `Closes #<n>` rule for live mode specifically, since that really is what Path Review's own template requires and the assignment's own instructions name it explicitly.

**Trade-offs**

Loosening `Standards-Met`'s issue-reference rule is also what gives up `pkg-13`'s agreement in the confirming full run (it passed `Standards-Met` on the `--only` re-grade but then failed on `Evidence-Observable` in the full run instead — a separate, pre-existing gap the loosened check didn't create). As a canary for the loosening itself, I re-ran the two `standards-wall` packages (`pkg-01`, `pkg-20`, both already `reject` in gold) with `--only` alongside the affected packages, and both still correctly graded `reject` — confirming the looser issue-reference rule didn't accidentally let a package with a real standards-wall problem (missing template sections, an unmet disclosure policy) pass just because it happened to reference its issue number somewhere.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
