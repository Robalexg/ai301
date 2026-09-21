# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73

**Verdict output**

```
Candidate: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73
Scope: codepath/pathreview-ai301-fa26-s3 — in scope. No house-rule check triggered (issue has zero comments, so no student claim to ignore).

- maintainer-alive: pass — newest of the last 5 default-branch commits (2f4e82f) is authored by Andrew Burke (human) on 2026-09-16, 5 days before today (2026-09-21), well within 90 days.
- not-archived: pass — `archived: false` on the repo.
- repo-in-use: pass — repo has never cut a release, but last push to the default branch was 2026-09-16, within 90 days of today.
- scope-fits-newcomer: pass — one bounded task: make `README.md` and `.env.example` agree on the LLM API key variable; not an umbrella, not an unsettled design, not core internals, not a support question. Desired behavior is specified ("Make the two files agree"), two files named, 1-2 hour estimate.
- nobody-already-on-it: pass — no assignees, no linked PRs, zero comments on the issue.
- ai-contribution-allowed: pass — no CONTRIBUTING.md, .github/CONTRIBUTING.md, AGENTS.md, or AI_POLICY.md/AI_USAGE_POLICY.md found in the repo (all 404); silence passes.
- good-first-issue-label (preferred): pass — issue carries the `good first issue` label.
- maintainer-filed (preferred): pass — opened by Andrew Burke, author_association COLLABORATOR.
- maintainer-response-latency (preferred): pass — sampled 7 recently-updated issues; only one (#52) got a maintainer reply, same day it was opened (0 days), so the median of replied issues is 0 days, under 30.
- adoption-scale (preferred): fail — repo has 2 stars, below the 100-star threshold.

All required checks pass -> verdict: accept. Preferred checks 3/4 pass, sunk only by low star count on an otherwise small, actively-maintained classroom repo.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
  "checks": [
    {"name": "maintainer-alive", "grade": "pass", "evidence": "Newest of last 5 default-branch commits (2f4e82f) by human Andrew Burke, dated 2026-09-16, 5 days before today"},
    {"name": "not-archived", "grade": "pass", "evidence": "repo `archived: false`"},
    {"name": "repo-in-use", "grade": "pass", "evidence": "No releases cut, but last push to default branch was 2026-09-16, within 90 days of today"},
    {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "Issue asks for one bounded change: reconcile README.md and .env.example on the LLM API key variable, with files and desired end-state named"},
    {"name": "nobody-already-on-it", "grade": "pass", "evidence": "No assignees, no linked PRs, zero comments on the issue"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md, AGENTS.md, or AI_POLICY.md found in repo root or .github/ (all 404) — silence passes"},
    {"name": "good-first-issue-label", "grade": "pass", "evidence": "Issue carries the `good first issue` label"},
    {"name": "maintainer-filed", "grade": "pass", "evidence": "Opened by Andrew Burke, author_association: COLLABORATOR"},
    {"name": "maintainer-response-latency", "grade": "pass", "evidence": "Of 7 sampled recently-updated issues, only #52 received a maintainer reply, same day (0 days) — median of replied issues is 0 days"},
    {"name": "adoption-scale", "grade": "fail", "evidence": "Repo has 2 stars, below the 100-star threshold"}
  ],
  "verdict": "accept"
}
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

One full run occurred: agreement 19/20 scored items (bar: 18/20: PASS), matching the
`agreement:` line in the committed `eval-run.txt`. No revision loop was needed — the
first full run already cleared the bar and every category floor, so no `--only` re-runs
were done.

**Issue analysis**

`issue-15` (source `zulip/zulip#19589`, gold `reject`) is graded `accept` by my rubric —
the run's only disagreement. The issue asks to split the outgoing-webhook payload into
separate `command` and `text` fields, which a maintainer (`timabbott`) engaged with and
the reporter clarified with a captured sample payload, so `scope-fits-newcomer` reads as
a single, specified change rather than an unsettled design. `nobody-already-on-it` also
passes on the bundle's literal wording: "this issue: assignees: none; linked PRs:
zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)" — both linked PRs are closed, and
my rubric's pass condition explicitly states "A closed, unmerged PR in the issue's
history is an abandoned attempt, not an active claim, and does not fail this check on its
own." With every required check reading pass on that literal evidence, the verdict rule
("Accept the issue if and only if every `required` check grades `pass`") produces
`accept`. The gold `reject` is telling me the two failed PR attempts, on top of the
comment thread's long history of contributors claiming the issue and then going quiet
(`LoganNiswander`, `leighadennis`, `blackbird7112`, `BrianMcDowell`, `sudhanshu154`,
`Kaustubhkongile`, `ikrambil` each unassigned after 14 days with no update, one saying "I
have worked on this issue and I have made a PR. Review is pending" that never merged),
is itself evidence the task is harder than its description — a signal my rubric doesn't
check for, since `nobody-already-on-it` only looks at currently-live claims, not a track
record of dead ones.

**Check rationale**

`nobody-already-on-it`, as written: "All three hold: no assignee is set; no linked pull
request is open; and no claim comment ('I'll take this', 'working on this', 'can I work
on this') was posted within 30 days of the capture date and left unanswered by a
maintainer. A closed, unmerged PR in the issue's history is an abandoned attempt, not an
active claim, and does not fail this check on its own." It's written this way because a
first-issue hunter needs to distinguish "someone is working on this right now" (which
should route them elsewhere) from "someone tried once and stopped" (which frees the issue
back up) — otherwise every issue with any PR history, however old or abandoned, would be
permanently unclaimable, which defeats the point of a first-issue search on an active
project.

**Trade-offs**

`issue-15` is the canary this trade-off costs me: the check correctly refuses to treat
one stale PR as a live claim, but it also can't see that *two* separate closed PRs on the
same issue, plus a chain of contributors claiming and abandoning it over roughly three
years, is a different kind of signal than one abandoned attempt — it's evidence the task
is harder to finish than to start. I accept this gap deliberately: catching "repeated
abandonment" would need a check that counts prior closed PRs and stale claims, which
isn't in my rubric today, and adding a numeric threshold for "how many failed attempts is
too many" felt like it would overfit to this one calibration case rather than generalize.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. My fit profile in `scope.md` leans JS/TS and React/Next.js, and this issue is a
   Python FastAPI project, so it doesn't match my language interests directly — but it
   also doesn't touch the heavy-math work I want to avoid, and the whole task is
   reconciling two config/doc files, which fits the small amount of time I have this
   week (the issue itself is estimated at 1-2 hours).
2. The verdict correctly identified that this is a live, single-maintainer repo with
   recent commits and no competing claims, and that the fix is fully specified (make
   `README.md` and `.env.example` agree, with `core/config.py` as the source of truth) —
   so there's no design ambiguity to resolve myself. What the rubric couldn't weigh is
   that this is a course-built sandbox repo rather than a real production project: its
   2-star `adoption-scale` fail is meaningless here since the repo was never meant to
   have outside users, so I discounted that preferred check entirely when deciding.
3. I expect claiming it to be low-difficulty: the fix is two small text edits, cross-
   checked against `core/config.py` for the exact variable names, with no code path to
   test beyond confirming the doc and the example file now agree. The main small risk is
   that a classmate claims it first (the Path Review house rule says that doesn't block
   me), so I'll claim it right away rather than treat the empty comment thread as a
   guarantee it stays open.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
