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
