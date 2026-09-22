# Rubric: is this a good first issue?

Grade one candidate issue against the checks below. All recency thresholds
are measured against the bundle's **capture date** in eval mode, and against
today in live mode. Every check names an evidence source a grader can point at.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-active` | Repo facts: the dates in "last 5 default-branch commits" (a commit by a bot username ending in `[bot]` counts only when it merges a human's PR). | The most recent non-bot default-branch commit is dated within **90 days** of the capture date. | required |
| `not-archived` | Repo facts: the `archived:` flag on the repo line. | The flag reads `archived: no`. An archived (read-only) repo fails. | required |
| `unclaimed` | Repo facts: `this issue: assignees:` and `linked PRs:` (with each PR's state). Per `scope.md`, other students' claim comments do **not** count in live mode. | Assignees is `none` **and** no linked PR is in state `open`. A `closed` or `merged` linked PR is a past attempt, not an active claim, and does not fail this check. | required |
| `scoped-for-newcomer` | The issue title, body, and comment thread. | The issue asks for **one bounded piece of work**. It fails if it is a self-described umbrella/tracking issue (a list of sub-items meant to be split), if the thread shows the design is still unsettled (maintainers still debating, or a long-open issue with multiple abandoned PRs), if it is a pure usage/support question ("how do I get this to work?"), or if it is a one-line feature wish that hides an unmade product decision. A terse bug report, a checklist, or a body with no reproduction steps still passes when the work asked for is bounded — grade the size of the work, not the polish of the writeup. | required |
| `ai-contribution-allowed` | Repo facts: the `contribution policy` line (CONTRIBUTING.md / AI-policy files). | The stated policy does **not** outright ban AI-generated or AI-assisted contributions. Disclosure, personal-understanding, testing, and human-review conditions still pass; silence (no stated policy) passes. Only an explicit ban fails. | required |
| `beginner-signal` | The issue's labels (e.g. `good first issue`, `documentation`, `help wanted`) and whether a maintainer filed it. | Passes when the issue carries a beginner-friendly label **or** was opened by a maintainer with a clear behavior description. | preferred |
| `maintainer-responsive` | Repo facts: the "maintainer first-response sample" latency figures. | At least one sampled issue shows an owner/member/collaborator first response within **45 days**. | preferred |

## Verdict rule

`accept` **only if every `required` check grades `pass`**. If any required
check grades `fail` or `unclear`, the verdict is `reject` (a first issue you
cannot verify is not one to take). `preferred` checks never change the
verdict; they only rank the accepted issues — an accepted issue that also
passes `beginner-signal` and `maintainer-responsive` ranks above one that
does not.
