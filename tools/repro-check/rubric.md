# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment Recorded | The repro report's environment record, read against the issue's stated target and the repo-facts bug-report template asks | The report names the OS/platform and the exact version (or commit) of the software it ran, plus any setting the issue or template says changes the behavior (e.g. driver, build profile, shell, language). Fail if there is no environment record, or a setting the issue says matters is missing. | required |
| Faithful Target | Versions and settings in the environment record, read against the version/branch the issue reports or confirms the bug on | The version tested matches the issue's target, or any difference (older/newer release, different platform) is explicitly stated in the report. Fail if the report silently tests a different version or platform than the issue targets. | required |
| Steps Rerunnable | The repro report's steps, read from starting state to trigger | A stranger could re-run the attempt using only what the package shows: exact commands or inputs, and any file/config they need is quoted or publicly available. Fail if a step depends on a private repo, an unshared config, or a vague instruction ("set up the project", "run it a few times") that hides the trigger. | required |
| Behavior Shown | The report's artifacts (output excerpts, logs, exit codes, screenshots) read against the exact error, output, or symptom the issue describes, and the trigger the issue names | The artifact shows the issue's own behavior, produced by the issue's own trigger (same input/syntax/expression, same error type or symptom). For a cannot-reproduce, the artifact shows the real attempt's output. Fail if there is no artifact, if the artifact shows only that the program runs, or if it shows a different error/symptom (e.g. a graceful validation or syntax error where the issue reports a crash) even if the narration says it matches. | required |
| Honest Outcome | Every claim in the claim comment and the report ("reproduced", root cause, "guaranteed", "on all versions"), read against the artifacts that back it | Every claim is backed by something shown; the report says exactly what happened. An evidenced cannot-reproduce passes when it names what differed. Fail if the report asserts a root cause, certainty, or broader scope (other releases, platforms) that no shown artifact supports. | required |
| Specific Honest Claim | The claim comment, read against the issue | The claim names something specific to this issue (its symptom, trigger, file, or component) and commits only to concrete next steps such as investigating, reproducing, or reporting back. Fail if it is interchangeable boilerplate ("assign me", "+1", "I can fix this") or promises a fix, a deadline, or a guaranteed outcome. | required |
| AI Disclosure | The contribution policy line in the repo-facts block (live: the repo's CONTRIBUTING / AI policy), read against the claim comment and repro report. Treat every package as AI-assisted work. | If the policy requires disclosing AI use in comments, issues, or "any form" of contribution, the comments must state the AI tool and the extent of its use. Pass when the policy has no AI rule, only asks for disclosure in pull requests, or only asks for comments in the contributor's own words. | required |

## Verdict rule

- **accept** (ready to post) only if every `required` check grades `pass`.
- Any `required` check graded `fail` or `unclear` makes the verdict **reject** (hold).
- In a claim-only live draft, checks marked `not yet applicable: claim-only draft` are left out of this rule, as SKILL.md describes; every other `unclear` still counts as fail.
