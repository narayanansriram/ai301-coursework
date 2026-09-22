# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62

**Verdict output**

## Issue #62 — Health check references `settings.redis_host`, which does not exist on Settings

| Check | Grade | Evidence |
|---|---|---|
| Maintainer-Alive | **pass** | Last push Sep 16 2026 (5 days ago), satisfies condition (c): within 7 days, sole human author Aburke225 |
| Repo-In-Use | **pass** | Not archived; last push 5 days ago, within 90-day window |
| Scope-Fits-Newcomer | **pass** | Single bounded fix: rename redis_host/redis_port → redis_url in api/routes/health.py; no design debate, no prior PRs, no claim-then-abandon pattern |
| Unclaimed | **pass** | No assignees, no open linked PRs, no claim comments (only open PR #74 is linked to #60) |
| AI-Contribution-Allowed | **pass** | No CONTRIBUTING.md, AI_USAGE_POLICY.md, or AGENTS.md found; silence passes |
| Good-First-Issue-Label | **pass** (preferred) | Carries labels: api, bug, good first issue, tier-1 |

**Verdict: ACCEPT**

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62",
  "checks": [
    {"name": "Maintainer-Alive", "grade": "pass", "evidence": "Last push Sep 16 2026 (5 days ago), satisfies condition (c): within 7 days, sole human author Aburke225"},
    {"name": "Repo-In-Use", "grade": "pass", "evidence": "Not archived; last push 5 days ago, within 90-day window"},
    {"name": "Scope-Fits-Newcomer", "grade": "pass", "evidence": "Single bounded fix: rename redis_host/redis_port → redis_url in api/routes/health.py; no design debate, no prior PRs, no claim-then-abandon pattern"},
    {"name": "Unclaimed", "grade": "pass", "evidence": "No assignees, no open linked PRs, no claim comments (only open PR #74 is linked to #60)"},
    {"name": "AI-Contribution-Allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md, AI_USAGE_POLICY.md, or AGENTS.md found; silence passes"},
    {"name": "Good-First-Issue-Label", "grade": "pass", "evidence": "Carries labels: api, bug, good first issue, tier-1"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

- `--limit 3` smoke run: `agreement: 2/3 scored items`
- `--only issue-01` (after loosening Maintainer-Alive's response-latency threshold from 14 to 45 days): `agreement: 1/1 scored items`
- `--only issue-04,issue-05,issue-06,issue-07,issue-08,issue-09`: `agreement: 5/6 scored items`
- `--only issue-06` (after loosening Repo-In-Use to not require a minimum star count or recent formal release): `agreement: 1/1 scored items`
- `--only issue-10,issue-11,issue-12,issue-13,issue-14`: `agreement: 4/5 scored items`
- `--only issue-01,issue-14` (after adding a distinct-recent-committers alternative to Maintainer-Alive): `agreement: 2/2 scored items`
- Full run: `agreement: 18/20 scored items (bar: 18/20: PASS)`
- `--only issue-15,issue-19` (after tightening Scope-Fits-Newcomer to catch claim-then-abandon history and to stop over-reading enumerated causes/suggestions as unbounded scope): `agreement: 2/2 scored items`
- Full run: `agreement: 20/20 scored items (bar: 18/20: PASS)`
- Full run, final (after adding a solo-maintainer/recent-push alternative to Maintainer-Alive, found while live-mode grading a classroom repo with a single instructor-author): `agreement: 20/20 scored items (bar: 18/20: PASS)`

**Issue analysis**

`issue-19`. My rubric's decision: **accept**. Gold label: **accept**. Reasoning: the issue (zxcalc/zxlive#517) is a bug filed by a collaborator describing a UI freeze with two enumerated possible causes and three optional follow-up suggestions. My rubric's Scope-Fits-Newcomer check initially misread this numbered-list structure as an unresolved/unbounded scope and rejected it (my rubric's fail, gold's accept — the actual first disagreement I found). After rereading the evidence guide's distinction between a genuine umbrella/tracking issue (explicitly meant to be split into separate issues) and a single bug that just lists a couple of possible causes alongside one primary ask, I tightened the check's wording to grade whether the *primary* fix is one piece of work rather than penalizing any issue that lists extra ideas. Re-running with `--only issue-19` after that change produced `accept`, matching gold.

**Check rationale**

> Scope-Fits-Newcomer | Issue body + comment thread | Pass if the issue is one bounded piece of work (not an umbrella/tracking issue explicitly meant to be split into separate sub-issues/PRs), the core fix is not still under unresolved design debate among maintainers, it is not a pure usage/support question, and no maintainer has said the fix requires touching core internals. A single bug that lists a couple of possible causes or optional follow-up suggestions alongside one primary ask is still bounded — grade whether the primary fix is one piece of work, not whether extra ideas are attached. Fail if the thread shows a long history of repeated claim-then-abandon cycles (multiple contributors auto-unassigned for inactivity, e.g. a claim bot) or multiple closed/unmerged PRs already attempted against it: that pattern means real difficulty hides under a friendly label | required

I wrote it this way because a plain "is this one bounded task" reading kept failing on two opposite mistakes: rejecting bounded bugs that happen to list a couple of extra ideas (issue-19), and accepting issues that look bounded on the surface but have a long, hidden history of other contributors already trying and failing on them (issue-15). Both signals needed to live in the same check because they both answer "does the scope actually fit a newcomer," just from different evidence (the issue body's structure vs. the thread's history).

**Trade-offs**

Tightening Scope-Fits-Newcomer to add the claim-then-abandon clause is what flipped `issue-15` from an incorrect `accept` to the correct `reject` (canary re-run: `--only issue-15,issue-19`, `agreement: 2/2 scored items`). The trade-off: this clause only fires when a repo has an automated claim-bot or an explicit history of closed/unmerged PRs recorded in the bundle. A repo that quietly lets contributors go stale without any bot message or closed PR record (no visible abandonment trail) would slip past this check even if it has the same underlying problem — the check can only see difficulty that leaves a paper trail.

---

## Selection rationale

1. **Fit and time.** Issue #62 is a one-attribute rename (`redis_host`/`redis_port` → `redis_url`) confined to a single file (`api/routes/health.py`), matching my stated fit profile of being comfortable in Python but wanting practice reading an unfamiliar codebase without touching deep internals. It's small enough to finish in a short session.

2. **What the verdict got right vs. what I weighed myself.** The verdict correctly confirmed the bug is real (the `Settings` class only defines `redis_url`, not `redis_host`/`redis_port`) and that the issue is unclaimed and unblocked. What the rubric couldn't weigh: among the three accepted issues, #62 required the least unfamiliar library-specific knowledge (unlike #68, which needs understanding `rank_bm25`'s `BM25Okapi` behavior) — that comparison came from my own fit profile, not from any rubric check, exactly as the skill is designed to separate verdict from ranking.

3. **Anticipated difficulty claiming it.** Low: it's Path Review, so the house rule means any existing student claim comments don't block me, and the fix itself is small and self-contained, so the main risk is just making sure I understand how `Settings`/`redis_url` are consumed elsewhere in the codebase before changing the health-check route.
