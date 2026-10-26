# Evidence guide: where proof lives in a reproduction package

## Environment

- Where it lives: in an eval bundle, the first lines of the candidate repro report (often an "Environment:" line), compared with the issue body (the version, platform, or branch the reporter or maintainers confirmed) and with the repo-facts "bug reports: template asks for…" line. In live mode, the top of the student's repro draft, compared with the issue body and the repo's issue template and README setup docs.
- What good looks like: the OS and its version, the exact version or commit of the software, and every setting the template or issue names as relevant (driver, build profile, shell, locale, config). If the tested version differs from what the issue targets, the report says so in words.

## Steps

- Where it lives: the numbered or listed steps in the repro report, including any quoted config files or inputs. In live mode, the draft's steps plus the repo's own setup docs (README, CONTRIBUTING) that the steps rely on.
- What good looks like: someone with only the package and public sources could re-run it from a clean start to the trigger. Commands are copy-pasteable, inputs are quoted inline, and nothing depends on a private repo, an unshared file, or "set up the project".

## Behavior shown

- Where it lives: the fenced output blocks, log excerpts, exit codes, and screenshots inside the repro report, plus any "Expected / Actual" lines. Read them against the issue body's exact error message, exit code, or symptom, and against the trigger (input, syntax, flag) the issue uses.
- What good looks like: the artifact itself shows the issue's behavior: the same error type, the same wrong output, the same crash vs. graceful failure, produced by the same trigger. A control run (the same steps with the trigger removed) is strong support. An artifact that only shows the tool starting, or shows a different error than the issue, is not proof, no matter what the narration claims.

## Honesty

- Where it lives: the claim comment's statements ("I reproduced…", "the cause is…", "guaranteed") and the report's Actual / conclusion lines, each matched to the artifact that backs it.
- What good looks like: every claim points at something shown. "Reproduced" means an artifact shows it; a root cause is labelled a hypothesis unless something demonstrates it; scope is not widened to versions or platforms that were not tested. An honest cannot-reproduce shows the attempt's output and names what differed and what might be needed to trigger it. That is a pass.

## Comms

- Where it lives: the candidate claim comment (read against the issue title and body), and both comments read against the repo-facts "contribution policy" line (CONTRIBUTING.md / AI_POLICY.md) and the bug-report template asks. In live mode, the student's drafts read against the live issue, the repo's CONTRIBUTING / AI policy files, and the `scope.md` house rules.
- What good looks like: the claim mentions this issue's specifics (symptom, file, component) and promises only investigation or a report, never a fix date or guarantee. If the repo's policy requires AI-use disclosure for comments or "any form" of contribution, both comments say which AI tool was used and how. A policy that only asks for disclosure in PRs, or only asks for comments in the contributor's own words, does not require a disclosure line in comments.
