# Procedure: how this skill grades a plan package

## Read order

Read the parts in this order. Write notes as you read; do not open the
candidate plan until step 4. You need your own view of the evidence
first, so a confident, polished plan cannot set it for you.

1. **Repro evidence block** (live mode: the student's posted repro
   comment on the issue; for a house issue, the repro pack as quoted in
   the drafts). Note:
   - every step and its result, numbered as in the report;
   - every control or variant step (same input with one thing changed,
     removed, or bypassed) and what it showed;
   - the Expected line and the Actual line, word for word;
   - in one sentence: which component the evidence points at, and
     which components it rules out.
2. **Issue** (title, body). Note the one bug being reported, in one
   sentence. This is the yardstick for scope.
3. **Thread highlights and Repo facts.** Note:
   - each maintainer comment (OWNER, MEMBER, COLLABORATOR, or a
     CONTRIBUTOR who opened the issue or proposes/rejects approaches
     for the project; never NONE) that gives direction (isolates a culprit, asks for testing,
     settles an approach, points at an existing PR or patch), quoted;
   - each claimed cause from anyone, with the author's role, marked
     "supported by repro" / "contradicted by repro" / "not tested by
     repro" using your note from step 1;
   - the contribution policy, and whether it requires AI-use
     disclosure, quoted.
4. **Candidate plan**, top to bottom.
5. **Candidate plan comment**, last, because it is graded against
   everything above.

## Evidence gathering

For each check in `rubric.md`, pull exactly these facts, quoting the
package text (see `references/evidence-guide.md` for where each lives).
Record "absent" when the package has no such text.

1. **grounded-cause**: quote the plan's stated cause and the change it
   makes. Put next to it your step-1 note on what the repro points at
   and rules out. List any repro step whose result the stated cause
   cannot explain. If the plan's cause comes from a thread comment,
   say which one.
2. **bounded-scope**: list every change the plan proposes, one line
   each, including anything under "also", "while here", "follow-up in
   this PR", or "Changes" sub-bullets. Mark each line "fixes or tests
   the reported bug" or "extra". Deferred or out-of-scope items are not
   changes.
3. **executable**: quote every file path, function, or code site the
   plan names as a change location. Quote any hedge words about where
   or how ("somewhere", "maybe", "not sure", "or", "whichever",
   "investigate", "profile").
4. **decisive-test**: quote the test plan. Write down (a) what will be
   run and (b) what result is expected after the fix. Put the repro's
   Actual line next to (b).
5. **thread-and-policy**: put each maintainer direction from step 3 of
   the read order next to the comment sentence that answers it, or
   "no answer". Put the AI-policy requirement next to the comment's
   disclosure sentence, or "no disclosure". Eval mode: treat every
   package as AI-assisted.
6. **risks-named**: quote any risks, unknowns, or side effects the
   plan names.

## Check execution

1. Run the checks in table order: grounded-cause, bounded-scope,
   executable, decisive-test, thread-and-policy, risks-named. Grade
   every check, even after one fails; the student needs all the
   feedback.
2. Grade each check from its gathered evidence alone, using the pass
   condition in `rubric.md` word for word. Do not re-read the whole
   package unless the gathered evidence is missing a quote the pass
   condition needs; then go back to that one part only.
3. Grade `pass` or `fail` whenever the package text settles the
   condition. Use `unclear` only when the condition depends on a fact
   the package does not contain and cannot be inferred from it (for
   example, the repro never touches the component the plan blames).
   Do not use `unclear` because a plan is short; a terse plan that
   meets the condition passes.
4. Evidence that is absent from the plan is a fail, not unclear, for
   the plan's own parts: no stated cause fails grounded-cause, no named
   location fails executable, no test plan fails decisive-test.
5. Specific rules so two graders agree:
   - grounded-cause: one repro step the cause cannot explain is enough
     to fail. A cause taken from the thread passes only if the repro
     supports it; a maintainer's role does not make it true, and
     agreeing with a confident thread comment is not grounding.
   - bounded-scope: one "extra" line is enough to fail, however good
     the core fix is. A regression test for the bug is never extra.
   - executable: one named file or code site plus one chosen approach
     is enough to pass; length and headings do not matter.
   - decisive-test: (b) must be something you could observe and that
     differs from the repro's Actual. "Run the full suite" alone fails.
   - thread-and-policy: if there is no maintainer direction and no
     disclosure requirement, pass. Silence on a direction fails, even
     if the plan itself happens to agree with it.
6. For each check, write one line of evidence: the quote or fact that
   decided the grade.

## Verdict assembly

1. Take the grades of the required checks (all except risks-named).
2. Change every `unclear` on a required check to `fail`.
3. If every required check is `pass`, the verdict is `accept`.
   Otherwise it is `reject`.
4. Ignore risks-named for the verdict; report its grade only.
5. In the summary above the JSON, list each check with its grade and
   evidence line. For a reject, name the first failing required check
   in table order as the deciding check and quote the evidence that
   failed it.
6. In live mode, add notes on any voice-guide rules the draft comment
   breaks, and any gaps in this procedure you met. Neither changes the
   verdict.
7. End with the JSON block from `SKILL.md`, one entry per check in
   table order, and nothing after it.
