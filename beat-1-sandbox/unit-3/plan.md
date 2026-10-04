# Plan: issue #64, relevance scorer "partial overlap" fixture has full overlap

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64
Repro: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5882830147
(PathReview `2f4e82f`, Windows 11, Python 3.11.0, pytest 9.1.1, structlog 26.1.0)

## Diagnosis

The scorer is correct. The test fixture is wrong.

My repro, with the `xfail` marker removed locally:

```
$ .venv-repro/Scripts/python -m pytest "tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap" -v
        assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9
tests/unit/test_relevance_scorer.py:54: AssertionError
relevance_scored  avg_score=1.0  chunks_count=1  query_len=4
```

`RelevanceScorer.score` (`rag/evaluator/relevance_scorer.py`) tokenizes with
`text.lower().split()` and returns `len(query_tokens & chunk_tokens) / len(query_tokens)`.
The query "Python Django web framework" gives the 4 tokens
`{python, django, web, framework}`. The fixture chunk
"Django is a Python web framework for rapid development" contains all 4, so
the score is `4 / 4 = 1.0`. That is the right answer for full coverage, so the
test's `0.3 < score < 0.9` fails against correct behavior. The log line
`avg_score=1.0 ... query_len=4` confirms all four tokens matched. The fix
belongs in the fixture, not the scorer.

As shipped, the issue's command shows `18 passed, 1 xfailed`, because the test
carries `@pytest.mark.xfail(strict=True, reason="issue #64: ...")` under the
repo's seeded-bug convention (`docs/CONTRIBUTING.md`).

## Scope

In scope, one file, `tests/unit/test_relevance_scorer.py`, two edits to
`test_query_with_partial_overlap`:

1. Change the fixture chunk text so it shares exactly 2 of the 4 query tokens:
   `"Django is a Python library for rapid development"`. Tokens shared: `python`,
   `django`. Not shared: `web`, `framework`. Expected score: `2 / 4 = 0.5`.
2. Delete the `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`
   decorator. CONTRIBUTING requires it: with the fixture fixed the test passes,
   and a strict xfail turns that pass into an `XPASS(strict)` CI failure.

Not in scope:

- `rag/evaluator/relevance_scorer.py`: the scorer is correct, so it stays as is.
  That includes its whitespace-only tokenizer (no punctuation stripping or
  stemming); changing that is a different issue.
- The test's assertions (`0.3 < score < 0.9`) and every other test in the file.
- Any lint/type baseline in `pyproject.toml`. Nothing there names #64; I
  checked with `grep -rn "#64"`.

## Files

- `tests/unit/test_relevance_scorer.py`: `TestRelevanceScorer.test_query_with_partial_overlap`
  (chunk text, and the `xfail` decorator directly above it). No other files.

## Approach

I chose 2 of 4 shared terms (0.5) rather than 3 of 4 (0.75) so the score sits
in the middle of the asserted `(0.3, 0.9)` range rather than near one end. The
new chunk keeps every remaining word as a plain whitespace-separated
lowercase-able token with no punctuation, so the score depends only on the
overlap I intend. "library" replaces "web framework" so the sentence still
reads naturally. Commit as `test(rag): make partial-overlap fixture genuinely
partial` with `Fixes #64`, on branch `fix/64-partial-overlap-fixture`.

## Test plan

Re-run my unit 2 repro steps against the change, same venv (`.venv-repro`):

1. The single test, which failed in the repro with `assert 1.0 < 0.9`:
   ```
   .venv-repro/Scripts/python -m pytest "tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap" -v -s
   ```
   Expected after the fix: `PASSED`, with the log line
   `relevance_scored avg_score=0.5 chunks_count=1 query_len=4`
   (it was `avg_score=1.0` before).
2. The issue's exact command:
   ```
   .venv-repro/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q
   ```
   Expected: `19 passed`, with no `x` and no `xfailed` (it was
   `18 passed, 1 xfailed` before). The other 18 tests are unaffected.
3. Repo checks before pushing: `make lint`, `make test-unit`. Expected: no
   new findings, and the unit suite passes with one fewer xfail than on `main`.

## Risks and unknowns

- If the scorer later gains stemming or punctuation stripping, the score could
  shift. I picked "library", which shares no stem with any query term, so a
  stemmer would still give 2/4. A scorer change is still a separate issue.
- I have not run `make typecheck` or the frontend/integration CI jobs locally.
  This change touches one test file only, so I expect no effect, and green CI
  on the PR will confirm it.
- A classmate (april-hpxd) has also reproduced this issue. Per the house rules
  that doesn't block this plan; mine is built from my own repro.

## Deviations

The code change matches the plan exactly: one commit on
`fix/64-partial-overlap-fixture` (`test(rag): make partial-overlap fixture
genuinely partial`, `Fixes #64`). It touches only
`tests/unit/test_relevance_scorer.py`: the chunk text is now
`"Django is a Python library for rapid development"` and the strict `xfail`
decorator is deleted (1 insertion, 5 deletions). The scorer and every other
test are untouched.

Test plan steps 1 and 2 ran as planned and gave the expected results:
- The single test now shows `PASSED` with `relevance_scored avg_score=0.5 chunks_count=1 query_len=4`.
- `pytest tests/unit/test_relevance_scorer.py -q` gives `19 passed`, where it gave `18 passed, 1 xfailed` before.
- Between the two edits, the fixed fixture with the marker still in place gave `XPASS(strict)`, confirming the marker had to go.

One deviation, in step 3: I did not run `make lint` and `make test-unit`. The
full dev environment (`make setup`: the `[dev]` dependencies plus the Docker
Postgres for migrations and seeding) was not set up on this machine. Instead
I ran `ruff check` (`All checks passed!`) and `black --check` (`1 file would be
left unchanged`) on the changed file, plus the repro tests above. The full
unit suite, typecheck, and the integration/frontend jobs will run in the PR's
CI, which CONTRIBUTING requires to be green. The change touches one test
function in one file, so I expect no effect elsewhere. The posted plan
comment is still accurate: it already listed CI as not yet verified.
