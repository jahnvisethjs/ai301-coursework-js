# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Jahnvi Seth

## Featured Tools

I chose issue-scout, repro-check, voice-guard, and ship-gate because each changed when my own work exposed a weakness. Together they cover finding work, checking what I can honestly claim, communicating upstream, and checking a branch before opening a PR. Their most useful results include rejecting my own drafts and exposing a hook I had trusted without testing. The record below runs through November 4, 2026.

**issue-scout**

**What it is, and the judgment hole I wrote:** A GitHub issue search that ranks candidates using free search fields, then grades only the top five. I wrote `search-strategy.md`: six queries tied to my interests in Python, RAG, agents, and evaluation, plus drop rules, ordering, and a two-per-repository cap. I also wrote the rubric that decides whether an issue is active, available, feasible, and open to AI-assisted contributions. Choosing the queries and defining what counts as an available issue were my judgment, rather than defaults supplied by the runner.

**How it evolved, dated:** On October 31, 2026, hand-running the queries exposed two silent failures. Quoted keyword groups returned no results; removing the quotes returned 100 per query. Sorting by update date limited the first 100 results to nine hours, making my 60-day filter useless. I removed that sort, and the filter then dropped 71 older results in the diagnostic run.

The first scan accepted FinPilot-AI#4 even though the owner had already approved another contributor's claim. That day I changed `unclaimed` to reject a claim acknowledged by a human maintainer within 30 days. I also added an established-organisation query and made those owners the first ordering key after the original ranking favoured days-old solo repositories. The regression run scored 19/20.

On November 1, I replaced `scoped-for-newcomer` with `scope-feasible` and added a separate difficulty note. Lasagna#3 had shown why I needed both: one identifiable adapter could still require a full-paper comparison, Docker, and documentation changes across many files. The new rule separates whether work is identifiable from how ambitious it is. I re-graded the final two picks; both still passed, with lasagna described as moderate, close to ambitious. The 19/20 regression score predates this last scope change; I have no saved full regression run for that revision.

**Evidence:** My dated scan notes record the original mistake and the rule it changed:

> 2026-10-31: rubric.md `unclaimed`: a claim comment acknowledged by an owner/member/collaborator (not a bot) within 30 days now fails the check.

The October 31 regression output in `ai301-unit1-starter/eval/eval-run-unit5.txt` ends with:

```text
issue-19  accept  reject   NO     failed: scoped-for-newcomer
categories: claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1  scope 4/4
agreement: 19/20 scored items  (bar: 18/20: PASS)
```

Scan 2 is also evidence of a judgment gap: it accepted two llama_index documentation requests while its own read-out recommended taking neither, because they were mass-filed outsider requests without maintainer endorsement. I did not treat `accept` as a reason to claim automatically. After direct grading and the difficulty re-grade, I chose lasagna#3, posted my [claim on November 1](https://github.com/dolev31/lasagna/issues/3#issuecomment-6562566317), and was assigned the issue that day.

**repro-check**

**What it is, and the judgment hole I wrote:** A reviewer for a claim and reproduction package. My `rubric.md` defines environment, followable steps, behavior-match, honesty, repository conventions, and a preferred claim-scope check. Every required check must pass; an unclear required check holds the package. I had to decide what proof a stranger would need and how to distinguish an honest non-reproduction from an unsupported reproduction claim.

**How it evolved, dated:** In the September 27–29, 2026 Unit 2 work, the first full run scored 17/20 and missed the disclosure category entirely. It falsely rejected honest cannot-reproduce reports in `pkg-09` and `pkg-10`, while accepting `pkg-20` despite the repository's required AI disclosure. I revised `behavior-match` to apply only when the report claims a reproduction, leaving non-reproduction conclusions to the honesty check. I made `conventions` explicitly fail a missing disclosure when the repository requires one. A seven-package check agreed on every item, followed by a full 20/20 run.

On November 1, my real lasagna claim exposed a smaller ambiguity. It promised findings by November 5, rather than a fix. I clarified `claim-scope` to permit a findings date. In live use, the reviewer accepted my lasagna reproduction but flagged wording that presented the dev split as settled before the maintainer had answered. I changed that sentence to say I planned to evaluate on dev and stated that the reproduction was Windows-only.

**Evidence:** The Unit 2 reproduction write-up records the before-and-after scores:

