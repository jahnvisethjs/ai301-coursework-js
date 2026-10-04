# Evidence guide: where evidence lives in a plan package

Eval mode: a package is one markdown file with these sections, in this
order: header (source, captured date), `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Repro evidence`, `## Candidate plan`,
`## Candidate plan comment`. Use only that text.

Live mode: Repo facts = the repo's README, CONTRIBUTING.md, AI_POLICY.md
(or an "AI" section), and issue templates under `.github/`. Issue and
Thread = the issue page (`gh issue view <n> --comments`), with each
commenter's role badge. Repro evidence = the student's own posted repro
comment on that issue (house issue: the repro pack as quoted in the
drafts). Plan = `plan.md`. Plan comment = `comment.md`.

## Diagnosis and grounding

- **Where it lives:** the plan's cause is under a `Cause:` or
  `Diagnosis:` line or heading, or in the plan's first paragraph or
  summary; the comment often restates it ("I traced this to..."). The
  behavior that cause has to explain is in `## Repro evidence`: the
  numbered steps, any line starting `Control`, variant runs (same input
  with one flag, component, or setting changed), timings, artifacts
  (stack traces, `--debug` output), and the `Actual:` line. Thread
  highlights may hold competing cause claims.
- **What good looks like:** the stated cause explains every repro step,
  including the control: the symptom appears where the blamed code
  runs and disappears where the control removes it. The planned change
  edits that code, not a later step that cannot undo the damage.
- **What bad looks like:** a control or variant step shows the symptom
  with the blamed part out of the loop, or shows the blamed part
  working fine (e.g. bat: the 26 s delay shows up with no pager at all,
  so key bindings cannot be the cause). Or the plan repeats a confident
  thread claim ("as identified in this thread") that the repro never
  tested or contradicts.

## Scope

- **Where it lives:** the plan's `Change:` / `Changes` / `Approach`
  lists and every sub-bullet, any `In scope:` / `Not in scope:` /
  `Out:` lines, and words like "also", "while we're here", "and
  migrate", "add an option". Compare against the single bug in
  `## Issue`.
- **What good looks like:** every change fixes or tests the reported
  bug; related work is named and deferred ("Not in scope: ...",
  "follow-up"). A terse one-change plan with a not-in-scope line is
  bounded.
- **What bad looks like:** the fix is one item among refactors,
  rewrites, dependency upgrades, new settings or options, UI work, CI
  changes, or porting to sibling components. A correct core fix does
  not save a plan that bundles these.

## Executability

- **Where it lives:** the `Change` / `Approach` section: file paths
  (`src/...`, `pkg/...`), function or type names, "the X callback",
  "the command builder in Y". Hedges show up as "somewhere", "maybe",
  "not sure", "A? B?", "whichever is easier", "profile first",
  "investigate".
- **What good looks like:** at least one named file or code site plus
  one chosen approach; a stranger could open that file and start the
  edit without asking the author anything.
- **What bad looks like:** no file named, several layers listed with
  no choice made, or the real decision deferred to build time.

## Test plan

- **Where it lives:** the plan's `Test:` / `Test plan` section; check
  it against the repro's steps and its `Expected:` and `Actual:`
  lines.
- **What good looks like:** a concrete run (the repro steps re-run, a
  named test, a fixture, `zig build test` with the issue's cases) and
  an observable result after the fix that differs from the repro's
  Actual: "the color flips at step 3", "exit 0 and a match from
  `-10.txt.gz`", "both fuzz cases pass". Manual repro re-runs count.
- **What bad looks like:** "run the full test suite", "should feel
  fast", "nothing else should break", or a test of something other
  than this bug.

## Honesty

- **Where it lives:** `Risk` / `Risks` / `Unknowns` / `Open question`
  lines in the plan, hedges in the comment, and (live, after the
  build) the plan's `## Deviations` section.
- **What good looks like:** unknowns are named as unknowns, with how
  they will be settled ("not yet measured; if the benchmark shows it,
  I will move the check"), and anything not verified is not stated as
  fact. A deviation is written in `## Deviations` with what changed and
  why.
- **What bad looks like:** certainty about things the repro never
  tested; no risks named for a change to a shared or hot path.

## Comms

- **Where it lives:** the `## Candidate plan comment`, read against two
  places:
  - `## Thread highlights`: maintainer comments. A maintainer is
    anyone tagged OWNER, MEMBER, or COLLABORATOR, and also a
    CONTRIBUTOR who opened the issue or speaks for the project's
    design (proposes or rejects approaches). People tagged NONE are
    not maintainers. Direction = isolating a culprit, rejecting an
    approach, asking for testing, posting a patch, or pointing at an
    existing PR.
  - `## Repo facts`: the `contribution policy` line, especially any
    AI-use policy (must disclose tool and extent; comments must be in
    the human's own words).
- **What good looks like:** the comment answers each maintainer
  direction (follows it or says why not), and does whatever the stated
  AI policy requires of comments (e.g. "AI-assisted with Claude; I
  reviewed every change"). Every package is treated as AI-assisted.
- **What bad looks like:** the comment proposes a different fix
  (e.g. docs only) while ignoring the owner's isolated culprit and
  test request, or a repo that requires AI disclosure gets a comment
  with none, however good the plan is.
