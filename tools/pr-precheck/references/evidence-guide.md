# Evidence guide: where evidence lives in a PR package

Worked against `calib-01.md` (the duplicate `file-decoration-style`
fix in `dandavison/delta#2211`) alongside each heading below.

## Plan fidelity (harness category: silent-drift)

Where it lives: in an eval bundle, the plan-context block's "Files" and
"Not in scope" lines and its test plan, read against the candidate
PR's commit list and unified diff; the description's own fidelity
claims ("implements the plan as posted") read against that same diff.
In live mode: your `plan.md` (its Files/scope lines and any
`## Deviations` note) read against `git diff main...HEAD` on your
branch, and your `pr_draft.md`'s Summary/Changes bullets read against
that same diff.

In calib-01, the plan names one file (`themes.gitconfig`) and one step
(remove the shadowed line). The diff touches exactly that file with
exactly that one hunk, and the description's one Changes claim matches
it word for word — a clean match, not silent drift.

What good looks like: every changed file and hunk falls inside the
plan's stated files/scope, or is named in a Deviations note explaining
why it was added or dropped; every plan step shows up in the diff, or
its absence is disclosed. Silent drift runs both directions: a diff
that does more than the plan (an unscoped file, a bonus refactor) and
a diff that does less (a promised step missing, with the description
still claiming completeness) are both failures here, as is a
description that asserts fidelity the diff itself contradicts.

## Test evidence (harness category: not-tested)

Where it lives: the candidate PR's "Test evidence" section, read
against the plan-context block's test plan and the repo-facts block's
stated checks. In live mode: your `test_evidence.md` (the repro re-run
before/after, plus `make test-unit`, `make test-integration`, `make
lint`, `make typecheck` output), read against your `plan.md`'s test
plan and Path Review's PR template's required checks.

Calib-01's test evidence re-runs the exact repro command from the plan
context (the `configparser` script) and shows the before error and the
after clean parse verbatim, plus a second confirming command
(`git config --get-all`) and a smoke check — decisive, not asserted.

What good looks like: a before/after result tied to the plan's named
repro behavior (an error becoming a stated pass, an output changing in
the stated way), not a bare "tests pass." The repo's own stated checks
appear with their real output, including an honestly-reported failure
("make test-integration: no tests ran" is expected and still counts as
evidence, pasted as-is) — a check silently never mentioned does not.

## Diff quality (harness category: unreviewable)

Where it lives: the candidate PR's unified diff and commit list. In
live mode: `git diff main...HEAD` on your branch.

Calib-01's diff is a single hunk deleting one line, with a plain commit
message naming exactly that — nothing to review past the fix itself.

What good looks like: every hunk is part of the stated fix. Debris
tells: a `print`/`console.log`/debugger line left in, a commented-out
block, a formatting-only pass over untouched logic, a second file
touched for a drive-by cleanup the issue never asked for. One unrelated
hunk is enough to fail this check even if the rest of the diff is the
real fix.

## Standards and comms (harness category: standards-wall)

Where it lives: the repo-facts block's PR-template section list and
stated contribution/AI-use policy, read against the candidate PR's
description (every template heading, `Closes #<n>`, and any disclosure
sentence). In live mode: Path Review's `.github/PULL_REQUEST_TEMPLATE.md`
and `docs/CONTRIBUTING.md`, read against your `pr_draft.md`.

Calib-01's repo states no PR template and no AI policy, so this check
passes by default there; it is not the package to study for this
family (see a `standards-wall`-category package instead, e.g. one
where the repo-facts block lists required template sections or a
disclosure rule).

What good looks like: every section the repo's template requires has
real content (not "N/A" where the section applies, not left blank),
the issue is referenced via `Closes #`, and — when the repo states a
disclosure requirement — the description contains a specific sentence
naming what AI assistance was used, not a generic templated line. A
section quietly dropped, or a disclosure requirement answered with
boilerplate, is standards-wall, even when the code change itself is
sound. (Whether the description's claims match the diff is plan
fidelity, above — a filled-in but inaccurate section fails there, not
here.)
</content>
