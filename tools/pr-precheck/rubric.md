# Rubric: is this pull request ready to submit?

Every check reads the PR itself against the plan, the issue, or the
repo's stated asks. Locations named in the Evidence column are bundle
sections; `references/evidence-guide.md` gives the live-mode location
of each. "Deviation note" means a statement, in the plan's deviation
notes or in the PR description, that names a difference from the plan
and says why.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | Every changed file and hunk in the diff, read against the plan's scope statement, its named files, its "not in scope" line, and any deviation note | Every hunk is either part of the change the plan describes, a test for it, a file the repo's template requires for it (changelog, whatsnew, docs entry), or covered by a deviation note. Fail if any hunk changes behavior or a file the plan does not cover and no deviation note names it, or if a hunk does something the plan lists as not in scope | required |
| plan-delivered | Each change the plan's scope statement promises, read against the diff | Every promised change is present in the diff, or a deviation note says which part was not done and why. A disclosed shortfall passes. Fail if a promised change is missing with no note, or if the diff never implements the plan's fix at all | required |
| description-matches-diff | Each claim the title and description make about what the PR changes or does not change, read against the diff | Every such claim is true of the diff. Fail if the description claims a change the diff does not contain, omits or denies a behavior change the diff does contain (for example "no changes beyond the plan" over a hunk the plan never names), or claims fidelity to the plan that diff-within-plan or plan-delivered contradicts | required |
| evidence-observable | The test-evidence section, read against the diff and the repro evidence's steps | The evidence shows captured output for a named command or action that exercises the code the diff changes, with the result after the fix and the failing result before it (shown or quoted from the repro). Fail on "tests pass" or "works now" with nothing named, on evidence that only exercises a path the diff did not change, on no evidence, or on an admission that it was not tested | required |
| test-plan-covered | Each item in the plan's test plan, and each check command or suite the plan or the repo's contributing instructions name (test suite, linter, formatter, pre-commit), read against the test-evidence section | Every test-plan item is addressed in the evidence with its outcome: either captured output, or a specific statement naming the item and what happened ("control with a tracked change still stashes", "`cargo test -p nu-protocol` passes (312 tests)"). Captured output is required only by evidence-observable, for the main repro; a specific stated outcome is enough for controls, secondary cases, and suite runs. A deviation note saying an item was not run, and why, also passes. Fail only if a test-plan item or a failure mode the plan names is not mentioned in the evidence at all, or if no run of the repo's own test suite or checks is reported anywhere. A template checklist item is template-honored's to grade, not this check's | required |
| diff-reviewable | The unified diff and the commit list | A reviewer can see the fix without wading through noise: no debug prints or logging left in, no commented-out code, no dead or unused code added, no whitespace or formatting churn on lines the fix does not need, no generated or scratch files, no unrelated drive-by edits. Fail if any of these is present. Size alone never fails this check | required |
| template-honored | The repo facts' PR template asks, read against the description and the diff | Every stated ask is met with real content: each required section or checklist item is present and filled, an issue-closing reference is there if asked for, and any file the template requires (changelog or whatsnew entry) is in the diff. An ask that does not apply may be answered with a stated "n/a" and a reason. Pass if the repo states no template asks. Fail if a stated ask is missing, left as boilerplate, or ticked while the diff shows it was not done | required |
| disclosure-met | The repo facts' contribution policy and stated AI-use policy, read against the description | If the policy requires AI use to be disclosed, the description contains a disclosure that says how AI was used. If the policy bans AI-generated contributions, fail unless the description states none was used. Pass if no AI policy is stated | required |
| thread-direction-followed | Thread highlights from maintainers (author association MEMBER, OWNER, or COLLABORATOR), read against the diff and the description | Where a maintainer gave explicit direction about the fix or the PR, the PR follows it or the description says why it differs. Pass if no maintainer gave direction. Comments from non-maintainers are context, not direction | required |
| title-names-change | The PR title, read against the diff | The title says what the diff changes and where, specifically enough to tell it from another PR on the same file | preferred |

## Verdict rule

Accept (submit) only if every `required` check grades `pass`. If any
`required` check grades `fail`, reject (hold).

`unclear` on a `required` check counts as `fail`: a PR that cannot be
verified from the package is not ready to submit. Grade `unclear` only
when the package gives no way to decide the check; when the evidence
is simply missing, the pass condition already says fail.

`preferred` checks never change the verdict, whatever their grade.
Report them so the author can fix them.

Checks are graded independently: one problem may fail more than one
check (an unrelated hunk can fail both diff-within-plan and
diff-reviewable), and a pass on one never excuses a fail on another.
A PR that does less than the plan and says so can still be accepted:
the disclosure is what plan-delivered and test-plan-covered read.
