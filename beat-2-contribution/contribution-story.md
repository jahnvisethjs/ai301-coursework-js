# Contribution: Benchmark adapter: MIRACL (multilingual retrieval, 18 languages)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name**” the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jahnvi Seth
**GitHub Username:** jahnvisethjs
**Issue:** [dolev31/lasagna#3](https://github.com/dolev31/lasagna/issues/3)
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end â€” the sections below ask for evidence that
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

 (2026-10-31 to 11-01) I wrote a search strategy of six `gh search issues` queries traced to my fit profile (RAG, agents, evaluation, Python libraries), with free ranking filters and a top-5 grading cap. Running the queries by hand exposed two silent bugs: quoted keywords returned zero results, and sorting by update date shrankthe window to the last nine hours, so my 60-day filter never fired. My first live scan accepted an issue the owner had already handed to someone else and filled thetop 5 with days-old solo repos, so I made maintainer-approved claims fail `unclaimed` (still 19/20 on the unit-1 eval) and added a query and ordering key for established LLM organisations. The second scan still let through mass-filed requests no maintainer had asked for, and when I hand-checked 17 issues, 16 had open PRs, claims, or maintainer refusals that the course site's list never showed, which is why I graded my two picks with the scout before claiming either.

**Unit 6: Installed the voice guard, a hook that checks every comment I post through Claude Code against my own voice guide, and wrote a repo memory file for lasagna**

(2026-11-01) I merged my three carry-over voice guides into one canonical file, added Upstream rules for claims, questions, and pings, and installed the guard hook, but the wiring test ran unblocked: on Windows Claude called `gh` by its full path (`.../GitHub CLI/gh.exe`) and the guard only matched the bare word `gh`, so I patched it to match by file name and the next test was stopped. The guard then stopped my own lasagna#3 claim, because my rule says a claim asks rather than announces while the issue says to comment "I'm taking this", so I added an exception (use a repo's claim phrase word for word, and still ask and name a first step) and the same draft passed. I then wrote a CLAUDE.local.md for my lasagna fork (CONTRIBUTING rules, the adapter subsystem, test commands, AI policy). Orienting for it turned up two problems: on Windows the repo's CLAUDE.md symlink checks out as a text file, so I kept the `@AGENTS.md` import to load the maintainers' rules, and a benchmark merged after my issue was filed already moved the count from 31 to 32, so my adapter makes it 33, not the 32 the issue says.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my issue-scout skill: six `gh search issues` queries built from my fit profile (RAG, agents, evaluation, Python libraries), ranked by free filters, with only the top 5 graded against my rubric. Scan 1 surfaced dolev31/lasagna#3, a MIRACL benchmark adapter that adds a multilingual retrieval benchmark (Swahili split) to a Python retrieval-evaluation toolkit. I graded it again in direct mode with my backup, Raftersecurity/rafter-cli#32 (a CI auto-label workflow), and both wereaccepted. After this week's scope patch I re-graded both: lasagna#3 still passed every check (active repo, unclaimed, one adapter file named by the owner, a CONTRIBUTING policy that welcomes AI-assisted work with disclosure), with a difficulty note of moderate, close to ambitious. rafter-cli#32 came back straightforward, but itis CI YAML in a TypeScript repo, so I unclaimed it.

lasagna#3 fits what I want to get better at: retrieval evaluation (BM25, nDCG@10), Hugging Face datasets, and pytest, in a project where outside contributors are already getting PRs reviewed. I hope to learn how a benchmark harness stays reproducible when someone new adds to it, since the issue asks me to match the MIRACL paper's BM25 score and give commands a maintainer can rerun for the same numbers.

The scout was not trustworthy out of the box. Hand-running its queries showed quoted keywords returned nothing and date-sorting hid every issue older than nine hours. Scan 1 also accepted an issue the owner had already given to someone else, so I made maintainer-approved claims fail `unclaimed` and re-ran the unit-1 eval (19/20). Its biggest limit showed up when I hand-checked 17 issues, including 13 from the course Issue Selection site: 16 were already taken by open PRs, recent claims, ormaintainer refusals the site's list doesn't show. Its scope check had the opposite problem: it was strict about open design questions but blind to size. The difficulty note now says that lasagna#3 is a big job: an nDCG@10 comparison with the paper, a Docker Compose run, and a benchmark count to update in about ten files. I am taking that on knowingly.

Claim: [my claim comment](https://github.com/dolev31/lasagna/issues/3#issuecomment-6562566317) (posted 2026-11-01; the issue asks for a draft PR within two weeks)

**Toolkit**

- Tool: issue-scout (my search strategy + rubric), run as a full scan twice, in direct mode on my two final picks, and again on both after the unit-6 scope patch
- What it produced: scan 1 graded 5 and accepted all 5, including lasagna#3; scan 2 accepted 2 llama_index issues and rejected 3 (open PRs, unsettled design); the direct grade and the re-grade accepted both lasagna#3 (moderate, close to ambitious) and rafter-cli#32 (straightforward)
- Difficulties: two silent query bugs (quotes, sort order); it accepted a claimed issue and mass-filed outsider requests; ranking cannot see linked PRs or star counts; `maintainer-responsive` is unclear on brand-new repos; my unit-1 scope check `scoped-for-newcomer` rejected any issue with an unsettled design, yet passed lasagna#3 without saying how big it is
- What changed: unquoted keywords, dropped `--sort updated`, `unclaimed` now counts maintainer-approved claims (eval 19/20), added an established-org query and ordering key; in unit 6 I replaced `scoped-for-newcomer` with `scope-feasible` (fails only umbrella issues) and added a difficulty note (straightforward / moderate / ambitious) that informs the choice but never changes the verdict; still logged, not patched: a maintainer-endorsement requirement for outsider-filed issues

---

## Understanding the Issue

**Problem Description**
Lasagna is a framework for testing RAG pipelines fairly: you swap one stage, hold the rest fixed, and score the change on a benchmark. All 32 of its public benchmarks are in English, which its paper names as a limitation, so nobody can measure how a retriever does in another language. MIRACL (arXiv 2210.09984) is a retrieval benchmark with relevance judged by native speakers in 18 languages. The issue asks for an adapter that loads its Swahili split as `miracl/sw`.

**Expected Behavior**

`lasagna benchmark list` shows `miracl/sw` with its license and paper, and `lasagna benchmark run miracl/sw --profile speed` runs on a sample. Every qrels doc ID exists in the corpus at a pinned dataset revision, BM25's nDCG@10 on the dev split lands near the paper's figure, and the docs say 33 benchmarks.

**Current Behavior**


On main (`10f9c25`), `lasagna benchmark list` ends with "32 benchmarks, plus smoke" and has no MIRACL entry, and `lasagna benchmark run miracl/sw --profile speed` exits with `Error: Unknown benchmark 'miracl/sw'`. The issue itself was out of date: it said the count was 31, but `2wikimultihopqa` had already raised it to 32. The maintainer confirmed this and updated the issue on 2026-11-01.

**Affected Components**
- New: `src/lasagna/knowledge_evaluation_sdk/benchmarks/adapters/miracl.py` (the adapter) and `tests/test_miracl.py` (an offline fixture test).
- `benchmarks/adapters/__init__.py`: one import and one call in `register_all`.
- Used as is: `BaseBenchmarkAdapter` (`_base.py`), the registry (`_registry.py`), and the run path that calls `load()` (`runner.py:557`) and `load_corpus()` (`experiment.py:3775`) and checks qrels IDs against the corpus (`experiment.py:3833`).
- Regenerated or updated: `research/DATASETS.md`, the catalog doc, the 7 lines that state the count of 32, and `CHANGELOG.md`.

**Toolkit**

- Tool: Claude Code, used to trace how lasagna runs a benchmark from the CLI to the score, and to list the MIRACL files through the Hugging Face Hub API
- What it produced: the run path with file and line numbers (registry lookup, `load()`, `load_corpus()`, the qrels-in-corpus check); the finding that Swahili has public qrels only for dev and train (test-a and test-b have queries without judgments); the pinned commits for both data repos
- Difficulties: the issue's count (31) was stale, and the registry lists 33 names while the docs say 32, which was confusing until the `benchmark list` footer showed `smoke` is a wiring check that isn't counted
- What changed: I plan to evaluate on dev, the split the paper reports BM25 on, and the PR raises the count from 32 to 33, not to 32 as the issue first said
---

## Reproduction Process

**Environment Setup**

Windows 11 Home (10.0.26300) with PowerShell 5.1, Python 3.11.0, pip 26.2.2, and lasagna 0.2.0 installed editable from `dolev31/lasagna` main @ `10f9c25` (2026-11-01). I cloned my fork, made a venv, and ran `pip install -e ".[eval]" --group dev` (CONTRIBUTING.md's pip route).
 The repo's `CLAUDE.md` is a symlink, which on Windows checks out as a text file that just says "AGENTS.md", so my local notes import AGENTS.md directly. My first install skipped `--group dev`, so pytest and ruff were missing until I reinstalled with it. On Windows, the full test suite on clean main has 46 failures (milvus-lite isn't available on Windows, `\` vs `/` paths, a POSIX-only `fcntl` import), none of them in benchmark code, so I'll run the full suite on Linux in a Codespace.


**Steps to Reproduce**
1. `git clone https://github.com/dolev31/lasagna.git; cd lasagna; git checkout 10f9c25`
2. `python -m venv .venv; .venv\Scripts\python -X utf8 -m pip install -e ".[eval]" --group dev`
3. `.venv\Scripts\lasagna benchmark list`: 32 benchmarks, none of them MIRACL, ending with "32 benchmarks, plus smoke (a wiring check, not a result)"
4. `.venv\Scripts\lasagna benchmark run miracl/sw --profile speed`: exits 1 with `Error: Unknown benchmark 'miracl/sw'.`
5. `git grep -n "32 benchmarks\|32 public benchmarks\|thirty-two"`: 7 lines state the count the adapter will raise to 33

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** reproduced: `miracl/sw` doesn't exist on main and lasagna rejects it by name. The count was already 32, not 31, so the adapter makes it 33 (the maintainer confirmed and updated the issue). The registry holds 33 names because `smoke` isn't counted. On Hugging Face, `miracl/miracl` @ `5be20db` has Swahili topics for dev, train, test-a and test-b but qrels only for dev and train, and `miracl/miracl-corpus` @ `d921ec7` has the corpus as one 10.2 MB `docs-0.jsonl.gz`. Not yet checked: whether every qrels doc ID is in the corpus; that is the data report I promised on the issue for Nov 5.
**Toolkit**

- Tool: repro-check (my unit-6 rubric), run with `claude -p` on my posted claim and this repro report
- What it produced: accept, with all five required checks passing (environment, steps, behavior-match, honesty, conventions) and the preferred claim-scope check passing; it verified the error text and the "plus smoke" footer against `cli.py` at `10f9c25`
- Difficulties: in `claude -p` it couldn't call `gh`, so it read the issue facts from my notes instead of the live thread; it also flagged that "the adapter evaluates on dev" read as a settled decision before the maintainer had a say
- What changed: I reworded it to "I plan to evaluate on dev" and added that this run was Windows-only; in unit 6 I had already fixed the rubric's claim-scope check to say "no fix and no fix date (a date for reporting findings is fine)", since my claim promises findings by Nov 5
---

## Solution Approach

## Solution Approach

**Analysis**

Nothing is broken; the benchmark was never added. `lasagna benchmark run miracl/sw` looks the name up with `BenchmarkRegistry.get()` (`benchmarks/_registry.py:164`). The registry fills itself from `register_all()` in `benchmarks/adapters/__init__.py` (`_registry.py:81-83`), and no adapter there registers `miracl/sw`, so the CLI prints `Unknown benchmark 'miracl/sw'` (`cli.py:505`).

Once a name is registered, the run path needs two things from its adapter. The runner calls `adapter.load()` (`runner.py:557`) for `BenchmarkSample`s whose `expected_docs` carry the qrels (`_base.py:60-78`). The experiment then calls `adapter.load_corpus()` (`experiment.py:3775`), and before it searches, it checks every qrels doc ID against the corpus: it warns when some are missing and aborts when more than half are (`experiment.py:3833-3849`). So the adapter has to read MIRACL's topics, qrels and corpus files, and its qrels doc IDs must match its corpus doc IDs exactly.

The count is the second part. `lasagna benchmark list` ends with "32 benchmarks, plus smoke", and `git grep -n "32 benchmarks\|32 public benchmarks\|thirty-two"` finds 7 lines that state it. `test_readme_counts_match_the_registry.py` and `test_catalog_doc_current.py` fail until the docs match the registry.

**Proposed Solution**

Add one adapter, `MiraclAdapter`, in `adapters/miracl.py`, following `adapters/twowikimultihopqa.py` (merged in #24): download files with `hf_hub_download` at pinned revisions, implement `info`, `load` and `load_corpus`, and end with `register(registry)`.

- `info()`: name `miracl/sw`, `BenchmarkEvalMode.RETRIEVAL`, license Apache-2.0, paper `arxiv:2210.09984`. `num_queries` and `num_documents` come from the downloaded data, never from an estimate.
- `load()`: reads `topics.miracl-v1.0-sw-<split>.tsv` and `qrels.miracl-v1.0-sw-<split>.tsv` from `miracl/miracl`. The splits are `dev` (the default, and the split the paper reports BM25 on) and `train`; test-a and test-b have no public qrels. Each sample keeps MIRACL's query ID. `expected_docs` keeps only judgments with relevance above 0, and a query with no relevant passage is dropped, as `_convert_beir_with_qrels` does for BEIR (`datasets.py:654-661`). `n_samples` and `seed` pick a reproducible subset.
- `load_corpus()`: reads `miracl-corpus-v1.0-sw/docs-0.jsonl.gz` from `miracl/miracl-corpus` with `gzip` and `json`, and returns `{docid: title + "\n\n" + text}`. It respects `max_docs` (0 means all) and sets `corpus_truncated` when it stops early.
- Pinning: `hf_revision` holds the `miracl/miracl` commit, and the corpus commit gets its own attribute, because the data lives in two repos. I'm asking the maintainer whether they want it recorded differently.

What I am not changing: `BaseBenchmarkAdapter`, the registry, the qrels check in `experiment.py`, any other adapter, and any other MIRACL language. The adapter registers only `sw`; each further language is its own PR, as the maintainer said. Count lines that the issue's `git grep` and the two count tests don't point at stay as they are.

**Implementation Plan**

1. Before code (the Nov 5 data report): download the Swahili topics, qrels and corpus at the pinned commits, check that every dev qrels doc ID exists in the corpus, count the queries and passages, and post the result on the issue.
2. Branch `feat/3-miracl-sw` from `main`.
3. Add `adapters/miracl.py` as described above, with a module docstring.
4. In `adapters/__init__.py` `register_all`, add `from .miracl import register as register_miracl` and call it.
5. Add `tests/test_miracl.py` with a tiny offline fixture (a few TSV lines and a small `.jsonl.gz`), monkeypatching `hf_hub_download` as `tests/test_twowikimultihopqa.py` does. It checks that the metadata loads without a download, that relevance-0 judgments are dropped, that every expected doc is in the corpus, that each download asks for the pinned revision, and that `max_docs` truncates and sets the flag. I'll confirm each test fails without the adapter.
6. Regenerate `DATASETS.md` (`lasagna benchmark licenses -o src/lasagna/knowledge_evaluation_sdk/research/DATASETS.md`) and the catalog (`python scripts/gen_catalog_doc.py`).
7. Raise the count from 32 to 33 in every line the `git grep` finds, until the two count tests pass.
8. Add a `CHANGELOG.md` line under Unreleased.
9. Check: `lasagna benchmark list` shows `miracl/sw`; `lasagna benchmark run miracl/sw --profile speed` runs on a sample; the manifest checks pass; the full suite passes on Linux (Codespace).
10. Paper check: run `bm25` on the full dev split (a sweep with `{name: bm25, retriever: bm25}` and `tracks: [retrieval]`, `n_runs: 1`), and give my nDCG@10 beside the paper's (table and page), with any gap explained. Show one real query with its judged passages and the top five `bm25` results, plus the exact commands and seed.
11. Run it in the Docker Compose stack built from the branch (`IMAGE_EXTRAS=eval`), and keep the commands, `docker compose ps` image lines and output for the PR.

- Tool: plan-check (my unit-3 rubric), run with `claude -p` on `plan.md` and a draft plan comment `plan-comment.md` that I kept off the issue
- What it produced: accept on the first run: grounded-cause, bounded-scope, executable, decisive-test and thread-and-policy all passed, and so did the preferred risks-named check (the two-repo revision pin as a question for the maintainer, the qrels/corpus ID match left to the Nov 5 report)
- Difficulties: it couldn't read the live thread in `claude -p`, so it used my notes; its evidence guide expects a file named `comment.md` and `Cause:` / `Test:` / `Risks` headings, while the course template uses `plan-comment.md`, Analysis and Implementation Plan, so it had to infer where things were; my voice guide caught that the comment didn't say the ID match was still unchecked
- What changed: I added that half sentence to the comment; the evidence-guide naming mismatch is logged, not patched yet

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
