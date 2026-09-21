# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62

**Verdict output**

```
Issue #62 — "Health check references settings.redis_host, which does not exist on Settings" (labels: bug, good first issue, api, tier-1)

┌─────────────────────┬───────┬─────────────────────────────────────────────────────────────────────────┐
│        Check        │ Grade │                                Evidence                                 │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│ maintainer-commits  │ pass  │ Last 5 main commits all authored by human Aburke225 (Andrew Burke),     │
│ (req)               │       │ newest 2026-09-16 — 4 days ago, well inside 90                          │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│ maintainer-answers  │ pass  │ Only two comment threads repo-wide (#69, #68), both student claims ≤2   │
│ (pref)              │       │ days old; no maintainer-engaged thread is being ignored                 │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│ repo-alive (req)    │ pass  │ archived: false, pushed_at: 2026-09-16T21:50:20Z — 4 days, inside 180   │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│ repo-used (pref)    │ fail  │ Releases API returns none, and 2 stars is far under the 500 fallback    │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│                     │       │ One bounded fix: probe in api/routes/health.py should read              │
│ scope-bounded (req) │ pass  │ settings.redis_url instead of the nonexistent redis_host/redis_port; no │
│                     │       │  umbrella, no design debate (0 comments), no "core internals" note, not │
│                     │       │  a support question                                                     │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│ unclaimed (req)     │ pass  │ assignees: [], timeline holds only 4 labeled events (no                 │
│                     │       │ cross-referenced PRs), comments: 0                                      │
├─────────────────────┼───────┼─────────────────────────────────────────────────────────────────────────┤
│ ai-policy-open      │       │ docs/CONTRIBUTING.md (189 lines) has zero matches for                   │
│ (req)               │ pass  │ AI/assisted/generated/disclosure; PR template has no AI checkbox —      │
│                     │       │ silence passes                                                          │
└─────────────────────┴───────┴─────────────────────────────────────────────────────────────────────────┘

All five required checks pass → accept. The lone preferred fail (repo-used) is expected for a seeded classroom repo and never touches the verdict; it would only matter for ranking against another accepted candidate.

Fit: strong match for your profile — a FastAPI backend bug in Python with reproduction steps you can run locally (GET /health with Redis up returns 503 + AttributeError), and docs/CONTRIBUTING.md says seeded bugs carry an @pytest.mark.xfail marker you remove as part of the fix, which is exactly the "write the test that pins the bug first" practice you're after. Note the PR template requires all five CI jobs green.

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
  "checks": [
    {"name": "maintainer-commits", "grade": "pass", "evidence": "All 5 latest main commits authored by human Aburke225; newest 2026-09-16, 4 days before today"},
    {"name": "maintainer-answers", "grade": "pass", "evidence": "Only comment threads in repo (#69, #68) are student claims from the last 2 days; no maintainer-engaged thread left ignored"},
    {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16T21:50:20Z (4 days, within 180)"},
    {"name": "repo-used", "grade": "fail", "evidence": "No releases published and only 2 stars, below the 500-star fallback"},
    {"name": "scope-bounded", "grade": "pass", "evidence": "Single fix in api/routes/health.py to use settings.redis_url; 0 comments so no unsettled design debate, not an umbrella or support question"},
    {"name": "unclaimed", "grade": "pass", "evidence": "assignees: [], no cross-referenced PRs in the timeline (4 labeled events only), comments: 0"},
    {"name": "ai-policy-open", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no mention of AI, assisted, or generated code — silence passes"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

Run 1 — smoke run, `--limit 3`: 2/3. issue-01 came back reject against a gold label of accept, failing on `maintainer-answers` and `scope-bounded`.

Run 2 — `--only issue-01` after two rubric edits: 1/1, flipped to accept.

Run 3 — full run: 19/20. Categories: claimed 4/4, clear-accept 8/8, dead-repo 3/3, policy 1/1, scope 3/4. The only miss was issue-20, graded accept against a gold label of reject.

Run 4 — full run with `--save-run eval-run.txt`: 19/20, same verdicts. That is the run committed in this directory.

**Issue analysis**

issue-20 (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar"). My rubric graded it accept; the gold label is reject.

Every required check passed honestly. `scope-bounded` passed because my four fail conditions all ask about the shape of the work — umbrella, unsettled design debate, core internals, support question — and this issue is none of them. It names a surface (`packages/excalidraw`, toolbar plus element) and a success condition (place, resize, move, export correctly). `unclaimed` passed on no assignee, no linked PR, no comments. `maintainer-commits` and `repo-alive` passed on a repo pushed to the day before capture.

What my rubric never asks is whether the issue is legitimate and wanted. It was opened by `cursor[bot]` with author_association `NONE`, requesting a fixed "company logo" shape for a general-purpose whiteboard with 129k stars — a request that only makes sense inside a private fork. It carries no labels, so no maintainer triaged it; no comments, so no maintainer acknowledged it; the roadmap checkbox is left unticked; and the logo asset itself is marked TBD. It is also a feature request sitting in a 3,289-item backlog, not a bug.

The missing check is a provenance one: fail an issue opened by a bot or by an author with association NONE when no maintainer has labelled, commented on, or otherwise engaged with it. I left the rubric as committed rather than add it after the fact, since `eval-run.txt` fingerprints the file that produced the run.

**Check rationale**

Quoting `scope-bounded` as currently written in `tools/issue-select/rubric.md`:

> **Evidence:** Issue body and the full comment thread
>
> **Pass condition:** Fail only if one of these is present: the issue is an umbrella or tracking issue whose sub-items are meant to become separate issues; the thread shows an unsettled design debate no maintainer has closed; a maintainer says the fix touches core internals; or the issue is a usage or support question. Otherwise pass. A detailed multi-step plan confined to one area is one piece of work — sub-headings are not sub-issues. A terse body, a missing reproduction, or no checklist never fails this check.
>
> **Weight:** required

The check defaults to pass and enumerates what kills an issue, rather than defaulting to fail and asking whether the scope is bounded. My first version read "asks for one bounded change," which invited the grader to count files and sections: it failed issue-01, a conda docs issue whose body carries five headed sections that all land in one PR. The evidence guide warns that a terse body, a missing reproduction, or no checklist can still be a bounded first issue, so I named the four fail conditions explicitly and made everything else pass.

**Trade-offs**

The looser `scope-bounded` is what let issue-20 through. Fixing issue-01 cost me the scope category's fourth item, and the committed run shows scope 3/4 as a result. I kept the trade because the other four families stayed fully covered and the bar is 18.

A second trade showed up only in live mode. `repo-used` failed on every Path Review candidate — 0 releases, 2 stars — because its thresholds were tuned on real open source repositories. It is `preferred`, so it changed no verdict. Had I made it required, my rubric would have rejected every issue in the repository the course assigned me.

---

## Selection rationale

**1. Fit and time available**

Python and FastAPI are what I work in, the bug lives in one file, and the fix is small enough that I can finish it inside Unit 2 alongside my other coursework.

**2. What the verdict got right, and what I weighed that the rubric could not**

The verdict identified correctly that this is one defect in `api/routes/health.py`: the Redis probe builds its client from `settings.redis_host` and `settings.redis_port`, which `Settings` in `core/config.py` does not define, and the probe's bare `except Exception` swallows the resulting `AttributeError` into a false "unhealthy". No assignee, no linked PR, no comments, in a repo pushed to four days before the run.

What my rubric could not weigh is that the `AttributeError` fires before any Redis connection is attempted, so I can reproduce it in a unit test with no infrastructure running. The skill also surfaced two things from `docs/CONTRIBUTING.md` that the issue body never mentions: seeded bugs carry an `@pytest.mark.xfail` marker I remove as part of the fix, and the PR template requires all five CI jobs green.

**3. Anticipated difficulty in claiming it**

Claiming should be low friction. The Path Review house rules say classmate claim comments do not block an issue, since course credit attaches to the pull request I open rather than to whether it merges. Issue #62 currently has no assignee, no linked PR, and no comments. The one thing working against me is that an api tier-1 good first issue is an obvious pick, so other students may well open PRs against the same bug. That costs me nothing under the house rules.
