# Contribution: Auto-focus URL field while editing in social media list

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Sriram Narayanan
**GitHub Username:** narayanansriram
**Issue:** https://github.com/Ocelot-Social-Community/Ocelot-Social/issues/4800
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

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
next time.

**Unit 6: Installed a voice-guard hook and patched issue-scout's rubric for wild-caught scope**

I folded the three per-unit voice-guide copies into one canonical `voice-guide.md`, added an
"Upstream: how I talk to maintainers" section covering claims/questions/pings, and wired a
`PreToolUse` hook (`check_voice.py`) that blocks a comment-posting command unless it matches
the guide's rules — verified it by running `gh issue comment --help` in a fresh session and
confirming the hook stopped it. I also patched `issue-scout/rubric.md`: the unit-1
`Scope-Fits-Newcomer` check was too strict for real repos (it had rejected real, claimable
issues over open design edges), so I replaced it with a `scope-feasible` check plus a
difficulty note (straightforward/moderate/ambitious) that no longer gates the verdict. One
thing I changed my mind on: the patch flipped `home-assistant/frontend#5276` from reject to
accept, and I nearly switched my pick to it, but a live repro check showed the issue's most
recent investigator called it "not reproducible" and recommended closing it — so I stayed
with Ocelot #4800, whose open edge (an undocumented Vue slot boundary) is a scoping question
rather than a does-this-bug-exist question. Nothing broke in the hook itself once the user
added the `PreToolUse` block to `settings.json` by hand (I'm blocked from self-editing that
file) — confirmed present via a verification one-liner before relying on it.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I picked this issue directly (a hand-fed URL rather than a scan hit) because the fit profile
I'd written for issue-scout is centered on reading unfamiliar codebases and getting
comfortable with real UI component boundaries, and this issue — "auto-focus the URL field
while editing in a social media list" — is a small, self-contained Vue behavior change that
fits that profile closely. I graded it with my scout and it came back **ACCEPT**: the
maintainers pushed to the default branch the same day I graded it, the repo isn't archived
and has an active release, there's no AI-contribution ban in CONTRIBUTING.md, and nobody has
ever claimed or commented on the issue in the roughly four years it's been open.

What the scout's verdict doesn't fully capture is the issue's real difficulty. Under my Unit
6 rubric patch (`scope-feasible` replacing the stricter `Scope-Fits-Newcomer` check), it still
accepts, but now carries a difficulty note of **ambitious**: the issue body itself admits the
fix "seems to be a bit more difficult" because the editable field lives inside a Vue slot, and
nothing in the thread names where that slot boundary actually is. I nearly switched my pick
after the same patch flipped `home-assistant/frontend#5276` from reject to accept, but a live
repro check on that issue showed its most recent investigator explicitly called the original
bug report "not reproducible" and recommended closing it — a much worse bet than an issue with
an honest open edge. Staying with this issue means my first real step upstream is a scoping
question, not a does-the-bug-still-exist question, which is the trade I'd rather make as a
first contribution.

**Claim comment:** https://github.com/Ocelot-Social-Community/Ocelot-Social/issues/4800#issuecomment-6645861233

**Follow-up:** no maintainer response as of several days after the claim (no assignee, no
volunteers-team invite). Per my voice guide's ping rule — follow up only once the repo's own
rhythm suggests a reply is overdue, not on my own impatience — I checked other recent threads
on the repo and found the maintainer responding same-day elsewhere, so I posted a single,
plain follow-up rather than waiting indefinitely:
https://github.com/Ocelot-Social-Community/Ocelot-Social/issues/4800#issuecomment-6753356256

**Toolkit**

- Tool: issue-scout (my search strategy and unit-1 rubric, patched in Unit 6), used in direct
  mode by hand-feeding it this issue's URL rather than via a scan
- What it produced: an ACCEPT verdict with all six checks passing, plus (after the Unit 6
  patch) a `scope-feasible` pass and an "ambitious" difficulty note flagging the undocumented
  Vue slot boundary as the issue's real open edge
