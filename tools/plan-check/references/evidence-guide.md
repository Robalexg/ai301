# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## How a package is laid out

An eval package (for example `eval/packages/calib-01.md`) has these
sections, in this order: `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Repro evidence`, `## Candidate plan`,
`## Candidate plan comment`. The candidate plan is not always laid out
the same way: some plans use labelled lines (`Cause:`, `Change:`,
`Test:`), others use sub-headings (`### Diagnosis`, `### Scope`,
`### Approach`, `### Changes` or `### Proposed changes`, `### Files`
or `### Files and areas`, `### Test plan`). Find each part by what it
says, not by its heading; a part with no heading still counts.

In live mode the same parts live here:

- Repo facts: the repo's `CONTRIBUTING.md`, issue and PR templates,
  and any AI-use policy file.
- Issue and thread: the issue page (`gh issue view <n> --comments`).
- Repro evidence: the student's posted repro comment on that issue
  (for the house issue, the repro pack as quoted in the drafts).
- Candidate plan and plan comment: the student's `plan.md` and draft
  comment.

## Diagnosis and grounding

Where it lives:

- The plan's cause: the `Cause:` line or the `### Diagnosis` section
  of `## Candidate plan` (sometimes `### Background`,
  `### Problem statement`, or `### Summary`).
- What the cause must explain: `## Repro evidence`, in its numbered
  `Steps` and its `Expected:` and `Actual:` lines. The steps that vary
  one condition (a flag on and off, a different view, a different
  version) show where the fault is.
- A cause someone else proposed: `## Thread highlights`. A commenter's
  claimed root cause is a claim, not evidence.

What good looks like:

- Every repro step's result is what you would expect if the stated
  cause were true. In calib-01 the cause (the view is not refreshed
  after a push) explains step 3 (color stays) and step 4 (color flips
  on re-entry).
- The cause contradicts no step. A cause that blames one component
  when a repro step shows the behavior with that component removed is
  a contradiction, even if a thread comment proposed that cause.
- The proposed change acts on the place the evidence locates the
  fault, not only on the place the symptom shows.

## Scope

Where it lives:

- The plan's `In:` and `Out:` lines, or its `### Scope` section.
- The files and areas named in `Change:`, `### Changes`,
  `### Proposed changes`, `### Files`, or `### Files and areas`.
- What the issue needs: the `## Issue` body and the `Expected:` line
  of `## Repro evidence`.

What good looks like:

- Each file or area named is needed to change the behavior the issue
  reports. calib-01 names one callback in one file and says what it
  leaves alone.
- Nothing is added that the issue does not ask for: no refactor,
  rename, cleanup, dependency bump, or second bug fixed along the way.
- A numbered change that only adds a test for this fix is in scope.

## Executability

Where it lives:

- The `Change:` line, or `### Approach`, `### Changes`,
  `### Proposed changes`, and `### Files` in `## Candidate plan`.

What good looks like:

- The first step names a file, function, or area and says what will
  be done to it ("the push completion callback in
  `pkg/gui/controllers/sync_controller.go` adds the commits context
  to its refresh scope").
- When there is more than one step, the order is stated or obvious.
- A step such as "investigate the rendering code" or "fix the refresh
  logic" names no place and no action; a stranger would have to ask
  where to start.

## Test plan

Where it lives:

- The `Test:` line or `### Test plan` section of `## Candidate plan`.
- What it maps onto: the numbered `Steps` and the `Expected:` and
  `Actual:` lines of `## Repro evidence`.

What good looks like:

- It names an action someone can perform (a command, a key press, a
  repro step number) and the result that shows the fix worked
  ("at step 3 the color must flip without leaving the view").
- That result is the opposite of the `Actual:` behavior in the repro
  evidence, so the same check fails before the fix and passes after.
- "Run the test suite" or "verify it works" names no observable
  result. A test that checks something the repro never showed broken
  proves nothing about this issue.

## Honesty

Where it lives:

- Stated unknowns: a `Risks`, `Unknowns`, or `Open questions` part of
  `## Candidate plan`, or hedged sentences in the plan and in
  `## Candidate plan comment`.
- What is established: `## Repro evidence`. It establishes the
  behavior and the conditions it was reproduced under (environment,
  versions, steps). It usually does not establish the code-level
  cause.
- Confident wording to check: "traced to", "root cause is",
  "confirmed", "will fix", in the plan and the comment.
- A deviation from an earlier posted plan: a deviation note in the
  plan itself. A deviation that shows only in a diff is not recorded.

What good looks like:

- No claim stated as settled is contradicted by a repro step, and
  none was disputed by a maintainer in `## Thread highlights`.
- The repro evidence is an excerpt. The author saying they also ran
  an extra local check the excerpt does not show is not a failure.
- A code-level cause that fits every repro step is a diagnosis. It
  does not need hedging, and a plan with no unknowns section or with
  "Risk: none identified" is fine when nothing above applies.
- Naming open questions and untested cases is good, but leaving them
  out is not on its own a failure.

## Comms

Where it lives:

- The words being graded: `## Candidate plan comment`.
- Maintainer signals: `## Thread highlights`. Role tags mark who
  speaks for the project: `(OWNER)`, `(MEMBER)`, `(COLLABORATOR)`.
  `(NONE)` and `(CONTRIBUTOR)` are usually other users, but when one
  of them sets a direction the thread treats as the project's, read
  it as maintainer direction. Look for a stated
  direction, a linked existing PR, a "known problem" note, or a
  request to wait.
- Repo asks: the `## Repo facts` bullets `bug reports:` (what the
  template asks for) and `contribution policy` (review bandwidth,
  AI-use disclosure, PR rules).

What good looks like:

- When a maintainer gave a direction or pointed at an existing PR,
  the comment answers it: it follows the direction, or says why the
  plan differs, and does not race an open PR without saying so.
- Every stated repo ask that applies to a plan comment is met. If the
  policy requires AI-use disclosure in comments or "in any form", the
  comment states the tool and the extent of its use. A policy that
  asks for disclosure only in pull requests does not bind the plan
  comment.
  calib-01's comment keeps the fix minimal and cites the
  review-bandwidth note in `CONTRIBUTING`.
- With zero comments and no stated asks, there is nothing to follow.
- A comment that could be pasted under any issue unchanged is
  boilerplate.
