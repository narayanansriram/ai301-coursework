# Contribution: Evals: write 30 tool calls from a domain you know, without reading our rules (a blind pool)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Sriram Narayanan
**GitHub Username:** narayanansriram
**Issue:** https://github.com/Cygnux-Labs/Tracekit/issues/740
**Pull Request:** https://github.com/Cygnux-Labs/Tracekit/pull/741 (closed, superseded by and
merged as https://github.com/Cygnux-Labs/Tracekit/pull/777 — see Pull Request section)

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

<!--
Note on how this contribution came about: my original pick, Ocelot-Social#4800, stalled
on maintainer response for over a week (claimed, pinged, eventually assigned, but still no
write access). Rather than let the whole submission sit blocked on someone else's response
time, I picked up this Tracekit issue as a second, independently-workable contribution, and
it's the one with an actual open pull request, so it's the one recorded here. The
Ocelot-Social work (scout build, repro, solution analysis) is preserved in
`contribution-story-2.md` as an extra contribution, since it's real work but has no PR yet.
-->

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, a search-strategy-driven scanner over my unit-1 rubric**

I installed the scout over my unit-1 skill, wrote a fit profile and a three-query search
strategy (`good first issue` across Python, TypeScript, and JavaScript, ranked by
unassigned/recently-updated/fewest-comments), and ran one scan over the top five ranked
results. The scan's Maintainer-Alive check almost mis-graded three candidates from the same
repo as alive, because the repo's `pushedAt` field reflected a dependabot branch push rather
than default-branch activity; reading the actual default-branch commit dates caught it. I
didn't change the rubric's wording, since it already names "the last 5 default-branch commit
dates" as the evidence source — I just had to actually read the branch directly rather than
trust the repo summary field, and logged that in `scan-notes.md` so I don't skip the step
next time. (This week's work was spent on my original pick, Ocelot-Social#4800; see
`contribution-story-2.md` for that build in full. This issue was picked up later, in Unit 6,
once that claim stalled.)

**Unit 6: Patched the scout's rubric, then used the patched version to find and ship this
contribution after my first pick stalled**

I folded three per-unit voice-guide copies into one canonical `voice-guide.md`, added an
"Upstream: how I talk to maintainers" section, and wired a `PreToolUse` hook (`check_voice.py`)
that blocks a comment-posting command unless it matches the guide's rules. I also patched
`issue-scout/rubric.md`: the unit-1 `Scope-Fits-Newcomer` check was too strict for real repos,
so I replaced it with a `scope-feasible` check plus a difficulty note that no longer gates the
verdict. My original pick, Ocelot-Social#4800, went a full week with no maintainer response
(claimed, then pinged per the voice guide's "nudge on the repo's rhythm" rule, then eventually
assigned — but still no write access to actually open a branch, since that repo requires
branching off the main repo rather than a fork). Rather than let the whole submission stay
blocked on someone else's timeline, I ran my patched scout again and direct-graded a new hand-
fed candidate, `Cygnux-Labs/Tracekit#740`. It passed every required check, including an
unusually low-risk `Unclaimed` check: the issue states "several people can take this at once,"
so there's no single-claimant blocking risk. I claimed it, wrote a 40-entry JSONL blind test
pool (`eval/e21_blind/narayanansriram.jsonl`) matching the project's existing pool schema,
forked the repo, committed with a DCO sign-off, and opened PR #741 the same day — no wait
needed, since this issue explicitly allows starting without a prior assignment.

**Unit 7: Installed the ship-gate, wrote a real manifest, and ran it against my already-open
Tracekit branch**

I installed the ship-gate against my Tracekit fork clone and wrote `ship-manifest.md` from the
repo's actual `CONTRIBUTING.md`/Makefile/CI, rather than inventing checks: install, lint, and
build+package all run clean against my branch. The full test suite was a real problem — it
takes ~46 minutes, longer than the ship-gate's own 1500-second per-check timeout, and the repo
names no quicker command, so I ran it by hand once (4923 passed, 10 failed, all 10 confirmed
pre-existing and unrelated via `git diff --stat` showing my branch touches zero existing code)
and moved it to the manifest's excluded table with that reasoning, rather than silently
dropping it. The gate's internal `pr-precheck` call itself failed twice in a row with "no
fenced JSON block in the pr-precheck output" — a tool-side flakiness I couldn't fix from my
end, only retry, exactly as its own stop message suggested. By the time I'd normally have
needed the gate's green light to open a PR, the maintainer had already reviewed, corrected,
and merged #741 as #777 — so the gate ran retroactively here, confirming the repo-level checks
I'd need before a future PR rather than gating this one, which was already real work in
flight before Unit 7's materials existed.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I picked this up as a second, independently-workable contribution after my original pick,
Ocelot-Social#4800, stalled on maintainer response for over a week (I cover that work in full
in `contribution-story-2.md`, since it's real but has no PR to show yet). Rather than let the
whole submission sit blocked on someone else's reply time, I ran my patched issue-scout again
and direct-graded `Cygnux-Labs/Tracekit#740`, a real request for the next entry in an ongoing
series: write a "blind pool" of 30-60 tool calls in a domain you know, without reading the
project's own policy rules first, so the maintainers get an honest, unbiased test of their
agent-safety classifier's recall.

