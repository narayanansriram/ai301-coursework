# Procedure: how this tool grades a PR package

## Read order

1. Read the repo-facts block first (or, live, the repo's PR template
   and `docs/CONTRIBUTING.md`): note the template's required sections,
   the repo's stated checks (e.g. `make test-unit`, `make lint`), and
   any AI-use disclosure policy. This sets what Standards-Met and the
   checks half of Evidence-Observable require before anything else is
   graded.
2. Read the plan context (the plan excerpt and its repro evidence, or
   live, `plan.md` including any `## Deviations` note) next, and note
   down verbatim: the stated files/scope, the stated test plan, and
   any deviation note with its reason. This is read before the diff so
   the diff gets judged against what was promised, not the other way
   around.
3. Read the full diff next, hunk by hunk. For each hunk, note the file,
   a one-line description of what it does, and whether it looks like
   debris (debug output, commented-out code, formatting-only, unrelated
   to the issue).
4. Read the candidate PR title and description last of the PR's own
   text, then its Testing/evidence section (or, live, `test_evidence.md`).
5. Read the issue and thread highlights (or, live, the issue thread)
   to confirm the issue number the PR claims to close and any
   maintainer-stated direction the description should not contradict.

## Evidence gathering

- **Scope-Faithful**: list every file the plan names and every file the
  diff actually touches. Mark each diff file as "in plan," "in plan's
  Deviations note," or "unaccounted." Separately, for each plan step,
  mark it "present in diff" or "missing from diff," and if missing,
  check whether a Deviations note says it was dropped on purpose.
- **Description-Matches-Diff**: list each bullet in the description's
  Changes section as a short phrase. Walk the diff's hunk notes (step 3
  above) and match each hunk to a bullet. Mark any hunk with no
  matching bullet, and any bullet with no matching hunk.
- **Evidence-Observable**: pull the plan's test plan as one sentence
  and the repo's stated checks from step 1. Walk the Testing/evidence
  section and mark, for each: does it show a concrete before/after
  result tied to the test plan (not just asserted), and was each
  repo-stated check actually run with its real output pasted (a
  reported failure counts; a check never mentioned does not).
- **Diff-Reviewable**: reuse the per-hunk debris notes from step 3;
  list any hunk flagged as debris or unrelated.
- **Standards-Met**: list every section or item the repo's own PR
  template actually asks for (step 1, in that repo's own wording —
  do not assume Path Review's Summary/Issue/Changes/Testing/
  Screenshots/Notes-for-Reviewers headings apply to an eval package
  from a different repo) against what's present in the description,
  marking each "filled," "placeholder," or "missing." Separately, scan
  the whole description for any clear reference to the issue number
  (`Closes #<n>`, a parenthetical `(#<n>)`, "per the plan posted on
  issue #<n>", etc.) and note whether one exists and matches the issue
  read in step 5 — in live mode, require the literal `Closes #<n>`
  form specifically, since that is Path Review's own template ask. If
  an AI-use disclosure is required, pull the disclosure sentence
  verbatim and note whether it reads as specific (names what was
  AI-assisted) or generic/templated.
- **Honest-About-Gaps**: collect every "unaccounted" or "missing"
  mark from the checks above (an unscoped file, a dropped plan step, an
  unmet repo check, an untied description claim). For each, check
  whether the description or the plan's Deviations note discloses it
  in its own words.

## Check execution

Grade in this order: Scope-Faithful, Description-Matches-Diff,
Evidence-Observable, Diff-Reviewable, Standards-Met, Honest-About-Gaps.
Scope-Faithful goes first because the other checks' readings of the
diff assume you already know which hunks are in scope versus which are
undisclosed drift.

For each check, decide strictly from the gathered evidence above: if
the gathered notes give a clean pass or fail per the rubric's pass
condition, grade it that way and quote the deciding fact (the file
name, the bullet, the pasted check output). If the gathered notes leave
real ambiguity (a hunk's purpose cannot be determined from the diff
alone and nothing in the description explains it; the Testing section
is present but the result it shows cannot be tied to the plan's test
plan either way), grade `unclear` and say what would resolve it. Do not
re-read the whole package for each check; the read order and
evidence-gathering steps above front-load everything each check needs.

## Verdict assembly

Apply the rubric's verdict rule exactly: `accept` only if every
`required` check (Scope-Faithful, Description-Matches-Diff,
Evidence-Observable, Diff-Reviewable, Standards-Met) graded `pass`.
Any `required` check graded `fail` or `unclear` forces `reject`.
Honest-About-Gaps (`preferred`) is reported in the summary and JSON but
never changes the verdict on its own. When more than one required
check fails, quote the first failing required check in rubric order
(Scope-Faithful, then Description-Matches-Diff, then
Evidence-Observable, then Diff-Reviewable, then Standards-Met) as the
deciding check in the output summary, but still report every other
failing check's own evidence in its own JSON entry — the summary names
one decider; the JSON always reports all of them. In the JSON output's
`evidence` field for each check, quote the specific fact from
evidence-gathering that decided it (a file name, a diff hunk, a pasted
check result, a template section) — never a restatement of the pass
condition itself.
</content>
