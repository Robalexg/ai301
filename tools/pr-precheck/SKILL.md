---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package. Do not answer from gut feel and do not
invent a method: execute `procedure.md`, which applies the checks in
`rubric.md` to evidence located per `references/evidence-guide.md`.
This file tells you what to read, when to refuse, and what to emit.

## The question

Answer exactly one question about exactly one PR package: **is this
ready to submit?**

A PR package is a candidate pull request: its title, its description,
its commit list, its diff, and its test evidence. Read it against two
things: the plan it claims to implement (deviation notes included, with
the repro evidence that plan built on) and the issue that plan belongs
to (the issue body, its thread, and the repo's stated asks: the PR
template and the contribution policy).

Do not answer any other question. Do not judge whether the fix is the
best possible one, whether the plan was a good plan, whether a
maintainer would merge it, or whether the issue was worth taking. Do
not grade more than one package per run: if you are handed two, stop
and ask which one.

## Inputs and modes

Decide the mode before reading anything else. If you were handed a
package bundle (a file from `eval/packages/`, or bundle text inlined in
the prompt), or the prompt says eval mode, you are in eval mode.
Otherwise you are in live mode.

### Live mode

Live mode checks the student's own submission before the pull request
is opened. Run it from the student's working copy, on the PR branch.
Read these inputs, and only these:

| Input | Where it comes from |
|---|---|
| The plan | `plan.md`, the file the invoking command names, deviation notes included. |
| Draft PR title and description | `pr_draft.md`, the file the invoking command names. |
| Test evidence | `test_evidence.md`, the file the invoking command names: captured output, not a retelling. |
| The diff | The output of `git diff main...HEAD` (three dots), run from the working copy. This is everything the branch changes relative to the default branch. |
| The commit list | `git log main..HEAD --oneline`, from the same working copy. |
| The issue and its thread | The issue the plan belongs to, in the scoped repo, with every comment: `gh issue view <number> --repo <scoped repo> --comments`. |
| The repo's stated asks | The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md`, read from the working copy. |

Rules for reading them:

- Get the diff by running the command. Never reconstruct it from the
  description, the commit messages, or files you happen to have open.
- The package is what those inputs contain. Do not pull in other files
  from the working directory to rescue a claim the package does not
  support, and do not run the test suite yourself to fill in missing
  test evidence: absent evidence is what the checks grade.
- A house-chain student has no posted plan of their own. For them, the
  plan is the house plan and the repro evidence is the house repro
  pack, each read from the file the invoking command names. Grade the
  same checks against the same things.

Refuse to grade, and say exactly what is missing, when:

- the invoking command names a file that does not exist or is empty
  (a missing test-evidence file is the exception: grade it as absent
  evidence);
- `git diff main...HEAD` is empty, or the working copy is on `main`:
  there is no candidate change to grade;
- you cannot tell which issue the plan belongs to;
- the scope seam below refuses;
- the component rule in "Component reads" refuses.

A refusal is not a verdict. When you refuse, name the reason and the
fix in one or two sentences, emit no checks, no verdict, and no JSON
block, and stop.

### Eval mode

Eval mode grades a practice submission: one frozen package bundle. The
bundle is the whole world. It carries the issue context, the thread
highlights, a repo-facts block (the repo's PR-template asks and
contribution policy), a plan-context block (the accepted plan and the
repro evidence it built on), and the candidate PR (title, description,
commit list, unified diff, test evidence).

- Take every fact from the bundle text. Fetch nothing: no `gh`, no
  `git`, no web, no other files. The bundle's `source:` line names a
  real issue; never open it. When the bundle and the live issue could
  disagree, the bundle wins.
- Always grade the complete package: every check in `rubric.md`, the
  full verdict rule. Never refuse in eval mode because something in the
  bundle is thin or missing; grade the absence.
- Ignore `scope.md` and `voice-guide.md` entirely.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else, before the plan and before the diff.

- Find its `Repo:` line. If that line still carries an unfilled
  placeholder (angle-bracket text such as `<ORG>/<PATH-REVIEW-REPO>`),
  stop without grading and tell the student to fill the `Repo:` line in
  `scope.md` with their section's Path Review repo. Never guess a
  scope, and never infer one from the working copy's remotes.
- Confirm the work lives in the scoped repo: the working copy's remotes
  (`git remote -v`) point at the scoped repo or a fork of it, and the
  issue is an issue in the scoped repo. If either is not true, refuse
  to grade and name the repo you found next to the repo the scope
  allows.
- Read the scope's house rules and apply them as the rules of the room:
  they change how evidence is read there (for example, a classmate's
  open PR on the same issue is not a blocker). A house rule never adds
  a check and never changes a pass condition; where the rubric has a
  check that a house rule bears on, grade that check with the rule in
  hand, and otherwise report a broken house rule in the summary.

