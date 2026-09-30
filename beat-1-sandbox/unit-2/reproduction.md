# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

narayanansriram

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5902974754

I'd like to take https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62 as a my AI301 PathReview contribution.

The health probe in `api/routes/health.py` builds its Redis client from `settings.redis_host`/`settings.redis_port`, but `Settings` in `core/config.py` only defines `redis_url` — so the probe's `except Exception` swallows the resulting `AttributeError` and reports Redis as unhealthy even when it's reachable. I'm going to set up the sandbox, hit `GET /health` with Redis running, and confirm the 503/`redis_health_check_failed` log entry the issue describes, then report back what I find.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5903082398

> I'd like to take [#62](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62) as a my AI301 PathReview contribution.
>
> The health probe in `api/routes/health.py` builds its Redis client from `settings.redis_host`/`settings.redis_port`, but `Settings` in `core/config.py` only defines `redis_url` — so the probe's `except Exception` swallows the resulting `AttributeError` and reports Redis as unhealthy even when it's reachable. I'm going to set up the sandbox, hit `GET /health` with Redis running, and confirm the 503/`redis_health_check_failed` log entry the issue describes, then report back what I find.

Repro here:

**Environment:** Python 3.11.0, pydantic 2.13.5, pydantic-settings, redis-py 8.1.0, macOS. Repo at `main`, no `.env` overrides (default `Settings`). No Redis server running locally for this test.

**Scope:** this isolates the `Redis` block of `api/routes/health.py`'s `health_check` (the exact three lines it runs against `settings`), rather than standing up the full docker-compose stack — the bug is in that code path itself, independent of whether Redis or Postgres are actually up.

**Steps:**

1. From the repo root, confirm `Settings` only defines `redis_url`, not `redis_host`/`redis_port`:
```
$ python3 -c "from core.config import settings; print([f for f in ('redis_url','redis_host','redis_port') if hasattr(settings, f)])"
['redis_url']
```
2. Run the exact code `health_check` executes for the Redis dependency:
```python
import redis
from core.config import settings

try:
    r = redis.Redis(host=settings.redis_host, port=settings.redis_port, db=0, decode_responses=True)
    r.ping()
    print("redis healthy")
except Exception as exc:
    print("redis_health_check_failed", type(exc).__name__, str(exc))
```
Output:
```
redis_health_check_failed AttributeError 'Settings' object has no attribute 'redis_host'
```
3. Control — same block, but built from the field that actually exists (`redis_url`) instead of the missing `redis_host`/`redis_port`:
```python
from urllib.parse import urlparse
u = urlparse(settings.redis_url)
r = redis.Redis(host=u.hostname, port=u.port, db=0, decode_responses=True)
r.ping()
```
Output:
```
redis_health_check_failed (using redis_url) ConnectionError Error 61 connecting to localhost:6379. Connection refused.
```

**Expected:** with no Redis server running, the health check should report `redis: unhealthy` via a `ConnectionError` from a real connection attempt (as the control shows) — the same failure mode it would show if Redis were genuinely down.

**Actual:** the shipped code raises `AttributeError: 'Settings' object has no attribute 'redis_host'` before it ever attempts a connection. `except Exception` catches this and logs `redis_health_check_failed`, so `/health` reports Redis as unhealthy (503) even when Redis is reachable — confirming the issue's root cause exactly: the probe reads `settings.redis_host`/`settings.redis_port`, which don't exist, instead of the `redis_url` field `Settings` actually defines.

## Eval iterations

**Run history**

