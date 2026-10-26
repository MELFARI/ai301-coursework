# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer Responsiveness | Maintainer comments in issue thread or commit history in default-branch repo facts | At least 1 maintainer comment or commit exists within the last 90 days | required |
| Repository Activity | Default-branch commit timestamps and archived flag in repo facts | The repo is not archived and the default branch has at least 1 commit within the last 180 days | required |
| Issue Availability | Issue assignees list, PR links in comment thread, and user claim comments | The issue has 0 assignees, no linked open PRs, and no user comment expressing active work within the last 14 days | required |
| Policy Compliance | Contribution policy line in repo facts (CONTRIBUTING.md / AI policy), issue labels, and comments | Fail if the repo's contribution policy refuses AI-generated or AI-assisted code or documentation outright (e.g. "we do not accept AI-generated code"). A policy that allows AI with human review, or that only discourages it, passes. Also fail if the issue requires an approved design/RFC first, is labeled `blocked` or `needs design`, or requires maintainer assignment before a PR. | required |
| Single Concrete Change | Issue title and body | Fail if the issue is a tracking list, umbrella, or "megaissue" that collects many separate tasks or links (e.g. a bulleted list of other issue numbers), rather than one change a single PR would close. | required |
| Settled Spec | Issue body, labels, author association, maintainer comments, and linked PR states | Pass when the expected behavior is stated and the project has endorsed it (filed by a maintainer/collaborator, a maintainer comment accepting it, or a triage label such as `good first issue` / `help wanted` / `bug`). Fail if: (a) it is an unlabeled feature request no maintainer has responded to, or the issue leaves a design input undecided ("TBD", "maybe we could…"); or (b) 2 or more linked PRs were closed without merging, which shows earlier attempts stalled on an unsettled design. | required |
| Bounded Effort | Issue body text | The request is a manageable fix, feature, or docs update. (Pass unless it explicitly asks for a massive architectural overhaul, codebase-wide sweep, or multi-system rewrite). | required |

## Verdict rule

An issue is **accepted** if and only if every `required` check receives a grade of `Pass`.

- Any `required` check receiving a `Fail` or `unclear` results in an immediate **reject**.
- `preferred` checks or secondary notes never override a `Fail` on any required check.
