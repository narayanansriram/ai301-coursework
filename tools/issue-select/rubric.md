# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer-Alive | "last push to any branch" and "maintainer first-response sample" in Repo facts (live: front-page commit date + recently-updated issues) | Pass if last push is within 90 days of the capture/today date AND at least one of the following: (a) a sampled issue shows a maintainer/owner/collaborator first response within 45 days, or (b) the last 5 default-branch commits include at least 2 distinct human (non-bot) authors with a commit in the last 7 days, showing an actively staffed team even when the response sample is thin or empty, or (c) the last push to the default branch is within 7 days — covers a solo-maintainer repo (e.g. a classroom/course repo where one instructor authors both the issues and the commits) where there is no maintainer-replies-to-issues pattern to sample at all | required |
| Repo-In-Use | "archived:", stars, "latest release", and last push in Repo facts (live: archived banner, star count, Releases box, front-page commit date) | Pass if archived is "no" AND last push is within 90 days of the capture/today date. If last push is older than 90 days, still pass if a release shipped within the last 12 months (a repo can be between commits but still shipping and used). Low star count alone does not fail this check; only fail on stale + unreleased or an archived repo | required |
| Scope-Fits-Newcomer | Issue body + comment thread | Pass if the issue is one bounded piece of work (not an umbrella/tracking issue explicitly meant to be split into separate sub-issues/PRs), the core fix is not still under unresolved design debate among maintainers, it is not a pure usage/support question, and no maintainer has said the fix requires touching core internals. A single bug that lists a couple of possible causes or optional follow-up suggestions alongside one primary ask is still bounded — grade whether the primary fix is one piece of work, not whether extra ideas are attached. Fail if the thread shows a long history of repeated claim-then-abandon cycles (multiple contributors auto-unassigned for inactivity, e.g. a claim bot) or multiple closed/unmerged PRs already attempted against it: that pattern means real difficulty hides under a friendly label | required |
| Unclaimed | "this issue: assignees" and "linked PRs" in Repo facts, plus claim comments in the thread (live: Assignees box, Development box, thread text) | Pass if there is no assignee AND no open linked PR already attempting the fix AND no unanswered "I'll take this / working on this" claim from another contributor. In live mode, apply the Path Review house rule in scope.md: ignore other students' claim comments when grading this check | required |
| AI-Contribution-Allowed | "contribution policy" line in Repo facts (live: CONTRIBUTING.md / AI_USAGE_POLICY.md / AGENTS.md) | Fail only on an outright ban on AI-generated contributions. Conditions (disclosure, human review, testing) pass; silence passes | required |
| Good-First-Issue-Label | Issue labels | Pass if the issue carries a "good first issue" (or equivalent) label | preferred |

## Verdict rule

Accept only if every required check passes. Any required check graded
fail rejects the issue. `unclear` on a required check counts as fail: a
first issue you cannot verify is not a first issue to take. Preferred
checks never change the verdict; among accepted issues, prefer the ones
with more preferred checks passing.