- `--limit 3` smoke run: `agreement: 2/3 scored items` (pkg-03 failed on `Evidence-Not-Assertion`)
- `--only pkg-03` (after loosening `Evidence-Not-Assertion` to allow a secondary confirmatory detail, like a control run's result, to be stated in prose rather than requiring its own separate output block): `agreement: 0/1 scored items` (pkg-03 now failed on `AI-Disclosure` instead)
- `--only pkg-03,pkg-01,pkg-02` (after splitting `AI-Disclosure` into two distinct sub-rules — a disclosure requirement vs. a "comments must be human-voiced" requirement — since ripgrep's policy is the latter, not the former, and my first version conflated them): `agreement: 3/3 scored items`
- Full run, final (`--save-run eval-run.txt`): `agreement: 18/20 scored items (bar: 18/20: PASS)`, categories: `clear-accept 6/8 disclosure 1/1 no-evidence 4/4 unfollowable-comms 3/3 wrong-target 4/4`

**Package analysis**

`pkg-05`. My rubric's decision: **reject** (failed `Steps-Followable`). Gold label: **accept**. Reasoning: the candidate repro report describes writing "a minimal `env.yml` containing a valid `dependencies:` list plus a `category:` section (the section conda does not recognize)" but never shows the literal file contents inline — it's a prose description of what the file contains, not a copy-pasteable block. My `Steps-Followable` check's pass condition requires "inline config content, inline input files ... shown inline, not referenced from a location only the author can reach," and I graded this prose description as falling short of that bar. Looking at it again, the gold label's implicit read is more lenient: the description is precise enough (a valid `dependencies:` list plus one added `category:` key) that a stranger could reconstruct an equivalent file and hit the same code path, even without a verbatim block. This is one of the genuinely arguable calls the assignment describes — my rubric drew the "shown inline" line more strictly than the gold labels do, and I left it as-is rather than loosen it further, since loosening `Steps-Followable` broadly risks accepting reports that gesture at "a config like the issue's" without pinning down anything a stranger could actually reproduce (which is exactly what several `unfollowable-comms` and `wrong-target` packages in this set do wrong).

**Check rationale**

> AI-Disclosure | The repo-facts block's stated contribution/AI-use policy, read against the claim comment and repro report text | Read the policy for which of two distinct rules it states, if any. (a) A disclosure rule: comments must state that AI was used and to what extent. Pass only if the comments contain an explicit disclosure statement; fail if the policy states this rule and neither comment discloses (course packages are treated as AI-assisted work). (b) A human-voice rule: comments must be written by a human in their own words (no disclosure statement required, just no AI-generated-sounding text passed off as the contributor's own). Pass if the comments read as a specific, human-voiced account in the contributor's own words (not templated or AI-boilerplate-sounding); this rule does not require an explicit disclosure sentence. If the repo states neither rule (silent or a generic/permissive policy), pass by default. | required

I wrote it this way after my first version (a single "policy requires disclosure → comments must disclose, else fail" rule) incorrectly failed `pkg-03` (ripgrep). Ripgrep's stated policy is "comments to maintainers must be written by humans in their own words" — a human-voice rule, not a disclosure rule — but my rubric treated any stated AI policy as triggering a disclosure requirement, which flipped a correct `accept` into an incorrect `reject`. Splitting the check into two named sub-rules let it read ghostty's actual disclosure requirement (`pkg-20`, the one-item `disclosure` category, correctly rejected for not disclosing) separately from ripgrep's human-voice requirement (correctly passed, since the comment reads as specific and human-voiced), instead of collapsing both into one condition that could only get one of them right at a time.

**Trade-offs**

Splitting `AI-Disclosure` into the disclosure/human-voice sub-rules is what flipped `pkg-03` from an incorrect `reject` to the correct `accept` (canary re-run: `--only pkg-03,pkg-01,pkg-02`, `agreement: 3/3 scored items`, confirming `pkg-01` and `pkg-02` still agreed after the change). The trade-off: the human-voice sub-rule passes on my own judgment of whether a comment "reads as human-voiced," which is a softer, more subjective signal than the disclosure sub-rule's simpler presence-check for an explicit disclosure sentence. A repo with a human-voice rule and a candidate comment that is AI-generated but well-disguised as human writing could slip past this check — it can only catch what reads as templated or boilerplate-sounding, not what is well-written but not actually human-authored.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
