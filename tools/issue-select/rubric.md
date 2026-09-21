# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | The "last 5 default-branch commits" list under Repo facts, including each commit's author and, for a merge commit, whose pull request it merged (live: the commit history linked from the repo front page) | The newest commit authored by a human, or authored by a bot that merged a human pull request, is dated within 90 days of the capture date (live: within 90 days of today). A bot's own commit, with no human PR behind it, does not count. Commit activity is the whole of this check: the "maintainer first-response sample" is deliberately NOT read here, because a sample of recently updated issues is full of issues opened days before capture, and an unanswered two-day-old issue is evidence of nothing. Response latency is graded separately, and only ranks. | required |
| not-archived | The `archived:` field on the repo line under Repo facts (live: the "This repository has been archived" banner on the repo front page) | `archived` is false and no archive banner is present. | required |
| repo-in-use | The "latest release" field under Repo facts (live: the Releases box in the repo front page sidebar) | A release is dated within 12 months of the capture date (live: within 12 months of today). A repo that has never cut a release fails this check unless "last push to any branch" is within 90 days of the capture date. | required |
| scope-fits-newcomer | The issue body and the full comment thread | The issue asks for one bounded change. It fails if any of these is true. (a) UMBRELLA OR TRACKING ISSUE: the body is a list of independent sub-items, each of which would become its own pull request, with contributors invited to self-select among them (a "megaissue", a checklist of dozens of sub-issues, "PRs welcome both big and small"). A single coherent change that happens to touch several files is NOT an umbrella: one docs task spanning four pages is one change. Neither is a bug report naming two candidate causes for the one broken behavior, nor one carrying optional "additional suggestions" beyond the core fix — grade the core ask and treat the optional extras as outside the first PR. Nor is a maintainer-filed bug that names several concrete instances of one repeated, patterned gap (several named items each missing the same kind of feature, e.g. "missing previews for remove identity, fuse spiders, remove self loops, etc.") — that is one bounded task applied to a short named list, even when it ends in "etc.", not an open invitation to self-select scope. (b) UNSETTLED DESIGN: the thread shows the design still being debated with no maintainer having settled it, or a history of abandoned pull requests that stalled on that disagreement. (c) UNSPECIFIED FEATURE WISH: the issue asks for a new feature without stating the desired behavior, leaving a product or design decision no maintainer has made. Feature requests only — a bug report describing concrete wrong behavior has specified its desired behavior, the correct behavior, and does not fail here however short it is. (d) CORE INTERNALS: a maintainer states the fix reaches core internals. (e) SUPPORT REQUEST: the issue is a usage or support question rather than a request for a change. A terse body, a bare acceptance-criteria checklist, or a bug report with no reproduction steps does NOT fail this check on its own: grade the size of the work asked for, not the polish of the writeup. | required |
| nobody-already-on-it | The `assignees` and `linked PRs` fields under Repo facts, plus every claim in the Comments section (live: the Assignees and Development boxes in the issue sidebar, plus the thread; when sidebar and thread disagree, believe the thread) | All three hold: no assignee is set; no linked pull request is open; and no claim comment ("I'll take this", "working on this", "can I work on this") was posted within 30 days of the capture date and left unanswered by a maintainer. A closed, unmerged PR in the issue's history is an abandoned attempt, not an active claim, and does not fail this check on its own. Per the Path Review house rule in `scope.md`, claim comments from fellow students in the course's Path Review repo do not count as claims at all. | required |
| ai-contribution-allowed | The "contribution policy" line under Repo facts (live: `CONTRIBUTING.md` in the repo root or `.github/`, any contributor docs it links, `AI_POLICY.md` / `AI_USAGE_POLICY.md`, and the PR template) | No outright ban on AI-generated or AI-assisted contributions. Conditions — disclosing AI use, personally understanding and testing the change, having a human review the output — pass this check; they are terms to follow, not bans. A policy that refuses *fully* AI-generated contributions while allowing assistive AI use is a condition, not a ban, and passes. Silence passes: most repos state nothing, and that is not a restriction. An `AGENTS.md` file passes, being instructions written for AI agents. | required |
| good-first-issue-label | The `labels:` field on the issue's byline in the Issue section (live: the Labels box in the issue sidebar) | The issue carries a `good first issue`, `good-first-issue`, or `beginner friendly` label. This is the maintainer's claim that the issue is friendly, not that it is free, so it only ranks accepted issues. | preferred |
| maintainer-filed | The `author_association` in parentheses after "opened by" on the issue's byline (live: the Owner/Member/Collaborator badge beside the opener's name) | The issue was opened by an OWNER, MEMBER, or COLLABORATOR. Someone with commit rights wrote the request themselves, so the behavior described is the behavior they want. Note the deliberate absence of a comment-activity check here: a good first issue with zero comments is normal, and an empty thread is a point in its favor under `nobody-already-on-it`, not against it. | preferred |
| maintainer-response-latency | The "maintainer first-response sample" under Repo facts | Among the sampled issues that actually received a maintainer reply, the median first response is under 30 days. Entries reading "no maintainer comment in thread" are excluded from the median rather than counted as slow: the sample is drawn from recently updated issues, so many are only days old at capture and have had no chance to be answered. If no sampled issue received a reply, grade `unclear`, which costs nothing here because this check only ranks. | preferred |
| adoption-scale | The `stars` field on the repo line under Repo facts (live: the star count at the top of the repo page) | The repo has 100 or more stars. Set low on purpose: a small repo is often the better first contribution, so this row separates the abandoned toy from the modest working project, and is not a popularity contest. | preferred |

## Verdict rule

Accept the issue if and only if every `required` check grades `pass`. A
single `required` check grading `fail` rejects the issue, and the summary
should name that check as the one that sank it.

`unclear` counts as `fail` on every check. The reasoning: each required
check here is a way a first contribution dies, and an issue whose risks I
cannot verify is not an issue I should spend my first pull request on. If
the evidence for a required check is genuinely absent from the bundle, the
issue is rejected.

`preferred` checks never change the verdict. Report their grades, and use
them to rank the issues that were accepted: among accepted candidates,
prefer the one with more preferred checks passing. Fit ranking from
`scope.md` applies on top of that, and likewise never changes a verdict.
