# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70

**Verdict output**

Live-mode run: `claude "issue-select: grade these candidate first issues: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/71 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/65"` — output pasted verbatim below. Issue #70 is ranked first with verdict `accept`.

````
All three issues are inside the scoped repo (`codepath/pathreview-ai301-fa26-s3`), public and not archived. The Path Review house rule applies: fellow students' claim comments do **not** block an issue, so the activity on #71 and #65 is normal and non-blocking.

Shared evidence (today = 2026-10-26):
- **Maintainer Responsiveness / Repo Activity**: `Aburke225` (COLLABORATOR) committed on 2026-09-16 — within 90/180 days; repo not archived. Pass for all.
- **Policy Compliance**: AI-assisted is the course's own workflow; no AI ban, no `blocked`/`needs design` labels, no required design/assignment. Pass for all.

**Ranked read-out (all three accepted):**

1. **#70 — README parser fixture indented** — Best clean first pick: filed by a collaborator, labeled `bug`, tightly scoped (dedent one fixture, remove the `H-03` xfail), 2–4 h, **zero prior activity** so it's uncontested. Combines reading parser code with fixing a test — exactly the "read an unfamiliar codebase + write tests" goals.
2. **#71 — Heading hierarchy fixture indented** — Same tiny, well-described shape (dedent 8-space fixture, remove `H-04` xfail), 1–2 h. Heavily trafficked (7 student comments) but the house rule neutralizes that; a touch less pristine than #70.
3. **#65 — review_service async mocks misconfigured** — Strongest pure test-writing practice (rework `AsyncMock`/`MagicMock` so 13 tests run), but `tier-1` and the largest scope of the three; still small and finishable in a week.

