# Evidence guide: where evidence lives in a PR package

Eval mode: a package is one markdown file with these sections, in this
order: header, `## Repo facts`, `## Issue`, `## Thread highlights`,
`## Plan context` (the repro evidence, then the accepted plan), and
`## Candidate PR` with `### Title`, `### Description`, `### Commits`,
`### Diff`, `### Test evidence`. Use only that text.

Live mode, from the top folder of the fork's clone:
- **Plan:** `plan.md`, including `## Deviations`.
- **Diff:** `git diff main...HEAD`.
- **Commits:** `git log --oneline main..HEAD`.
- **Draft title and description:** the draft file the student names (e.g. `pr.md`).
- **Test evidence:** the captured before/after output (e.g. `evidence.md`), or the draft's Testing section.
- **Repo standards:** `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the house rules in the skill's `scope.md`.
- **Thread:** the issue page.

## Plan fidelity (harness category: silent-drift)

- **Where it lives:**
  - Plan side: in `## Plan context`, look for the plan's steps ("Plan:", "Change:", "Scope", numbered steps), its `Files:` line, its "Not in scope" or "Out:" wording, and any deviation or deferral note ("Known limit", "deferred", "Deviation note"). Live: `plan.md`'s Scope, Files, and `## Deviations`.
  - Diff side: every `--- a/` / `+++ b/` file header and `@@` hunk in `### Diff`.
  - Description side: every "adds / fixes / updates / removes" claim, and every fidelity phrase ("exactly as planned", "no changes beyond the plan", "no functional changes outside ...").
- **What good looks like:**
  - Every changed file is one the plan names (or a test or required changelog for it).
  - Every hunk does a planned step.
  - Every planned step appears in the diff or in a deviation note.
  - Every description claim can be pointed at a hunk.
  - An honest deviation is written in the plan's note and restated in the description, so it re-ties the mismatch. Example: neovim's `%VAR%` deferral stated in both places.
- **What drift looks like:**
  - More than planned: a second hunk in a file the plan never touches, a new config option or flag, a rename pass, or a helper rewrite, under a description that says "exactly as planned".
  - Less than planned: the plan promises a warning and a docs update, the diff has only the warning, and the description claims the docs were updated.

## Test evidence (harness category: not-tested)

- **Where it lives:**
  - The bar: the plan's `Test plan:` (each repro case and its expected result) and the repro evidence at the top of `## Plan context`.
  - The proof: `### Test evidence` (live: the evidence file or the description's Testing section).
- **What decisive looks like:** each repro case the test plan names appears with:
  - the command run;
  - the before output (the failure);
  - the after output (the expected observable: a clean parse, exit 0, a full URL, `avg_score=0.5`);
  - a run that goes through the code the diff changes.
- **What not-tested looks like:**
  - "tests pass", "verified locally", or "works on my machine" with no output.
  - `cargo test passes` with the plan's repro never shown.
  - Only the no-bug control run (single-file when the bug is two-file; a GET when the fix is in POST).
  - Only one of two failure modes the plan names.
- **Repo checks:** test-runner, lint, or CI output in the evidence, or an honest line saying which were not run and why.

## Diff quality (harness category: unreviewable)

- **Where it lives:** the `+` and `-` lines of every hunk in `### Diff` (live: `git diff main...HEAD`), and the messages in `### Commits`.
- **What reviewable looks like:**
  - Every hunk is part of the fix or its test.
  - Unchanged lines stay unchanged.
  - Commit messages say what changed.
- **Debris tells** (any one fails):
  - Debug output left in: `println!`, `eprintln!`, `print(`, `console.log`, "DEBUG".
  - Commented-out code: `// let x = ...`, or a commented-out first attempt.
  - An unused function, or one behind `#[allow(dead_code)]` or `_unused` names.
  - A new `TODO` or `FIXME`.
  - Re-indented or re-wrapped lines that are otherwise identical.
  - Reordered imports.
  - Hunks in unrelated files.
  - Commit messages such as "wip", "fmt + cleanup", or "misc cleanups while debugging" point to where debris may be.

## Standards and comms (harness category: standards-wall)

- **Where it lives:**
  - The asks: the `pull requests:` and `contribution policy` lines of `## Repo facts`. These cover the template sections or checklist, the issue-reference form ("closes #xxxx"), changelog or whatsnew asks, and the AI-use policy.
  - Where they are met: `### Title`, `### Description`, `### Commits` (when a commit convention is asked), and `### Diff` (for a changelog or whatsnew file).
  - Live, Path Review: the template sections in `.github/PULL_REQUEST_TEMPLATE.md` are Summary, Issue (`Closes #`), Changes, Testing (checklist, including the xfail-marker item for seeded bugs), Screenshots, and Notes for Reviewers. Also: CONTRIBUTING's branch and commit conventions, and `scope.md`'s rule that every template section gets real content, including disclosure where it applies.
- **What compliant looks like:**
  - Each stated section or checklist item is present with real content about this change.
  - Unchecked boxes carry a reason.
  - The issue is referenced in the asked form.
  - Any required changelog entry is in the diff.
  - Where the policy asks for AI disclosure, the description states the AI use in the author's own words.
- **What a wall looks like:**
  - A required checklist missing.
  - No `closes #N` where asked.
  - A required whatsnew entry absent from the diff.
  - Template placeholder text left in.
  - A repo whose policy requires disclosing all AI use gets a description with none, however good the fix.
- **Thread direction:** maintainer direction quoted in `## Thread highlights` should be engaged by the description. Whether the description's claims match the diff belongs to plan fidelity, not here.
