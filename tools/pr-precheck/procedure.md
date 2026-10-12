# Procedure: how this tool grades a PR package

Follow these steps in order. "Record" means write the quote or fact
into your working notes with the place it came from; every check is
later graded from those notes. Bundle section names are used below;
`references/evidence-guide.md` gives the live-mode location of each.

## Read order

Read the standards and the plan before the PR, and the diff before the
description. The order matters because the checks are side-by-side
reads: if the description is read first, its account of the change
becomes the lens the diff is read through, and drift it does not
mention goes unseen.

1. In live mode, read `scope.md` and apply the scope seam in
   `SKILL.md`. If it refuses, stop. Record the house rules. In eval
   mode, skip this step.
2. Read `rubric.md` and `references/evidence-guide.md`. Record every
   check's name and weight, in table order, and the verdict rule.
3. Read the repo facts (live: the PR template and
   `docs/CONTRIBUTING.md`). Record each stated ask as its own line:
   every template section or checklist item, every check the repo says
   to run, and the AI-use policy, or "no AI policy stated".
4. Read the issue and the thread highlights. Record in one line the
   behavior the issue reports. Record each comment from a maintainer
   (MEMBER, OWNER, COLLABORATOR) that gives direction about the fix or
   the PR, or "no maintainer direction".
5. Read the plan context. Record five lists:
   - the repro steps and the failing result each one showed;
   - the changes the plan's scope statement promises;
   - the files the plan names;
   - what the plan says is not in scope;
   - the test-plan items, one per line, including each failure mode
     the plan names and each repo check it says to run.
   Record any deviation notes in the plan, or "no deviation notes".
6. Read the diff, hunk by hunk, before the title and description.
   Record for each hunk: the file, and one line saying what it changes
   in behavior or content. Then read the commit list and record each
   commit subject.
7. Read the test evidence. Record each command or action shown, the
   output shown for it, and whether a before result is shown or quoted.
8. Read the title and description last. Record each claim about what
   the PR changes or does not change, each statement that names a
   difference from the plan (these are deviation notes too), each
   template section or checklist item it fills, and any AI-use
   disclosure.
9. In live mode, read `voice-guide.md` now. In eval mode, skip it.
10. Do not grade any check until these steps are done.

## Evidence gathering

Build the pairings below from the notes. Each is a two-column list:
the left column from the PR, the right from what it is read against.
In eval mode, use bundle text only.

1. **Diff against plan scope** (diff-within-plan). For each hunk from
   step 6, put next to it one label: `plan` (it makes a change the
   scope statement promises, in a file the plan names or one that
   change necessarily touches), `test` (a test of that change),
   `template` (a file a recorded template ask requires, such as a
   changelog entry), `deviation` (a deviation note names it; quote the
   note), `out-of-scope` (the plan's not-in-scope line covers it; quote
   the line), or `unexplained` (none of the above).
2. **Plan scope against diff** (plan-delivered). For each promised
   change from step 5, put next to it the hunk that delivers it, or
   the deviation note that says it was not done, or `missing`.
3. **Description against diff** (description-matches-diff). For each
   claim from step 8, put next to it the hunk that makes it true, or
   `no hunk`. Then for each hunk labelled `unexplained` or
   `out-of-scope` in pairing 1, record whether the description
   mentions it. Record any sentence that claims the PR matches the
   plan or changes nothing else.
4. **Evidence against the diff and the repro** (evidence-observable).
   For each command or action from step 7, record whether it runs code
   a hunk changed, the after result shown, and the before result
   (shown, quoted from the repro, or absent). If the evidence section
   is empty, says "tests pass" with nothing named, or admits it was
   not tested, record that in its words.
5. **Evidence against the test plan and the repo's checks**
   (test-plan-covered). For each test-plan item from step 5 and each
   check command or suite the plan or the contributing instructions
   name, put next to it one of: the captured output that shows its
   result; the evidence sentence that names it and states its outcome
   (quote it; prose counts); the deviation note saying it was not run;
   or `not mentioned`. Mark `not mentioned` only when the evidence
   section says nothing about that item. Do not list template
   checklist items here; they belong to pairing 7.
