# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Grounded Diagnosis | The plan's stated cause read against every step, control run, and artifact in the repro-evidence block (live: the student's posted repro comment as quoted in the drafts) | The stated cause is consistent with all of the repro evidence. Fail if any control run or step in the repro evidence rules the stated cause out (e.g. the same input works without the blamed component, or the bad state already exists before the blamed step runs), or if the plan blames a different place than where the evidence localizes the behavior. | required |
| Bounded Scope | The plan's in-scope / not-in-scope statements and the list of changes, read against what the issue asks for | The plan makes the change the issue needs and nothing the issue didn't ask for. Explicitly deferring harder variants or follow-ups is fine. Fail if the plan bundles extra work into the fix: refactors, dependency migrations, new options or settings, redesigns or rewrites, or "while I'm in the area" changes. | required |
| Executable Approach | The plan's files/locations and approach | A stranger could start the work without asking the author: the plan names the file(s) or function(s) to change and commits to one approach. Fail if key decisions are left open ("somewhere", "not sure which layer", "upstream or vendored, whichever is easier", "investigate and see"). | required |
| Decisive Test Plan | The plan's test plan read against the repro evidence's steps and artifacts | The test plan names an observable outcome that would show this fix worked: the repro steps re-run with the expected new output/exit code, or a specific regression test with its expected result. Fail if the only test is "run the full suite", "nothing else should break", or a feeling ("should feel faster"). | required |
| Thread and Conventions | The plan comment read against the thread highlights (maintainer direction, prior-art PRs) and the repo-facts contribution policy. Treat every package as AI-assisted work. | The comment engages any explicit maintainer direction in the thread (follows it, or says why not) and does not ignore an existing PR or patch the maintainers pointed to. If the repo's policy requires disclosing AI use in comments or "any form" of contribution, the comment states the tool and the extent of its use. Fail if the comment ignores explicit maintainer direction, or omits a disclosure the policy requires. | required |
| Honest Unknowns | The plan's risks / unknowns section read against its claims | Things the author has not verified are stated as unknowns or risks, not as certainties. | preferred |

## Verdict rule

- **accept** (ready to post and build from) only if every `required` check grades `pass`.
- Any `required` check graded `fail` or `unclear` (`?`) makes the verdict **reject** (hold). `unclear` counts as fail because a plan we cannot verify from the package is not ready to build from.
- `preferred` checks are reported but never change the verdict.