Per-check grades — every required check passes on all three, so all three **accept**:

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70",
    "checks": [
      {"name": "Maintainer Responsiveness", "grade": "pass", "evidence": "Collaborator Aburke225 committed 2026-09-16, within last 90 days"},
      {"name": "Repository Activity", "grade": "pass", "evidence": "archived:false; default-branch commit 2026-09-16 within 180 days"},
      {"name": "Issue Availability", "grade": "pass", "evidence": "assignees:[]; no linked PRs; comments_count:0 — no claims at all"},
      {"name": "Policy Compliance", "grade": "pass", "evidence": "AI-assisted is the course workflow; no AI ban; labels bug/ingestion/tier-2, no blocked/needs-design"},
      {"name": "Single Concrete Change", "grade": "pass", "evidence": "Dedent sample_readme_text fixture and remove the H-03 xfail — one PR closes it"},
      {"name": "Settled Spec", "grade": "pass", "evidence": "Filed by COLLABORATOR, labeled 'bug', expected behavior stated ('fixture shouldn't be indented')"},
      {"name": "Bounded Effort", "grade": "pass", "evidence": "Estimated effort 2-4 hours; single fixture fix"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/71",
    "checks": [
      {"name": "Maintainer Responsiveness", "grade": "pass", "evidence": "Collaborator Aburke225 committed 2026-09-16, within last 90 days"},
      {"name": "Repository Activity", "grade": "pass", "evidence": "archived:false; default-branch commit 2026-09-16 within 180 days"},
      {"name": "Issue Availability", "grade": "pass", "evidence": "assignees:[]; no linked PRs; 7 comments are all student (author_association NONE) claims — non-blocking under Path Review house rule"},
      {"name": "Policy Compliance", "grade": "pass", "evidence": "No AI ban; labels bug/ingestion/tier-2, no blocked/needs-design, no required design/assignment"},
      {"name": "Single Concrete Change", "grade": "pass", "evidence": "Remove 8-space indentation from one fixture and the H-04 xfail — one PR"},
      {"name": "Settled Spec", "grade": "pass", "evidence": "Filed by COLLABORATOR, labeled 'bug', clear expected behavior ('Remove the indentation')"},
      {"name": "Bounded Effort", "grade": "pass", "evidence": "Estimated effort 1-2 hours"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/65",
    "checks": [
      {"name": "Maintainer Responsiveness", "grade": "pass", "evidence": "Collaborator Aburke225 committed 2026-09-16, within last 90 days"},
      {"name": "Repository Activity", "grade": "pass", "evidence": "archived:false; default-branch commit 2026-09-16 within 180 days"},
      {"name": "Issue Availability", "grade": "pass", "evidence": "assignees:[]; no linked PRs; 3 comments all from student daianx (NONE) — non-blocking under Path Review house rule"},
      {"name": "Policy Compliance", "grade": "pass", "evidence": "No AI ban; labels bug/tests/tier-1, no blocked/needs-design"},
      {"name": "Single Concrete Change", "grade": "pass", "evidence": "Rework mock setup in test_review_service.py so the existing CRUD tests run — one PR"},
      {"name": "Settled Spec", "grade": "pass", "evidence": "Filed by COLLABORATOR, labeled 'bug', expected fix stated (AsyncMock for execute, MagicMock for result)"},
      {"name": "Bounded Effort", "grade": "pass", "evidence": "Single test-file mock rework; no architectural overhaul requested"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

1. Run 1 (full run, 2026-10-05): `agreement: 16/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)`. It missed issue-10, issue-12, issue-15 and issue-20, all `graded accept` against a gold `reject`. Categories: `policy 0/1  scope 1/4`.
2. Run 2 (full confirming run with revised rubric, 2026-10-26, saved as `eval-run.txt`): `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Categories: `claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1  scope 4/4`. The only miss was issue-19 (`failed: Single Concrete Change`).

**Issue analysis**

**issue-12** (bookwyrm-social/bookwyrm#1133, "Include in-progress books in the reading-goal progress-bar"). Gold label: `reject`. Run 1 decision: `accept`. Run 2 decision: `reject`.

In run 1 my rubric accepted it because on the surface it is a good first issue: "assignees: none; linked PRs: none", it carries the `good first issue` label, the last default-branch commit was "2026-08-12 by Mouse Reeve", and a maintainer (`mouse-reeve (MEMBER)`) had already explained the approach ("You could definitely create one with html and css (we use the bulma css library)"). My old Policy Compliance check only looked at "Issue body text, comments, and labels" for things like `blocked` or `needs design`. So it never looked at the repo's contribution policy line, which says: "We do not accept AI-generated code or documentation." Our whole course workflow is AI-assisted, so this repo would close the PR I'd open. That makes it a reject no matter how clean the issue is, and it's why the gold note says it "passes every liveness, scope, and claim check" but the "contributing docs ban AI-generated code and documentation outright". In run 2 I pointed Policy Compliance at the "Contribution policy line in repo facts" and made an outright AI ban a fail, so it now rejects and the policy category went from 0/1 to 1/1.

**Check rationale**

Quoted from `tools/issue-select/rubric.md` as currently written:

> | Single Concrete Change | Issue title and body | Fail if the issue is a tracking list, umbrella, or "megaissue" that collects many separate tasks or links (e.g. a bulleted list of other issue numbers), rather than one change a single PR would close. | required |

Why it has this form: in run 1, issue-10 (tldr-pages/tldr#18405, "Documentation request megaissue") was accepted. My old "Scope Clarity" check passed anything that was "not a completely vague issue", and a list of ~120 page requests isn't vague. It is clearly stated, it just isn't one change. The issue says itself "This issue is for listing those so that anyone could try their hand at making pages." The real question for a first contribution is "would one PR close this?", so I wrote the pass condition around that. I gave the "bulleted list of other issue numbers" example because that is exactly what issue-10's body is. I replaced the old Scope Clarity check with this one instead of loosening it further.

**Trade-offs**

The check costs me issue-19 (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the UI"). Gold is `accept`, but run 2 graded it `reject` with `failed: Single Concrete Change`. That issue is one performance bug, but the maintainer wrote it as numbered lists: "There are two potential causes which should be fixed: 1. The matchers are slow... 2. UI update is waiting..." and then "Additional suggestions: 1. ... 2. ... 3. ...". The grader read the five list items as "many separate tasks", which is the same shape as the megaissue the check was built for. It is the only clear-accept I lost (`clear-accept 7/8`). I accept that miss for now: the check still gets all four scope issues (`scope 4/4`), and the total is above the bar at 19/20. The fix I'd try next is to say the list has to be links to *other issues* or independent deliverables, not sub-points of one bug, and then re-run `--only issue-19,issue-10` as the canary pair to check that the megaissue still fails.

---

## Selection rationale

**Selection rationale**

1. **Fit and time.** #70 is a test-fixture bug in the ingestion part of the app: the `sample_readme_text` fixture in `tests/conftest.py` is indented four spaces, so the README parser finds no headings and `test_parse_standard_readme` fails (it's marked `xfail` with id H-03). It's Python and pytest, which I know, and it makes me read the parser code to confirm the parser is fine and the fixture is wrong. That lines up with what I want to practice: reading an unfamiliar codebase and working with tests. The issue estimates 2–4 hours, which fits around my classes, and leaves time in Unit 2 for environment setup and a careful reproduction.
2. **What the verdict got right, and what I weighed that it couldn't.** The skill correctly saw that it's filed by a collaborator, labeled `bug`, names the exact files, says what the fix is ("the fixture shouldn't be indented"), and has no assignees, PRs or comments. What the rubric can't weigh is that it is *small*. Since the fix is basically a dedent plus removing an `xfail` marker, I'll have to make sure my Unit 3 PR also shows real understanding (e.g. checking the other fixtures and the parser's behavior on indented input) so it doesn't look trivial. I also noticed #71 is almost the same bug in a different fixture, so the two might end up needing to stay consistent.
3. **Difficulty claiming it.** It's the least contested of my three candidates (0 comments vs. 7 on #71), so I don't expect a pile-up. Under the Path Review house rule, other students' claims wouldn't block me anyway. The main risk is that a classmate finds it before I post my claim in Unit 2. If so, I'll still post my own claim and reproduce in my own words, not piggyback. I'm not claiming it yet: the claim comment comes in Unit 2, after I write my voice guide.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
