# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/87

My pr-precheck verdict on the draft before opening: **accept**, with all six checks passing
(diff-matches-plan, description-matches-diff, decisive-evidence, clean-diff, standards-met,
repo-checks-shown). Its voice note flagged one line that claimed more than the evidence
showed ("exactly this test moving from xfail to pass"). I checked with `pytest -rx` (52
xfails, none of them #64) and reworded the line to say what was actually shown, including
where the 1-vs-4 warning difference comes from.

**Branch**

fix/64-partial-overlap-fixture

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (full, 20 scored packages): `agreement: 19/20 scored items  (bar: 18/20: PASS)`,
   with `categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.
   This is the run saved in `eval-run.txt`. The one miss was pkg-05. I stopped after one run:
   the only change that would flip pkg-05 is loosening decisive-evidence, which is the check
   that holds the not-tested packages and calib-04.

**Package analysis**

pkg-05 (nushell/nushell#18848, category clear-accept). My tool decided **reject**; the gold
label is **accept**. The table note reads `failed: decisive-evidence`.

Why my tool read it that way: the plan's test plan names two cases, "re-run the issue's
script, expect both `atuin` rows; re-run with two same-name same-key bindings, expect one row
plus a warning; cargo test on the config crate."
- The PR's Test evidence shows the first case decisively: a before table with one `atuin`
  row (`none | up`) and an after table with two (`control | char_r` and `none | up`).
- The second case appears only as a sentence: "Same-key redefine prints the one-time
  warning." There is no command, no before, and no output.
- My pass condition requires "for every repro case or failure mode the plan's test plan
  names, the evidence shows that same case run on the changed code path, with the before
  result ... and the after result". My procedure says "every repro case the plan names must
  be shown run". So the warning case counted as not shown, and the check failed.

Why the gold accepts it: the gold note reads "repro table before/after shown; template
sections filled". The main bug (the dropped `atuin` binding) is decisively shown, and the
warning is a secondary behaviour stated by the author alongside `cargo test -p nu-protocol
passes (312 tests)`. My tool is stricter than the gold on secondary cases by design (see
Trade-offs).

**Check rationale**

> | decisive-evidence | The Candidate PR's Test evidence read against the Plan context's Test plan and the repro evidence it cites. | Pass if, for every repro case or failure mode the plan's test plan names, the evidence shows that same case run on the changed code path, with the before result (the failure) and the after result (an observable output, exit code, value, or behaviour that matches the plan's expected outcome). Fail if the evidence is only "tests pass", "works on my machine", or "verified locally" with no output; if it runs only a control or a path the fix does not touch; or if it skips any repro case the plan's test plan names, unless a recorded deviation note defers that case. | required |

Why it reads this way:
- **Where it started.** It grew out of my Unit 3 `decisive-test` check, which asked a
  *plan* to name "a concrete run ... AND the observable outcome expected after the fix".
  At the PR stage the question is no longer whether a test is planned but whether the
  evidence shows it ran, so the check now reads the Test evidence against the plan's Test
  plan.
- **"For every repro case ... the plan's test plan names".** I wrote this because of
  calib-04: a solid fix whose plan names two failure modes while "the evidence re-runs repro
  1 only". A rule that one decisive before/after is enough would have accepted it.
- **"runs only a control or a path the fix does not touch".** This came from pkg-07, which
  runs only "the single-file control (the path that never breaks)", and pkg-14, whose
  evidence "exercises a GET with header casing (the unchanged path)".
- **What I rejected.** I rejected making the repo's own suite part of this check. calib-01
  is gold accept with no suite run at all, just "parsed OK" before and after. So a suite run
  lives in a separate, preferred check (`repo-checks-shown`) that never changes the verdict.
- **The deviation escape.** I added "unless a recorded deviation note defers that case" so
  an honest, documented deferral (pkg-13, pkg-16) is not held for missing evidence.

**Trade-offs**

decisive-evidence gives up PRs that prove the main fix decisively but only describe a
secondary case in prose. pkg-05 is the package whose result it changes: gold accept, my
tool reject, and the only disagreement in the run. I accept that miss. The same "every
named case must be shown" wording is what holds calib-04 (gold reject, "re-runs repro 1
only, silent on the --exec abort the plan named"). A looser wording, such as "the plan's
primary repro is shown", would flip pkg-05 to agree but would also let calib-04 through,
and could flip not-tested packages that show some evidence (pkg-07 runs a real control;
pkg-14 runs a real GET).

How I know it cost nothing else: in the run, all four not-tested packages agreed
(`not-tested 4/4`), and pkg-05 was the only package failed on decisive-evidence that the
gold accepts. The other six clear-accepts all passed it, including the terse pkg-19 and the
honest-deferral packages pkg-13 and pkg-16.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
