# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause, read against what the repro evidence shows | The cause explains the reproduced behavior and contradicts nothing in the repro steps or output | required |
| targets-cause | The plan's proposed change, read against where the repro evidence locates the fault | The change acts on the cause the evidence points at, not only on where the symptom appears | required |
| bounded-scope | The plan's scope statement and the files or areas it names | It is one change that fixes this issue; nothing listed is unrelated cleanup or refactoring | required |
| executable | The plan's approach: files, change, order of work | A stranger could start the first step without asking the author anything | required |
| test-observable | The test plan, read against the repro evidence's steps | It names a concrete action and the result you would see before and after the fix, tied to the repro steps | required |
| honest-unknowns | The confident claims in the plan and the plan comment, read against the repro evidence and the thread highlights | No claim is presented as settled that a repro step contradicts or that a maintainer disputed in the thread. A code-level cause that fits every repro step passes without hedging, and so does the author's own report of extra local checks the excerpt does not show | required |
| thread-aware | The plan comment, read against the thread highlights and the repo-facts block | The comment follows any direction a maintainer gave, or says why the plan differs, and meets every stated repo ask that applies to comments. When the contribution policy requires AI use to be disclosed in comments or in any form, the comment must contain a disclosure | required |

## Verdict rule

Accept (ready) only if every `required` check grades pass. Any
`required` check that grades fail means reject (hold).

A `?` (unclear) on a `required` check counts as fail: a plan that
cannot be verified from the package is not ready to build from, so it
holds.

All seven checks are `required`. If a `preferred` check is added
later, it never changes the verdict; report its grade in the summary.