```text
Before: agreement: 17/20 scored items
        category floor unmet: disclosure 0/1
After:  agreement: 20/20 scored items  (bar: 18/20: PASS)
```

The saved full run in `ai301-unit2-starter/eval/eval-run.txt` shows the three corrected decisions:

```text
pkg-09  accept  accept   yes
pkg-10  accept  accept   yes
pkg-20  reject  reject   yes
```

My later rubric wording makes the findings/fix distinction explicit:

> Claims only investigation — promises no fix and no fix date (a date for reporting findings is fine).

This also carried into real work: the lasagna review checked the `Unknown benchmark 'miracl/sw'` error against `cli.py` at `10f9c25`, and my report separated that reproduced missing feature from the dataset-ID checks I had not yet done. Those checks were subsequently measured and posted in my [November 3 data report](https://github.com/dolev31/lasagna/issues/3#issuecomment-6597190664).

**voice-guard**

**What it is, and the judgment hole I wrote:** A PreToolUse hook that checks outgoing GitHub comments and PR text against my voice guide. I wrote the rules for evidence, uncertainty, investigation promises, claims, questions, and pings. The guide asks me to state what I observed, disclose what remains unchecked, and ask a maintainer one answerable question. In Unit 6 I consolidated the carry-over guides into one canonical file so those rules would not drift between tools.

**How it evolved, dated:** On November 1, 2026, the wiring test ran unblocked. The guard recognised the bare command `gh`, but Claude invoked `/c/Program Files/GitHub CLI/gh.exe`. I patched command detection to match the executable's file name, including `gh.exe`; the next full-path test stopped.

Later that day, the guard stopped my own lasagna claim twice. My guide said a claim should ask rather than announce, while the issue explicitly required the phrase "I'm taking this". I added a narrow exception: use the repository's required phrase verbatim, while still asking and naming a concrete first step. The draft then passed manually and through the posting hook. This changed a general rule in response to an actual repository convention.

**Evidence:** The November 1 entries in `~/.claude/voice-guard/guard-log.md` show the progression. These are shortened excerpts; times are as recorded in the log:

```text
11:24 [hook] stop: no body the guard can read
      target: /c/Program Files/GitHub CLI/gh.exe issue comment --help
11:59 [manual] stop: rule broken: "A claim asks to take the issue rather than announcing ..."
12:00 [manual] stop: rule broken: "A claim asks to take the issue rather than announcing ..."
12:02 [manual] pass | target: claim-draft.md
12:10 [hook] pass | target: .../gh.exe issue comment ... --body-file
```

The revised guide says:

> When the issue names its own claim phrase ("Comment 'I'm taking this' to claim it"), I use that phrase verbatim, and the rest of the claim still asks and names a first step.

Later log entries show use beyond the wiring test: the data-report posting passed on November 2 at 23:38, the draft PR creation passed on November 3 at 22:44, and the contribution-story draft passed manually on November 4 at 09:24. A voice pass is evidence of the guide being applied; it does not establish that the technical work is complete.

**ship-gate**

**What it is, and the judgment hole I wrote:** A gate that runs repository checks from my manifest and then applies my pr-precheck rubric to the branch and PR text. I wrote `lasagna/ship-manifest.md`, including the command, source, and reason for each required check, plus a separate table of exclusions with dates. The judgment was deciding which local checks protect this adapter change and explaining what still needs evidence elsewhere.

**How it evolved, dated:** On November 1, 2026, the full suite on clean Windows main produced 46 failures and two errors. I recorded the environment problems and made the local manifest run lint, format, and the relevant adapter, registry, and documentation tests. I kept the Linux full-suite comparison and Docker evidence as separate obligations.

On November 3, the first two gate runs blocked on `test_component_list_imports_no_component`. It passed in the agent's shell but failed in my PowerShell: the child process wrote cp1252 while the parent decoded UTF-8. I added `PYTHONIOENCODING=utf-8` to the manifest command, which the gate executes through `cmd.exe`. The third run passed the repository checks, then correctly rejected the missing sample, Docker, paper-comparison, and real-query evidence. I recorded the maintainer-invited draft-first order as a dated plan deviation.

The draft opened without the gate running. Reading the log exposed another failure: the installed hook called `python3`, which was unavailable on Windows. I corrected it to `python -X utf8` and confirmed the hook stopped a test invocation. That restored a demonstrated stop; it did not produce an overall gate PASS.

**Evidence:** Shortened excerpts from `~/.claude/ship-gate/gate-log.md` show two different reasons to hold the same branch:

```text
2026-11-03 22:23 [manual]
verdict: BLOCK (block)
FAILED tests/test_registry_listing_is_lazy.py::test_component_list_imports_no_component
1 failed, 163 passed, 2 skipped, 3 deselected

2026-11-03 22:41 [manual]
Benchmark, registry and docs tests: ... -> ok (35s)
decisive-evidence [fail]: test_evidence.md: sample run 'TODO paste',
Docker stack 'TODO', paper-check ... 'TODO', real query 'TODO';
plan's test plan names these cases and no deviation note defers them.

2026-11-03 22:48 [hook]
verdict: BLOCK (setup)
no PR text to check ... If this was the hook test, this stop is expected
```

The later evidence file records `164 passed, 2 skipped, 3 deselected` for the Windows manifest checks. Its Linux comparison on base `4984d5b` records main with `5930 passed, 2 failed` and the branch with `5943 passed, 2 failed`: 13 additional passing adapter tests, with the same two baseline failures. These are scoped check results, not a clean full suite or a gate PASS. All recorded gate verdicts remain BLOCK; the hook test proves only that the repaired hook now intercepts the command.

## Inventory

- **issue-select:** My Unit 1 issue-selection rubric checks activity, availability, scope, and contribution policy. Its saved eval scored 18/20. I deliberately kept the strict scope rule despite rejecting two gold accepts; the later live-search version evolved separately into issue-scout.
- **plan-check:** My rubric checks that a cause is grounded, scope is bounded, a change is executable, and the test can distinguish before from after. It scored 19/20 and accepted my lasagna plan; its evidence guide still assumes filenames and headings that differ from the course template.
- **pr-precheck:** My PR-review rubric checks the diff against the plan, description against the diff, decisive evidence, cleanliness, and repository standards. It scored 19/20 and later rejected my real lasagna branch for missing evidence when called by ship-gate; I preserved that requirement.
- **Repository memory (`CLAUDE.local.md`):** Local lasagna notes begun November 1 and expanded with the measured MIRACL data, upstream count rules, and the Codespace fix. It imports `AGENTS.md` directly to handle the Windows symlink problem; dated notes still need rechecking when upstream moves.
- **pr-monitor:** A runner guided by my response and staleness playbook, first used November 4. It caught a merge conflict on PR #73 and drafted a rebase plan; I followed it and reran checks. The playbook permits no nudge while the PR is a draft and considers one after seven quiet days once ready. Review-response and long-stall behavior have not yet been exercised.

## The Arc

**Beat 1**

Date: September 22, 2026  
Friction: issue-select rejected two gold accepts because my scope rule read their work as too broad or unsettled.  
Change: I retained the strict rule and documented the trade-off, because relaxing it could admit umbrella work and unresolved design debates.  
Result: The full run scored 18/20 and matched all four scope rejects. This established a deliberate boundary rather than a claim of perfect agreement.

**Beat 2**

Date: September 27–29, 2026  
Friction: repro-check scored 17/20, rejected honest non-reproductions, and missed a required disclosure.  
Change: I separated behavior-match from honesty and made repository-required disclosure explicit in conventions.  
Result: All seven packages in the focused recheck agreed, then the saved full run scored 20/20 with every category matched.

**Beat 3**

Date: October 31, 2026  
Friction: Scout queries returned zero results with quoted keywords; sorting by update date made the 60-day filter inspect only nine hours of issues.  
Change: I unquoted the keyword groups and removed the update-date sort.  
Result: The diagnostic queries returned 100 results each, and the older-result filter dropped 71 instead of zero.

**Beat 4**

Date: October 31–November 1, 2026  
Friction: The first scout scan accepted a maintainer-approved claim and favoured very new repositories. The next scan still accepted outsider requests without maintainer endorsement.  
Change: I added recent maintainer-approved claims to unclaimed, introduced the established-organisation query and ordering, and manually checked availability before grading my final picks.  
Result: The October 31 regression scored 19/20. Scan 2 exposed two occupied PR slots and two accepts I declined to take; the final direct grades supported choosing lasagna#3, which I claimed and was assigned on November 1.

**Beat 5**

Date: November 1, 2026  
Friction: The scout treated a bounded adapter as beginner-sized despite its expensive completion requirements. My reproduction rubric also blurred a findings deadline with a fix promise.  
Change: I replaced the scout's scope check with scope-feasible plus a separate difficulty note, and clarified that repro-check allows a date for reporting findings.  
Result: The two final picks still passed the live re-grade, with different difficulty assessments. My lasagna reproduction passed with its findings deadline, and I revised wording that had presented the dev split as already settled.

**Beat 6**

Date: November 1, 2026  
Friction: The voice-guard wiring test bypassed the guard when Claude invoked gh.exe by its full Windows path.  
Change: I patched detection to match the executable file name, including gh.exe.  
Result: The full-path help invocation was intercepted and stopped, demonstrating that this command path now reached the guard.

**Beat 7**

Date: November 1, 2026  
Friction: The guard stopped my real claim twice because my general ask-before-announcing rule conflicted with the issue's required claim phrase.  
Change: I added an exception for a repository's explicit claim wording while keeping the request and concrete first step.  
Result: The draft passed at 12:02 and the posting hook passed at 12:10; the claim was posted and the maintainer assigned the issue that day.

**Beat 8**

Date: November 1–3, 2026  
Friction: Clean Windows main had 46 suite failures and two errors, and my branch's registry test behaved differently in the agent's shell and my PowerShell.  
Change: I recorded the Windows exclusions, used targeted local checks alongside a Linux baseline comparison, and set the child-process encoding in the manifest.  
Result: The third gate run passed the repository checks, then exposed the next real gap: pr-precheck rejected the unfinished sample, Docker, and benchmark evidence.

**Beat 9**

Date: November 3, 2026  
Friction: The draft PR opened without a gate run because the hook called python3, which was missing on Windows.  
Change: I corrected the hook to python -X utf8 and tested interception. I also recorded the draft-first evidence deferral in the plan.  
Result: The 22:48 hook test stopped as expected and appeared in the gate log. No later overall PASS is recorded, so the repair is demonstrated only for interception.

**Beat 10**

Date: November 4, 2026  
Friction: PR-monitor found merge conflicts after upstream moved, despite an earlier quiet-state read. The PR's Linux suite figures also belonged to the previous base.  
Change: I followed its rebase plan onto 7574477 using a backup branch, resolved CHANGELOG.md, reran the manifest and registration-removal check, and force-pushed the rebased branch.  
Result: The manifest again had 164 passing tests, and removing registration still made the new test fail. The 09:23 monitor read returned quiet with mergeability UNKNOWN/UNKNOWN; that read alone does not prove mergeability. The old Linux figures still need rerunning or explicit identification as measurements on the old base.

## Limitations and What's Next

The toolkit has become better at asking for evidence, but several tools can still agree while relying on incomplete or old inputs. The scout's 19/20 score belongs to an earlier scope rule. Live repro-check and plan-check used my notes when they could not access the thread. The monitor caught a changed base, which also changed what my existing test numbers could prove. I need to record the rubric revision and code base beside each result, then refresh the relevant evidence when either changes.

The most immediate unfinished work is the sample run, Docker Compose evidence, BM25 comparison, and real-query example for the lasagna draft. The recorded Linux runs still contain two baseline failures, including one whose cause I have not traced. I need to finish those runs, identify their exact base, update the PR evidence, and rerun ship-gate before marking the draft ready. A successful manifest is only one stage of that work.

For the tools, I would next regression-test the scout's scope-feasible revision and test a maintainer-endorsement rule against the outsider requests that passed scan 2. I would align plan-check's evidence guide with the filenames I actually use. For both guards, I would add a repeatable installation check that exercises the full Windows executable path, a bad draft, and a valid draft in my own terminal. The failed ship-gate installation showed why reading the installed configuration is insufficient. PR-monitor also needs evidence from an actual review round before I can say its response rules work in practice.

**Tool limits.**

issue-scout: Ranking cannot see linked PRs, and the rubric still accepts outsider-filed work without human maintainer endorsement. The scope-feasible revision has live re-grades but no saved full regression run.

repro-check: It judges the supplied evidence rather than independently rerunning the reproduction. In the lasagna run it could not read the live thread, so its acceptance depended partly on my notes being current and complete.

voice-guard: The installed hook inspects the Bash tool; posting through PowerShell bypasses it, and browser posts require a manual check. A passing voice review also cannot establish technical correctness.

ship-gate: The local manifest excludes documented Windows failures and does not itself complete the Docker or paper runs. Its repaired hook has demonstrated a stop, but the log contains no overall PASS or successful end-to-end shipping run.
