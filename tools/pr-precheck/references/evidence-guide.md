# Evidence guide: where evidence lives in a PR package

For each evidence family: where to look in an eval bundle, where to
look in live mode, and what good looks like. In live mode the files
are the ones the invoking command names, in the student's working
copy: `plan.md`, `test_evidence.md`, and `pr_draft.md`.

An eval bundle has these sections, in this order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Plan context`, and
`## Candidate PR` with `### Title`, `### Description`, `### Commits`,
`### Diff`, and `### Test evidence` under it.

## Plan fidelity (harness category: silent-drift)

**Where it lives, eval bundle.**

- The plan side is `## Plan context`. It is prose, not a form, so find
  the parts by their lead words: "Plan:" or "Scope" opens the promised
  change; "Files:" lists the files the plan names; "Not in scope:" is
  the boundary; "Test plan:" belongs to the next family. Deviation
  notes, when the plan has any, sit in the same block and say what
  changed from the posted plan and why.
- The PR side is `### Diff` under `## Candidate PR`: the `--- a/` and
  `+++ b/` lines give the changed files, each `@@` starts a hunk.
- Fidelity claims are in `### Description`: sentences saying what the
  PR does, what it leaves alone, or that it follows the plan. A
  description sentence that names a difference from the plan and
  gives the reason is also a deviation note.

**Where it lives, live mode.**

- The plan side is `plan.md`: its scope statement, named files,
  not-in-scope line, and its deviation notes section. A house-chain
  student's plan side is the house plan file.
- The PR side is the output of `git diff main...HEAD`, run from the
  working copy. Use `git diff main...HEAD --stat` for the file list.
- Fidelity claims are in `pr_draft.md`, in the Summary, Changes, and
  Notes for Reviewers sections.

**What good looks like.** Every hunk is the promised change, a test
of it, a file the repo's template requires for it, or named in a
deviation note; and every change the plan promises has a hunk or a
deviation note saying it was not done. Every description sentence
about what changed is true of the diff. Silent drift is any of three
things: a hunk in a file or behavior the plan never names, with no
note (more than the plan); a promised change with no hunk and no note
(less than the plan); or a description that says "implements the
plan" or "no other changes" over either of those. A mismatch with an
honest note is not drift.

## Test evidence (harness category: not-tested)

**Where it lives, eval bundle.**

- The proof is `### Test evidence` under `## Candidate PR`: commands,
  their captured output, and any before/after comments.
- It is read against the "Test plan:" sentences in `## Plan context`
  (each item and each named failure mode is its own line to cover) and
  the "Repro evidence" sentences in the same block (the steps and the
  failing result that count as "before").
- The repo's own checks are named in `## Repo facts`, on the
  "pull requests:" and "contribution policy" lines (for example
  `cargo test`, pre-commit, a linter), and often again at the end of
  the test plan.

**Where it lives, live mode.**

- The proof is `test_evidence.md`: captured terminal output. If the
  file is missing, the evidence is absent.
- It is read against the test plan section of `plan.md` and the repro
  steps the plan quotes (a house-chain student: the house repro pack).
- The repo's own checks are in `docs/CONTRIBUTING.md` and the Testing
  checklist of `.github/PULL_REQUEST_TEMPLATE.md`: `make test-unit`,
  `make test-integration`, `make lint`, `make typecheck`.

**What good looks like.** The evidence names a command or action that
runs the code the diff changed, shows its output after the fix, and
shows or quotes the failing result before it. That captured before/after is needed for the
main repro only. Every other test-plan item (a control, a second case,
a suite run) is addressed by name with its outcome, as captured output
or as a specific sentence ("`go test ./...` passes (4108 tests)",
"controls unchanged"), or has a note saying it was not run and why. Not decisive: "tests pass" or "works
now" with no command and no output; output from a path the diff did
not touch; one of two named failure modes shown and the other silent;
a suite the repo asks for that never appears.

## Diff quality (harness category: unreviewable)

**Where it lives, eval bundle.** `### Diff` and `### Commits` under
`## Candidate PR`. Read the `+` and `-` lines themselves; context
lines are not part of the change.

**Where it lives, live mode.** The output of `git diff main...HEAD`
for the lines, and `git log main..HEAD --oneline` for the commits.

**What good looks like.** Every added or removed line is needed for
the fix or its test, so a reviewer can find the fix on first read.
Debris is any of: a leftover debug print or log call (`print(`,
`console.log`, `dbg!`, `println!` used for tracing); commented-out
code; a function, import, or variable added and never used; lines
that differ only in whitespace, quoting, or import order outside the
fix; a scratch, editor, or generated file; an edit to something the
issue does not concern. In the commit list, a commit whose subject is
not about the fix ("wip", "format", "misc cleanup") points at where
to look. A large diff is not debris if every line serves the fix.

## Standards and comms (harness category: standards-wall)

**Where it lives, eval bundle.**

- The asks are in `## Repo facts`: the "pull requests:" line states
  the PR template's sections and checklist items (or "no PR
  template" and what the docs ask instead); the "contribution policy"
  line states the contributing rules and the AI-use policy, or "no
  stated AI policy".
- Maintainer direction is in `## Thread highlights`. Each comment
  shows the author's association in parentheses; only MEMBER, OWNER,
  or COLLABORATOR counts as direction. NONE and CONTRIBUTOR are
  context.
- The PR's answer is `### Title` and `### Description`, plus the
  diff for any file an ask requires (a changelog or whatsnew entry).

**Where it lives, live mode.**

- The asks are `.github/PULL_REQUEST_TEMPLATE.md` (sections: Summary,
  Issue with `Closes #`, Changes, Testing checklist, Screenshots /
  Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md`, both read
  from the working copy, plus the house rules in `scope.md`.
- Maintainer direction is in the issue thread:
  `gh issue view <number> --repo <scoped repo> --comments`.
- The PR's answer is `pr_draft.md`: its title line and its body.

**What good looks like.** Each stated section is present with content
about this change, the issue-closing reference carries the real issue
number, each checklist item is either ticked with the evidence to
back it or marked n/a with a reason, and any file the template
requires is in the diff. Where the policy requires AI-use disclosure,
the description says that AI was used and for what. Where a
maintainer gave direction, the PR follows it or the description says
why not. Not compliant: a section missing or still holding the
template's placeholder comment; `Closes #` with no number; a box
ticked for a check the test evidence never shows; a required
disclosure absent. Whether the description's claims match the diff is
plan fidelity, above, not this family.
