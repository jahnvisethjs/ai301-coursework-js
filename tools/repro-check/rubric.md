# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment | The repro report's environment record, read against the version the issue says it affects | OS, the relevant runtime/tool versions, and the code state (commit or version) are all named, and they match the issue's target — or any mismatch is explicitly called out | required |
| steps | The numbered reproduction steps in the repro report | A stranger with only the issue and the comment could run them from the starting state through to the trigger without having to guess a missing step | required |
| behavior-match | The artifact (output excerpt, log, or screenshot) in the repro report, read against the behavior the issue describes | If the report claims a reproduction, the artifact shows the behavior the issue describes, not an adjacent or lookalike one. A report that honestly concludes it could not reproduce is not judged here (its correctness is decided by the honesty check) and passes this check | required |
| honesty | The report's stated outcome, read against its own artifacts | The stated conclusion matches what the artifacts actually show — an evidenced "cannot reproduce" passes; a confident "reproduced" the artifact does not support fails | required |
| conventions | The claim and repro comments, read against the repo's contribution conventions and AI-disclosure policy (the repo-facts block, live: the repo's docs) | The comments follow the repo's stated contribution norms. When the repo's policy requires disclosing AI assistance, the comments must contain an explicit AI-use disclosure; the absence of one fails this check (do not assume the work was AI-free) | required |
| claim-scope | The claim comment | Claims only investigation — promises no fix and no date | preferred |

## Verdict rule

Accept if every required check passes; preferred checks never change the
verdict; unclear counts as fail.