- Difficulties: the pre-patch rubric's scope check conflated "is this one piece of work" with
  "is this confirmed-reproducible," which nearly cost me a correct pick (it flipped
  `home-assistant/frontend#5276` to accept even though that issue's own thread says the bug
  can't be reproduced) — the scout's verdict alone wasn't enough there; I had to run a live
  repro check to catch it
- What changed: replaced the `Scope-Fits-Newcomer` check with `scope-feasible` plus a
  difficulty note (logged in `scan-notes.md`), so scope now informs rather than silently
  rejecting or silently accepting a real risk

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

macOS, Docker 20.10.21 / Compose v2.13.0 available locally. I forked the repo to
`narayanansriram/Ocelot-Social` and cloned it, copied `webapp/.env.template` and
`backend/.env.template` to `.env`, and had `docker compose up` ready to go per the README's
Quick Start. The sandboxed shell I'm working in blocks spinning up long-running background
containers, so I was not able to actually bring the stack up and drive the browser — I did a
static code-level reproduction instead, tracing the exact code path through the source rather
than observing it live, and said so plainly in my findings below rather than claiming a
browser-verified repro.

**Steps to Reproduce**

1. Open `webapp/pages/settings/my-social-media.vue` and click "edit" on an existing social
   media link. This calls `callbackEditSocialMedia(thisList, link)` (my-social-media.vue:142).
2. `MySomethingList.vue` sets `isEditing` true and renders the `edit-item` slot under
   `v-if="isEditing"` (MySomethingList.vue:3) — a fresh mount on every edit, not a show/hide.
3. The slot content is `<ocelot-input id="editSocialMedia" ...>` with no `autofocus` prop
   passed (my-social-media.vue:17-23).
4. Observed result (from source, not the browser): `callbackEditSocialMedia`'s body only sets
   `formData.socialMediaUrl`; every focus-setting line is commented out:
   ```js
   // try to set focus on link edit field
   // thisList.$refs.socialMediaUrl.$el.focus()
   // thisList.$scopedSlots['edit-item']()[0].$el.focus()
   ```

**Reproduction Evidence**

- **Commit showing reproduction:** no fix branch yet; evidence below is a read of `master` at
  my fork, `narayanansriram/Ocelot-Social`.
- **Screenshots/logs:** none — I could not run the live app in this environment, see findings.
- **My findings:** The commented-out focus attempts in `my-social-media.vue:142-152` are a
  direct trail from a prior contributor confirming the field never gets focus today, and that
  someone already tried (and apparently abandoned) reaching across the slot boundary
  imperatively from the parent via `$refs`/`$scopedSlots`. What the trace surfaced that I
  didn't expect: `OcelotInput.vue:13-29` already accepts an `autofocus` prop and forwards it
  straight to the native element's `autofocus` attribute, and `MySomethingList.vue:3`'s
  `v-if="isEditing"` confirms the edit slot is freshly mounted each time (not just shown),
  which is exactly what the HTML `autofocus` attribute needs to fire. So the fix the issue
  calls "a bit more difficult" is likely a one-line `:autofocus="true"` on the `ocelot-input`
  in the edit-item slot — no slot-crossing `$refs` hack needed. I plan to confirm this by
  actually running the app once I have push access and can retry `docker compose up` outside
  this sandbox, before finalizing the implementation plan.

**Toolkit**

- Tool: repro-check (my Unit 2 skill, patched for the wild in Unit 6's scope.md), graded in
  live mode against this claim + repro draft
- What it produced: an ACCEPT verdict across all seven checks, with one honest caveat it made
  me state explicitly — Behavior-Matches-Issue passed on a code trace, not a browser-observed
  symptom, since I couldn't run the stack here
- Difficulties: the sandboxed environment blocked `docker compose up` as a background/risky
  action, so I couldn't get a live browser reproduction; repro-check's Honest-Outcome check is
  exactly what pushed me to state that delta plainly instead of writing around it
- What changed: nothing in the skill itself — it worked as designed and caught the gap between
  "traced" and "observed" that I'd have been tempted to blur over

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
