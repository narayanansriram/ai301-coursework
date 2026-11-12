# Contribution: Auto-focus URL field while editing in social media list

**Contribution Number:** [2]
**Student:** Sriram Narayanan
**Issue:** https://github.com/Ocelot-Social-Community/Ocelot-Social/issues/4800
**Pull Request:** [blocked — see note below]

<!--
This is the blank template for an extra contribution. Copy this file
to contribution-story-2.md (then -3, and so on) to record another
one, and set Contribution Number to match. Extra contributions are
welcome and never graded; contribution-story.md is the graded one.
-->

This was my original pick. It's real, substantial work (scout build, reproduction, root-cause
analysis, implementation plan), but it has no pull request yet: Ocelot-Social's own
CONTRIBUTING.md requires branching on the main repo rather than a fork (for CI credential
reasons), which requires a volunteers-team invite. I checked the issue's `assignees` field
after my follow-up ping and found `["ulfgebhardt"]` — that's the maintainer's own GitHub
username, not mine, so I have not actually been assigned this issue; I'd misread that signal
in an earlier draft of this story. I still have no invite and no write access, so there's no
branch to open a PR from. Rather than leave my whole submission blocked on that invite, I
picked up a second, independently-workable issue (`Cygnux-Labs/Tracekit#740`, PR #741) as the
graded contribution in `contribution-story.md`, and kept this one going in parallel here.

---

## Toolkit

### Unit 5: Built issue-scout, a search-strategy-driven scanner over my unit-1 rubric

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

### Unit 6: Installed a voice-guard hook and patched issue-scout's rubric for wild-caught scope

I folded three per-unit voice-guide copies into one canonical `voice-guide.md`, added an
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
rather than a does-this-bug-exist question. I claimed the issue, got no response for a week,
and posted a single follow-up ping once I confirmed (by checking other recent threads on the
repo) that the maintainer typically responds same-day elsewhere. That ping still hasn't drawn
a reply or a volunteers-team invite — see the note at the top of this file for the assignee
mix-up I caught and corrected, and for why this contribution has no PR to show yet.

### Unit 7: [one line on the tool work]

[2-4 sentences: what you built or changed (the ship-gate, your
manifest), what broke or fell short, what changed because of it.]

### Unit 8: [one line on the tool work]

[2-4 sentences: the monitor: what it read, what it drafted, what you
did with the draft (sending and declining both count), what changed
because of it.]

---

## Why I Chose This Issue

I picked this issue directly (a hand-fed URL rather than a scan hit) because the fit profile
I'd written for issue-scout is centered on reading unfamiliar codebases and getting
comfortable with real UI component boundaries, and this issue — "auto-focus the URL field
while editing in a social media list" — is a small, self-contained Vue behavior change that
fits that profile closely. I graded it with my scout and it came back **ACCEPT**: the
maintainers pushed to the default branch the same day I graded it, the repo isn't archived
and has an active release, there's no AI-contribution ban in CONTRIBUTING.md, and nobody had
ever claimed or commented on the issue in the roughly four years it had been open.

What the scout's verdict doesn't fully capture is the issue's real difficulty. Under my Unit
6 rubric patch (`scope-feasible` replacing the stricter `Scope-Fits-Newcomer` check), it still
accepts, but now carries a difficulty note of **ambitious**: the issue body itself admits the
fix "seems to be a bit more difficult" because the editable field lives inside a Vue slot, and
nothing in the thread names where that slot boundary actually is. I nearly switched my pick
after the same patch flipped `home-assistant/frontend#5276` from reject to accept, but a live
repro check on that issue showed its most recent investigator explicitly called the original
bug report "not reproducible" and recommended closing it — a much worse bet than an issue with
an honest open edge. Staying with this issue meant my first real step upstream was a scoping
question, not a does-the-bug-still-exist question, which was the trade I'd rather make as a
first contribution. It's also the trade that ended up costing me time: real newcomer-facing
scoping work on an under-maintained part of the repo took a full week of waiting, which is
exactly the kind of risk the scout's verdict doesn't model.

**Claim comment:** https://github.com/Ocelot-Social-Community/Ocelot-Social/issues/4800#issuecomment-6645861233

**Follow-up:** https://github.com/Ocelot-Social-Community/Ocelot-Social/issues/4800#issuecomment-6753356256

**Toolkit**

- Tool: issue-scout (my search strategy and unit-1 rubric, patched in Unit 6), used in direct
  mode by hand-feeding it this issue's URL rather than via a scan
- What it produced: an ACCEPT verdict with all six checks passing, plus (after the Unit 6
  patch) a `scope-feasible` pass and an "ambitious" difficulty note flagging the undocumented
  Vue slot boundary as the issue's real open edge
- Difficulties: the pre-patch rubric's scope check conflated "is this one piece of work" with
  "is this confirmed-reproducible," which nearly cost me a correct pick; the scout's verdict
  alone also doesn't model claim-response latency, which is what actually cost me time here
- What changed: replaced the `Scope-Fits-Newcomer` check with `scope-feasible` plus a
  difficulty note (logged in `scan-notes.md`); separately logged (but didn't yet patch) the
  missing claim-latency signal as a gap for a future rubric revision

---

## Understanding the Issue

### Problem Description

When a user clicks "edit" on one of their social media links in account settings, the URL
input field that appears does not receive keyboard focus automatically. The user has to click
into the field themselves before they can start typing, which is a small but avoidable extra
step on every edit.

### Expected Behavior

The URL input should be focused as soon as it's rendered for editing, the same way most
inline-edit UI patterns work: click "edit," the field appears already focused, start typing.

### Current Behavior

The field renders but stays unfocused. A prior contributor already tried to fix this by
reaching into the field from the parent component (`thisList.$refs.socialMediaUrl.$el.focus()`
and a `$scopedSlots` variant), but both lines are commented out in the current code — they
either didn't work or were abandoned mid-attempt, with no explanation left behind.

### Affected Components

- `webapp/pages/settings/my-social-media.vue` — owns `callbackEditSocialMedia`, where the
  abandoned focus attempts live, and supplies the `edit-item` slot content
- `webapp/components/_new/features/MySomethingList/MySomethingList.vue` — the generic list
  component whose `v-if="isEditing"` controls when the slot (and the input inside it) mounts
- `webapp/components/_new/generic/OcelotInput/OcelotInput.vue` — the actual input component
  rendered inside the slot; already accepts an `autofocus` prop today, it's just never passed

**Toolkit**

- Tool: direct reading of the three files above (via a scoped Explore sub-agent limited to the
  social-media-editing subsystem, per my repo's `CLAUDE.local.md` size-discipline note)
- What it produced: confirmation that the issue's own "it's inside a Vue slot, more difficult"
  framing is accurate, and that the fix target is different from what I first guessed
  (`my-social-media.vue`, not the read-only `SocialMedia.vue`/`SocialMediaListItem.vue`)
- Difficulties: the two read-only display components share similar names with the real
  edit-path files, which cost me one wrong turn before the scoped search corrected it
- What changed: nothing in the tooling; I just had to actually read the component tree instead
  of guessing from the issue title alone

---

## Reproduction Process

### Environment Setup

macOS, Docker 20.10.21 / Compose v2.13.0 available locally. I forked the repo to
`narayanansriram/Ocelot-Social` and cloned it, copied `webapp/.env.template` and
`backend/.env.template` to `.env`, and had `docker compose up` ready to go per the README's
Quick Start. The sandboxed shell I'm working in blocks spinning up long-running background
containers, so I was not able to actually bring the stack up and drive the browser — I did a
static code-level reproduction instead, tracing the exact code path through the source rather
than observing it live, and said so plainly in my findings below rather than claiming a
browser-verified repro.

### Steps to Reproduce

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

### Reproduction Evidence

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

### Analysis

The root cause is that `callbackEditSocialMedia` in `my-social-media.vue` (around lines
142-152) only updates `formData.socialMediaUrl` — it never sets focus, because the two
imperative attempts to do so from the parent (`thisList.$refs.socialMediaUrl.$el.focus()` and
`thisList.$scopedSlots['edit-item']()[0].$el.focus()`) are commented out. Both of those were
trying to reach across the `edit-item` scoped slot boundary from `MySomethingList.vue` — Vue's
`$refs`/`$scopedSlots` don't reliably expose a child element rendered inside a slot the way
the parent needs here, which is almost certainly why they were abandoned rather than why they
were buggy in some fixable way.

The simpler path: `OcelotInput.vue` (lines 13-29) already accepts and forwards an `autofocus`
prop straight onto the native `<input>` element's `autofocus` attribute — it's just never
passed when the component is used in the edit-item slot in `my-social-media.vue` (lines
17-23). And `MySomethingList.vue`'s `v-if="isEditing"` (line 3) means the slot's `ocelot-input`
is freshly mounted every time edit mode opens, not just shown/hidden — which is exactly the
condition under which the native HTML `autofocus` attribute fires. So there's no need to
cross the slot boundary imperatively at all; the declarative prop path already exists and is
simply unused.

### Proposed Solution

Pass `:autofocus="true"` to the `ocelot-input` in the `edit-item` slot in
`my-social-media.vue`, and delete the two commented-out, abandoned `$refs`/`$scopedSlots`
lines in `callbackEditSocialMedia` rather than leaving dead code behind. I'm deliberately NOT
touching `MySomethingList.vue` itself — the `v-if="isEditing"` remount behavior it already has
is exactly what makes the prop-only fix work, so there's nothing to change there.

### Implementation Plan

1. Add `:autofocus="true"` to the `<ocelot-input id="editSocialMedia" ...>` element in the
   `edit-item` slot, `my-social-media.vue` (around lines 17-23)
2. Delete the two commented-out focus-attempt lines in `callbackEditSocialMedia`,
   `my-social-media.vue` (lines 142-152), since the prop makes them unnecessary
3. Add or extend a test for `my-social-media.vue`'s edit-mode rendering to assert the
   `autofocus` attribute is present on the rendered input (no existing spec file covers this
   component's edit path; `SocialMedia.spec.js`/`SocialMediaListItem.spec.js` only test the
   read-only display components)

No deviations yet — this plan is unverified against a running app (see Reproduction Process's
stated delta); I expect to confirm or revise step 1 once I have push access and can actually
run `docker compose up` to watch focus behavior live, since HTML `autofocus` has known browser
quirks (it can be ignored on some re-renders depending on timing) that a static code trace
can't fully rule out.

**Toolkit**

- Tool: plan-check (patched for the wild via Unit 6's `scope.md`), used to sanity-check this
  plan against the issue and my own reproduction evidence before writing it here
- What it produced: confirmed the plan's scope matches what the reproduction surfaced (the
  `autofocus` prop path) rather than re-attempting the already-abandoned `$refs` approach
- Difficulties: none met — the plan follows directly from the reproduction trace
- What changed: nothing; the plan as graded is the plan as written above

---

## Testing Strategy

Not yet run — this contribution is blocked before implementation: I have no push access to
open a branch on Ocelot-Social's main repo (their CONTRIBUTING.md requires branching there
directly, not from a fork, for CI credential reasons), so there's no running build to test
against yet. Once unblocked, the plan is the same two-check shape as my other contribution:
(1) re-run the reproduction steps above against the built change and confirm the `autofocus`
attribute is present and functional in a live browser, not just in source; (2) run the repo's
own webapp test suite (`npm run lint -- --fix && npm test` per `CLAUDE.local.md`) plus the new
or extended spec from the Implementation Plan's step 3.

**Toolkit**

- Tool: none yet — this step is blocked on write access
- What it produced: N/A
- Difficulties: blocked on the volunteers-team invite (see note at top of this file)
- What changed: nothing yet

---

## Implementation Notes

**2026-11-04 through 2026-11-11**

Claimed the issue, got no response, and posted one voice-guide-compliant follow-up ping.
Still no write access as of this writing, so no branch and no implementation yet. Kept the
reproduction and solution-planning work going in parallel rather than waiting idle, so the
actual implementation can start immediately once I have access.

**2026-11-11 (correction)**

Re-checked `gh api repos/Ocelot-Social-Community/Ocelot-Social/issues/4800 --jq
'{assignees:[.assignees[].login]}'` directly and got `{"assignees":["ulfgebhardt"]}` —
`ulfgebhardt` is the maintainer's own username, so this issue is not assigned to me. An
earlier draft of this story said "assigned shortly after," which was wrong; I'm correcting it
here rather than leaving the mismatch between that claim and this section's own evidence.

**Toolkit**

- Tool: `gh api`, checked periodically for assignment/invite status
- What it produced: `{"assignees":["ulfgebhardt"]}` — confirms the issue carries no assignment
  to me, only the maintainer's own username; no invite exists either
  (`gh api "user/memberships/orgs?state=pending"` returns `[]`)
- Difficulties: no control over the maintainer's response timeline; also caught my own
  misreading of this exact output in an earlier pass of this story and corrected it above
- What changed: picked up a second, independent contribution (`contribution-story.md`) rather
  than let the whole submission wait on this one

---

## Pull Request

**PR Link:** Not yet — blocked on a volunteers-team invite (see note at top of this file)

**PR Description:** N/A yet

**Maintainer Feedback:**

- 2026-11-11: no comment reply, no assignment to me, and no invite; silence is a normal state
  and costs nothing

**Status:** Blocked (pre-implementation, waiting on write access)

**Toolkit**

- Tool: none yet
- What it produced: N/A
- Difficulties: no push access to Ocelot-Social's main repo
- What changed: nothing yet; will update this section once unblocked

---

## Learnings and Reflections

### Technical Skills Gained

Learned to trace a Vue 2 scoped-slot boundary issue to its actual root cause (a declarative
prop path that already existed but was unused) rather than assuming the fix required the more
complex imperative `$refs`/`$scopedSlots` approach a prior contributor had already abandoned.

### Challenges Overcome

The real challenge here wasn't technical, it was process: a maintainer-response bottleneck
entirely outside my control. Rather than treat that as a dead end, I used the wait productively
(finishing the reproduction and solution plan) and picked up independent work elsewhere so the
overall submission wasn't hostage to one repo's response time.

### What I'd Do Differently Next Time

I'd weigh a repo's historical claim-to-assignment latency more heavily before committing to an
issue as a primary pick, especially for a repo with an unusual contribution model (branch, not
fork) that gates even reproduction work behind an invite.

---

## Resources Used

- Ocelot-Social's own `CONTRIBUTING.md` — branch-not-fork policy, PR requirements
- The prior contributor's commented-out focus attempts in `my-social-media.vue` — the single
  most useful piece of evidence in this whole investigation, since it showed exactly what had
  already been tried and ruled out
