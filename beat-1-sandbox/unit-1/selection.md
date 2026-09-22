# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64

**Verdict output**

Live-mode grade of issue #64 ("Relevance scorer 'partial overlap' test fixture actually
has full query overlap"), scoped to `codepath/pathreview-ai301-fa26-s3`:

- `repo-active` — pass: latest default-branch commit `2f4e82f` by Aburke225, 5 days ago (within 90 days of today).
- `not-archived` — pass: repo is Public with no "archived" banner.
- `unclaimed` — pass: Assignees "No one assigned"; Development "No branches or pull requests"; no comments on the issue.
- `scoped-for-newcomer` — pass: one bounded fixture fix with an exact repro (`pytest tests/unit/test_relevance_scorer.py -q`), opened by a Collaborator.
- `ai-contribution-allowed` — pass: `docs/CONTRIBUTING.md` states no AI ban; the repo's latest commit is co-authored by claude.
- `beginner-signal` (preferred) — pass: labels `good first issue`, `tests`, `tier-1`; maintainer-filed.
- `maintainer-responsive` (preferred) — pass: maintainer Aburke225 is actively triaging (filed and labeled the issue; commit activity 5 days ago).

Every required check passes, so by the verdict rule the issue is accepted.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64",
  "checks": [
    {"name": "repo-active", "grade": "pass", "evidence": "latest default-branch commit 2f4e82f by Aburke225, 5 days ago — within 90 days of today"},
    {"name": "not-archived", "grade": "pass", "evidence": "repo is Public with no 'archived' banner"},
    {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: No one assigned; Development: No branches or pull requests; no comments"},
    {"name": "scoped-for-newcomer", "grade": "pass", "evidence": "one bounded fixture fix with exact repro 'pytest tests/unit/test_relevance_scorer.py -q'; maintainer-filed"},
    {"name": "ai-contribution-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI ban; latest repo commit co-authored by claude"},
    {"name": "beginner-signal", "grade": "pass", "evidence": "labels: good first issue, tests, tier-1; opened by a Collaborator"},
    {"name": "maintainer-responsive", "grade": "pass", "evidence": "maintainer Aburke225 actively triaging — filed and labeled the issue; commit activity 5 days ago"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- Run 1 — smoke test (`--limit 3`): `agreement: 2/3 scored items`. `issue-01` disagreed
  (`failed: scoped-for-newcomer`); `issue-02` and `issue-03` agreed.
- Run 2 — full run (`--save-run eval-run.txt`): `agreement: 18/20 scored items  (bar: 18/20: PASS)`.

The last score, `18/20`, matches the agreement line in the committed `eval-run.txt`.
(An earlier invocation crashed on Windows with `UnicodeEncodeError: 'charmap' codec can't
encode` before it graded anything and produced no score; re-running with `python -X utf8`
fixed it. I did not tune the rubric between Run 1 and Run 2 — the smoke run only confirmed
the harness read my rubric, and the full run confirmed the same disagreement at scale.)

**Issue analysis**

Scored issue: `issue-01` (source `conda/conda#16475`).

- My rubric's decision: `reject`. Gold label: `accept`. The run table records
  `issue-01  accept  reject   NO     failed: scoped-for-newcomer`.
- Reasoning that produced my result: the issue is one documentation task, but its body
  is written as several sub-sections. It asks to "Create a new task page," then to
  "Update `manage-pkgs.rst`," "Update `pip-interoperability.rst`," and
  "Update `new-features.md`," and finally to "Consider a global `troubleshooting.rst`
  entry." My `scoped-for-newcomer` check treats a list of sub-items as a possible
  umbrella/tracking issue, so the grader graded scope `fail`, and my verdict rule
  (accept only if every required check passes) rejected it — even though `repo-active`,
  `not-archived`, `unclaimed`, and `ai-contribution-allowed` all passed.
- Why the gold label differs: the answer key calls it a "docs task with a stated home
  and scope; active repo, unclaimed," reading the sub-sections as one cohesive
  deliverable (a page plus the edits that point to it), not independent work to be
  split. So the disagreement is my scope check being too aggressive about multi-section
  bodies, not a whole family my rubric cannot see.

**Check rationale**

The `scoped-for-newcomer` check, quoted as currently written in `rubric.md`
(pass condition):

> The issue asks for **one bounded piece of work**. It fails if it is a self-described
> umbrella/tracking issue (a list of sub-items meant to be split), if the thread shows
> the design is still unsettled (maintainers still debating, or a long-open issue with
> multiple abandoned PRs), if it is a pure usage/support question ("how do I get this to
> work?"), or if it is a one-line feature wish that hides an unmade product decision. A
> terse bug report, a checklist, or a body with no reproduction steps still passes when
> the work asked for is bounded — grade the size of the work, not the polish of the
> writeup.

Reasoning behind this form: the scope family is the one that does not live in the
repo-facts block, so the check has to name concrete disqualifiers the grader can point
at in the issue text — umbrella/tracking issues (`issue-05`, `issue-10`), unsettled
design with abandoned PRs (`issue-15`), and one-line wishes hiding a product decision
(`issue-20`) — rather than an adjective like "small enough." The final sentence exists
to stop the check over-rejecting: the evidence guide warns that "short is not the same
as unscoped," so a terse bug report or a checklist must still pass. I graded the size of
the work asked for, not the polish of the write-up.

**Trade-offs**

`scoped-for-newcomer` gives up two clear-accept issues whose result it changes:
`issue-01` and `issue-19`. Both are gold `accept`, and my rubric rejected both on this
one check — the run table shows `issue-01  accept  reject   NO     failed:
scoped-for-newcomer` and `issue-19  accept  reject   NO     failed: scoped-for-newcomer`.
issue-01's multi-section docs body reads as an umbrella, and issue-19's maintainer-named
internal causes read as "still touching core internals," so both trip the "one bounded
piece of work" test even though the answer key considers them takeable.

That is the deliberate cost of writing the condition aggressively enough to catch the
four real scope-rejects, which it did: the categories line reads `scope 4/4`
(`issue-05`, `issue-10`, `issue-15`, `issue-20` all correctly rejected). I accept these
two misses. The run still passes at `18/20`, and loosening the check to rescue issue-01
and issue-19 risks letting a genuine umbrella like issue-05 or a design-debate like
issue-15 back through — a worse failure than under-accepting two good issues.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to my interests and the time available.** My focus is LLM/RAG engineering and
   retrieval/LLM evaluation, and this issue sits exactly there: it's a fix to a relevance
   scorer's evaluation fixture. Fixing it means reasoning about how the scorer treats full
   vs. partial keyword coverage — real retrieval-evaluation logic — without a large
   surface area. It's labeled `tier-1` / `good first issue`, is a single test-fixture edit
   with a one-line reproduction, so it fits the limited time I have for a first
   contribution far better than a multi-file feature would.

2. **What the verdict identified correctly, and what I weighed beyond it.** The verdict
   correctly confirmed the parts a rubric can check from evidence: the repo is alive
   (commit 5 days ago), not archived, AI contributions are allowed, the issue is unclaimed
   (no assignee, no linked PR), and the scope is one bounded piece of work. Beyond the
   rubric I weighed things it cannot see: among the three issues my skill accepted, #64 is
   the only one with *no* classmate already commenting (unlike #68, where a peer said they
   would take it), so there is less overlap; the repo's CONTRIBUTING says each seeded bug
   ships with a failing `xfail` test I flip on the fix, which makes reproduction and
   verification concrete; and the fix is genuinely self-contained (one fixture), which
   suits a first repro-and-fix cycle. Those are fit and learning-value judgments, which
   the rubric deliberately leaves to me.

3. **Anticipated difficulty in claiming it.** Claiming should be low-friction: the issue
   has no assignee and no linked PR, and the Path Review house rule says to claim and open
   a PR even on a shared issue because credit attaches to the PR, not the merge. The main
   friction is procedural rather than social — per CONTRIBUTING, a first fork PR can sit at
   "waiting for approval to run workflows" until a maintainer releases CI. The real work is
   in the fix, not the claim: I'll need to understand why the scorer returns 1.0 for full
   coverage and rewrite the fixture so the overlap is genuinely partial, then remove any
   `xfail` marker so CI goes green.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
