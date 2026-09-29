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

jahnvisethjs

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5882830147

Hi! I'd like to take issue #64 as a first contribution to this repo.

My read of the bug: `test_query_with_partial_overlap` runs the query
"Python Django web framework" against a chunk that contains all four of those
terms, then asserts the score is `< 0.9`. But the scorer returns `1.0` for full
keyword coverage, so the test is asserting against correct behavior — the
fixture is what is wrong, not the scorer.

My next step is to reproduce it locally with
`pytest tests/unit/test_relevance_scorer.py -q`, confirm the `assert 1.0 < 0.9`
failure, and check whether the scorer's `1.0` on full coverage is the intended
behavior. If it is, the fix belongs in the fixture (dropping at least one query
term so the overlap is genuinely partial) rather than in the scorer.

I will report back here with what I find before I change anything.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5882830147

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

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

- Run 1 — full run: `agreement: 17/20 scored items` (below the bar; category floor
  unmet: `disclosure 0/1`). Disagreements: `pkg-09` and `pkg-10` (my `behavior-match`
  check false-rejected honest cannot-reproduce reports) and `pkg-20` (my `conventions`
  check missed the required AI-disclosure and graded it `accept`).
- Run 2 — partial re-run `--only pkg-01,pkg-06,pkg-09,pkg-10,pkg-13,pkg-16,pkg-20`
  after revising `behavior-match` (only judge reports that claim a reproduction) and
  `conventions` (a missing disclosure fails when the repo requires one). All 7 packages
  agreed, including the canaries `pkg-01,pkg-06,pkg-13,pkg-16` that already agreed, so no
  prior agreement flipped. Partial runs print no bar line.
- Run 3 — full saved run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with all
  five categories matched: `clear-accept 8/8`, `disclosure 1/1`, `no-evidence 4/4`,
  `unfollowable-comms 3/3`, `wrong-target 4/4`. This is the run recorded in `eval-run.txt`.

**Package analysis**

Scored package: `pkg-20` (`ghostty-org/ghostty#13604`).

- My rubric's decision: `reject`. Gold label: `reject`. The saved run records
  `pkg-20  reject  reject   yes`.
- The package's environment, commands, control case, and captured terminal responses make
  the technical reproduction strong. However, the repo-facts block says Ghostty requires
  all AI usage to be disclosed, including the tool and extent of assistance. Neither the
  candidate claim comment nor the repro report contains that disclosure. My required
  `conventions` check therefore fails, and the verdict rule rejects the package because
  every required check must pass. The rubric reached the gold verdict for the policy
  violation rather than mistaking a technically convincing report for a post-ready one.

**Check rationale**

The `conventions` check, quoted exactly as it currently appears in my `rubric.md`, is:

> | conventions | The claim and repro comments, read against the repo's contribution conventions and AI-disclosure policy (the repo-facts block, live: the repo's docs) | The comments follow the repo's stated contribution norms. When the repo's policy requires disclosing AI assistance, the comments must contain an explicit AI-use disclosure; the absence of one fails this check (do not assume the work was AI-free) | required |

I made the evidence source explicit so the grader compares the comments with the
repository's actual policy instead of applying a generic communication preference. I also
made disclosure observable: when the policy requires it, the disclosure must appear in the
comment. I rejected the weaker rule that missing disclosure could pass merely because the
package contains no visible signs of AI use. The check is required because a package that
violates a repository's stated submission policy is not ready to post even when its
technical reproduction is otherwise complete.

**Trade-offs**

The required `conventions` check changes the result for `pkg-20`. Its environment, steps,
expected-versus-actual comparison, and artifacts could otherwise support acceptance, but
the missing AI-use disclosure makes the final verdict `reject`. The trade-off is that the
rubric deliberately gives repository process requirements the same gating force as the
technical proof: it will hold an otherwise reproducible report until the required
disclosure is added. I limited that cost by making the condition apply only when the
repository states an AI-disclosure requirement, rather than requiring disclosure in every
repository.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
