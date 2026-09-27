# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

- **Where it lives.** Eval bundle: the environment lines of the repro
  report, read against the "affects / target version" line in the issue
  context and the repo-facts block. Live: the setup or "Environment"
  lines of the draft repro comment, read against the version the issue
  thread names.
- **What good looks like.** OS, the relevant runtime/tool version, and
  the code state (a commit SHA or release tag) are all named, and each
  matches the version the issue targets — or a mismatch is stated in
  words rather than left for the reader to notice.

## Steps

- **Where it lives.** Eval bundle: the numbered (or clearly ordered)
  reproduction steps inside the repro report. Live: the steps section of
  the draft repro comment.
- **What good looks like.** The steps begin from a named starting state
  (fresh clone, install, seed data — whatever the trigger needs) and run
  through to the action that triggers the bug, with no step a stranger
  would have to invent or guess. A reader could paste them into their own
  shell and arrive where the report did.

## Behavior shown

- **Where it lives.** Eval bundle: the artifact block of the repro report
  — the output excerpt, log, stack trace, or screenshot — read against
  the behavior described in the issue context. Live: the pasted output or
  attached image in the draft repro comment, read against the issue
  thread's description.
- **What good looks like.** When the report claims a reproduction, the
  artifact shows the specific behavior the issue describes (the same error
  text, the same wrong result, the same failure point), not a different or
  adjacent failure that merely looks similar; the match is checkable
  against the issue's own wording. When the report instead concludes it
  could not reproduce, this family does not apply — the artifact is not
  expected to show the bug, and honesty is what decides the report.

## Honesty

- **Where it lives.** Eval bundle: the report's stated conclusion or
  summary line, read against its own artifacts above it. Live: the
  concluding sentence of the draft repro comment against the evidence it
  presents.
- **What good looks like.** The conclusion claims only what the artifacts
  actually show. An "I could not reproduce" backed by the steps tried and
  the output seen is honest and complete. A confident "reproduced /
  confirmed" whose artifact does not show the issue's behavior claims more
  than its evidence — that is the failure to catch.

## Comms

- **Where it lives.** Eval bundle: the claim comment and repro comment,
  read against the repo-facts block (contribution norms, issue-template
  expectations, and any AI-use disclosure policy). Live: the draft
  comments read against the repo's CONTRIBUTING, issue template, and any
  stated AI-disclosure rule on GitHub.
- **What good looks like.** The comment is specific to this issue rather
  than boilerplate and follows whatever template or norm the repo states.
  When the repo's policy requires disclosing AI assistance, an explicit
  AI-use disclosure must be present in the comment; a missing disclosure
  fails here regardless of how good the rest reads. Do not treat the
  absence of visible AI markers as evidence the work was AI-free — the
  requirement is that the disclosure appear, not that you prove AI was
  used.
