# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives:
- Eval bundle: the `## Plan context` block (the plan's scope, its "Not in scope" or "deliberately deferred" notes, its `Files:` list, and any deviation note); the candidate PR's `### Diff` (each `--- a/` / `+++ b/` file and its hunks); and the `### Description` claims about what changed.
- Live: `plan.md` (Scope, Files I'll touch, Approach, and `## Deviations`); `git diff main...HEAD` and `git diff main...HEAD --stat`; the description in `pr_draft.md` (Summary and Changes).

What good looks like: every file in the diff is in the plan's files list or a deviation note, and every hunk does something the plan's approach names. Every change the plan commits to appears in the diff, or the plan's notes and the description both say it was left out. The description's claims ("exactly as planned", "no other changes", "docs updated") are each true of the diff. Drift runs both ways: more than the plan (a new flag, option, setting, rename, or a rewrite of a function the plan never touches) or less (a promised docs or test change absent while the description says it is there).

## Test evidence (harness category: not-tested)

Where it lives:
- Eval bundle: the candidate PR's `### Test evidence` section and any before/after in the `### Description`, read against the plan's `Test plan:` and the `Repro evidence` line in the plan context.
- Live: `test_evidence.md` (the repro re-run on main and on the branch, plus the output of `make test-unit`, `make test-integration`, `make lint`, `make typecheck`) and the Testing section of `pr_draft.md`, read against the Test plan in `plan.md`.

What good looks like: the plan's own failing repro is re-run, with its observable result before (fails) and after (matches the plan's expected result), as pasted output or a stated outcome (exit code, printed error, rendered result). Each distinct failure mode the test plan names has its own after result. The repo's suite or checks are named with their outcome ("`go test ./pkg/...` passes" is fine), or a failing or unavailable check is reported with its reason; controls and smoke checks can be a prose line. Not good: "tested locally", "works now", "verified on my machine", the plan's repro never re-run, a second failure mode left silent, or only the control case (the path that never broke) re-run.

## Diff quality (harness category: unreviewable)

Where it lives:
- Eval bundle: the candidate PR's `### Diff` added (`+`) and removed (`-`) lines, and the `### Commits` list.
- Live: `git diff main...HEAD` and `git log --oneline main..HEAD`.

What good looks like: every hunk is part of the fix or the plan's tests, and nothing else rides along. Debris tells: debug prints (`print`, `eprintln!`, `console.log`, `dbg!`) left in, commented-out code or earlier attempts, dead functions or `allow(dead_code)` stubs, TODO/FIXME on touched lines, hunks that only re-indent or reorder imports, and drive-by edits to unrelated code. Commit names like "wip" or "misc cleanups while debugging" point to where debris usually hides.

## Standards and comms (harness category: standards-wall)

Where it lives:
- Eval bundle: the `## Repo facts` block (`pull requests:` line for template and checklist asks; `contribution policy:` line for contributing asks and any AI policy), read against the `### Title`, `### Description`, and the diff (for a required changelog or whatsnew file). Explicit maintainer direction sits in `## Thread highlights`.
- Live: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with `Closes #`, Changes, Testing checkboxes, Screenshots / Demo, Notes for Reviewers), `docs/CONTRIBUTING.md` (branch naming, conventional commits, seeded-bug xfail removal), scope.md's house rules, and the issue thread on GitHub; read against `pr_draft.md` and the branch name.

What good looks like: every section the template requires is present with real content (not the template's placeholder comment); required lines such as `Closes #<n>` are filled; any file the repo says a fix needs (whatsnew, CHANGELOG) is in the diff; and where a policy requires AI disclosure, the description says which tool was used and how, in the author's own words. Not good: a required checklist missing entirely, a required entry absent from the diff, or no disclosure under a policy that demands one. Optional asks ("screenshots if appropriate") left out are fine.
