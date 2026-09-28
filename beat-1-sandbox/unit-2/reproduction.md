# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Robalexg

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5861709037

Hi, I'd like to work on this. I'll follow the README setup on a fresh clone of `main`, run `cp .env.example .env`, and check whether the resulting `.env` has the `OPENROUTER_API_KEY` the README tells you to add. I'll also compare the `LLM_PROVIDER` options in `.env.example` with the fields in `core/config.py`. Next I'll post a repro report here with my environment, the commands I ran, and the output, including if I can't reproduce it.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5861752097

````markdown
## Repro report: README and `.env.example` disagree about the LLM API key

Reproduced on current `main`.

**Environment**

- OS: macOS 26.6.2 (arm64)
- Python 3.13.13 (venv), git 2.54.0
- Code: my fork of `codepath/pathreview-ai301-fa26-s3`, `main` at `2f4e82f`, no local changes

**Setup and steps** (from a fresh clone, following README Quick Start and `docs/SETUP.md`)

I did the setup steps that touch config: copy the env file, then the Python part of `make setup` (venv plus `pip install -e ".[dev]"`). I skipped `docker compose up -d` and the migrations/seed steps because Docker isn't installed on my machine. This bug is in the env file and settings loading, so those steps don't affect it.

```
$ git rev-parse --short HEAD
2f4e82f
$ cp .env.example .env
$ python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"

$ grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)

$ grep -nE "Options|LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER" .env
17:# Options: "mock" (default, no API key needed), "openai"
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here

$ grep -n "OPENROUTER_API_KEY" .env || echo "OPENROUTER_API_KEY not found in .env"
OPENROUTER_API_KEY not found in .env

$ grep -nE "llm_provider|openai_api_key|openrouter" core/config.py
18:    llm_provider: str = Field(default="mock")
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```

Then I loaded the app's settings from that `.env`:

```
$ .venv/bin/python -c "from core.config import settings as s; print('llm_provider=%r openrouter_api_key=%r openai_api_key=%r' % (s.llm_provider, s.openrouter_api_key, s.openai_api_key))"
llm_provider='mock' openrouter_api_key='' openai_api_key='sk-your-key-here'
```

**Control:** I added one line, `OPENROUTER_API_KEY=sk-or-test-123`, to `.env` and loaded settings again:

```
$ echo "OPENROUTER_API_KEY=sk-or-test-123" >> .env
$ .venv/bin/python -c "from core.config import settings as s; print('llm_provider=%r openrouter_api_key=%r' % (s.llm_provider, s.openrouter_api_key))"
llm_provider='mock' openrouter_api_key='sk-or-test-123'
```

So the config reads the variable once it's in `.env`. `.env.example` is missing the line, and its comment is missing the OpenRouter option.

**Expected:** after `cp .env.example .env`, the file has the `OPENROUTER_API_KEY` entry the README and SETUP tell you to fill in, and its `LLM_PROVIDER` comment matches the providers `core/config.py` supports.

**Actual:** the copied `.env` has no `OPENROUTER_API_KEY` line, its comment lists only `mock` and `openai`, and settings load with `openrouter_api_key=''`. `core/config.py` does define the OpenRouter fields.

I didn't start the app or call any LLM provider. This report covers the config and docs mismatch only.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 15/20 scored items  (bar: 18/20: below the bar)`. Categories: `clear-accept 3/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. All five misses (pkg-03, pkg-05, pkg-07, pkg-09, pkg-10) were gold accepts that my rubric rejected.
2. `--only` run on the five misses plus five canaries (pkg-06, pkg-08, pkg-18, pkg-19, pkg-20), after revising four checks: `agreement: 10/10 scored items` (partial run, no bar verdict).
3. Confirming full run with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

**Package analysis**

`pkg-09` (sharkdp/fd#2033, gold `accept`). On my first run my rubric graded it `reject`, failing `expected-vs-actual`, `outcome-honest`, and the preferred `control-run`. It is an honest cannot-reproduce: "Result: I could NOT reproduce scenario 2", with one pasted run where "all ONE batches were written before any TWO batch". My rubric read it wrong twice. For `expected-vs-actual` the grader said "'Expected (per the report): ... a TWO flush should appear in the log before the final ONE flush' states the bug outcome, not the documented in-order behavior", because my pass condition only allowed "what correct behavior would be". For a cannot-reproduce, the expected thing to look for IS the bug's symptom. For `outcome-honest` the grader said "'I ran this 5 times and also re-ran with the second command's arguments padded' and 'in every run' have no backing artifact", because my condition demanded an artifact for every breadth claim, including "ran it ten times". The report's main outcome was backed by the pasted log, and the repeat runs were specific enough to re-run. After I revised both checks, the final run graded pkg-09 `accept`, matching gold.

**Check rationale**

| outcome-honest | Every claim in the claim comment and repro report about what happened (reproduced / not reproduced, on which versions or platforms, how often, what the cause is), each matched to the artifact in the package that backs it. | The main outcome (reproduced, or could not reproduce) is backed by an artifact in the package, and it agrees with that artifact. Any claim that widens the bug's scope beyond what is shown (other versions, other platforms, other components, "on all my machines", "everyone has this") has its own artifact or is marked untested. A cause stated as fact needs evidence in the package (a stack frame counts). Supporting details about the shown run need no separate artifact when they are specific enough to re-run: a repeat count ("ran it 5 times"), or a named variant or control described with its exact change ("dropping `-r $1` gives 1, 4, 7, 10"). Vague breadth, an unbacked scope claim, or an outcome that contradicts the artifacts is a fail. | required |

It reads this way because my first version failed three good packages. The first version said: "Claims of breadth ("on all my machines", "also confirmed on 4.53.2", "ran it ten times") with no artifact, or a stated cause presented as fact without evidence, are a fail." That lumped two different things together. "On all my machines" and "also confirmed on 4.53.2" widen what the report claims about the bug, so they need proof. "Ran it 5 times" or "dropping `-r $1` gives 1, 4, 7, 10" describe the run that is already shown, and a stranger can check them from the words alone. The old wording failed pkg-03, pkg-09, and pkg-10 for the second kind. I split the rule so scope claims still need artifacts and specific re-runnable details don't. I rejected just dropping the breadth rule, because vague breadth ("every single day, on all my machines" in calib-02, "I confirmed" a global leak in pkg-19) is how no-evidence packages sound convincing.

**Trade-offs**

Loosening `outcome-honest` (and `steps-followable`) risked flipping rejects to accepts, so before the confirming run I checked which gold-reject packages had failed on those checks and re-ran them as canaries with `--only`: pkg-18 (private monorepo, "It reproduces every time in our repo" with no artifact), pkg-19 (claimed a global leak "which I confirmed" with no artifact), pkg-06 (silently dropped `--driver vmware`), pkg-08, and pkg-20 as the `disclosure` canary. All five still graded `reject` in the 10/10 `--only` run, and the full run confirmed it. The case I accept the check will miss: a report that invents a specific-sounding detail ("ran it 5 times") now passes without an artifact. A made-up repeat count gets through, but the main outcome still has to be backed by pasted output, so a package can't pass on invented details alone.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
