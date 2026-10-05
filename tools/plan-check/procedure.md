# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. In live mode, read `scope.md` and confirm the issue is in the
   scoped repo. If it is not, stop without grading and say so. Note
   the house rules. In eval mode, skip this step.
2. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   name of every check, its weight, and the verdict rule.
3. Read the issue context. Note in one line the behavior the issue
   reports, and note any direction a maintainer gave in the thread.
4. Read the repo-facts block (live: the repo's contributing docs and
   templates). Note every stated ask: templates, contributing rules,
   AI-use disclosure.
5. Read the repro evidence before the plan. Note three things: the
   steps that trigger the behavior, the output or artifact that shows
   it, and where the evidence locates the fault. Also note anything
   the evidence does not establish. Reading it first matters because
   the plan's cause is graded against what the evidence shows, not
   against how convincing the plan sounds.
6. Read the candidate plan. Note its stated cause, its proposed
   change, its scope statement and named files, its order of work,
   its test plan, and its risks or unknowns.
7. Read the candidate plan comment last. Note what it commits to and
   whether it answers the maintainer direction and repo asks noted in
   steps 3 and 4.
8. Do not grade any check until steps 1 to 7 are done.

## Evidence gathering

For each check, gather exactly the two things its Evidence column
names, from the places the evidence guide maps. In eval mode, quote
lines from the bundle only and fetch nothing. In live mode, the drafts
are the candidate side; the issue thread, the posted repro comment,
and the repo docs are the other side.

1. diagnosis-grounded: quote the sentence where the plan states the
   cause. Quote the repro lines that show the behavior (steps and
   output).
2. targets-cause: quote the plan's proposed change (which file or
   area, what changes). Quote the repro line that locates the fault.
   If the evidence locates no fault, record "evidence locates no
   fault".
3. bounded-scope: quote the plan's scope statement and list every
   file or area the plan names. Next to each, record whether the
   issue's reported behavior requires touching it.
4. executable: quote the plan's first step of work and record whether
   it names a file or area and an action.
5. test-observable: quote the test plan. Record the action it names,
   the result it expects before the fix, the result it expects after,
   and which repro step it reuses.
6. honest-unknowns: list the claims the plan and the plan comment
   state as settled (the cause, "confirmed", "traced", "tested",
   "will fix"). Next to each, record one of three things: a repro
   step contradicts it (quote the step), a maintainer disputed it in
   the thread (quote the comment), or neither. A claim the repro evidence simply does not reach
   is "neither", and so is the author's report of an extra local
   check (a trace, a second format, a debug log) that the repro
   excerpt does not show.
7. thread-aware: quote the plan comment's lines that respond to the
   maintainer direction and to each repo ask noted during reading. If
   the contribution policy requires AI-use disclosure, record whether
   it covers comments (or "any form") or only pull requests, and
   quote the comment's disclosure or record "no disclosure". If the
   thread has no maintainer direction and the repo states no asks,
   record that.

## Check execution

1. Grade the checks in the table's order, one at a time. Each check
   is graded on its own: a fail on one check does not change the
   grade of another.
2. For each check, compare the two gathered items and apply the Pass
   condition exactly as the rubric words it. Grade `pass` if the
   condition is met and `fail` if it is not.
3. Grade `unclear` only when the evidence the check needs is absent
   from the package after looking in every place the evidence guide
   names. Evidence that is present but weak or wrong is `fail`, not
   `unclear`.
4. When a part of the plan is missing altogether (no stated cause, no
   scope statement, no test plan, no first step), grade the check
   that reads it `fail`: the plan is the thing being graded, and a
   missing part does not meet the pass condition.
5. For thread-aware, when there is no maintainer direction and no
   repo ask to follow, grade `pass`. A disclosure ask that the policy
   limits to pull requests does not apply to the plan comment. A
   disclosure ask that covers comments or "any form" does: grade
   `fail` when the comment has no disclosure.
   For honest-unknowns, grade `fail` only on a claim recorded as
   contradicted or disputed. A
   plan with no risks or unknowns section still passes when none of
   its claims was recorded that way.
6. Write one line of evidence for every grade: the quote or fact that
   decided it. Do not grade from the write-up's length, headings, or
   tone.
7. Grade from the notes and quotes already gathered. Re-read a part
   of the package only when a quote needed for the check is missing
   from the notes.
8. If a check passes by its stated condition but the plan still looks
   wrong, keep the pass and name the tension in the summary.

## Verdict assembly

1. List the seven `required` checks with their grades.
2. Change every `unclear` on a `required` check to `fail`, as the
   rubric's verdict rule directs.
3. If every `required` check is `pass`, the verdict is `accept`. If
   any is `fail`, the verdict is `reject`.
4. A check marked `preferred` in the rubric never changes the
   verdict. The rubric has none at present.
5. On a `reject`, name each failed `required` check in the summary
   and quote the evidence line that decided it. On an `accept`, say
   that all required checks passed.
6. In live mode, compare the draft plan comment with `voice-guide.md`
   and list any broken rule in the summary, quoting the rule. This
   does not change the verdict. In eval mode, skip this step.
7. Output the summary, then the JSON block from `SKILL.md`, with
   every check's original grade (`pass`, `fail`, or `unclear`) and
   its evidence line. The JSON block comes last.
