# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

MELFARI

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70#issuecomment-7000029136

**Plan for #70**

Following my repro above (`assert 0 > 0` on `test_parse_standard_readme`; same parser gives `heading_count = 0` indented vs. `2` dedented), the cause is the fixture, not the parser: `ReadmeParser` only matches headings that start with `#` at the beginning of a line, and every line of `sample_readme_text` starts with 4 spaces.

What I plan to change:
- `tests/conftest.py`: wrap the `sample_readme_text` string in `textwrap.dedent(...)` so the headings start at column 0 (same content).
- `tests/unit/test_readme_parser.py`: remove the H-03 `xfail` marker from `test_parse_standard_readme`, as the issue asks.

Not changing: the parser itself, #71's heading-hierarchy fixture, or the other fixtures in `conftest.py`. `sample_readme_text` is only used by this one test.

How I'll check it: re-run my repro command with `--runxfail` and expect `1 passed` instead of `assert 0 > 0`, then run `tests/unit/test_readme_parser.py` normally and expect `PASSED` with no `XFAIL`/`XPASS(strict)`, then run the full unit suite to confirm nothing else changes.

One thing I'll double-check: `dedent` keeps the leading blank line from the `"""`, which I don't expect to matter. I'll report back here if the build differs from this plan.

(AI disclosure: I'm using Claude Code to help me read the code, draft this plan, and run the tests; I check the diffs and output myself.)

---

## Your branch

**Branch**

`fix/70-dedent-readme-fixture` (in https://github.com/MELFARI/pathreview-ai301-fa26-s3, commit `2b3e259`)

**Evidence**

Environment: Windows 11 Pro (10.0.26200), Git Bash, Python 3.11.9 in `.venv` with `pip install -e ".[dev]"`. Before = `main` at `2f4e82f`; after = branch `fix/70-dedent-readme-fixture` at `2b3e259`.

Before the fix (on `main`):

```
$ .venv/Scripts/python -m pytest "tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme" --runxfail
>       assert result.metadata["heading_count"] > 0
E       assert 0 > 0
============================== 1 failed in 0.43s ==============================

$ .venv/Scripts/python -m pytest tests/unit/test_readme_parser.py -v -rxX
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme XFAIL [  6%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_longer_than_10000_chars PASSED [ 13%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_with_code_blocks PASSED [ 20%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_without_code_blocks PASSED [ 26%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_empty_readme PASSED [ 33%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_with_only_whitespace PASSED [ 40%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_bytes_input PASSED [ 46%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_bytes_with_utf8 PASSED [ 53%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_invalid_content_type PASSED [ 60%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_extract_heading_hierarchy XFAIL [ 66%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_extract_heading_hierarchy_no_headings PASSED [ 73%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_with_badges PASSED [ 80%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_without_badges PASSED [ 86%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_metadata_structure PASSED [ 93%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_word_count_accuracy PASSED [100%]
XFAIL tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme - issue #70 (manifest H-03): README parser fixture is indented, so it has no headings
XFAIL tests/unit/test_readme_parser.py::TestReadmeParser::test_extract_heading_hierarchy - issue #71 (manifest H-04): heading hierarchy fixture is indented, so it has no headings
======================== 13 passed, 2 xfailed in 0.23s ========================

$ .venv/Scripts/python -m pytest tests/unit -m unit -q
375 passed, 53 xfailed, 3 warnings in 16.37s
```

After the fix (on `fix/70-dedent-readme-fixture`):

```
$ .venv/Scripts/python -m pytest "tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme" --runxfail
============================== 1 passed in 0.18s ==============================

$ .venv/Scripts/python -m pytest tests/unit/test_readme_parser.py -v -rxX
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme PASSED [  6%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_longer_than_10000_chars PASSED [ 13%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_with_code_blocks PASSED [ 20%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_without_code_blocks PASSED [ 26%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_empty_readme PASSED [ 33%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_with_only_whitespace PASSED [ 40%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_bytes_input PASSED [ 46%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_bytes_with_utf8 PASSED [ 53%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_invalid_content_type PASSED [ 60%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_extract_heading_hierarchy XFAIL [ 66%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_extract_heading_hierarchy_no_headings PASSED [ 73%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_with_badges PASSED [ 80%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_readme_without_badges PASSED [ 86%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_metadata_structure PASSED [ 93%]
tests/unit/test_readme_parser.py::TestReadmeParser::test_word_count_accuracy PASSED [100%]
XFAIL tests/unit/test_readme_parser.py::TestReadmeParser::test_extract_heading_hierarchy - issue #71 (manifest H-04): heading hierarchy fixture is indented, so it has no headings
======================== 14 passed, 1 xfailed in 0.24s ========================

$ .venv/Scripts/python -m pytest tests/unit -m unit -q
376 passed, 52 xfailed, 2 warnings in 9.53s
```

The repro command now passes (`assert 0 > 0` → `1 passed`), `test_parse_standard_readme` runs normally as `PASSED` with no `XFAIL`/`XPASS(strict)`, and across the whole unit suite the only change is that one test moving from xfailed to passed (375 → 376 passed, 53 → 52 xfailed). #71's H-04 test is still xfailed, since it is out of scope.

## Eval iterations

**Run history**

1. First full run with `--save-run eval-run.txt`: `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. The only disagreement was `pkg-14  clear-accept  accept  reject   NO     failed: Executable Approach, Honest Unknowns`. This is the committed `eval-run.txt`; I did not run again.

(Before running, I made the same Windows fix to `run_eval.py` as in Unit 2 so it can find the `claude` CLI: `shutil.which("claude")`, with the pinned `--model sonnet` unchanged.)

**Package analysis**

**pkg-01** (httpie/cli#1838). Gold: `reject` (wrong-cause). My rubric: `reject`, so they agree.

The plan's diagnosis says: "The `REQUEST_ITEM` tokenizer in `httpie/cli/requestitems.py` is the problem. Its separator regex fails to recognize `header1:xyz` and `x=1` as valid items when an optional flag appears earlier in the argument list". But the package's own repro evidence rules that out. Step 2 is a "Control run, same venv, same items, no `-v` flag", and it parses fine (`header1: xyz` and `{"x": "1"}` in the output), so the tokenizer handles those exact items. Step 4 says "`--debug` on the failing run shows the error is raised by argparse's `parse_args` while consuming positionals; the request items are never handed to HTTPie's item parser." The tokenizer never even runs on the failing input. My Grounded Diagnosis check fails a plan "if any control run or step in the repro evidence rules the stated cause out (e.g. the same input works without the blamed component...)", and my procedure tells the grader to list what each repro step and control shows *before* reading the plan. So the grader had "control: same items parse fine without `-v`" and "error raised in argparse" written down before it saw the tokenizer claim, and the claim contradicted both. A confident, detailed plan doesn't get a pass when its own control run disproves it.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

> | Executable Approach | The plan's files/locations and approach | A stranger could start the work without asking the author: the plan names the file(s) or function(s) to change and commits to one approach. Fail if key decisions are left open ("somewhere", "not sure which layer", "upstream or vendored, whichever is easier", "investigate and see"). | required |

Why it reads this way: the unbuildable packages (pkg-10, pkg-17, pkg-18) don't fail because they're short. They fail because the real decision is never made. pkg-17 says "gocui? tcell? not sure", and pkg-18 says to fix it "upstream or vendored, whichever is easier". So instead of a structure check like "the plan has a Files section", I wrote the pass condition around whether "a stranger could start the work without asking the author". The two concrete requirements are that the plan names where (file or function) and commits to one approach. I put the actual hedge phrases from those packages in the fail condition, so the grader can recognize them. A "files listed" check would have been easy to satisfy with vague entries, and it would have failed terse plans like pkg-02, which are short but decisive.

**Trade-offs**

This check cost me pkg-14 (zellij-org/zellij#5174). Gold is `accept`, but my run graded it `reject` with `failed: Executable Approach, Honest Unknowns`. The plan has a clear approach (drain pending OSC query responses before wiring pane input on reattach) and names the area ("the client attach/reattach path in `zellij-server` (session connection handling) and `zellij-client`'s terminal query issuance"). But it also says "exact functions to be pinned in the PR after tracing the query issuance with debug logs". My check asks for named "file(s) or function(s)" and lists "investigate and see" as a fail, so the grader read "to be pinned after tracing" as an open decision. The gold label treats it as ready because the approach is chosen and only the precise function is left to trace. I accept this miss for now: loosening the check to allow "area named, function to be found" risks letting pkg-17 and pkg-18 through, since they also name areas loosely. If I revised it, I'd pass a plan that names the component and a concrete method to locate the function (like the debug-log trace here), and re-run `--only pkg-14,pkg-17,pkg-18` with pkg-17/18 as canaries for the unbuildable category. Honest Unknowns also showed up in the note, but it's `preferred` and never changes the verdict.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
