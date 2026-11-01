# Contribution:  Benchmark adapter: MIRACL (multilingual retrieval, 18 languages)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jahnvi Seth
**GitHub Username:** jahnvisethjs
**Issue:**[ [GitHub issue link]](https://github.com/dolev31/lasagna/issues/3)
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, a GitHub-wide issue search that ranks for free and grades only the top 5 against my rubric**

 (2026-10-31 to 11-01) I wrote a search strategy of six `gh search issues` queries traced to my fit profile (RAG, agents, evaluation, Python libraries), with free ranking filters and a top-5 grading cap. Running the queries by hand exposed two silent bugs: quoted keywords returned zero results, and sorting by update date shrank the window to the last nine hours, so my 60-day filter never fired. My first live scan accepted an issue the owner had already handed to someone else and filled the top 5 with days-old solo repos, so I made maintainer-approved claims fail `unclaimed` (still 19/20 on the unit-1 eval) and added a query and ordering key for established LLM organisations. The second scan still let through mass-filed requests no maintainer had asked for, and when I hand-checked 17 issues, 16 had open PRs, claims, or maintainer refusals that the course site's list never showed, which is why I graded my two picks with the scout before claiming either.

**Unit 6: Installed the voice guard, a hook that checks every comment I post through Claude Code against my own voice guide, and wrote a repo memory file for lasagna**

(2026-11-01) I merged my three carry-over voice guides into one canonical file, added Upstream rules for claims, questions, and pings, and installed the guard hook. The wiring test ran unblocked at first: on Windows, Claude called `gh` by its full path (`.../GitHub CLI/gh.exe`), and the guard only matched the bare word `gh`, so a real claim would have posted unchecked; I patched it to match `gh` or `gh.exe` by file name, and the next test was stopped as expected. The guard then stopped my lasagna#3 claim, because my own rule says a claim asks rather than announces, but the issue says to comment "I'm taking this" to claim it, so I added an exception to the rule (use a repo's claim phrase word for word, and still ask and name a first step), and the same draft passed. [Memory file: one sentence on what you put in CLAUDE.local.md and what it fixed or what fell short.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my issue-scout skill: six `gh search issues` queries built from my fit profile (RAG, agents, evaluation, Python libraries), ranked by free filters, with only the top 5 graded against my rubric. Scan 1 surfaced dolev31/lasagna#3, a MIRACL benchmark adapter that adds a multilingual retrieval benchmark (Swahili split) to a Python retrieval-evaluation toolkit. I graded it again in direct mode with my backup, Raftersecurity/rafter-cli#32 (a CI auto-label workflow), and both were accepted. After this week's scope patch I re-graded both: lasagna#3 still passed every check (active repo, unclaimed, one adapter file named by the owner, a CONTRIBUTING policy that welcomes AI-assisted work with disclosure), with a difficulty note of moderate, close to ambitious. rafter-cli#32 came back straightforward, but it is CI YAML in a TypeScript repo, so I unclaimed it.

lasagna#3 fits what I want to get better at: retrieval evaluation (BM25, nDCG@10), Hugging Face datasets, and pytest, in a project where outside contributors are already getting PRs reviewed. I hope to learn how a benchmark harness stays reproducible when someone new adds to it, since the issue asks me to match the MIRACL paper's BM25 score and give commands a maintainer can rerun for the same numbers.

The scout was not trustworthy out of the box. Hand-running its queries showed quoted keywords returned nothing and date-sorting hid every issue older than nine hours. Scan 1 also accepted an issue the owner had already given to someone else, so I made maintainer-approved claims fail `unclaimed` and re-ran the unit-1 eval (19/20). Its biggest limit showed up when I hand-checked 17 issues, including 13 from the course Issue Selection site: 16 were already taken by open PRs, recent claims, or maintainer refusals the site's list doesn't show. Its scope check had the opposite problem: it was strict about open design questions but blind to size. The difficulty note now says that lasagna#3 is a big job: an nDCG@10 comparison with the paper, a Docker Compose run, and a benchmark count to update in about ten files. I am taking that on knowingly.

Claim: [my claim comment](https://github.com/dolev31/lasagna/issues/3#issuecomment-6562566317) (posted 2026-11-01; the issue asks for a draft PR within two weeks)

**Toolkit**

- Tool: issue-scout (my search strategy + rubric), run as a full scan twice, in direct mode on my two final picks, and again on both after the unit-6 scope patch
- What it produced: scan 1 graded 5 and accepted all 5, including lasagna#3; scan 2 accepted 2 llama_index issues and rejected 3 (open PRs, unsettled design); the direct grade and the re-grade accepted both lasagna#3 (moderate, close to ambitious) and rafter-cli#32 (straightforward)
- Difficulties: two silent query bugs (quotes, sort order); it accepted a claimed issue and mass-filed outsider requests; ranking cannot see linked PRs or star counts; `maintainer-responsive` is unclear on brand-new repos; my unit-1 scope check `scoped-for-newcomer` rejected any issue with an unsettled design, yet passed lasagna#3 without saying how big it is
- What changed: unquoted keywords, dropped `--sort updated`, `unclaimed` now counts maintainer-approved claims (eval 19/20), added an established-org query and ordering key; in unit 6 I replaced `scoped-for-newcomer` with `scope-feasible` (fails only umbrella issues) and added a difficulty note (straightforward / moderate / ambitious) that informs the choice but never changes the verdict; still logged, not patched: a maintainer-endorsement requirement for outsider-filed issues

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
