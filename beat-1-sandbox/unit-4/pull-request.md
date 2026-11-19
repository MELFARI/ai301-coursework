# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/286

**Branch**

`fix/70-dedent-readme-fixture`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run 1: **16/20** (below the bar). Categories: clear-accept 3/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. All four misses were clear accepts (pkg-05, pkg-13, pkg-16, pkg-19), each with the note "failed: Decisive Test Evidence".
2. Partial run, `--only pkg-05,pkg-13,pkg-16,pkg-19 --out r1.json` (same files, run to read the per-check evidence): pkg-16 failed on "Test evidence states '`go test ./pkg/minikube/machine/... ./cmd/...` passes' with no output or count shown", and another failed because controls were reported in prose with no command quoted. The other two passed this time, which told me the check was right on the edge.
3. Partial run after loosening Decisive Test Evidence, `--only pkg-05,pkg-13,pkg-16,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14` (the four misses plus every not-tested package as canaries): **8/8**.
4. Full run 2 (saved with `--save-run eval-run.txt`): **20/20** (bar: 18/20: PASS). Categories: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3.

**Package analysis**

**pkg-16** (kubernetes/minikube#23471, clear-accept). Gold label: **accept**. My rubric: **reject** in full run 1, **accept** in full run 2.

The PR makes `minikube image load` fail loudly. Its test evidence re-runs the plan's repro and shows `X Failed to load image ./notanimage.tar: docker load: invalid tar header` and `echo $?` giving `1`, plus the valid-tarball control still giving `0`. The suite is only reported in one sentence: "`go test ./pkg/minikube/machine/... ./cmd/...` passes."

In run 1 my Decisive Test Evidence check required "the outcome of the repo's own test suite or checks is shown with real output or a named count", so it failed the package with "go test ... passes with no output or count shown". That was the rubric applied as written, but it was the wrong rule. The decisive proof in this package is the repro before/after, and that is fully shown. The suite line names the exact command and its result, which is different from a bare "tests pass". The plan also openly defers the start-path behavior, and the description restates that deferral, so it is an honest shortfall rather than drift. After I changed the check to accept a named suite command with its outcome, the rubric accepted it, matching the gold label.

**Check rationale**

From `tools/pr-precheck/rubric.md`:

| Decisive Test Evidence | The test-evidence section (and any before/after in the description), read against the plan's test plan and the repro's failing case | The evidence re-runs the plan's repro (the failing case that motivated the fix, not a control or an unchanged path) and shows its observable result before and after the change (pasted output, or a stated observed outcome such as an exit code, printed error, or rendered result), with the after matching the plan's expected result; every distinct failure mode the test plan names gets its own after result; and the repo's suite or checks are named with their outcome (a sentence like "`go test ./pkg/...` passes" is enough). Controls and smoke checks may be reported in prose. Fail on "tested locally", "works now", "verified on my machine", "tests pass" with no command named, evidence that only runs a control or a path the fix does not touch, or a failure mode from the test plan left with no result. A check reported failing or unavailable with its reason still counts as shown. | required |

Why it reads this way: the first version said the suite outcome had to be "shown with real output or a named count" and that "every failure mode the test plan names is exercised" with its own output. That rejected four clear accepts (pkg-05, pkg-13, pkg-16, pkg-19), all of which re-run the real repro with before/after but report the suite or a control in a sentence. Reading the four not-tested rejects showed what actually separates them: pkg-04 and pkg-10 never re-run the plan's repro ("tested locally", "verified working on my machine"), and pkg-07 and pkg-14 only run the control or an unchanged path. So I kept the hard requirement on the failing repro and each distinct failure mode, and loosened only the parts that were catching good PRs: the suite can be a named command with its outcome, and controls can be reported in prose. I rejected dropping the suite requirement entirely, because then a bare "tests pass" (pkg-04's "cargo test passes" next to no repro) would read the same as a named command.

**Trade-offs**

Loosening Decisive Test Evidence risks letting through a PR whose suite claim is false, because a sentence like "`go test ./...` passes" can't be checked from the package the way pasted output can. To make sure the loosening didn't flip any reject, I re-ran all four not-tested packages (pkg-04, pkg-07, pkg-10, pkg-14) as canaries with `--only`. All four still rejected, and the confirming full run kept not-tested at 4/4. The other categories didn't move (silent-drift 4/4, unreviewable 3/3, standards-wall 2/2 in both full runs), because the change only touched the test-evidence check, its procedure steps, and its evidence-guide section. The case I accept it will miss is a PR that re-runs the right repro but lies about its suite; this check trusts a named suite result.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
