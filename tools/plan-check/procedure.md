# Procedure: how this skill grades a plan package

## Read order

1. Read the issue context first (title, body, thread highlights). Write down: the behavior the issue reports, and any explicit maintainer direction in the thread (a chosen approach, a file or function they isolated, a patch or PR they pointed to, something they asked contributors to do or not do).
2. Read the repo-facts block next. Write down the contribution policy's AI rule word for word, if there is one, and any contributing asks.
3. Read the repro evidence before the plan. For each step and control run, write down what it shows: which input fails, which variant works, and where the behavior first appears. This list is what the diagnosis will be checked against, so build it before seeing the plan's opinion.
4. Read the candidate plan: stated cause, scope / not-in-scope, files, approach, test plan, risks and unknowns.
5. Read the candidate plan comment last, since it is graded against everything above.
6. Live mode: before step 1, read `scope.md` and confirm the issue is in the scoped repo. The "repro evidence" is the student's posted repro comment on the issue as quoted in the drafts.

## Evidence gathering

1. **Grounded Diagnosis:** quote the plan's cause sentence. Next to it, list every repro step or control from Read order step 3 that bears on that cause.
2. **Bounded Scope:** list every change the plan says it will make, and the issue's ask in one line. Mark each change "needed by the issue" or "extra". Note any explicit deferral ("not in scope", "follow-up").
3. **Executable Approach:** list the files, functions, or locations named, and quote the approach sentence. Copy any hedge words ("somewhere", "maybe", "not sure", "whichever", "investigate").
4. **Decisive Test Plan:** quote the test plan. Write down what observable result it expects (an output, exit code, or assertion), or "none named".
5. **Thread and Conventions:** from your Read order notes, take the maintainer direction and the AI rule. Quote the plan comment's sentence that responds to the direction (or "none"), and any AI disclosure sentence (or "none").
6. **Honest Unknowns:** quote the risks / unknowns section (or "none").

## Check execution

1. Run the checks in rubric order: Grounded Diagnosis, Bounded Scope, Executable Approach, Decisive Test Plan, Thread and Conventions, Honest Unknowns.
2. Grade each check only from the evidence gathered for it, using the rubric's pass condition word for word. Give `pass`, `fail`, or `unclear`, with one quoted line as the reason.
3. Grounded Diagnosis fails if even one repro step or control from your list contradicts the stated cause. A confident or detailed write-up does not outweigh a control run.
4. Bounded Scope fails if any change is marked "extra". Deferring work is never extra.
5. If the evidence for a check is genuinely missing from the package (for example, there is no test plan at all), grade `unclear`, not `pass`. If it exists but you did not find it, go back to Evidence gathering; do not grade `unclear` for something you did not look for.
6. Keep running every check even after one fails, so the output lists all problems.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every `required` check is `pass`; any `required` `fail` or `unclear` gives `reject`.
2. `preferred` checks (Honest Unknowns) are listed in the output but never change the verdict.
3. In the summary, name the deciding check(s) for a reject and quote the evidence line that decided it.
4. Output the JSON block in the SKILL.md format, with one entry per rubric check in rubric order, and nothing after it.
5. Live mode: after the verdict, list any voice-guide rule the comment breaks, quoting the rule. This never changes the verdict.