It fits my profile (Python repo, the actual task is writing structured JSON test data, not
systems or math work I lack background in) and graded **ACCEPT/straightforward**: the repo is
real and active (646 stars, pushed the same day I graded it, multiple human commit authors in
its recent history), it has a real `CONTRIBUTING.md` with a DCO sign-off requirement, no
AI-contribution ban, and — the detail that made it a safe pick given my other claim's
single-claimant risk — the issue explicitly says several people can take it at once, since
each contributor adds a different domain's pool. I chose "payments" since it wasn't one of the
three domains already covered by prior contributors (cloud/databases, CI/CD and identity,
email and messaging), and it's a domain I could write realistic, varied tool calls for without
guessing.

The same scan session also taught me something about my scout's blind spots, on an issue I
*didn't* take: `anniebritton/news-agent#6` passed every required check by the letter of the
rubric, but it's a brand-new, zero-star, single-contributor repo with issues bulk-created
minutes before my scan and a CONTRIBUTING.md link that 404s. My rubric has no check for "is
this a real, staffed project" versus a fresh personal repo — the same family of gap as the
missing language-fit check that cost me a bad pick earlier (`llm4s/llm4s#2227`, Scala, logged
in `scan-notes.md`). I didn't patch the rubric over one bad scan; I logged it as a known blind
spot to watch for.

**Claim comment:** https://github.com/Cygnux-Labs/Tracekit/issues/740#issuecomment-6809477132

**Toolkit**

