# Plan: #73 README and `.env.example` disagree about which LLM API key to set

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73
Built from my repro report: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73#issuecomment-5861752097
Branch: `fix/73-env-example-openrouter-key` on my fork, from `main` at `2f4e82f`.

## Diagnosis

`.env.example` is missing the `OPENROUTER_API_KEY` line, and its
`LLM_PROVIDER` comment leaves out OpenRouter. The README and
`docs/SETUP.md` both tell you to set that key after copying the file,
so the copied `.env` has nothing to fill in.

The repro evidence I'm relying on, quoted from my report:

The docs ask for the key:

```
$ grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
```

The copied file doesn't have it:

```
$ grep -nE "Options|LLM_PROVIDER|OPENAI_API_KEY|OPENROUTER" .env
17:# Options: "mock" (default, no API key needed), "openai"
18:LLM_PROVIDER=mock
19:OPENAI_API_KEY=sk-your-key-here

$ grep -n "OPENROUTER_API_KEY" .env || echo "OPENROUTER_API_KEY not found in .env"
OPENROUTER_API_KEY not found in .env
```

The config already supports it:

```
$ grep -nE "llm_provider|openai_api_key|openrouter" core/config.py
18:    llm_provider: str = Field(default="mock")
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
21:    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
22:    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```

Settings load with an empty key from the copied file:

```
llm_provider='mock' openrouter_api_key='' openai_api_key='sk-your-key-here'
```

Control: after adding one line, `OPENROUTER_API_KEY=sk-or-test-123`, to
`.env`, settings loaded it:

```
llm_provider='mock' openrouter_api_key='sk-or-test-123'
```

The control shows the config reads the variable as soon as the line
exists. So the fault is in `.env.example`, and `core/config.py` needs
no change.

## Scope

In scope: one change to `.env.example` so it matches what the README,
`docs/SETUP.md`, and `core/config.py` already say.

Out of scope:

- `core/config.py` and any other Python code. The control shows the
  settings already work.
- Rewording `README.md` or `docs/SETUP.md`. They already name
  `OPENROUTER_API_KEY`, which is the key the config defines.
- The other lines of `.env.example`, including `OPENAI_API_KEY`. The
  embedding provider in `ingestion/embeddings/provider.py` still asks
  for it.
- The clone URLs in the README Quick Start, the database port, and
  anything else I noticed while reading. Those are separate issues.

## Files

- `.env.example`, the `# LLM provider` block (lines 16 to 19 at
  `2f4e82f`). This is the only file I plan to touch.

## Approach

1. Create `fix/73-env-example-openrouter-key` from `main` on my fork.
2. In `.env.example`, change the options comment on line 17 to list
   `"openrouter"` next to `"mock"` and `"openai"`.
3. Under `OPENAI_API_KEY`, add `OPENROUTER_API_KEY=sk-or-your-key-here`,
   in the same placeholder style as the line above it.
4. Leave `LLM_PROVIDER=mock` as the default so a fresh setup still
   needs no key.
5. Run the test plan below, then `make check` and `make test-unit`.
6. Commit as `docs(config): add OPENROUTER_API_KEY to .env.example`,
   following the Conventional Commits rule in `docs/CONTRIBUTING.md`.

## Test plan

Re-run my repro steps on the branch, from a fresh `.env`:

1. `rm .env && cp .env.example .env`
2. `grep -n "OPENROUTER_API_KEY" .env`
   - Before the fix: no match (`OPENROUTER_API_KEY not found in .env`).
   - After the fix: one match, the `OPENROUTER_API_KEY=` line.
3. `grep -n "Options" .env`
   - Before: `# Options: "mock" (default, no API key needed), "openai"`.
   - After: the same line with `"openrouter"` listed.
4. Load settings with the same command as the repro:
   `.venv/bin/python -c "from core.config import settings as s; print('llm_provider=%r openrouter_api_key=%r' % (s.llm_provider, s.openrouter_api_key))"`
   - Before: `openrouter_api_key=''`.
   - After: `openrouter_api_key='sk-or-your-key-here'`, and
     `llm_provider='mock'` unchanged.
5. `grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md .env.example`
   shows all three files naming the same variable.

There is no unit test for `.env.example` in `tests/`, and I found no
`xfail` marker for #73, so I'm not planning to add or remove a test.

## Risks and unknowns

- I searched the repo and found nothing outside `core/config.py` that
  reads `llm_provider`. I don't know which `LLM_PROVIDER` value the
  app expects for OpenRouter, or whether it reads the value at all.
  Listing `"openrouter"` in the comment is my best guess from the
  field names. I'll ask about this in the plan comment.
- A placeholder key gets loaded as if it were real. My repro showed
  `openai_api_key='sk-your-key-here'` coming from the existing
  placeholder, and the new line will behave the same way. I'm
  following the existing style. An empty value is the alternative if
  a maintainer prefers it.
- I didn't start the app or call an LLM provider in my repro, and
  Docker isn't installed on my machine. This plan is checked at the
  config level only.
- I haven't run `make check` or `make test-unit` on this machine yet.
  I expect a change to `.env.example` not to affect them, and I'll
  confirm before opening the PR.

## Deviations

The change held. I edited the `# LLM provider` block of `.env.example`
the way the plan says: one new line, `OPENROUTER_API_KEY=sk-or-your-key-here`,
and `"openrouter"` added to the options comment. No other file changed,
and the test plan gave the "after" results I predicted.

Three small things went differently around the change:

- Commit message. I planned `docs(config): ...`. `docs/CONTRIBUTING.md`
  lists the allowed scopes as `ingestion`, `rag`, `agent`, `safety`,
  `api`, `frontend`, and `config` isn't one of them. I committed as
  `docs: add OPENROUTER_API_KEY to .env.example` with no scope.
- Checks. I planned to run `make check`. That target runs `black .`,
  which rewrites files, so I ran `ruff check .`, `black --check .`,
  and `mypy` one at a time. `ruff` and `black --check` passed, and
  `make test-unit` gave 375 passed, 53 xfailed.
- Type check. `mypy` stopped on a syntax error in a numpy stub inside
  my `.venv` (Python 3.13) before it checked the project. The file is
  in site-packages and my change only touches `.env.example`. I'm
  counting on CI for the type check.

The open question from the plan is still open. Nobody has said which
`LLM_PROVIDER` value the app expects for OpenRouter.
