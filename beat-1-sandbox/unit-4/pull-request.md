# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/169

**Branch**

`fix/73-env-example-openrouter-key`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 17/20. `agreement: 17/20 scored items  (bar: 18/20: below the bar)`
2. Partial run on 8 packages (`--only`, the three disagreements, four canaries, and `calib-04`): 7/7 on the scored ones. `agreement: 7/7 scored items`
3. Full run: 20/20. `agreement: 20/20 scored items  (bar: 18/20: PASS)`

The last one is the run in `eval-run.txt`.

**Package analysis**

`pkg-05`. In my first full run my rubric decided `reject` and the gold label said `accept`.

Nine of the ten checks passed. The one that failed was `test-plan-covered`, which is `required`, so it held the package. The grader's line for it was: "Test-plan item 'same-name same-key: one row plus a warning' has no captured command or output, only the prose 'Same-key redefine prints the one-time warning'; fmt/clippy also stated without output."

The rubric read it that way because the check's pass condition then said every test-plan item needed "a shown result", and my procedure marked anything without captured output as `not shown`. The plan's test plan has three items: "re-run the issue's script, expect both `atuin` rows; re-run with two same-name same-key bindings, expect one row plus a warning; cargo test on the config crate." The PR's evidence shows before and after tables for the first item. For the other two it says: "Same-key redefine prints the one-time warning. `cargo test -p nu-protocol` passes (312 tests); fmt and clippy clean." Each item is named and has an outcome. My check wanted pasted output for all three, and the gold label treats a named outcome as enough for the secondary items. `pkg-08` and `pkg-19` failed the same check for the same reason.

**Check rationale**

> | test-plan-covered | Each item in the plan's test plan, and each check command or suite the plan or the repo's contributing instructions name (test suite, linter, formatter, pre-commit), read against the test-evidence section | Every test-plan item is addressed in the evidence with its outcome: either captured output, or a specific statement naming the item and what happened ("control with a tracked change still stashes", "`cargo test -p nu-protocol` passes (312 tests)"). Captured output is required only by evidence-observable, for the main repro; a specific stated outcome is enough for controls, secondary cases, and suite runs. A deviation note saying an item was not run, and why, also passes. Fail only if a test-plan item or a failure mode the plan names is not mentioned in the evidence at all, or if no run of the repo's own test suite or checks is reported anywhere. A template checklist item is template-honored's to grade, not this check's | required |

It started with this pass condition: "Every test-plan item has a shown result, or a deviation note says it was not run and why. Every repo check the plan or the repo facts name has a visible outcome. Fail if a named test-plan item or a named failure mode is silently absent from the evidence, or the repo's own checks were never run". The grader read "shown result" and "visible outcome" as captured output, and that rejected three PRs the gold labels accept (`pkg-05`, `pkg-08`, `pkg-19`).

I split the job between two checks. `evidence-observable` still requires captured before and after output for the main repro. `test-plan-covered` now asks whether each test-plan item is named in the evidence with what happened, and a specific sentence counts. It fails only when an item or a named failure mode isn't mentioned at all, or no run of the repo's checks is reported.

I also took template checklist items out of this check. On `pkg-08` the grader failed it with: "the template's 'code formatted' check has no formatter/lint outcome in the evidence." That is a template ask, so `template-honored` grades it now.

**Trade-offs**

Loosening `test-plan-covered` means I accept it will miss a PR that says a control or a test suite passed when it was never run. A specific sentence with no output behind it now passes this check.

I checked what that cost on the eval set. In my first full run, every `reject` package that failed `test-plan-covered` also failed at least one other required check (`pkg-01`, `pkg-04`, `pkg-07`, `pkg-10`, `pkg-12`, `pkg-14`, `pkg-17`, `pkg-20`), so loosening it could not turn any of those into an accept by itself. The grader could still read the other checks differently on a second pass, so I re-ran with `--only pkg-05,pkg-08,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`. `pkg-04`, `pkg-07`, `pkg-10`, and `pkg-14` are all four `not-tested` packages, the category this check belongs to, and they already agreed. `calib-04` is the calibration package where the plan names two failure modes and the evidence re-runs one. All five stayed `reject`, the three disagreements flipped to `accept`, and the full run after it was 20/20.

One more gap showed up when I ran the skill on my own PR. My procedure still said to fail a check that `docs/CONTRIBUTING.md` names and the evidence omits, and the rubric says to fail only an item that isn't mentioned. The run followed the rubric and reported the mismatch as a procedure gap. I left it, because changing `procedure.md` would change the fingerprint in `eval-run.txt`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
