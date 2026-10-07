# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

narayanansriram

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-6042341207

Here's my plan for this, building on my reproduction above. The Redis block in `health_check` (`api/routes/health.py`) builds its client from `settings.redis_host`/`settings.redis_port`, which don't exist on `Settings` — only `redis_url` does. My repro confirms it: the exact code the probe runs raises `AttributeError: 'Settings' object has no attribute 'redis_host'` before any connection attempt, and the broad `except Exception` reports that as `redis_health_check_failed` regardless of whether Redis is actually reachable. My control run (same block, rebuilt from `redis_url` instead) gets the real failure mode — a `ConnectionError` from an actual connection attempt — which pins the bug on the field being read, not on Redis itself.

The fix is one bounded change: swap the `redis.Redis(host=..., port=...)` construction for `redis.Redis.from_url(settings.redis_url, decode_responses=True)`, keeping the same try/except and log events, and drop the `attr-defined` suppression for this module from `pyproject.toml` as CONTRIBUTING.md asks (if mypy shows it also covers #61's code, I'll leave it and say so). I'm leaving the Postgres/vector-DB checks in the same function untouched, and not auditing other possible call sites for the same host/port assumption — this fix is scoped to the health probe this issue is about. For testing, I'll re-run my repro steps against the change and add a small unit test mocking the Redis client for both the healthy and unreachable-host cases, since no test file exists yet for this route.

---

## Your branch

**Branch**

fix/62-redis-health-url

**Evidence**

Before (original `api/routes/health.py`, no Redis server running), running the real `health_check` through a small script:

```
$ python repro.py
[debug] postgres_health_check_passed
[error] redis_health_check_failed  error="'Settings' object has no attribute 'redis_host'"
[debug] vector_db_health_check_passed
HTTP 503 {'postgres': 'healthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}
```

After (fixed, same setup):

```
$ python repro.py
[debug] postgres_health_check_passed
[error] redis_health_check_failed  error='Error 61 connecting to localhost:6379. Connection refused.'
[debug] vector_db_health_check_passed
HTTP 503 {'postgres': 'healthy', 'redis': 'unhealthy', 'vector_db': 'healthy'}
```

The `AttributeError` is gone; the failure is now a real `ConnectionError`.

```
$ python -m pytest tests/unit/test_health_redis.py -q
2 passed
$ python -m mypy api/routes/health.py   # with attr-defined suppression removed
Success: no issues found in 1 source file
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--limit 4` smoke run: 4/4
2. First full run: 19/20 (pkg-14 failed on Executable)
3. `--only pkg-14,pkg-02,pkg-09,pkg-10,pkg-17,pkg-18 --include-calibration`: 6/6 after loosening Executable
4. Final full run (`--save-run eval-run.txt`): 20/20 scored items

**Package analysis**

`pkg-14` (zellij OSC leak on reattach). My first rubric said reject (failed Executable); gold said accept. The plan names the client attach path in `zellij-server`/`zellij-client`, states the mechanism (drain pending OSC query responses before pane input is wired) and says the leak origin is already visible in `--debug` output. It defers only the exact function names to the PR. My first Executable check read that as a research task. After the revision it reads it as an ordinary implementation detail and accepts, matching gold.

**Check rationale**

> | Executable | The plan's approach/changes section: the files, functions, or code locations named and the steps to take. | Pass if a stranger with no access to the plan's author could start implementing immediately: at least one concrete file, module, or code path is named, the mechanism of the fix is stated (what will be added, changed, or drained/consumed, and roughly where in the flow), and any deferred detail is a normal implementation step (pinning the exact function name once inside the file) rather than the location or mechanism itself being unknown. Fail if the plan's steps are "investigate," "profile," "look into," "figure out where X lives," "dig into the code," or "poke around" with no location or mechanism named at all — i.e. the plan is a promise to go find a plan, not a plan. Naming a module or path plus a stated mechanism passes even without a specific function name. | required |

The first version just required a named file and edits rather than research. That rejected pkg-14 because the exact functions were deferred. I added that a module or path plus a stated mechanism is enough, and that deferring only the function name is fine, but still failed plans whose steps are "investigate" or "poke around" with no location or mechanism (calib-02, pkg-10).

**Trade-offs**

Loosening Executable flipped pkg-14 to the correct accept. I re-ran canaries `pkg-02`, `pkg-09` (clear-accept) and `pkg-10`, `pkg-17`, `pkg-18` (unbuildable): all still agreed, 6/6. The cost is that a plan naming a module and a plausible mechanism now passes even if the module is the wrong one. Diagnosis-Grounded and Test-Plan-Decisive are what catch that, not this check.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
