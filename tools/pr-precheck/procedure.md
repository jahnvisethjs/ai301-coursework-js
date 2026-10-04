# Procedure: how this tool grades a PR package

## Read order

Read the parts in this order and write notes as you go. Read the plan
before the PR: the plan is the yardstick, so you must list what it
promises before the description tells you what was delivered. Read
the diff before the description, so the description's claims are
checked against what you already saw, not the other way round.

1. **Repo facts** (live: the repo's PR template
   `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md` or
   `CONTRIBUTING.md`, and any AI policy file, plus the house rules in
   `scope.md`). Note:
   - every section or item the PR template or contribution docs state
     as required, as a checklist;
   - whether the AI policy requires disclosure, quoted.
2. **Issue and Thread highlights.** Note the one bug, and any
   maintainer direction quoted.
3. **Plan context** (live: `plan.md`, including `## Deviations`). Note:
   - the change steps, numbered;
   - the files the plan names, and everything it puts out of scope;
   - every deviation note, quoted;
   - every repro case or failure mode the test plan names, and the
     expected outcome for each.
4. **Diff** (live: `git diff main...HEAD` from the working copy) and
   the **Commits** list (live: `git log --oneline main..HEAD`). For
   each hunk, write one line: file, what it does.
5. **Test evidence** (live: the captured before/after output the
   student supplies, e.g. `evidence.md` or the draft's Testing
   section).
6. **Title and Description**, last.

## Evidence gathering

Quote the package text for each fact. Record "absent" when the
package has no such text. See `references/evidence-guide.md` for where
each lives.

1. **diff-matches-plan:** make a two-column table. Left: every hunk
   from step 4 of the read order. Right: the plan step it does, "test
   for step N", "required template entry", "deviation note: <quote>",
   or "NONE". Then list every plan step with the hunk that does it,
   or "missing".
2. **description-matches-diff:** list every claim the title and
   description make about what changed (each "adds", "fixes",
   "updates docs", "removes", and every fidelity phrase such as
   "exactly as planned" or "no other changes"). Put the hunk that
   proves each claim next to it, or "not in diff".
3. **decisive-evidence:** for every repro case from the plan's test
   plan, write: the command or steps run in the evidence, the before
   output, the after output, and whether the run goes through the
   code the diff changes. Mark a case "not run" if the evidence has
   no run of that case, and "control only" if it runs only the
   no-bug control.
4. **clean-diff:** scan every added (`+`) line for debug output,
   commented-out code, dead or unused functions, new TODO/FIXME,
   whitespace-only or re-indent changes, and import reordering. Quote
   each one found. Note commit messages such as "wip", "fmt",
   "cleanup while debugging" as pointers to where debris may be.
5. **standards-met:** put each required item from read-order step 1
   next to the place it is met (description section, checklist line,
   diff file), or "missing". Put the AI-policy requirement next to the
   disclosure sentence, or "none". Eval mode: treat every package as
   AI-assisted. Live mode on Path Review: the template sections and
   the house rules in `scope.md` are the required items.
6. **repo-checks-shown:** quote any test-runner, lint, or CI output,
   or any sentence saying which checks were not run.

## Check execution

1. Run the checks in table order: diff-matches-plan,
   description-matches-diff, decisive-evidence, clean-diff,
   standards-met, repo-checks-shown. Grade every check even after one
   fails.
2. Grade each check only from its gathered evidence, applying the pass
   condition in `rubric.md` word for word. Go back to the package only
   for a quote the condition needs and the gathering missed, and only
   to that part.
3. Grade `pass` or `fail` whenever the package text settles the
   condition. Use `unclear` only when the condition depends on a fact
   the package cannot show (for example, the evidence block is cut off
   mid-output). Brevity is never unclear: a terse PR that meets the
   condition passes.
4. Absent evidence is a fail for the PR's own parts: no test evidence
   fails decisive-evidence; a required template section with no
   content fails standards-met.
5. Rules so two graders agree:
   - diff-matches-plan: one "NONE" hunk fails it, however good the
     rest is. A regression test for the planned fix is never drift.
     Deleting a marker or suppression that the repo's rules say must
     go with the fix (e.g. an `xfail` on a seeded bug) is part of the
     fix.
   - description-matches-diff: judge claims against the diff, never
     against the plan. "Docs updated" with no docs hunk fails. A
     stated shortfall passes.
   - decisive-evidence: every repro case the plan names must be shown
     run, or deferred by a recorded deviation note. Running only the
     control, or only a different path (e.g. a GET when the bug is in
     POST), fails. Adding a test is not the same as showing it run.
   - clean-diff: one debris line fails it. Lines the fix must change
     are never churn.
   - standards-met: grade only what the repo states. Do not invent
     template asks. An honestly unchecked checklist item with a reason
     counts as addressed.
6. For each check, write one evidence line: the quote or fact that
   decided the grade.

## Verdict assembly

1. Take the required checks: every check except repo-checks-shown.
2. Turn every `unclear` among them into `fail`.
3. If all required checks pass, the verdict is `accept`; otherwise
   `reject`.
4. repo-checks-shown never changes the verdict; report its grade.
5. In the summary above the JSON, give one line per check (grade and
   evidence). For a reject, name the deciding check: the first failing
   required check in table order. Quote the evidence that failed it.
6. Live mode only: add the voice-guide notes (each rule the title or
   description breaks, quoting the rule) and any gaps in this
   procedure. Neither changes the verdict.
7. End with the JSON block from `SKILL.md`, one entry per check in
   table order, and nothing after it.
