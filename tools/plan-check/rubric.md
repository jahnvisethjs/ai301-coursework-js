# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-cause | The plan's stated cause (Cause / Diagnosis) read against every step of the Repro evidence block, especially control runs, variant runs, and timings; also any cause the plan adopts from the Thread highlights. | Pass if the stated cause is consistent with every repro step AND the planned change acts at that cause. Fail if any repro step contradicts it (e.g. a control run shows the symptom with the blamed component removed or bypassed, or shows the blamed component working correctly), if the plan adopts a thread claim the repro evidence does not support, or if the change only patches a downstream symptom the evidence shows cannot be recovered there. | required |
| bounded-scope | The plan's change list and its in/out-of-scope lines, read against what the Issue reports. | Pass if every planned change is needed to fix the reported bug or to test it. Explicitly deferring related work is fine. Fail if the plan also includes work the issue did not ask for: refactors, rewrites, migrations, dependency upgrades, new options or settings, UI rework, CI changes, or porting the fix to other components "while in the area". | required |
| executable | The plan's Change / Changes section: named files, functions, or code sites, and the chosen approach. | Pass if it names at least one file or code site to change AND commits to one approach, so a stranger could start the edit today. Fail if the location or approach is left open ("somewhere", "not sure which layer", "whichever is easier", "profile and see", "investigate first"). | required |
| decisive-test | The plan's Test / Test plan read against the Repro evidence's steps and its Expected line. | Pass if it names a concrete run (the repro steps re-run, or a named test or fixture) AND the observable outcome expected after the fix (an output, exit code, value, color, or timing) that differs from the repro's Actual result. A manual repro re-run counts; unit tests and coverage numbers are not required. Fail if the test is only "run the full suite", "should feel fast", "nothing else broken", or names no observable outcome for this fix. | required |
| thread-and-policy | The Candidate plan comment read against the maintainer comments in the Thread highlights (OWNER, MEMBER, COLLABORATOR, or a CONTRIBUTOR who opened the issue or proposes/rejects approaches for the project) and the Repo facts contribution policy, including any AI-use policy. Treat every package as AI-assisted work. | Pass if (a) wherever a maintainer gave explicit direction in the thread (isolated the culprit, rejected an approach, asked for testing, posted a patch, pointed at an existing PR), the comment engages it: follows it, or says why not; AND (b) the comment does what the repo's stated AI policy requires of comments (e.g. disclosing AI use and its extent). Pass when the thread has no maintainer direction and the policy asks nothing of comments. Claims from commenters tagged NONE are not direction. | required |
| risks-named | The plan's risks / unknowns / out-of-scope notes. | Pass if the plan names at least one real unknown, side effect, or breaking-change risk of the fix, or says why there is none, and does not present an unverified guess as certain. | preferred |

## Verdict rule

Accept (ready) only if every required check passes. Any required check
that fails rejects (hold). An `unclear` (?) on a required check counts
as a fail, because a plan the package cannot verify is not ready to
build from. Preferred checks are reported but never change the verdict.