- Tool: issue-scout (patched Unit-6 rubric: `scope-feasible` + difficulty note), run in a fresh
  scan, then in direct mode on three hand-fed URLs (Tracekit#740, nanocoder#1971, news-agent#6)
- What it produced: Tracekit#740 graded ACCEPT/straightforward; `nanocoder#1971` graded REJECT
  (fails Unclaimed — an open linked PR, #1888, already implements the exact fix, filed by the
  issue's own author who discloses a vendor conflict of interest); `news-agent#6` graded ACCEPT
  by the letter of the rubric but flagged and skipped on judgment
- Difficulties: the rubric still has no check distinguishing a real, actively-staffed project
  from a fresh solo/personal repo — `news-agent#6` exposed this the same way `llm4s#2227`
  exposed the missing language-fit check in Unit 6
- What changed: nothing in the rubric itself yet; logged the gap in `scan-notes.md` as a
  candidate for a future patch rather than reacting to one scan

---

## Understanding the Issue

**Problem Description**

Tracekit is an agent-safety policy tool that classifies tool calls (shell commands, API calls)
as harmful or benign before letting an agent execute them. Its policy rules are written by
people who know what the test set looks like, which risks the rules being tuned to the test
rather than to real-world coverage. The project asks outside contributors to write "blind
pools" — realistic tool calls from a domain they know, written *without* reading the policy
rules first — so the maintainers get an honest measurement of how well the default rules catch
real, unseen harmful behavior. Until this issue, the payments domain had no blind pool.

**Expected Behavior**

A new blind pool file exists at `eval/e21_blind/<contributor>.jsonl`, matching the schema of
the two existing pools, so the maintainers can run their `E21` evaluation against it and record
how many of the harmful calls the current policy actually catches.

**Current Behavior**

Before this PR, only three domains had blind pools: cloud/databases, CI/CD and identity, and
email and messaging (per the issue's own history, referencing PRs #357, #401/#368, and #728/
#734). Payments-domain tool calls (Stripe-style API operations, payment-database queries) were
untested.

**Affected Components**

- `eval/e21_blind/` — where each contributor's blind pool file lives; this PR adds
  `narayanansriram.jsonl` here
- `tracekit/policy2/packs/` — the actual policy rules being tested; explicitly **not** read
  before writing the pool, per the issue's own instructions, to keep the test honest
- `eval/e21_policy_robustness.py` — the script that scores a pool's JSONL entries against the
  policy packs (read only to confirm the expected field names, not run locally; see Testing
  Strategy)

**Toolkit**

- Tool: `gh api`, reading the two existing pool files (`juliasadler.jsonl`, `amandeavor.jsonl`)
  directly to confirm the exact field schema before writing my own
- What it produced: a confirmed field list (`tool`, `args`, `label`, `severity`, `technique`,
  `owasp`, `category`, `evasion`, `note`) and a confirmed set of harmful-call patterns already
  in use (irreversible cloud actions, SSRF, credential misuse) to model my payments-domain
  calls on
- Difficulties: none met — the existing pools were clear, complete references
- What changed: nothing

---

## Reproduction Process

**Environment Setup**

No local app environment was needed. This issue asks for a new data file, not a code change,
so there's no running application to set up. I used `gh api` directly against GitHub to read
the two existing pool files and confirm the schema, with no local clone needed until I was
ready to commit. Exact tool versions, for anyone trying to rerun the checks below: macOS
15.8.2, Python 3.11.0, `gh` 2.63.2, `git` 2.19.0.

**Steps to Reproduce**

N/A — there is no bug to reproduce here. This is a content-contribution issue (write a new
test-data file), not a defect report, so "reproducing" the issue means confirming the gap
(no payments-domain pool exists yet) rather than reproducing broken behavior.

**Reproduction Evidence**

- **Commit showing reproduction:** N/A, see above.
- **Screenshots/logs:** actual command output below, captured before I committed to the
  domain:
  ```
  $ gh api repos/Cygnux-Labs/Tracekit/contents/eval/e21_blind --jq '.[].name'
  amandeavor.jsonl
  juliasadler.jsonl

  $ gh api "search/issues?q=repo:Cygnux-Labs/Tracekit+payments+type:pr+is:open"
  {"total_count":0,"incomplete_results":false,"items":[]}
  ```
- **My findings:** The directory listing confirms only two pools existed (cloud/databases and
  CI/CD-and-identity authors, per the issue's own thread; the third, email/messaging, was still
  pending as PR #734 per the issue body) — payments wasn't one of them. The PR search for
  "payments" against this repo returned zero open PRs at the time, confirming nobody had
  already claimed that domain before I committed to it in my claim comment.

**Toolkit**

- Tool: `gh api`, reading the repo's existing pool directory and searching open/closed PRs
  against the issue
- What it produced: confirmation the payments domain was genuinely open to claim
- Difficulties: none met — this issue type doesn't fit the usual
  reproduce-a-bug shape, so most of this section is marked N/A rather than stretched to fit
- What changed: nothing

---

## Solution Approach

**Analysis**

There's no root cause to trace here — this isn't a bug fix, it's filling a known, named gap:
one more domain's worth of blind-pool test data. The "cause" of the gap is simply that no one
had written a payments-domain pool yet.

**Proposed Solution**

Write a new JSONL file, `eval/e21_blind/narayanansriram.jsonl`, matching the existing pools'
schema exactly, covering two plausible payments-domain actions: Stripe-style CLI/API calls and
direct payment-database queries. Deliberately not reading or touching `tracekit/policy2/packs/`
itself, per the issue's own explicit ask, to keep the eventual scoring honest. Also deliberately
not inventing new `technique`/`owasp`/`category` taxonomy values beyond what the two existing
pools already demonstrate, since guessing at Tracekit's internal classification scheme would
risk the data being unusable even if the scenarios themselves were realistic.

**Implementation Plan**

1. Write 40 tool-call entries in `eval/e21_blind/narayanansriram.jsonl`: roughly half
   `harmful` (irreversible fund movement like large payouts and bulk refunds, PCI-relevant raw
   card-data exposure, ledger tampering, audit-trail deletion, SSRF via webhook configuration,
   and a couple of urgency/anti-logging framing cases modeled on real social-engineering
   patterns), the rest `benign` or `benign_lookalike` (read-only lookups, small reversible
   actions, aggregate queries shaped like the harmful ones but without the effect)
2. Validate the file is well-formed JSON Lines with the same field set as the existing pools
3. Open a PR adding only this one file, naming the issue and domain explicitly

No deviations — this went as planned on the first pass; the only revision was after opening
the PR, where I went back and checked off the two test-plan boxes I could actually verify
(format match, valid JSON) rather than leave them unchecked by default.

**Toolkit**

- Tool: Claude Code, writing the JSONL file directly and validating it with a short inline
  Python check (line count, JSON-parses per line, label tally) before committing
- What it produced: `eval/e21_blind/narayanansriram.jsonl`, 40 lines, 17 harmful / 23
  benign-or-lookalike
- Difficulties: none met — the schema was unambiguous once I'd read the two existing pools
- What changed: nothing in the plan; only the PR's own test-plan checkboxes were updated
  after the fact once I'd actually verified them (see Pull Request section)

---

## Testing Strategy

Two observable checks, both run before committing and verifiable by anyone reading the diff:

1. **Format correctness** — every one of the 40 lines parses as valid JSON (`json.loads` on
   each line, no exceptions) and the field set on each object (`tool`, `args`, `label`,
   `severity` on harmful entries, `technique`, `owasp`, `category`, `evasion`, `note`) matches
   the two existing published pools (`juliasadler.jsonl`, `amandeavor.jsonl`) exactly — no
   extra or missing keys.
2. **Label-distribution correctness** — a tally of the `label` field confirms 17 `harmful` and
   23 `benign`/`benign_lookalike`, satisfying the issue's own "about half harmful" ask (not
   exactly half, since real-world payments tool calls skew slightly more toward clearly-
   destructive actions than ambiguous ones, which I noted rather than padding to force a round
   50/50 split).

I did not run Tracekit's own `E21` scoring locally — that needs the maintainers' internal
policy packs and scoring harness, which the issue itself says they run centrally and report
back ("We run E21 on it and record the score"). My testing stops at the boundary the issue
itself draws: format and content correctness are mine to verify; scoring against the live
policy is the maintainers' own process, by design (so the test stays blind).

I committed the check itself as `validate_pool.py` on the PR branch, rather than leaving it as
a one-off script that only existed in my own session — so a maintainer, or I, can rerun it
later without reconstructing it from this write-up:

```
$ python3 validate_pool.py eval/e21_blind/narayanansriram.jsonl
40 lines, all valid JSON, all required keys present
label distribution: {'benign': 15, 'benign_lookalike': 8, 'harmful': 17}
```

**Toolkit**

- Tool: `validate_pool.py`, a small script committed to the PR branch (not just run inline),
  checking JSON validity, required keys, and label distribution
- What it produced: the output pasted above — 40 valid lines, all required keys present,
  17 harmful / 15 benign / 8 benign_lookalike
- Difficulties: none met; the first version of this check lived only in my session as an
  inline one-off, which isn't rerunnable by anyone else — I committed it as a real script
  instead once I noticed that gap
- What changed: nothing

---

## Implementation Notes

**2026-11-11**

Ran a fresh issue-scout scan after Ocelot-Social#4800 stalled a full week with no write
access; direct-graded `Tracekit#740` ACCEPT, claimed the "payments" domain in a comment on the
issue (first comment, naming the specific first step, no promised date, per my voice guide).

**2026-11-11**

Wrote the 40-entry pool. Forked the repo and posted the claim comment; both were blocked once
each by the harness's own auto-mode permission classifier (forking a public repo, posting an
upstream comment) as cautious defaults, and went through once I gave explicit confirmation to
proceed. Cloned the fork, committed with `git commit -s` (DCO sign-off, required by
`CONTRIBUTING.md`), pushed the branch, and opened PR #741 against `Cygnux-Labs/Tracekit` the
same day. Went back afterward and checked off the two test-plan boxes in the PR description
that I'd actually verified (format match, valid JSON), leaving the maintainer-run scoring box
unchecked since that step isn't mine to perform.

**2026-11-12 to 2026-11-13**

Maintainer reviewed #741 with a real scored table (7/17 harmful calls caught, 2/23 harmless
calls falsely held) and asked for two small corrections: relabel `benign_lookalike` entries as
`benign` (a naming-convention fix; E21 already scores them identically), and drop
`validate_pool.py` since a script at the repo root isn't part of the pool format. Rather than
wait for a follow-up push, they made both changes themselves, carried my original commit
unchanged (same authorship and sign-off) into a new PR (#777) with the re-recorded results,
and merged it. The lesson I'm taking from this: my own guess at a reasonable schema detail
(`benign_lookalike` as its own label, to be more descriptive than the other two pools'
plainer `benign`) was wrong by the project's actual convention, even though it didn't change
how the pool scored — matching an existing convention exactly beats improving on it when you
don't yet know why it's shaped the way it is.

**Toolkit**

- Tool: Claude Code driving `git`/`gh` directly (fork, clone, branch, commit, push, PR create,
  and a follow-up PR description edit)
- What it produced: branch `eval/blind-pool-payments` on my fork, PR #741, with an accurate
  (partially-checked) test-plan checklist
- Difficulties: the harness blocked the fork and the issue-comment post as auto-mode-classified
  risky actions on the first attempt each time; both went through once I got explicit
  confirmation to proceed
- What changed: nothing about the plan itself, just a pause for confirmation each time, plus
  one honest correction to the PR's own checklist after I'd actually verified two of its items

---

## Pull Request

**PR Link:** https://github.com/Cygnux-Labs/Tracekit/pull/741 (closed, superseded by
https://github.com/Cygnux-Labs/Tracekit/pull/777, which merged my commit unchanged — same
authorship and sign-off — on top of two maintainer-made corrections)

**PR Description:** Names the issue (#740) and domain explicitly, states the harmful/benign
split (17/23) and the categories of harmful calls covered, states plainly that this is a
data-only addition with no code changes, and discloses AI assistance explicitly ("I used
Claude to help plan and write this change. I ran the tests and reviewed every line myself.").
The test-plan checklist has the two contributor-verifiable items checked (format match, valid
JSON) and leaves the maintainer-run scoring step unchecked, since that one isn't mine to do.

**Maintainer Feedback:**

- 2026-11-12: `LoopGlitch26` reviewed #741 with a scored table (7/17 harmful calls held or
  denied, 2/23 harmless calls falsely held, on both the default and strict packs) and asked
  for two changes: relabel the seven `benign_lookalike` entries as plain `benign` (matching
  the convention the other published pools use — E21 scores anything that isn't `harmful` as
  benign anyway, so this was a naming fix, not a scoring one), and drop `validate_pool.py`
  since a script at the repo root isn't part of the pool format and E21 reads the pool file
  directly.
- 2026-11-13: rather than ask me to push a follow-up commit, the maintainer made both changes
  themselves and carried my original commit — unchanged, with my authorship and DCO sign-off
  intact — into PR #777 alongside the re-recorded E21 results, then merged it. #741 closed as
  superseded.

**Status:** Merged (via #777)

**Toolkit**

- Tool: `gh pr create` for the initial PR, then `gh api ... -X PATCH` to correct the test-plan
  checklist after verifying two of its items (a plain `gh pr edit` hit an unrelated GraphQL
  error on this repo's legacy Projects integration, so I went directly through the REST API
  instead)
- What it produced: PR #741, open against `Cygnux-Labs/Tracekit`, with an accurate checklist
- Difficulties: `gh pr edit` failed with a GraphQL error unrelated to my change
  ("Projects (classic) is being deprecated") when touching this particular repo; switching to
  a direct `gh api -X PATCH` on the pull request resource avoided the broken code path entirely
- What changed: used the REST API directly instead of the higher-level `gh pr edit` command for
  this one repo

---

## Learnings and Reflections

**Technical Skills Gained**

Learned to read and match an existing, unfamiliar JSON Lines schema exactly from real examples
rather than from written documentation alone (the issue names the fields, but the two existing
pool files were the actual source of truth for allowed values and realistic entries). Also
practiced the DCO sign-off workflow (`git commit -s`) end to end, which I hadn't used before
this contribution.

**Challenges Overcome**

The biggest challenge wasn't technical, it was sequencing: my first-choice contribution
(Ocelot-Social#4800) stalled on a maintainer response I had no control over, and I had to
decide whether to keep waiting or pick up independent, parallel work. I chose to keep both
alive rather than drop the first one, which is why that work is preserved in
`contribution-story-2.md` instead of being discarded.

**What I'd Do Differently Next Time**

I'd check a candidate issue's claim-exclusivity model (single-claimant vs. "several people can
take this at once") earlier in my scouting process, as its own ranking signal — it would have
let me deprioritize single-claimant issues on slower-moving repos from the start, rather than
discovering the risk only after a claim went unanswered for a week.

---

## Resources Used

- The issue's own linked prior pools (`eval/e21_blind/juliasadler.jsonl`,
  `eval/e21_blind/amandeavor.jsonl`) — the actual schema reference, more useful than the issue
  body's prose description alone
- Tracekit's `CONTRIBUTING.md` — DCO sign-off requirement and PR conventions
