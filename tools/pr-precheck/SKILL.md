---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is this
pull request ready to submit?** A PR package is a candidate pull
request (its title, its description, its commit list, its diff, and its
test evidence), read against the accepted plan it claims to implement
(including any deviation notes recorded in that plan) and the issue the
plan belongs to, together with the repo's stated standards (PR
template, contributing guide, AI-use policy). Do not grade the issue,
the plan, or the code style on its own terms, and do not grade more
than one package per run. You never answer from gut feel: you answer by
executing `procedure.md`, which applies the checks in `rubric.md` to
evidence located with `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a markdown
file headed `# Eval package:`), you are in eval mode. Otherwise you are
in live mode.

**Live mode** grades the student's own PR before it is opened. Read
these inputs, and only these:

- **The plan:** `plan.md` in the current working directory, including
  its `## Deviations` section. A house-chain student reads the house
  plan and house repro pack they were given instead; the same checks
  grade the same things.
- **The diff:** the output of `git diff main...HEAD` (three dots), run
  from the working copy on the student's branch. Also run
  `git log --oneline main..HEAD` for the commit list and
  `git diff main...HEAD --stat` for the changed-file list. Only
  committed changes count; uncommitted edits and untracked files are
  not part of the PR.
- **The title and description:** `pr_draft.md` in the working
  directory. Its first line is the PR title; everything under it is the
  description.
- **The test evidence:** `test_evidence.md` in the working directory,
  plus any before/after or check output pasted into the description.
- **The issue:** the issue URL the student gives you. Read its body and
  thread on GitHub (via `gh`, the GitHub API with `curl`, or the web).
- **The repo's standards:** `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the working copy (read them from `main`,
  e.g. `git show main:docs/CONTRIBUTING.md`, if the branch changed
  them).

If `plan.md`, `pr_draft.md`, or `test_evidence.md` is missing, do not
invent its content: grade the checks that depend on it from what
exists, which usually means `unclear`.

**Eval mode** grades a frozen bundle. The bundle is the whole world:
every fact comes from the bundle text (its repo-facts block, issue and
thread highlights, plan context, and candidate PR sections). Do not
fetch anything, do not run git, and do not read any other file. Eval
mode always grades a complete package: every check in the rubric, and
the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else.

- If its `Repo:` line still carries the placeholder
  (`<ORG>/<PATH-REVIEW-REPO>` or anything in angle brackets), stop
  without grading and tell the student: "Fill the `Repo:` line in
  `scope.md` with your section's Path Review repo, then re-run."
- Confirm the issue URL and the working copy's upstream repo are the
  repo named on the `Repo:` line. If they are not, refuse to grade and
  say which repo is in scope. Never guess a scope.
- Apply the house rules in `scope.md` as evidence-reading rules: the PR
  must come from a `fix/<issue-number>-<slug>`-style branch on the
  student's fork, deliver one plan for one issue, and use the repo's
  PR template with every section filled, including the AI-use
  disclosure the course always asks for.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`. Hold the outgoing text (the
PR title and description in `pr_draft.md`) against each rule in it. In
the summary, after the per-check lines, list every rule the draft
breaks, quoting the rule's name and the offending sentence. If the
draft breaks none, say so in one line. The voice guide never changes
the verdict on its own: only a rubric check can do that, because voice
is the student's personal standard, not the repo's bar.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read all three components before grading anything:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It is the only source of checks; never
  add one at runtime.
- `references/evidence-guide.md` is the map: for each evidence family,
  where it lives in a bundle and in live mode, and what good looks
  like.
- `procedure.md` is the operating procedure. Execute it as written,
  step by step: its read order, its evidence gathering, its check
  execution, and its verdict assembly.

Where the procedure is silent on a step you need, do the smallest
thing the rubric's own wording requires and name the gap in the
summary ("procedure gap: ..."). Never improvise a new check or a new
rule around it.

If `rubric.md` has no checks in its table or no verdict rule, or
`procedure.md` has no steps under its headings, stop and say so: this
tool cannot grade without a rubric and a procedure, by design.

## Verdict and output

The verdict space is binary: `accept` means ready to submit (open the
PR), `reject` means hold. There is no third verdict and no score;
reservations go in the check evidence lines.

Before the JSON, print a short readable summary: one line per check
(name, grade, the deciding fact), the deciding check for a reject, and
in live mode the voice-guide notes. Then end your reply with the
fenced JSON block below, filled in, with one entry per rubric check in
rubric order. It must be valid JSON, it must be the last thing in your
reply, and nothing may follow it. In live mode `item` is the issue URL
(or the PR URL if one exists); in eval mode it is the bundle id.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it: a file in the diff, a line of the plan, a
  line of test output, a sentence of the description. "Looks fine" is
  not evidence.
- **Grade the thing, not the polish.** A terse PR that matches its plan
  and shows its proof is ready; a long, confident description can be
  hiding drift. Always read the diff itself against the plan, and the
  evidence itself against the test plan, never the description's word
  for it.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it passes. Note the tension in the
  summary; the fix belongs in the rubric.
- **The procedure decides how, not the run.** Follow `procedure.md`
  as written and report its gaps instead of inventing steps.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict
  rule directs; where it is silent, an unverifiable claim is a failing
  one. A PR you cannot verify from the package is not ready to submit.
- **Honest shortfalls are not failures.** A PR that does less than
  everything, but whose plan or deviation notes and description both
  say so plainly, can be ready. Hold a PR for what it hides, not for
  what it openly defers.