In eval mode, ignore `scope.md` entirely: do not read it, do not apply
its rules, do not refuse on it.

## The voice seam (live mode only)

In live mode, after the scope passes, read `voice-guide.md`: the
student's own rules for how they write upstream.

- Hold the outgoing PR text, the draft title and the draft description,
  against those rules. Nothing else is outgoing text: do not hold the
  plan, the commit messages, or the test evidence to the voice guide.
- For each rule the draft breaks, report it in the summary: quote the
  rule, quote the words in the draft that break it, and say which of
  the two (title or description) they are in.
- If the voice guide has no rules written in it, say so in one line and
  move on.
- The voice guide never changes the verdict on its own. It is the
  student's personal standard, not the readiness bar; a broken voice
  rule moves the verdict only if `rubric.md` has a check that reads the
  voice guide, and then only through that check's grade.

In eval mode, ignore `voice-guide.md` entirely: voice is personal and
carries no gold labels. Universal communication-quality checks live in
the rubric.

## Component reads

Three files do the grading. Read all three, in this order, before
grading any check:

1. `rubric.md` decides **what**. Its checks table names each check, the
   evidence it reads, its pass condition, and its weight: `required`
   checks gate the verdict, `preferred` checks never change it. Its
   verdict rule says how the grades combine into accept or reject.
2. `references/evidence-guide.md` decides **where**. For each evidence
   family a check names, it says where that evidence lives in a bundle
   and in live mode, and what good looks like there. Use it as the map;
   it defines no checks and no pass conditions of its own.
3. `procedure.md` decides **how**. Execute it as written, stage by
   stage: its read order, its evidence gathering, its check execution,
   its verdict assembly.

Where the procedure is silent on a step you need, do not improvise a
step and do not quietly skip one. Do the smallest thing the rubric and
the evidence guide support, and report the gap in the summary under a
"Procedure gaps" line, naming the stage and what it failed to decide.
Do the same when a check names evidence the evidence guide cannot
locate. A gap is feedback the student needs.

Refuse to grade if `rubric.md` has no checks filled in or no verdict
rule, or if `procedure.md` has no steps filled in. Instruction comments
and bare headings are not content. Say which file is empty, that this
tool cannot grade without a rubric and a procedure by design, and stop.
Never invent checks, pass conditions, or steps to cover for an empty
file: a verdict from invented checks looks like judgment and is noise.

## Verdict and output

The verdict space is binary: `accept` means ready, submit it; `reject`
means hold. There is no third verdict, no "accept with reservations",
and no score. A reservation goes in the evidence line of the check it
belongs to.

Before the JSON block, show a short readable summary: one line per
check with its grade and the fact that decided it, the deciding check
for a reject, any procedure gaps, and in live mode any broken voice
rules and house rules. Then end your reply with the fenced JSON block
below, valid and last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Fill it this way:

- `item`: in eval mode, the bundle id from the bundle's title (for
  example `pkg-07`). In live mode, the PR URL if the pull request
  already exists; otherwise the scoped repo and branch, written
  `owner/repo:branch`.
- `checks`: one entry per row of the rubric's checks table, every row,
  `required` and `preferred` alike, in rubric order. `name` is the
  rubric's check name, copied exactly. `grade` is exactly one of
  `pass`, `fail`, `unclear`. `evidence` is one line.
- `verdict`: exactly `accept` or `reject`, as the rubric's verdict rule
  produces it from those grades.

Add no fields, rename none, reorder none. The harness parses the last
fenced JSON block in your reply, so emit exactly one, and write nothing
after it.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it, and where in the package it sits. "Looks
  fine" is not evidence. If you cannot name the fact, you have not
  graded the check.
- **Grade the thing, not the polish.** A terse complete PR can be ready
  and a long confident one can be hiding drift. Read the artifact
  itself against the plan, the issue, and the repo's stated asks; never
  grade length, formatting, or tone. A claim in the description is not
  evidence for itself: check it against the diff and the test evidence.
- **An honest shortfall can be ready.** A PR that delivers less than
  the plan and says so, in a deviation note or in the description, is
  not the same as one that drifts silently. Grade the disclosure by the
  rubric's conditions; do not equate "less than everything" with hold.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it passes; if it fails by its stated
  condition but feels harsh, it fails. Note the tension in the summary
  if you want. The fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Grade `unclear` only when the package
  does not let you decide a check either way, and say what was missing.
  Then treat it as the rubric's verdict rule directs. Where the rule is
  silent, count `unclear` as `fail`: a PR you cannot verify from the
  package is a PR that is not ready to submit.
