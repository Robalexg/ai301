# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Robalexg

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5986062311

Plan for #73, built from my repro report above (`main` at `2f4e82f`).

**Diagnosis.** `.env.example` is missing the `OPENROUTER_API_KEY` line that `README.md:24` and `docs/SETUP.md:47` tell you to fill in. From my repro, after `cp .env.example .env`:

```
$ grep -n "OPENROUTER_API_KEY" .env || echo "OPENROUTER_API_KEY not found in .env"
OPENROUTER_API_KEY not found in .env
```

`core/config.py` already defines `openrouter_api_key`. In my control run I added the line to `.env` by hand and settings loaded `openrouter_api_key='sk-or-test-123'`, so the config side works.

**Scope.** One change to the `# LLM provider` block of `.env.example`:

- add `OPENROUTER_API_KEY=sk-or-your-key-here` under `OPENAI_API_KEY`
- list `"openrouter"` in the `LLM_PROVIDER` options comment
- keep `LLM_PROVIDER=mock` as the default

I'm leaving `core/config.py`, the README and SETUP wording, and the rest of `.env.example` alone.

**Test.** Re-run my repro on the branch: `cp .env.example .env`, then the same grep finds the `OPENROUTER_API_KEY` line, and the settings load prints the placeholder value where it printed `''` before.

**One thing I'm unsure about.** I couldn't find any code outside `core/config.py` that reads `llm_provider`, so `"openrouter"` as the value is my guess from the field names. If the app expects a different value, I'll use that.

Branch will be `fix/73-env-example-openrouter-key` on my fork, with a PR to follow.

---

## Your branch

**Branch**

fix/73-env-example-openrouter-key

**Evidence**

My unit 2 repro steps for #73, run on the branch. The before has the change stashed, so `.env.example` matches `main`. The after is the committed change.

````text
Test plan run for #73, 2026-10-05T00:31Z
Repo: fork of codepath/pathreview-ai301-fa26-s3, branch fix/73-env-example-openrouter-key, based on main at 2f4e82f

===== BEFORE: .env.example as on main (change stashed) =====

$ git rev-parse --abbrev-ref HEAD
fix/73-env-example-openrouter-key

$ git status --short .env.example

$ rm .env && cp .env.example .env

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

$ .venv/bin/python -c "from core.config import settings as s; print('llm_provider=%r openrouter_api_key=%r openai_api_key=%r' % (s.llm_provider, s.openrouter_api_key, s.openai_api_key))"
llm_provider='mock' openrouter_api_key='' openai_api_key='sk-your-key-here'

===== AFTER: .env.example with the change =====

$ git rev-parse --abbrev-ref HEAD
fix/73-env-example-openrouter-key

$ git status --short .env.example
 M .env.example

$ rm .env && cp .env.example .env

$ grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)

$ grep -nE "Options|LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER" .env
17:# Options: "mock" (default, no API key needed), "openai", "openrouter"
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here
20:OPENROUTER_API_KEY=sk-or-your-key-here

$ grep -n "OPENROUTER_API_KEY" .env || echo "OPENROUTER_API_KEY not found in .env"
20:OPENROUTER_API_KEY=sk-or-your-key-here

$ grep -nE "llm_provider|openai_api_key|openrouter" core/config.py
18:    llm_provider: str = Field(default="mock")
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")

$ .venv/bin/python -c "from core.config import settings as s; print('llm_provider=%r openrouter_api_key=%r openai_api_key=%r' % (s.llm_provider, s.openrouter_api_key, s.openai_api_key))"
llm_provider='mock' openrouter_api_key='sk-or-your-key-here' openai_api_key='sk-your-key-here'

$ git diff .env.example
diff --git a/.env.example b/.env.example
index 1be8b38..6077a90 100644
--- a/.env.example
+++ b/.env.example
@@ -14,9 +14,10 @@ REDIS_URL=redis://localhost:6379/0
 VECTOR_DB_URL=http://localhost:8001
 
 # LLM provider
-# Options: "mock" (default, no API key needed), "openai"
+# Options: "mock" (default, no API key needed), "openai", "openrouter"
 LLM_PROVIDER=mock
 OPENAI_API_KEY=sk-your-key-here
+OPENROUTER_API_KEY=sk-or-your-key-here
 
 # App settings
 APP_ENV=development
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 15/20. `agreement: 15/20 scored items  (bar: 18/20: below the bar)`
2. Partial run on 8 packages (`--only`, the five disagreements plus three canaries): 6/8.
3. Partial run on 2 packages (pkg-03 and pkg-14): 0/2.
4. Partial run on 9 packages (all seven `clear-accept` plus both `thread-convention`): 9/9.
5. Full run: 20/20. `agreement: 20/20 scored items  (bar: 18/20: PASS)`

The last one is the run in `eval-run.txt`.

**Package analysis**

`pkg-20`. In my first full run my rubric decided `accept` and the gold label said `reject`.

Every check that could change the verdict passed. The only check that failed was `thread-aware`, and at that point I had it weighted `preferred`. The grader's line for it was: "Comment follows mitchellh's recompute-on-capacity-change direction, but AI_POLICY.md requires AI-use disclosure and the comment contains none (preferred; no verdict effect)."

So the rubric saw the problem and was told to ignore it. The package's repo facts say: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". The plan comment has no disclosure. I changed `thread-aware` to `required` and wrote the disclosure rule into its pass condition. After that `pkg-20` graded `reject`, which matches the gold label.

**Check rationale**

> | honest-unknowns | The confident claims in the plan and the plan comment, read against the repro evidence and the thread highlights | No claim is presented as settled that a repro step contradicts or that a maintainer disputed in the thread. A code-level cause that fits every repro step passes without hedging, and so does the author's own report of extra local checks the excerpt does not show | required |

It started as: "Anything the evidence does not establish is stated as unknown, not asserted as fact". That failed four plans the gold labels accept (`pkg-02`, `pkg-05`, `pkg-09`, `pkg-14`), because a repro shows behavior and almost never shows the code-level cause. Every plan that named a cause in the code without hedging got failed.

My first rewrite also failed a plan that says a test or trace was run that the package doesn't show. That flipped `pkg-03` to reject. The grader's reason was: "Plan says 'I have verified gzip, xz, and zstd locally' but the repro evidence shows only a gzip run". It also kept `pkg-14` rejected. The gold label accepts both, and the repro block in a package is an excerpt, so I took that clause out. The check now fails only a claim that a repro step contradicts or a maintainer disputed.

**Trade-offs**

Narrowing `honest-unknowns` means I accept it will miss a plan that sounds sure about a cause the repro never tests. If no repro step contradicts the claim and no maintainer disputes it, the check passes it.

I checked what that cost on the eval set. In my first full run, every `reject` package that failed `honest-unknowns` also failed at least one other required check (`pkg-01`, `pkg-06`, `pkg-07`, `pkg-11`, `pkg-12`, `pkg-15`, `pkg-16`, `pkg-18`, `pkg-19`), so loosening it could not turn any of those into an accept. To be sure the loosening and the `thread-aware` change did not flip anything else, I re-ran with `--only pkg-02,pkg-03,pkg-05,pkg-08,pkg-09,pkg-13,pkg-14,pkg-20,pkg-04`. `pkg-03`, `pkg-08`, `pkg-13`, and `pkg-04` were the canaries that already agreed. That run was 9/9, and the full run after it was 20/20.

One more miss showed up when I ran the skill on my own plan. The check only counts a dispute from a maintainer. On #73 several classmates disagreed about listing `"openrouter"` as a provider option, and the check had nowhere to put that.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
