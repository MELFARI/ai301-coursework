# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer Responsiveness | Maintainer comments in issue thread or commit history in default-branch repo facts | At least 1 maintainer comment or commit exists within the last 90 days | required |
| Repository Activity | Default-branch commit timestamps in repo facts | The default branch has at least 1 commit within the last 180 days | required |
| Issue Availability | Issue assignees list, PR links in comment thread, and user claim comments | The issue has 0 assignees, no linked open PRs, and no user comment expressing active work within the last 14 days | required |
| Policy Compliance | Issue body text, comments, and labels | The issue does not violate explicit project policies. Fail if the issue requires an approved design/RFC first, is marked `blocked` or `needs design`, or explicitly mandates maintainer assignment before a PR can be opened. | required |
| Scope Clarity | Issue body text | The problem statement or requested feature is clearly stated. (Pass as long as it's not a completely vague issue or an open-ended research question). | required |
| Bounded Effort | Issue body text | The request is a manageable fix, feature, or docs update. (Pass unless it explicitly asks for a massive architectural overhaul or multi-system rewrite). | required |

## Verdict rule

An issue is **accepted** if and only if every `required` check receives a grade of `Pass`. 

- Any `required` check receiving a `Fail` or `unclear` results in an immediate **reject**.
- `preferred` checks or secondary notes never override a `Fail` on any required check.