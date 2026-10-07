# Plan: `/health` reports Redis unhealthy even when reachable (#62)

## Diagnosis

The health probe's Redis check in `api/routes/health.py` builds its
client from `settings.redis_host` and `settings.redis_port`:

```python
r = redis.Redis(
    host=settings.redis_host,
    port=settings.redis_port,
    db=0,
    decode_responses=True,
)
```

`Settings` in `core/config.py` defines only `redis_url` (`redis_url:
str = Field(default="redis://localhost:6379/0")`); it has no
`redis_host` or `redis_port` fields. My week-2 reproduction confirmed
this directly:

> `python3 -c "from core.config import settings; print([f for f in
> ('redis_url','redis_host','redis_port') if hasattr(settings, f)])"`
> → `['redis_url']`

and confirmed the failure mode the issue describes:

> Running the exact code `health_check` executes for the Redis
> dependency raises `redis_health_check_failed AttributeError
> 'Settings' object has no attribute 'redis_host'` — before any
> connection attempt is made. The broad `except Exception` in
> `health_check` swallows this `AttributeError` the same way it would
> swallow a real `ConnectionError`, so `/health` reports `redis:
> unhealthy` (503) regardless of whether Redis is actually reachable.

The control run in that same reproduction (rebuilding the client from
`settings.redis_url` instead) produced the correct failure mode
instead — a `ConnectionError` from a real connection attempt — which
is what confirms the bug is in *which field the probe reads*, not in
reaching Redis itself.

## Scope

In scope: the Redis client construction in `health_check`
(`api/routes/health.py`), changed to build its connection from the
`redis_url` field `Settings` actually defines, via
`redis.Redis.from_url(settings.redis_url, ...)` rather than
non-existent `host`/`port` fields.

Not in scope:
- The Postgres and vector-DB checks in the same function (unaffected,
  no reported bug).
- `Settings` itself — `redis_url` already exists and is correct; no
  new config fields are needed.
- Any other consumer of Redis in the codebase (this issue and my
  week-2 repro are scoped to the health probe only; I have not audited
  other call sites for the same host/port assumption, and I'm leaving
  that out of this fix rather than expanding scope on a hunch).

## Files touched

- `api/routes/health.py` — the Redis block inside `health_check`.
- `pyproject.toml` — the `[[tool.mypy.overrides]]` entry for
  `api.routes.health`: CONTRIBUTING.md says to remove the `attr-defined`
  suppression when fixing #62.
- `tests/unit/` — a new small test file for the health route (none
  exists yet), covering the fixed behavior.

## Approach

1. In `api/routes/health.py`, replace the `redis.Redis(host=...,
   port=...)` construction with `redis.Redis.from_url(settings.redis_url,
   decode_responses=True)`, keeping the same `try/except` structure and
   log events (`redis_health_check_passed` / `redis_health_check_failed`)
   so the rest of the endpoint's behavior and logging shape are
   unchanged.
2. In `pyproject.toml`, drop `attr-defined` from the `api.routes.health`
   mypy override's `disable_error_code`, then run `mypy api/routes/health.py`.
   The pyproject note says #61 is related, so if mypy reports other
   `attr-defined` errors in that module, I will keep the suppression and
   say so under Deviations rather than fix #61 here.
3. Add a small unit test that mocks `redis.Redis.from_url` (or patches
   the Redis client) to confirm: (a) when the mocked client's `ping()`
   succeeds, the route reports `redis: healthy`, and (b) when
   `redis_url` points at an unreachable host, the route reports
   `redis: unhealthy` via a real `ConnectionError`-shaped path rather
   than an `AttributeError` — i.e. the same distinction my week-2
   control run drew by hand.

## Test plan

Re-run my week-2 reproduction steps against the fix and confirm the
`AttributeError` is gone:

- **Before** (from week-2, reproduced 2026-09-29): the Redis block as
  shipped raises `AttributeError: 'Settings' object has no attribute
  'redis_host'`, caught by `except Exception`, logged as
  `redis_health_check_failed`.
- **After** (expected): the same block, now built from
  `settings.redis_url` via `redis.Redis.from_url`, raises no
  `AttributeError`. With no Redis server running, it raises a real
  `ConnectionError` (the same failure mode my week-2 control run
  showed when manually rebuilding the client from `redis_url`) and
  logs `redis_health_check_failed` for that reason instead. With a
  Redis server running and reachable, `ping()` succeeds and the route
  reports `redis: healthy`.

I will capture the actual before/after commands and output when I run
this against the built change (Assignment 3, step 11) and paste both
into `plan-and-implement.md`.

## Risks and unknowns

- The `attr-defined` suppression may also be covering #61's code in the
  same module; I have not run mypy without it yet, so whether it can be
  fully removed is unknown until the build.
- I have not checked whether any other module in the codebase makes
  the same `redis_host`/`redis_port` assumption; if one exists, it is
  a separate instance of the same class of bug and out of scope for
  this fix (noted above).
- `redis.Redis.from_url` parses standard `redis://[:password@]host:port/db`
  URLs; if `redis_url` in some deployment ever carries options this
  helper doesn't parse (e.g. TLS query params needing `rediss://`), the
  fix would need `redis.Redis.from_url` to also handle that scheme,
  which I have not tested — `redis-py`'s `from_url` does support
  `rediss://`, but I have not exercised it against this repo's
  deployment configs.

## Deviations

The plan held. The change is the one-line `redis.Redis.from_url(settings.redis_url, ...)` swap in `api/routes/health.py`, the `attr-defined` removal from the `api.routes.health` mypy override in `pyproject.toml`, and a new `tests/unit/test_health_redis.py`. The unknown I listed about the suppression resolved cleanly: `mypy api/routes/health.py` reports no issues without `attr-defined`, so #61 is not affected. One small detail I had not planned: the route raises a 503 `HTTPException` when any dependency is unhealthy, so the unreachable-Redis test asserts that exception instead of a returned dict. I did not test `rediss://` URLs.