6. **Debris scan** (diff-reviewable). Go through the diff's added and
   removed lines once and record each instance, with file and line, of:
   a debug print or log call, commented-out code, code added but never
   used, a whitespace-only or reformatted line the fix does not need, a
   scratch or generated file, an edit unrelated to the issue. Record
   "none found" if there are none. Check the commit list for commits
   that are not part of the fix (for example "wip", "format").
7. **Template asks against the PR** (template-honored). For each
   template ask from step 3, put next to it the description text or
   diff file that meets it, a stated "n/a" with its reason, or
   `not met`. For each ticked checklist item, confirm the diff or the
   evidence shows it was done; if not, record `ticked, not done`.
8. **Policy against the description** (disclosure-met). Record the AI
   policy line from step 3 and the description's disclosure, or
   "no disclosure".
9. **Thread against the PR** (thread-direction-followed). For each
   maintainer direction from step 4, put next to it the hunk or
   description line that follows it, or the description's stated
   reason for differing, or `not addressed`.
10. **Title against the diff** (title-names-change). Record the title
    and the one-line summary of what the `plan` hunks change.

## Check execution

1. Grade the checks in the rubric table's order, one at a time, each
   from its own pairing. A grade on one check never changes the grade
   on another, even when the same hunk decides both.
2. Apply the pass condition exactly as the rubric words it:
   - diff-within-plan fails on any hunk labelled `unexplained` or
     `out-of-scope`.
   - plan-delivered fails on any promised change marked `missing`.
   - description-matches-diff fails on any claim with `no hunk`, on an
     unmentioned `unexplained` or `out-of-scope` hunk that changes
     behavior, or on a fidelity sentence when either check above
     failed.
   - evidence-observable fails when no recorded command runs changed
     code with an after result shown, or when no before result exists
     anywhere in the package.
   - test-plan-covered fails on any item marked `not mentioned`. A
     stated outcome without captured output passes here.
   - diff-reviewable fails when the debris scan recorded any instance.
   - template-honored fails on any ask marked `not met` or
     `ticked, not done`.
   - disclosure-met fails when the policy requires disclosure and the
     record says "no disclosure".
   - thread-direction-followed fails on any direction marked
     `not addressed`.
3. When the thing a check reads is absent from the PR (no test
   evidence, no description, a template section left out), grade
   `fail`: the PR is what is being graded, and a missing part does not
   meet the pass condition. When the other side is absent (no template
   asks, no AI policy, no maintainer direction), grade `pass`, as the
   rubric says.
4. Grade `unclear` only when the package does not let the pairing be
   built: the plan context does not state its scope or test plan, or
   the diff is visibly cut off. Say what was missing.
5. Write one evidence line for every grade: the quote or fact from the
   pairing that decided it, naming the file or section. For a pass,
   name the fact that met the condition, not "no problems found".
6. Grade from the notes. Re-read a part of the package only when a
   quote the check needs is not in the notes.
7. Do not grade from length, formatting, tone, or commit count. If a
   check passes by its condition but the PR still looks wrong, keep
   the pass and name the tension in the summary.

## Verdict assembly

1. List every check with its grade and weight, in rubric order.
2. For the verdict only, count each `unclear` on a `required` check as
   `fail`, as the rubric's verdict rule directs. Keep the original
   grade in the output.
3. If every `required` check is `pass`, the verdict is `accept`. If
   any is `fail`, the verdict is `reject`. `preferred` checks never
   change it.
4. On a `reject`, the deciding check is the first failing `required`
   check in rubric order. Name it in the summary and quote its
   evidence line, then list any other failed required checks by name.
   On an `accept`, say that all required checks passed.
5. In live mode, add to the summary: each voice-guide rule the draft
   title or description breaks (quote the rule and the words), and
   each house rule from `scope.md` the PR breaks. Neither changes the
   verdict. In eval mode, skip this step.
6. Add a "Procedure gaps" line if a step here did not say what to do.
7. Output the summary, then the JSON block from `SKILL.md`: one entry
   per check in rubric order, the rubric's check name, the original
   grade, the evidence line. The JSON block comes last.
