# Rubric: is this reproduction package ready to post?

A package is ready when a stranger on the issue thread could trust it:
it says where it ran, how to get there, shows the issue's own behavior
(not a neighbor's), claims no more than its artifacts show, and speaks
the repo's language. Each check below judges one of those outcomes,
never the write-up's length, headings, or tone of confidence.

The questions a stranger asks of any repro, and the check that answers
each one:

| Question | Answered by |
|---|---|
| What version? What OS? Where did you run it? The environment? | environment-recorded |
| What did you run? What steps? | steps-followable |
| What did it show? | behavior-matches-issue |
| Expected vs actual? | expected-vs-actual |

A question can be answered in any wording or layout; the checks ask
whether the answer is in the package, not whether it sits under a
heading with that name.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment record, read against the issue's stated environment and the repo-facts block's bug-report template asks. Answers: what version, what OS, where it ran (install method or build: release binary, package manager, built from source, debug vs. release build, container), and any other environment detail. | The report names the exact version of the software it ran AND the OS/platform AND where/how it ran (install method or build). It also records every environment detail the issue says changes the behavior (e.g. debug vs. release build, a platform the issue singles out). If the tested version differs from the issue's, the report says so. A missing version, a missing OS, or a missing detail the issue says matters is a fail. | required |
| steps-followable | The repro report's steps and commands, read from the stated starting state to the trigger, compared with the issue's own reproduction (inputs, commands, flags). Answers: what did you run, and what steps got you there. | A stranger could reach the trigger using only what the package contains: every command and flag is given literally (or the report explicitly says it ran the issue's exact reproduction), and every input is either shown or described precisely enough to rebuild, naming the element that triggers the bug (e.g. "an env.yml with a valid `dependencies:` list plus a `category:` section"). A minimal stand-in input that keeps the issue's trigger is fine. A change that alters the trigger (different syntax, a dropped flag the issue uses) that is not called out is a fail, and so is an input a stranger cannot get (a private repo, an internal config). | required |
| behavior-matches-issue | The report's artifacts (output excerpts, logs, error text, `git status`/`list` output), read against the specific symptom the issue describes (its error message, panic text, or observable state). Answers: what did it show. | The artifact shows the issue's specific symptom: the same error/panic message, or the same observable state the issue names. A different error, even of the "same class" (a parse error where the issue reports a panic), is a fail. No artifact at all (only a description in words) is a fail. An evidenced cannot-reproduce (the report ran the issue's steps and the artifact shows the symptom absent) passes this check. | required |
| expected-vs-actual | The repro report's statement of what should have happened and what did happen, read against the issue's expected behavior and against the report's own artifacts. | The report makes clear what it expected to see (the correct behavior, or, for a cannot-reproduce, the symptom the issue predicts; in its own words or by explicit reference to the issue) AND states what actually happened, and that stated actual is the thing its artifact shows. A report that only says "it's broken" / "same here" with no expected behavior, or whose stated actual is not what its artifact shows, is a fail. | required |
| outcome-honest | Every claim in the claim comment and repro report about what happened (reproduced / not reproduced, on which versions or platforms, how often, what the cause is), each matched to the artifact in the package that backs it. | The main outcome (reproduced, or could not reproduce) is backed by an artifact in the package, and it agrees with that artifact. Any claim that widens the bug's scope beyond what is shown (other versions, other platforms, other components, "on all my machines", "everyone has this") has its own artifact or is marked untested. A cause stated as fact needs evidence in the package (a stack frame counts). Supporting details about the shown run need no separate artifact when they are specific enough to re-run: a repeat count ("ran it 5 times"), or a named variant or control described with its exact change ("dropping `-r $1` gives 1, 4, 7, 10"). Vague breadth, an unbacked scope claim, or an outcome that contradicts the artifacts is a fail. | required |
| claim-specific-and-thread-aware | The claim comment, read against the issue title/body and the thread highlights (other claims, linked or pushed fixes, a reporter who says they have a fix). | The claim names this issue's specific behavior, says concretely what the claimant will do next, and does not ignore a fix that already exists: if the thread shows a fix or PR that has been opened, pushed, or is ready (or a maintainer saying the bug lives in another project), the claim acknowledges it and positions the work accordingly (e.g. testing, coordinating). Discussion, triage, or a "looking into this" note is not an existing fix and needs no acknowledgment. A generic claim that could be pasted on any issue, or one that ignores an existing fix, is a fail. | required |
| repo-policy-respected | The repo-facts block's contribution policy (including any AI-use disclosure requirement) and bug-report template, read against both comments. | Both comments comply with every stated policy. If the repo requires AI-use disclosure, the comments contain a disclosure that meets the stated requirement; if the policy restricts outside contributions, the claim respects that restriction. A missing required disclosure is a fail regardless of how good the repro is. If the repo states no policy, this passes. | required |
| comms-register | The words of both comments: requests, demands, and statements addressed to maintainers. | The comments carry information, not pressure: no "+1/same here" as the substance, no demands about priority or timelines, no complaints about the project, no promises of a fix on a timeline. Emoji or informality alone do not fail this; content that pressures maintainers or substitutes feeling for evidence does. | required |
| control-run | The repro report's artifacts, looking for a run that varies one input away from the trigger. | The report includes a control (the same command with the trigger removed or shifted) whose artifact shows normal behavior, isolating the trigger. | preferred |

## Verdict rule

Accept if every `required` check passes. Any `required` check graded
`fail` or `unclear` rejects the package: proof that cannot be verified
from the package is proof that is not ready to post. `preferred`
checks are reported but never change the verdict. In a claim-only
live draft, the checks that need the repro report (environment-recorded,
steps-followable, behavior-matches-issue, expected-vs-actual,
control-run) are reported as `unclear` with `not yet applicable:
claim-only draft` and left out of this rule; the remaining required
checks decide.
