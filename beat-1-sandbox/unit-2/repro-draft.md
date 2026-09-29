Reproduced. **Environment:** PathReview at commit `2f4e82f`, Windows 11,
Python 3.11.0, pytest 9.1.1, structlog 26.1.0, in a fresh virtual environment.
`tests/unit/` is marked `unit: Unit tests (fast, no external dependencies)` in
`pyproject.toml`, so I did not bring up the Docker/Postgres stack for this one.

**Steps:**

1. Clone the repo, create a venv, and install the two packages this test needs:

```
$ git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
$ cd pathreview-ai301-fa26-s3
$ python -m venv .venv-repro
$ .venv-repro/Scripts/pip install pytest structlog
```

2. Run the issue's exact command:

```
$ .venv-repro/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q
..x................                                                      [100%]
18 passed, 1 xfailed in 0.26s
```

This passes as shipped because `test_query_with_partial_overlap` carries
`@pytest.mark.xfail(strict=True, reason="issue #64: ...")`, per this repo's
seeded-bug convention (`docs/CONTRIBUTING.md`). To see the behavior underneath
the marker, I removed just that decorator locally (not committed) and reran the
single test:

```
$ .venv-repro/Scripts/python -m pytest "tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap" -v
```

**Expected (per the issue):** the fixture's query/chunk pair should have partial
keyword overlap, landing the score strictly between 0.3 and 0.9.

**Actual:**

```
        assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9
tests/unit/test_relevance_scorer.py:54: AssertionError
--------------------------- Captured stdout call ---------------------------
relevance_scored  avg_score=1.0  chunks_count=1  query_len=4
1 failed in 0.22s
```

**Why:** `RelevanceScorer.score` (`rag/evaluator/relevance_scorer.py`) tokenizes
with `text.lower().split()` and computes
`overlap = len(query_tokens & chunk_tokens); relevance = overlap / len(query_tokens)`.
The fixture chunk is `{"text": "Django is a Python web framework for rapid development"}`,
so `query_tokens = {python, django, web, framework}` (4 tokens) and all 4 appear
in the chunk, giving `4 / 4 = 1.0`. This matches the issue exactly: the scorer is
correct for full coverage, and the fixture is the bug. The chunk needs to drop at
least one of the four query terms to produce genuine partial overlap.

I confirmed this only on the environment above; the test has no version-sensitive
logic, so I did not pin down to other OS or Python versions.
