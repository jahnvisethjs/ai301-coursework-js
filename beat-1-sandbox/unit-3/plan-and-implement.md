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

jahnvisethjs

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5975976012

```
Plan for #64, built on my repro
(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5882830147;
with the `xfail` removed locally,
`assert 1.0 < 0.9` and `relevance_scored avg_score=1.0 ... query_len=4`).

The scorer is right and the fixture is wrong: all 4 query tokens
(`python`, `django`, `web`, `framework`) appear in the chunk, so `4/4 = 1.0`
is the correct score for full coverage.

Change, one file only (`tests/unit/test_relevance_scorer.py`,
`test_query_with_partial_overlap`):

- Chunk text becomes `"Django is a Python library for rapid development"`.
  It shares 2 of the 4 query tokens, so the expected score is `2/4 = 0.5`,
  in the middle of the asserted `(0.3, 0.9)`.
- Remove the `@pytest.mark.xfail(strict=True, reason="issue #64: ...")` marker,
  per CONTRIBUTING's seeded-bug rule (otherwise CI fails with `XPASS(strict)`).

Not changing the scorer, its tokenizer, the test's assertions, or any other test.

Test: re-run the repro steps from that comment. The single test should pass with `avg_score=0.5`, and
`pytest tests/unit/test_relevance_scorer.py -q` should give `19 passed` with no
xfail (it was `18 passed, 1 xfailed`). Then `make lint` and `make test-unit`.
Not yet verified: typecheck and the integration/frontend CI jobs, which I
expect to be unaffected by a one-test change and will confirm on the PR.
```

---

## Your branch

**Branch**

fix/64-partial-overlap-fixture

**Evidence**

Unit 2 repro steps re-run against the change. Environment: Windows 11, Python 3.11.0,
pytest 9.1.1, structlog 26.1.0, venv `.venv-repro`. "Before" is `main` at `2f4e82f`
in a separate worktree. "After" is `fix/64-partial-overlap-fixture` at `8a74506`.

Before (`main`, `2f4e82f`). The issue's command, repo as shipped (the strict xfail hides the failure):

```
$ .venv-repro/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q
..x................                                                      [100%]
18 passed, 1 xfailed in 0.21s
```

Before: the same test with only the `@pytest.mark.xfail` decorator removed locally, as in my unit 2 repro:

```
$ .venv-repro/Scripts/python -m pytest "tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap" -v
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9
tests\unit\test_relevance_scorer.py:54: AssertionError
[info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
============================== 1 failed in 0.17s ==============================
```

After (`fix/64-partial-overlap-fixture`, `8a74506`). The same single test:

```
$ .venv-repro/Scripts/python -m pytest "tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap" -v -s
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap [info     ] relevance_scored               avg_score=0.5 chunks_count=1 query_len=4
PASSED
============================== 1 passed in 0.09s ==============================
```

After: the issue's command:

```
$ .venv-repro/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q
...................                                                      [100%]
19 passed in 0.10s
```

After: lint on the changed file:

```
$ .venv-repro/Scripts/python -m ruff check tests/unit/test_relevance_scorer.py
All checks passed!
$ .venv-repro/Scripts/python -m black --check tests/unit/test_relevance_scorer.py
All done! ✨ 🍰 ✨
1 file would be left unchanged.
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (full, 20 scored packages): `agreement: 19/20 scored items  (bar: 18/20: PASS)`,
   with `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   This is the run saved in `eval-run.txt`. The one miss was pkg-14. I did not run again:
   loosening a check to flip one package the gold notes call "arguable" risked flipping
   unbuildable packages that already agreed.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, category clear-accept). My rubric decided **reject** and
the gold label is **accept**. The table note reads `failed: grounded-cause, executable`.

Why my rubric read it that way:
- **executable.** My pass condition requires the plan to name "at least one file or code
  site to change AND commit to one approach", and fails it if the location is left open.
  The plan names a component, not a code site: "Files: the client attach/reattach path in
  `zellij-server` (session connection handling) and `zellij-client`'s terminal query
  issuance; exact functions to be pinned in the PR after tracing the query issuance with
  debug logs". My procedure lists "investigate" and deferral wording as hedges, so
  "to be pinned in the PR after tracing" read as the location being decided at build
  time, the same pattern that rightly fails pkg-17 and pkg-18.
- **grounded-cause.** This check fails when a cause doesn't explain every repro step. The
  plan's explanation of the cache control is a stretch: "with an empty cache the color
  data is refetched along the fresh-attach path once". The repro shows only that the
  attach after a cache clear is clean and the next one leaks; no step shows the
  stdin-before-responses ordering the diagnosis claims. Read strictly, the grader found
  one step the cause doesn't cleanly explain.

Why the gold accepts it: the gold note says "reattach handshake fix with a
regression-window repro; defers the untestable Windows variant and says so; arguable on
the deferral, ready as scoped". A named component plus a working `zellij --debug` trace is
enough to start, and the 0.44.1-vs-0.44.2 control does ground the cause. My rubric is
stricter than the gold here by design (see Trade-offs).

**Check rationale**

> | decisive-test | The plan's Test / Test plan read against the Repro evidence's steps and its Expected line. | Pass if it names a concrete run (the repro steps re-run, or a named test or fixture) AND the observable outcome expected after the fix (an output, exit code, value, color, or timing) that differs from the repro's Actual result. A manual repro re-run counts; unit tests and coverage numbers are not required. Fail if the test is only "run the full suite", "should feel fast", "nothing else broken", or names no observable outcome for this fix. | required |

Why it reads this way: my group's worksheet rubric had "Tests- considering edge cases and
unit tests", and our procedure graded it "If test coverage is > 80%, P else F". Graded
against calib-01 (gold accept), that holds a ready plan: its whole test plan is a manual
repro re-run ("at step 3 the color must flip without leaving the view"), with no unit test
and no coverage number. So I dropped coverage and unit tests as requirements and wrote
"A manual repro re-run counts; unit tests and coverage numbers are not required." What
matters is whether the test can tell fixed from broken, so the pass condition asks for an
observable outcome that "differs from the repro's Actual result". I also folded my group's
separate "Should add expected behaviour once the bug is fixed" check into it, because the
expected behaviour is that observable outcome. The named failures ("run the full suite",
"should feel fast", "nothing else broken") are quoted from the vague test plans in the
unbuildable packages, so the grader matches wording instead of judging "vagueness".

**Trade-offs**

decisive-test gives up plans that are solid everywhere else but whose only test is the
generic suite. calib-04 is the case I accept it will miss either way: the gold note calls
it "genuinely solid bounded plan engaging the owner's WAI note, but the test plan is 'run
the full test suite'". My check holds it, matching the gold, but a maintainer who would
take "CI is green" as enough would disagree. It also asks for an outcome that differs from
the repro's Actual, so a plan whose fix can't be observed directly (a race or flaky timing
bug) has to name a proxy outcome or be held.

What it did not cost in the scored set: it caused none of my run's disagreements. The only
miss, pkg-14, was failed on `grounded-cause, executable`, not decisive-test, and all six
other clear-accept packages agreed. So no ready plan in the eval set was held for lacking
unit tests or coverage, which is the failure the old coverage rule would have caused.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
