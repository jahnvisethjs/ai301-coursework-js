---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: **is this pull request ready
to submit?** A PR package is a candidate pull request (its title,
description, commit list, diff, and test evidence), read against the
accepted plan it claims to implement (with that plan's deviation notes
and the repro evidence it built on), the issue the plan belongs to,
and the repo's stated standards (PR template, contribution docs, AI
policy). Do not grade the plan itself, the issue, or the code's style
beyond what the rubric checks, and never grade more than one package
per run.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** (the student's own PR, before it is opened). Gather, from
the top folder of the student's fork clone:
- `plan.md`: the plan, including its `## Deviations` notes.
- The branch's diff: `git diff main...HEAD` (three dots), run from the working copy. Also the commits: `git log --oneline main..HEAD`.
- The draft PR title and description: the draft file the student names (e.g. `pr.md`).
- The test evidence: the captured before/after output the student names (e.g. `evidence.md`), or the draft's Testing section.
- The issue the plan belongs to: fetch its thread from the issue URL the student gives.
- The repo's standards: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md` (or `CONTRIBUTING.md`), and any AI policy file.

A house-chain student has no plan of their own: read the house plan and
the house repro pack wherever this says plan or repro. The same checks
grade the same things. If an input is missing, grade the checks that
need it as the procedure directs; do not invent the input.

**Eval mode** (a package bundle). The bundle is the whole world: every
fact comes from the bundle text. Fetch nothing, run nothing, and read
no other file. Always grade the complete package: every check, full
verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Refuse to grade a
PR whose repo is not the one on its `Repo:` line, and say why. If the
`Repo:` line still holds a bracketed placeholder, stop without grading
and tell the student to get their cohort's scope file from the
instructor; never guess a scope. Treat the house rules in `scope.md`
as stated standards: they feed the standards check alongside the
repo's template. Examples: a PR from your own fork's
`fix/<issue>-<slug>` branch, one PR per issue, and every template
section given real content. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and check the draft PR title and
description against each of its rules. In the summary, list every rule
the draft breaks: quote the rule, then quote the line that breaks it.
The voice guide never changes the verdict on its own, because no check
in `rubric.md` reads it; voice is the student's own standard, not the
repo's. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks (each with its evidence, pass condition, and weight) and the verdict rule. It is the only source of checks.
- `references/evidence-guide.md` maps where each evidence family lives in a bundle and in live mode, and what good looks like.
- `procedure.md` is the operating procedure. Execute it as written, in order: read order, evidence gathering, check execution, verdict assembly.

Where the procedure is silent on a step you need, do not improvise
around it: grade with what the procedure does say, and name the gap in
the summary. If `rubric.md` has no checks, or `procedure.md` has no
steps, refuse to grade and say which file is empty.

## Verdict and output

The verdict is binary: `accept` means the PR is ready to submit, and
`reject` means hold it. There is no third verdict; put reservations in
check evidence lines. Before the JSON you may give a short readable
summary: one line per check, the deciding check for a reject, and the
live-mode voice notes and procedure gaps. End the reply with this
fenced JSON block, valid, with one entry per rubric check in table
order. It must be last, with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, the item is the PR URL, or the branch name if the PR is
not opened yet.

## Grading discipline

- **Evidence first.** Never grade a check without naming the quote or fact that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** Read the diff against the plan, the evidence against the test plan, and the description against the diff. A terse complete PR can be ready; a confident, well-formatted one can hide drift. Never trust the description's word about the diff.
- **The rubric decides.** If a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary; the fix belongs in the rubric.
- **The procedure decides how.** Follow `procedure.md` as written and report its gaps instead of inventing steps.
- **Unclear fails.** Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent, an unverifiable claim counts as a fail: a PR you cannot verify from the package is not ready to submit.
- **Honest shortfalls are not failures.** A limit or deferral recorded in the plan's deviation notes and stated in the description is scope, not drift.
