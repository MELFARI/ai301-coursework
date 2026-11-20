# Contribution: `aggregate.time_aggregate` should be able to handle a CubeList

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Melvin Sere
**GitHub Username:** MELFARI
**Issue:** https://github.com/MetOffice/CSET/issues/911
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my Unit 1 issue-select skill into issue-scout, with org-targeted search queries and free ranking filters**

I built issue-scout on top of my Unit 1 rubric: a fit profile (Python and JS/TS, small bug fixes, AI/data and web apps), a search strategy with `gh search issues` queries, and ranking filters (no assignee, updated in the last 60 days, 10 comments or fewer, no feature or infra labels, bug label first, then fewest comments). Before scanning, my first queries returned 100 results each, mostly auto-generated issues from tiny personal repos, so I rewrote them with `--owner` lists of established AI/data and web orgs (33 and 25 real results). The first scan accepted 1 of 5 (gradio-app/fastrtc#140); the other 4 failed Issue Availability on an open PR or a fresh "assign me" comment, which my ranking can't see because `gh search` doesn't return linked PRs, so I accepted that as the cost of a free ranking stage and fixed my label filter, which missed crewAI's `feature-request` spelling.

**Unit 6: Installed the voice guard with an Upstream section in my voice guide, and wrote a CLAUDE.local.md memory file for CSET**

I merged my three voice-guide copies into one guide at `~/.claude/voice-guard/voice-guide.md` and added four Upstream rules (claims, never claiming over someone, questions, pings). I also changed my never-post line about dates so a "report back by Monday" time is allowed but a fix date still isn't. The guard didn't run on Windows at first, because there's no `python3` and `claude` can't be launched by bare name from Python, so I pointed the hook at `python` and patched `check_voice.py` to find `claude` with `shutil.which` and read files as UTF-8. After that, the wiring test was stopped ("no body the guard can read") and my real claim passed (`guard-log.md`: "[hook] pass | target: gh issue comment .../911"). Writing `CLAUDE.local.md` for CSET showed that CSET doesn't run on native Windows at all (`plot.py` imports the Unix-only `fcntl`), so I recorded the working WSL + micromamba commands there, which every later session in that repo will read.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I picked MetOffice/CSET #911 from the AI301 Issue Selection site. My profile asks for small bug fixes in Python or JS/TS, in AI/data or web projects. My first scout scan used `gh search issues` limited to established AI/data and web orgs. It graded five issues and accepted only gradio-app/fastrtc#140; the other four already had an open PR or a recent "assign me" comment. On the site, I checked about fifteen issues that fit my profile, and almost all of them were taken or already fixed (simpler-grants-gov #8245, #9817, #9561, Daft #7707 had open PRs; #9806, #9805, #9550 had already been fixed by merged PRs but never closed). I graded the two that looked free, CSET#911 and simpler-grants-gov#5277. After the Unit 6 scope patch, the scout accepted #911 with the difficulty note **moderate** ("the body is empty, so the CubeList-handling behavior is mine to pin down") and rejected #5277, because its tests were already merged in PR #5625. CSET fits me: it's a Python data library (weather and climate data with `iris` cubes), the maintainers are active (commits the day I graded it), and they allow AI help as long as it's disclosed.

Before claiming, I read the code and found that PR #2467 (merged 2026-09-23) had already made `time_aggregate` loop over a `CubeList`. It never linked or closed #911, but no test passes a `CubeList` into `time_aggregate`. So I didn't claim a fix that was already done. My claim says what I found, offers a PR that adds the missing test and closes the issue, and asks whether the maintainers would rather just close it. I want to learn how a real scientific Python project tests its operators, and how to raise "this looks already done" politely with maintainers I don't know. My honest read of the scout: its checks were right every time I checked them by hand, but its blind spot is work that's already finished. It caught #5277 only after I patched Issue Availability to look at merged PRs, and it still accepted #911 because #2467 never linked to the issue. Only reading the code caught that. Next I'd add an evidence step that searches merged PRs and the current code for the function the issue names.

Claim comment: https://github.com/MetOffice/CSET/issues/911#issuecomment-7048140279

**Toolkit**

- Tool: issue-scout (my search strategy plus my Unit 1 rubric). I ran one full scan, then graded my picks from the Issue Selection site directly with `issue-scout: grade <URL> <URL>`, before and after the Unit 6 scope patch.
- What it produced: scan 1 graded the top 5 of 58 org-targeted results and accepted 1 (fastrtc#140). The final grading accepted CSET#911 (all 7 checks pass, difficulty "moderate") and rejected simpler-grants-gov#5277 on Issue Availability ("Merged PR #5625 ... already did the work").
- Difficulties: my first queries were flooded by auto-generated issues from tiny repos. 4 of 5 scan slots went to issues already taken, because `gh search` results don't show linked PRs. The rubric passed #5277 even though its work was merged. It also couldn't see that #911 was done in unlinked PR #2467; I found that by reading `aggregate.py`.
- What changed: rewrote the queries around `--owner` lists. Patched Issue Availability to also fail when a linked PR has already merged the work. Applied the Unit 6 scope patch, replacing my **Single Concrete Change** check with **Scope Feasible** plus a difficulty note. I'd next add a check that searches merged PRs and the current code for the function the issue names.

---

## Understanding the Issue

**Problem Description**

CSET's `time_aggregate` operator sums (or otherwise aggregates) weather data over time windows, like turning hourly rainfall into 3-hourly totals. CSET recipes often load several models at once as a `CubeList` (one iris cube per model), but `time_aggregate` was written for a single `Cube`, so passing a list broke it. The issue asks for it to accept a `CubeList`, as part of the maintainers' tracking issue #721 ("Handle CubeLists in all operators"). PR #2467 has since added that support, but nothing tests it, so it could quietly break again.

**Expected Behavior**

Given a `CubeList`, `time_aggregate` aggregates each cube and returns a `CubeList` of results, each one the same as calling it on that cube alone. A single `Cube` still returns a single `Cube`. A test in `tests/operators/test_aggregate.py` should check this.

**Current Behavior**

Before #2467 (commit `4d6f5f0f`), a `CubeList` crashed with `AttributeError: 'CubeList' object has no attribute 'coord'`. On `main` today it works, but none of the four `time_aggregate` tests pass a `CubeList` in, and #911 is still open because #2467 never linked to it.

**Affected Components**

- `src/CSET/operators/aggregate.py`: `time_aggregate` (it calls `iter_maybe` from `src/CSET/_common.py`, then `_aggregate_in_time_single_frt` / `_aggregate_in_time_multiple_frt` per cube).
- `tests/operators/test_aggregate.py`, using the `cube` and `long_forecast_multi_day` fixtures from `tests/conftest.py`.

**Toolkit**

- Tool: Claude Code, reading `aggregate.py`, PR #2467's diff, and the test file, guided by my `CLAUDE.local.md` notes for CSET.
- What it produced: the finding that #2467 already loops with `cubes = iter_maybe(cubes)`, and that no `time_aggregate` test passes a `CubeList`.
- Difficulties: the issue body is empty, so the expected behavior came from the title, the tracking issue #721, and the sibling PR #2548 for `cape_ratio`.
- What changed: I reframed my contribution from "add CubeList support" to "test the CubeList support that already exists", and said so in my claim.

---

## Reproduction Process

**Environment Setup**

Windows 11 with WSL2 Ubuntu 22.04. CSET doesn't import on native Windows (`src/CSET/operators/plot.py` imports the Unix-only `fcntl`), which I found when my first Windows venv failed with `No module named 'fcntl'`. In WSL I installed micromamba 2.10.0 and created the `cset-dev` env from CSET's own lock file, `requirements/locks/py313-lock-linux-64.txt` (Python 3.13.18, iris 3.16.2), then ran `pip install --no-deps -e .`, the setup steps in CSET's getting-started guide minus `make`. I forked MetOffice/CSET to MELFARI/CSET and ran everything from the clone at `main` = `e9134662`. Check: `pytest tests/operators/test_aggregate.py -q` gives `19 passed`.

**Steps to Reproduce**

1. Save `repro_911.py` in the repo root. It loads the same files as the `cube` and `long_forecast_multi_day` fixtures (`tests/test_data/air_temp.nc`, `tests/test_data/long_forecast_air_temp_multi_day.nc`), then calls `aggregate.time_aggregate(..., method="SUM", interval_iso="PT2H")` on a single cube, on a 2-cube `CubeList`, and (with `PT1H`) on a mixed single- and multi-forecast-time `CubeList`.
2. Run `python -W ignore repro_911.py` at commit `4d6f5f0f` (the parent of #2467's merge) and at `main` (`e9134662`).
3. Observed result: before #2467, the single cube works and the `CubeList` crashes with `AttributeError: 'CubeList' object has no attribute 'coord'`. On `main`, the `CubeList` returns a `CubeList` whose cubes match the single-cube result.

**Reproduction Evidence**

- **Commit showing reproduction:** none (the script is local and isn't part of the PR; the PR's new test will carry the check).
- **Screenshots/logs:**

```
# before: 4d6f5f0f (parent of #2467 merge)
single Cube  -> Cube (2, 17, 13)
  File ".../src/CSET/operators/aggregate.py", line 98, in time_aggregate
AttributeError: 'CubeList' object has no attribute 'coord'

# now: main e9134662
single Cube  -> Cube (2, 17, 13)
2-cube list  -> CubeList [(2, 17, 13), (2, 17, 13)]
each matches single result: True
mixed list   -> CubeList [(3, 17, 13), (3, 121, 3, 3)]
```

- **My findings:** the bug in #911 was real, and PR #2467 (merged 2026-09-23) fixed it without linking the issue, so the issue stayed open. None of `test_aggregate`, `test_aggregate_multi_frt`, `test_aggregate_in_time_single_frt`, or `test_aggregate_in_time_multiple_frt` pass a `CubeList`; the file's only `*_cubelist` test is for `rolling_window_time_aggregation`. The remaining work is a test that pins the `CubeList` path, which I offered in my claim.

**Toolkit**

- Tool: repro-check (my Unit 2 skill, with the new wild scope), run on `claim-draft.md` alone and then on `claim-draft.md` plus `repro-draft.md`.
- What it produced: accept on both runs. On the full run, all 7 checks passed (for example Behavior Shown: "CubeList trigger crashes pre-#2467 ... and returns a CubeList on main — before/after artifact"), and it independently confirmed my code references against the repo.
- Difficulties: on the claim-only run, the four repro checks graded "unclear: not yet applicable", which is right but means a claim can't pass on its own without reading the verdict carefully. Getting the "before" took a git worktree at the pre-#2467 commit, and WSL's git couldn't read a worktree created from Windows, so I ran git from Windows and Python from WSL.
- What changed: I added the pre-#2467 run so the report shows a real failure, not just "it works now".

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
