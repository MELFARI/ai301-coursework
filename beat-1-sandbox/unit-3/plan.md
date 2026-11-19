# Plan: issue #70 — README parser test fixture is indented, so it contains no headings

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70#issuecomment-6410214128

## Diagnosis

The `sample_readme_text` fixture in `tests/conftest.py` returns a triple-quoted string where every line starts with 4 spaces (`    # Weather App`, `    ## Features`, `    ## Tech Stack`). `ReadmeParser._extract_heading_hierarchy` in `ingestion/parsers/readme_parser.py` matches headings with `re.match(r"^(#{1,6})\s+(.+)$", line)`, so a line has to start with `#` to count. With 4 leading spaces, none of the fixture's lines match, so `heading_count` is 0 and `test_parse_standard_readme` fails `assert result.metadata["heading_count"] > 0`.

The parser is not the cause. My repro shows this directly:

```
$ .venv/Scripts/python -m pytest "tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme" --runxfail
>       assert result.metadata["heading_count"] > 0
E       assert 0 > 0
1 failed
```

and the control, same parser and same text, indented vs. dedented:

```
indented heading_count = 0
dedented heading_count = 2
```

So the same parser finds headings once they start at column 0; only the fixture's indentation is wrong. This matches the issue: "The parser isn't wrong; the fixture shouldn't be indented."

## Scope

In scope:
- Make `sample_readme_text` in `tests/conftest.py` return the README with no leading indentation.
- Remove the `@pytest.mark.xfail(strict=True, reason="issue #70 (manifest H-03) ...")` marker from `test_parse_standard_readme` in `tests/unit/test_readme_parser.py`, as the issue and `docs/CONTRIBUTING.md` ask.

Not in scope:
- No change to `ReadmeParser` or its heading regex (the control shows it works on unindented markdown).
- Not #71 (the heading-hierarchy fixture with the same kind of problem). That is a separate issue.
- Not the other indented fixtures in `conftest.py` (e.g. `sample_resume_text`). Nothing in #70 asks for them, and they may be covered by other issues (e.g. #54).

## Files I'll touch

- `tests/conftest.py`: the `sample_readme_text` fixture, plus `import textwrap` at the top.
- `tests/unit/test_readme_parser.py`: delete the xfail marker on `test_parse_standard_readme`.

## Approach

1. In `tests/conftest.py`, add `import textwrap` and change the fixture to `return textwrap.dedent("""...""")`, keeping the same README content. `textwrap.dedent` strips the common 4-space indent, so the headings start at column 0 while the source stays readable. `sample_readme_text` is only used by `test_parse_standard_readme` (checked with `grep -rn sample_readme_text`), so nothing else sees the change.
2. Delete the 4-line `@pytest.mark.xfail(...)` decorator above `test_parse_standard_readme`.
3. Run the test plan below.

## Test plan

1. Re-run my repro step 3 (the test with the marker ignored):
   `.venv/Scripts/python -m pytest "tests/unit/test_readme_parser.py::TestReadmeParser::test_parse_standard_readme" --runxfail`
   - Before (from my repro): `E       assert 0 > 0` / `1 failed`
   - Expected after: `1 passed`.
2. Run the test normally, with the marker removed:
   `.venv/Scripts/python -m pytest tests/unit/test_readme_parser.py -v`
   - Expected after: `test_parse_standard_readme PASSED` and no `XFAIL` or `XPASS(strict)` for H-03. All the other tests in the file still pass.
3. Run the whole unit suite to check nothing else uses the fixture in a way I missed:
   `.venv/Scripts/python -m pytest tests/unit -m unit -q`
   - Expected after: the same pass/xfail counts as before, except H-03 moves from xfailed to passed.

## Risks and unknowns

- `textwrap.dedent` keeps the leading blank line from the opening `"""`. I expect this doesn't matter (the parser skips non-heading lines, and `result.text == sample_readme_text` compares the same string), but I'll confirm it in test step 2.
- If I've missed a use of the fixture (e.g. a dynamic lookup), the full unit run in step 3 should show it.
- I haven't checked CI's ruff/mypy config for `tests/`. I'll run `make check` (or ruff on the two files) before pushing.

## Deviations

Nothing changed; the plan held. I made exactly the two planned edits on branch `fix/70-dedent-readme-fixture` (commit `2b3e259`): `import textwrap` plus `textwrap.dedent(...)` around `sample_readme_text` in `tests/conftest.py`, and removing the H-03 xfail marker in `tests/unit/test_readme_parser.py`. No other files were touched.

Checking the risks I listed:
- The leading blank line kept by `dedent` didn't matter: `test_parse_standard_readme` passes, including `assert result.text == sample_readme_text`.
- No other test used the fixture: the full unit run went from `375 passed, 53 xfailed` to `376 passed, 52 xfailed`, so the only change is H-03 moving from xfailed to passed. #71's H-04 test is still xfailed, as expected.
- `ruff check` and `ruff format --check` on both files: `All checks passed!` / `2 files already formatted`.

Because the build matched the posted plan, I didn't post a follow-up comment on the issue.
