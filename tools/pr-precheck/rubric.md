# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Scope-Faithful | The diff's changed files and hunks, read against the plan's stated files/scope (including any `## Deviations` note in the plan). | Pass if every changed file and every hunk falls inside the plan's stated files/scope, or is explicitly named in a `## Deviations` note explaining why it was added or dropped. Fail if the diff touches a file or makes a change the plan never named and no deviation note accounts for it (a silent "while I'm in there" addition), or if the diff does less than the plan promised (a named fix step missing from the diff) with no deviation note saying it was left out on purpose. Unclear (cannot tell from the package whether a changed file was in scope) counts as fail. | required |
| Description-Matches-Diff | The PR description's Summary and Changes bullets, read line-by-line against the actual diff. | Pass if every bullet in the description's Changes section corresponds to a real hunk in the diff, and the diff has no substantive change absent from the description. Fail if the description claims a change the diff does not make, omits a substantive change the diff does make, or asserts fidelity to the plan ("implements the plan as posted") that the diff, read against Scope-Faithful's evidence, contradicts. | required |
| Evidence-Observable | The Testing section of the PR description / `test_evidence.md`, read against the plan's test plan and the repo's own stated checks (e.g. a PR template's required checks, `make test-unit`, `make lint`). | Pass if the evidence shows a concrete, observable before/after result tied to the plan's test plan (an error becoming a clean pass, a specific output changing in the stated way) AND shows the repo's own stated checks were actually run, with their real results pasted (a failing check reported honestly, with a reason, still counts). Fail if the evidence is asserted without a shown result ("tests pass", "verified manually" with nothing pasted), checks something other than what the plan's test plan named, or the repo's stated checks are never run/mentioned at all. Unclear counts as fail. | required |
| Diff-Reviewable | The full diff, read hunk by hunk. | Pass if every hunk is part of the stated fix: no commented-out code, no debug prints/logging left in, no unrelated formatting-only or drive-by edits mixed into the same diff. Fail if the diff contains debris (dead code, stray debug statements, commented-out blocks) or an edit unrelated to the issue, even a small one, even if explained elsewhere. | required |
| Standards-Met | The PR description's sections against the repo's own stated PR-template asks (from the repo-facts block, or live, Path Review's actual template) and the repo-facts block's stated contribution/AI-use policy; the issue reference. | Pass if every section or item the repo's own template actually asks for (whatever it calls them — Description, Problem/Solution, a requirements checklist, etc.) has real, non-placeholder content; the PR clearly identifies the issue it addresses by number, in whatever form the PR uses (an explicit `Closes #<n>`, a parenthetical `(#<n>)`, or a sentence naming "issue #<n>" — any unambiguous reference counts, there is no required phrasing unless the repo's own template mandates one); and — if the repo states an AI-use disclosure requirement — the description contains a specific, human-voiced disclosure of how AI was used (not a generic template sentence). Fail if a section or item the repo's template actually asks for is left as a placeholder or skipped entirely, the PR never identifies its issue number anywhere, or a stated AI-use disclosure requirement goes unmet. If the repo states no AI policy, that half of the check passes by default. Live mode exception: Path Review's own template requires the literal `Closes #<n>` form, so grade that half strictly there. | required |
| Honest-About-Gaps | Any place the PR falls short of the plan or of full coverage (an unticked testing box, a deferred edge case, a known limitation), read against whether the description or `plan.md`'s Deviations note says so in its own words. | Pass if every shortfall the package reveals is also disclosed in the description or the plan's Deviations note, in the author's own words (not silently discovered by inspecting the diff alone). Pass trivially if the package reveals no shortfall. Fail if a shortfall is visible in the diff/evidence but never disclosed anywhere in the PR text or plan. | preferred |

## Verdict rule

Accept only if every `required` check (Scope-Faithful, Description-Matches-Diff,
Evidence-Observable, Diff-Reviewable, Standards-Met) grades `pass`. Any
`required` check graded `fail` holds the package at `reject`.
`Honest-About-Gaps` is `preferred`: it is reported but never changes the
verdict on its own — a disclosed shortfall should already let the
relevant required check (most often Description-Matches-Diff or
Evidence-Observable) pass rather than fail, per the honest-outcome
rule; this check exists to flag the opposite case, where something is
true but no one said so. `unclear` is treated as `fail` for every
check, required or preferred, per each check's own pass condition and
as the default whenever a check does not resolve cleanly from the
package: a PR you cannot verify from the package is not ready to
submit.
</content>
