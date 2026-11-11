# Contribution: Evals: write 30 tool calls from a domain you know, without reading our rules (a blind pool)

**Contribution Number:** [2]
**Student:** Sriram Narayanan
**Issue:** https://github.com/Cygnux-Labs/Tracekit/issues/740
**Pull Request:** https://github.com/Cygnux-Labs/Tracekit/pull/741

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

```
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

### Unit 5: [one line on the tool work]

N/A — this contribution was picked after Unit 5's work, as a backup while my primary
contribution (Ocelot #4800) and a second claim (Nimbus #1529) were both waiting on maintainer
response. See contribution-story.md for the Unit 5 scout build.

### Unit 6: Patched scout caught a language-fit gap, direct-graded this pick, posted claim and PR same week

While waiting on two unresponsive claims, I ran a fresh issue-scout scan (patched rubric) over
my usual search strategy, then direct-graded a hand-fed candidate, `Cygnux-Labs/Tracekit#740`.
It passed every required check, including `Unclaimed` on an unusual basis: the issue itself
states "several people can take this at once," so there's no single-claimant risk the way
Ocelot/Nimbus have. I also direct-graded two other hand-fed URLs the same session —
`nanocoder#1971` (REJECT: an open linked PR, #1888, already attempts the exact fix, and the
issue's own author discloses a vendor conflict of interest) and a bulk scan hit,
`anniebritton/news-agent#6` (technically ACCEPT by the rubric, but flagged and skipped: zero
stars, single contributor, all issues bulk-created minutes before the scan, and the issue body
references a CONTRIBUTING.md that 404s — logged as another rubric gap, same family as the
llm4s language-fit miss from Ocelot's week). I claimed Tracekit#740 (comment with my chosen
domain, "payments," per the issue's own ask-before-starting instruction), wrote a 40-call
blind pool (`eval/e21_blind/narayanansriram.jsonl`, 17 harmful / 23 benign-or-lookalike,
matching the existing pools' exact JSON schema), forked the repo, committed with a DCO
sign-off, and opened PR #741 same day — no claim-and-wait step needed here since the issue
explicitly allows starting without an assignment.

### Unit 7: [one line on the tool work]

[2-4 sentences: what you built or changed (the ship-gate, your
manifest), what broke or fell short, what changed because of it.]

### Unit 8: [one line on the tool work]

[2-4 sentences: the monitor: what it read, what it drafted, what you
did with the draft (sending and declining both count), what changed
because of it.]

---

## Why I Chose This Issue

Both of my first two claims (Ocelot #4800, Nimbus #1529) were stalled on maintainer response,
so I asked my scout to find another backup. A fresh scan over my existing strategy turned up
mostly low-signal, zero-star personal repos (bulk-created issues, broken CONTRIBUTING links),
so I pivoted to grading hand-fed candidates directly, including a real one I spotted,
`Cygnux-Labs/Tracekit#740`. It fits my profile (Python, writing structured test data, no
deep-systems or math background needed) and graded ACCEPT/straightforward: a real, active
repo (646 stars, pushed the same day, multiple human commit authors), a real CONTRIBUTING.md
with a DCO requirement, no AI-contribution ban, and — unusually — the issue says explicitly
that several people can take it at once, which removes the single-claimant risk my other two
picks are stuck on.

The scout's biggest miss this round wasn't on this issue, it was on the ones I didn't take:
grading `anniebritton/news-agent#6` taught me the rubric has no "is this a real, staffed
project" check — a brand-new, zero-star, single-contributor repo with issues dumped in bulk
minutes before my scan passes every required check as written. I didn't patch the rubric for
this (same call I made with the language-fit gap after llm4s): logging it in `scan-notes.md`
as a known blind spot rather than overfitting the rubric to one bad scan.

**Toolkit**
- Tool: issue-scout (patched Unit-6 rubric: `scope-feasible` + difficulty note), run in a
  fresh scan, then in direct mode on three hand-fed URLs (Tracekit#740, nanocoder#1971,
  news-agent#6)
- What it produced: Tracekit#740 ACCEPT/straightforward; nanocoder#1971 REJECT (fails
  Unclaimed — an open linked PR, #1888, already implements the exact fix); news-agent#6
  ACCEPT by the letter of the rubric but flagged and skipped on judgment
- Difficulties: the rubric still has no check for "is this a real, actively-staffed project"
  versus a fresh solo/personal repo — news-agent#6 exposed this the same way llm4s#2227
  exposed the missing language-fit check in Unit 6
- What changed: nothing in the rubric itself yet; logged the gap in `scan-notes.md` as a
  candidate for a future patch rather than reacting to one bad scan

---

## Understanding the Issue

### Problem Description

[In your own words, what is broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

**Toolkit**
- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

### Environment Setup

No local app environment needed — this issue asks for a data file (a JSONL eval pool), not a
code change, so there's nothing to reproduce in the usual sense. Instead, I fetched the two
existing published pools (`eval/e21_blind/juliasadler.jsonl`, `amandeavor.jsonl`) via the
GitHub API to confirm the exact schema before writing my own: `tool`, `args`, `label`
(harmful/benign/benign_lookalike), `severity` (harmful only), `technique`, `owasp`,
`category`, `evasion`, `note`.

### Steps to Reproduce

N/A — there is no bug to reproduce; this is a content-contribution issue (add a new blind
test pool), not a defect report.

### Reproduction Evidence

- **Commit showing reproduction:** N/A, see above
- **Screenshots/logs:** N/A
- **My findings:** Confirmed the schema from the two existing pools matched what the issue
  body described, and confirmed via `gh api .../pulls/<n>` that no one else had already
  claimed the "payments" domain specifically (the issue's history names "cloud/databases,"
  "CI/CD and identity," and "email and messaging" as the three domains taken so far).

**Toolkit**
- Tool: `gh api` directly, reading the repo's existing pool files and PR history
- What it produced: a confirmed schema to match and a confirmed-open domain to claim
- Difficulties: none met — this issue type doesn't fit the Reproduction Process section's
  usual shape, so most of it is marked N/A above rather than stretched to fit
- What changed: nothing

---

## Solution Approach

### Analysis

No root cause — this isn't a bug fix. The "gap" is a missing test pool: Tracekit's agent-call
policy packs are tuned by people who've read the rules, so the project explicitly wants
pools written by people who haven't, to get an honest read of recall against unseen domains.
Payments was an open domain (not yet covered by the three existing contributor pools).

### Proposed Solution

Write a new JSONL file, matching the existing pools' schema exactly, covering Stripe CLI
operations and direct payment-table database queries — the two places a payments-domain
agent would plausibly act. Deliberately not touching `tracekit/policy2/packs/` itself (the
issue explicitly asks contributors not to read it before writing their pool, to keep the test
honest), and not trying to guess at Tracekit's internal severity/technique taxonomy beyond
matching the patterns already visible in the two published pools.

### Implementation Plan

1. Write 40 tool-call entries in `eval/e21_blind/narayanansriram.jsonl`: roughly half
   `harmful` (irreversible fund movement, PCI-relevant data exposure, ledger tampering,
   audit-trail deletion, SSRF via webhook config, prompt-injection/urgency framing), the rest
   `benign` or `benign_lookalike` (read-only lookups, small reversible actions, aggregate
   queries that resemble the harmful ones in shape but not effect)
2. Validate the file is well-formed JSONL with the same field set as the existing pools
3. Open a PR adding only this one file

No deviations — went as planned on the first pass.

**Toolkit**
- Tool: Claude Code, writing the JSONL file directly and validating it with a short Python
  check (line count, label distribution, JSON-parses) before committing
- What it produced: `eval/e21_blind/narayanansriram.jsonl`, 40 lines, 17 harmful / 23
  benign-or-lookalike
- Difficulties: none met — the schema was unambiguous once I read the two existing pools
- What changed: nothing

---

## Testing Strategy

Checked the file is valid JSONL (one JSON object per line, all 40 parse), confirmed the
label distribution is roughly half harmful per the issue's ask (17/40 harmful), and diffed
my field set against the two existing published pools to confirm I used the same keys in the
same shape. I did not run `make eval`/E21 itself locally — that requires the maintainers'
policy packs and scoring harness, which the issue says they run centrally and report back
("We run E21 on it and record the score"), so my testing stops at format correctness, same
boundary the issue itself draws.

**Toolkit**
- Tool: a short inline Python check (line count, `json.loads` per line, label tally)
- What it produced: confirmation of 40/40 valid lines, 17 harmful / 23 benign-or-lookalike
- Difficulties: none met
- What changed: nothing

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

### 2026-11-11

Graded Tracekit#740 ACCEPT, claimed the "payments" domain in a comment on the issue.

### 2026-11-11

Wrote the 40-entry pool, forked the repo (blocked once by the harness's own permission
classifier on creating a public fork — the user approved it explicitly, then it went
through), cloned it, committed with `git commit -s` (DCO sign-off, required by
CONTRIBUTING.md), pushed the branch, and opened PR #741 against `Cygnux-Labs/Tracekit`
same day.

**Toolkit**
- Tool: Claude Code (git/gh operations: fork, clone, branch, commit, push, PR create)
- What it produced: branch `eval/blind-pool-payments` on my fork, PR #741
- Difficulties: the harness blocked the fork (`gh repo fork`) and the issue-comment post
  (`gh issue comment`) as auto-mode-classified risky actions on the first attempt each time;
  both went through once I got explicit confirmation to proceed
- What changed: nothing about the plan, just required a pause for confirmation

---

## Pull Request

**PR Link:** https://github.com/Cygnux-Labs/Tracekit/pull/741

**PR Description:** Adds `eval/e21_blind/narayanansriram.jsonl` per #740: 40 tool calls in
the payments domain, 17 harmful / 23 benign-or-lookalike, written without reading
`tracekit/policy2/packs/` first, matching the existing pools' schema. Notes that this is
format-correctness-tested only; scoring runs centrally per the issue's own process.

**Maintainer Feedback:**

- 2026-11-11: none yet

**Status:** Awaiting review

**Toolkit**
- Tool: `gh pr create` with a body written to match the repo's own PR conventions (a Summary
  and a Test plan section, matching the style other merged pools' PRs likely used)
- What it produced: PR #741, open against `Cygnux-Labs/Tracekit`
- Difficulties: none met
- What changed: nothing

---

## Learnings and Reflections

### Technical Skills Gained

[What you learned technically, named concretely.]

### Challenges Overcome

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

### What I'd Do Differently Next Time

[Reflection on your process.]

---

## Resources Used

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
