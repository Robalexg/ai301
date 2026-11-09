# Contribution: UX: recent searches on search focus (with clear)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Robert Guajardo
**GitHub Username:** Robalexg
**Issue:** https://github.com/lolimmlost/aidj/issues/286
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

**Unit 5: turned my issue-select skill into issue-scout and wrote its search strategy (2026-11-08)**

I installed the scout over my Unit 1 skill and wrote a search strategy with three
`gh search issues` queries, one per sentence of my fit profile (everyday-life apps,
state and data-flow libraries, JS/TS games), ranked by no assignee, updated within 90 days,
10 comments or fewer, and fewest comments first. Plain keyword queries returned mostly tiny
personal repos, so I aimed the queries at named projects instead; my first scan then graded
five candidates and rejected all five, and two of those rejections exposed rubric bugs:
`repo-in-use` failed lichess, which deploys daily but stopped tagging releases in 2017, and
tldraw's "pull requests are turned off" could only be caught by stretching my AI-policy check.
I let `repo-in-use` fall back to recent pushes, added a required `accepts-outside-prs` check,
removed a leftover classroom claim rule, and dropped tldraw and next.js from the queries.

**Unit 6: installed the voice guard over one merged voice guide, and wrote a repo memory file for aidj (2026-11-08)**

I folded my three voice-guide copies into one canonical guide, added Upstream rules for claims,
questions and pings, and installed the guard as a hook; the wiring test was stopped as expected,
and my real claim went through it with a logged pass before it posted. Two things fought me: my
old rule "never promise a date" contradicted the course's ask to say when I'll report back, so I
narrowed it to "no date for a fix or PR", and repro-check later flagged that my posted claim ran
four paragraphs against my own new "one or two sentences" claim rule, which the guard had let
through. My `CLAUDE.local.md` for aidj (34 lines, kept out of git with `.git/info/exclude`)
records that search history already lives in `search.tsx` and that my machine can't run the full
app, so future sessions start from those facts instead of rediscovering them.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My scout's own scan of named React, state-library and game repos graded five issues and rejected
all five, so this issue came from the Issue Selection site. I filtered it to JavaScript and
TypeScript with "good first issue", screened the JS/TS repos with gh for open PRs and recent claims
(only 180 of 364 were actually free), and graded six picks over two rounds. The first three
(nusmods#4602, nusmods#3069, stump#559) were rejected; the second three (aidj#286,
UT-Registration-Plus#864, stoatchat/for-web#1610) were accepted, and after this week's scope patch
the regrade accepted all three again, with aidj rated "moderate". I chose aidj#286 because it is
the closest match to my profile: React and TypeScript, client-side state and data flow, in an app
people use every day. I want to learn how to land a feature in someone else's codebase cleanly,
with tests, in a repo where I have to read the owner's conventions instead of setting my own.
Stoat ranked above it, but its guide says "Please do not open PRs generated with LLMs", which
doesn't fit course work that is AI-assisted by design.

Reading the code changed the picture: the issue says no search history exists, but `search.tsx`
already keeps the last 10 queries with remove and Clear all (from #98, before the issue was
filed), and two of the asks depend on open issues #271 and #284. So I'd now call it ambitious, not
moderate, and my claim asks the owner to scope it to per-user history and saved clicked results.
Honest read of the scout: it checks the thread and policies well, but it trusts the issue text, so
it never noticed the feature was half built; only reading the code caught that. The repo is also a
solo project with 2 stars, which my rubric records but can't weigh.

Claim: https://github.com/lolimmlost/aidj/issues/286#issuecomment-6731680176

**Toolkit**

- Tool: issue-scout, my search strategy plus my rubric; I ran one scan, then graded my site picks
  directly, twice, and regraded after this week's patch.
- What it produced: a scan that rejected all five candidates, then "accept, difficulty moderate"
  for aidj#286 with the caveat that it's a 2-star solo repo.
- Difficulties: my unit-1 scope check rejected every issue with an open design edge (4 of 8
  rejections), `repo-in-use` failed live repos that stopped tagging releases, and the scout took the
  issue's "no history exists" at face value.
- What changed: I replaced `scope-fits-newcomer` with `scope-feasible` plus a difficulty note
  (unit 6 patch), added `accepts-outside-prs`, let `repo-in-use` fall back to recent pushes, and
  I'd add a check that the asked-for behavior isn't already in the code.

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

macOS (Darwin 25.6), Node v22.18.0, git and gh. I forked lolimmlost/aidj to Robalexg/aidj, cloned
the fork, added `upstream`, and pinned the repro to upstream `main` at `b81906e` (2026-11-06).
Challenges: the full app needs PostgreSQL and a Navidrome server, and this machine has no Docker,
so I reproduced against the source at that commit with commands anyone can rerun without
installing anything. Cloning on macOS also flags `docs/BACKLOG.md` and `docs/backlog.md` as a
case collision, which shows up as a modified file I leave out of every commit.

**Steps to Reproduce**

1. `git clone https://github.com/lolimmlost/aidj.git && cd aidj && git checkout b81906e`
2. Baseline: `grep -n "RECENT_KEY\|MAX_RECENT" src/routes/library/search.tsx` shows history
   already exists (key `aidj.search.recent`, cap 10), from #98.
3. `grep -n "saveRecent(" src/routes/library/search.tsx`: only line 84, the typed query, adds
   entries.
4. `sed -n 184,190p src/routes/library/search.tsx`: `handleSongClick` plays the song and never
   saves it. Clicked results are not saved.
5. `grep -c "useSession\|session" src/routes/library/search.tsx` prints `0`: the key is one fixed
   string, so history is not per user.
6. `gh issue view 271` and `284`: both OPEN, so Private Session and the command palette don't
   exist yet.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** command output at `b81906e`:

  ```
  $ sed -n 184,190p src/routes/library/search.tsx
    const handleSongClick = (songId: string) => {
      const song = songs.find(s => s.id === songId);
      if (!song) return;
      setAIUserActionInProgress(true);
      playSong(songId, [song]);
      setTimeout(() => setAIUserActionInProgress(false), 1000);
    };
  $ grep -c "useSession\|session" src/routes/library/search.tsx
  0
  ```
- **My findings:** the issue is partly out of date. Query history, per-item remove and Clear all
  already exist. What's really missing and buildable now is saving clicked results and keying
  history per user. The Private Session and command palette parts wait on #271 and #284. I asked
  the owner in my claim whether to scope it that way. The per-user gap is inferred from the code;
  I haven't tested it with two real accounts.

**Toolkit**

- Tool: repro-check, grading my claim and repro report together against the issue.
- What it produced: a reject on the first draft (`behavior-matches-issue` failed because I
  described the grep and sed results instead of pasting them), then an accept after I pasted the
  raw output, linked #98, and marked the per-user gap as inferred from code.
- Difficulties: I couldn't run the app (no Docker for PostgreSQL and Navidrome), and installing
  the repo's dependencies was blocked, so the repro is source-level only. `control-run` still fails
  because a code-reading repro has no "same command without the trigger".
- What changed: the report now carries real command output pinned to a commit. Next time I'd set
  up Navidrome first so the repro can show the behavior in the running app.

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
