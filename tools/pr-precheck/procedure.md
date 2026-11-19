# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` first and confirm the issue and the working copy are in the scoped repo (SKILL.md's scope seam). Eval mode skips this step.
2. Read the repo facts (eval: the repo-facts block; live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and scope.md's house rules). Write down every ask stated as required (template sections, checklist lines like "closes #", files a fix must include such as a whatsnew or changelog entry) and the AI policy word for word, or "no stated AI policy".
3. Read the issue and thread highlights. Write down the behavior reported and any explicit maintainer direction.
4. Read the plan before any part of the PR. Write down: the files it names, every change it commits to, its not-in-scope list, any deferral or deviation note, and every item of its test plan with its expected result. This list is the yardstick for the PR, so build it before reading the PR's own account of itself.
5. Read the diff next, before the description: list every changed file and, for each hunk, one line saying what it changes. Then read the commit list.
6. Read the test evidence.
7. Read the title and description last, because they are graded against everything above. Note every claim about what the PR does or does not change.

## Evidence gathering

1. **Plan Fidelity:** make a two-column table. Left: each hunk from Read order step 5. Right: the plan item or deviation note that covers it, or "not in plan". Then go the other way: for each change the plan commits to, mark "in diff", "deferred in plan and description", or "missing". Finally list each description claim about the diff and mark it "true", "false", or "not checkable".
2. **Decisive Test Evidence:** first find the plan's failing repro (and every other distinct failure mode its test plan names) and quote its before and after result, or write "not shown". Note whether that run is the failing case or only a control or unchanged path. Then, next to each remaining test-plan item (controls, smoke checks, the suite), write what the evidence says about it, even a one-line prose report. Note whether the repo's suite or checks are named with an outcome ("`go test ./...` passes", a count) or only claimed with nothing named ("tests pass").
3. **Clean Diff:** scan every added and removed line and list each debris line with its file: debug output, commented-out code, dead or unused functions, TODO/FIXME, whitespace or re-indent-only hunks, import reshuffles, edits unrelated to the fix.
4. **Repo Standards:** for each required ask from Read order step 2, quote where the PR meets it (section heading, checklist line, file in the diff) or write "missing". If an AI policy requires disclosure (or in live mode, always), quote the description's disclosure sentence or write "none".
5. **Maintainer Direction:** quote the direction from Read order step 3 and the diff or description line that follows or answers it, or "no direction in thread".

## Check execution

1. Run the checks in rubric order: Plan Fidelity, Decisive Test Evidence, Clean Diff, Repo Standards, Maintainer Direction.
2. Grade each check only from the evidence gathered for it, applying the rubric's pass condition word for word. Give `pass`, `fail`, or `unclear` with one quoted line as the reason.
3. Plan Fidelity fails on any "not in plan" functional hunk, any "missing" plan item, or any "false" description claim. A deferral recorded in the plan and restated in the description is not missing.
4. Decisive Test Evidence fails if the failing repro or any distinct failure mode the plan names is "not shown", if the only run is a control or an unchanged path, or if the suite is claimed with no command named. A control or suite reported in a prose sentence passes; do not fail a check for missing pasted output when the observed result is stated.
5. Clean Diff fails if the debris list has any entry. Tests the plan names are not debris.
6. Repo Standards fails if any required ask is "missing" or a required disclosure is "none".
7. If the evidence for a check is genuinely absent from the package, grade `unclear`. If it exists and you did not find it, go back to Evidence gathering instead.
8. Run every check even after one fails, so the output lists every problem. Do not re-read the whole package for a check whose evidence list is complete.

## Verdict assembly

1. Apply the rubric's verdict rule: `accept` only if every `required` check is `pass`; any `required` `fail` or `unclear` gives `reject`.
2. The preferred check (Maintainer Direction) is reported but never changes the verdict.
3. For a reject, the deciding check is the first failing or unclear required check in rubric order. Name it in the summary and quote its evidence line.
4. Live mode: after the per-check lines, list any voice-guide rule the title or description breaks, quoting the rule. This never changes the verdict.
5. Output the JSON block in SKILL.md's format, with one entry per rubric check in rubric order, and nothing after it.
