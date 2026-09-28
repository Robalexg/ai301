# Evidence guide: where proof lives in a reproduction package

The rubric's checks each name a kind of proof. This guide says where to
find it and how to tell whether what you found is good enough. In eval
mode the bundle is the whole world: every location below is a section
of the package file. In live mode the issue side comes from GitHub and
the candidate side is the student's draft file(s).

## Environment

Answers: what version, what OS, where did you run it, the environment.

- Where it lives:
  - Eval: the repro report's environment line or opening paragraph
    (often "Environment:"), plus any version shown in the output
    itself (a `--version` line, a banner). Compare against the issue's
    stated environment (in the issue body) and the repo-facts block's
    "bug reports: template asks for" line and "latest release" line.
  - Live: the draft repro comment; the issue body and its template
    fields on GitHub; the repo's `.github/ISSUE_TEMPLATE/` for what the
    template asks; the Releases page for the current version.
- What good looks like:
  - The exact version tested is named (a number, or a commit hash for
    a source build). "Latest" alone is not a version.
  - The OS/platform is named.
  - Where/how it ran is named: install method (release binary,
    Homebrew, asdf, package manager, built from source) and build
    profile (debug vs. release) when the software has one.
  - Every environment detail the issue says changes the behavior is
    recorded. Example: if the issue says debug builds panic and
    release builds hang, the report must say which build it ran.
  - If the tested version differs from the issue's, the report says so
    out loud ("filed on 0.63.1, tested 0.64.1").

## Steps

Answers: what did you run, what steps.

- Where it lives:
  - Eval: the repro report's command blocks and step list, read in
    order from the starting state (fresh repo, input file contents,
    clean install) to the trigger. Compare each input, command, and
    flag against the issue's own reproduction in the issue body.
  - Live: the draft repro comment against the issue body's steps on
    GitHub.
- What good looks like:
  - A stranger could run it with nothing but the comment: commands are
    literal, flags are all present, and each input is shown or
    described precisely enough to rebuild (the triggering element is
    named). Inputs from a private repo or internal config fail this.
  - A minimal stand-in input is fine when it keeps the issue's trigger
    (a local yml with the same bad section in place of the issue's
    URL).
  - Interactive steps (a TUI keypress, a UI click) are stated in words
    next to the command that opens the UI.
  - "Ran the issue's exact reproduction" counts as a step when the
    issue's reproduction is itself complete and literal.
  - Inputs match the issue's character for character. Any difference
    is labeled as deliberate. Watch for small silent changes: `:`
    instead of `=`, a missing flag, a different file.

## Behavior shown

Answers: what did it show, expected vs actual.

- Where it lives:
  - Eval: the repro report's pasted output, logs, error text, and
    follow-up state commands (`git status`, `stash list`, exit codes).
    Compare against the symptom in the issue body: its exact error or
    panic message, stack frame, or the observable state it names.
    Expected vs. actual lives in the report's own statements, usually
    near the end, and in the issue's "Expected" wording.
  - Live: the draft repro comment's code blocks against the issue body
    on GitHub.
- What good looks like:
  - The pasted output contains the issue's specific symptom: the same
    error string or panic text, or the same end state (e.g. "no stash
    created, file still untracked" shown by `git stash list` printing
    nothing and `git status` showing `??`).
  - A different error does not count, even if it is "the same kind of
    failure". A parse error is a different bug from a panic in the
    decoder.
  - A description in words ("the editor disappears") with no pasted
    output is not an artifact.
  - The report states what it expected to see (the correct behavior,
    or for a cannot-reproduce the symptom the issue predicts; its own
    words or a pointer to the issue) and what did happen, and
    the "actual" it states is what the pasted output shows.
  - A control run (same command with the trigger removed or moved)
    that behaves normally is a strong sign the trigger is isolated.

## Honesty

- Where it lives:
  - Eval: every sentence in the claim comment and repro report that
    says what happened, where, how often, or why. Match each one to
    the pasted output that backs it.
  - Live: the same, in the draft files.
- What good looks like:
  - Each outcome claim points at output in the package. "Reproduced on
    0.64.1" is backed by a 0.64.1 run shown in the report.
  - Claims that widen the scope beyond what is shown (other versions,
    platforms, components, "on all my machines", "everyone has this")
    have their own output, or are labeled untested.
  - Specific supporting details about the shown run are fine in prose:
    a repeat count, or a named variant/control with its exact change
    ("dropping `-r $1` gives 1, 4, 7, 10"). A stranger can re-run
    those from the words alone.
  - A cause is stated as a guess unless the package shows it (a stack
    frame pointing at the function counts as showing it).
  - The stated outcome agrees with the output. Calling a different
    error "exactly the failure the issue describes" is a mismatch.
  - An honest cannot-reproduce is good: it ran the issue's steps,
    shows the output without the symptom, and says so plainly.

## Comms

- Where it lives:
  - Eval: the claim comment, read against the issue title and body and
    the "Thread highlights" section (other claims, "pushed a fix",
    linked PRs, a reporter who says a fix is ready). Both comments,
    read against the repo-facts block's "contribution policy" line,
    including any AI-use disclosure requirement or limit on outside
    PRs.
  - Live: the draft comments against the issue thread on GitHub, the
    repo's `CONTRIBUTING.md`, any `AI_POLICY.md` or AI section, and the
    issue/PR templates. The Path Review house rules in `scope.md`
    change how classmates' claims are read.
- What good looks like:
  - The claim names this issue's specific behavior and one concrete
    next step. It could not be pasted onto a different issue.
  - The claim reacts to an existing fix: if the thread shows a fix or
    PR opened, pushed, or ready (or a maintainer placing the bug in
    another project), the claim says so and offers something that fits
    (testing, review, coordinating). Triage chatter and "looking into
    this" notes are not fixes.
  - If the repo requires AI-use disclosure, the comments include it in
    the form the policy asks for. A missing required disclosure fails
    even when everything else is strong.
  - The words carry information: no "+1" as the whole point, no
    demands about priority, no complaints about the project, no
    timeline promises.
