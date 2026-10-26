# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

---

## Your identity upstream

**GitHub username**

MELFARI

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70#issuecomment-6410213901

Hi! I'd like to work on this as my first Path Review issue. I'll look into why `test_parse_standard_readme` gets `heading_count == 0` from the `sample_readme_text` fixture in `tests/conftest.py`. Next I'll reproduce it locally on the current `main` and post what I find here, including whether the parser or the fixture is the cause.

(AI disclosure: I'm using Claude Code to help me run the tests and draft my comments; I check the steps and output myself.)

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70#issuecomment-6410214128

**Reproduced.** The `sample_readme_text` fixture is indented, and `ReadmeParser` finds 0 headings in it, so `test_parse_standard_readme` fails its `heading_count > 0` assertion.

**Environment:** Windows 11 Pro (10.0.26200), Git Bash, Python 3.11.9 in a `.venv`, deps installed with `pip install -e ".[dev]"` (the Python part of `make setup`; Docker/DB not needed for unit tests). Repo at `main` commit `2f4e82f`.

**Steps:**

1. Fork + clone, then `py -3.11 -m venv .venv` and `.venv/Scripts/pip install -e ".[dev]"`
2. Run the test normally (it's marked xfail for H-03):

```
$ .venv/Scripts/python -m pytest tests/unit/test_readme_parser.py -k test_parse_standard_readme -rxX
XFAIL tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme - issue #70 (manifest H-03): README parser fixture is indented, so it has no headings
1 xfailed
```

3. Run it with the xfail ignored to see the real failure:

```
$ .venv/Scripts/python -m pytest "tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme" --runxfail
>       assert result.metadata["heading_count"] > 0
E       assert 0 > 0
1 failed
```

4. Control: same parser, same short README text, indented vs. dedented with `textwrap.dedent`:

```
$ .venv/Scripts/python -c "
import textwrap
from ingestion.parsers.readme_parser import ReadmeParser
t = '''
    # Weather App
    Some text.

    ## Features
'''
p = ReadmeParser()
print('indented heading_count =', p.parse(t).metadata['heading_count'])
print('dedented heading_count =', p.parse(textwrap.dedent(t)).metadata['heading_count'])
"
indented heading_count = 0
dedented heading_count = 2
```

The fixture in `tests/conftest.py` (line 28 on) starts every line with 4 spaces, e.g. `    # Weather App`.

**Expected:** the standard README fixture has headings, so `heading_count > 0`.
**Actual:** `heading_count` is 0 for the indented fixture; the same text dedented gives 2.

This matches the issue: the parser handles headings fine once they start at column 0, so the fixture is what needs fixing (plus removing the H-03 xfail marker). Next I'll try dedenting the fixture in my fork and report back here with the test result.

(AI disclosure: I used Claude Code to help run these commands and draft this report; I checked the steps and output myself.)

## Eval iterations

**Run history**

1. First full run attempt: crashed before grading anything (`FileNotFoundError: [WinError 2] The system cannot find the file specified`, because the harness could not find `claude` on Windows). No score. I changed the harness to resolve the CLI path with `shutil.which("claude")`, keeping the pinned `--model sonnet`.
2. Full run with `--save-run eval-run.txt`: `agreement: 18/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 6/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. This is the committed `eval-run.txt`.

**Package analysis**

**pkg-03** (BurntSushi/ripgrep#2779). Gold: `accept`. My rubric: `reject` (`failed: Faithful Target`).

The issue was reported with "Version: ripgrep 13.0.0. OS: Kubuntu 23.10, installed via APT." The candidate report ran "ripgrep 15.2.0 (cargo install), Arch Linux (x86_64)" and says "The issue was filed against 13.0.0; behavior is unchanged on 15.2.0." So the version difference *is* stated. But my Faithful Target pass condition says "any difference (older/newer release, different platform) is explicitly stated in the report", and the report only calls out the version, not the different OS and install method (Arch + cargo vs. Kubuntu + APT). The grader read the unstated platform change as a silent deviation and failed it. The gold note says the "version delta acknowledged" is enough here, because a ripgrep search bug does not depend on the distro. My check is stricter than it needs to be: it should only require calling out platform differences when the issue says the platform matters.

**Check rationale**

Quoted from `tools/repro-check/rubric.md`:

> | AI Disclosure | The contribution policy line in the repo-facts block (live: the repo's CONTRIBUTING / AI policy), read against the claim comment and repro report. Treat every package as AI-assisted work. | If the policy requires disclosing AI use in comments, issues, or "any form" of contribution, the comments must state the AI tool and the extent of its use. Pass when the policy has no AI rule, only asks for disclosure in pull requests, or only asks for comments in the contributor's own words. | required |

Why it reads this way: the assignment says the set has exactly one disclosure-wall package, and without a conventions check that category can't be matched. A simple "fail if the repo has an AI policy and the comment doesn't disclose" would be wrong, because several repos have AI policies that don't ask for disclosure in comments: pkg-09's policy says AI use must be stated "in the pull request" and "states no disclosure ask for issue comments", and pkg-03's only says comments "must be written by humans in their own words". Both are gold accepts. pkg-20's ghostty policy is different: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". So I wrote the trigger around *where* the policy asks for disclosure (comments, issues, or "any form"), and listed the PR-only and own-words cases as explicit passes. "Treat every package as AI-assisted work" is there because the course work is AI-assisted, so the grader shouldn't excuse a missing disclosure by assuming no AI was used.

**Trade-offs**

The run shows it worked without collateral damage: `disclosure 1/1` (pkg-20 rejected) and pkg-07, pkg-09 and pkg-12 (all repos with AI policies) still agreed as `accept`, so the check didn't fire on policies that don't ask for comment disclosure. What it gives up: it only reads the policy text as written, so a repo that requires disclosure somewhere the package doesn't capture (e.g. a PR template, or a rule stated only in a maintainer's comment) would pass. I accept that miss because the grader can only judge the policy it is shown. It is also a strict, required check: a package that discloses vaguely ("I used AI") would still fail in a disclosure-required repo because it doesn't name the tool and extent. That matches the ghostty policy's wording, so I kept it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
