# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: the plan's cause/diagnosis paragraph (eval: the candidate plan; live: `plan.md`), read against the repro-evidence block's numbered steps, control runs, and outputs (live: the student's posted repro comment on the issue, as quoted in the drafts).
- What good looks like: the stated cause explains every repro result, including the control runs. If a control shows the same input working without the blamed component, or shows the bad state already present before the blamed step, the diagnosis is wrong no matter how confident it sounds.

## Scope

- Where it lives: the plan's scope / in-scope / not-in-scope statements and its list of files and changes; compared with the issue title and body.
- What good looks like: one bounded change the issue asks for, with harder variants or follow-ups explicitly deferred. A drive-by rewrite adds refactors, migrations, new options, UI changes, or framework work the issue never mentioned.

## Executability

- Where it lives: the plan's "files I'll touch" and "approach" sections.
- What good looks like: named files or functions and one chosen approach, so a stranger could open the file and start. Hedges like "somewhere", "not sure which layer", or "whichever is easier" mean the real decision hasn't been made.

## Test plan

- Where it lives: the plan's test plan section, compared with the repro-evidence steps and artifacts.
- What good looks like: the repro steps re-run with the exact expected result after the fix (output, exit code, passing assertion), or a named regression test with its expected result. "Run the full test suite" or "should feel faster" names no observable outcome for this fix.

## Honesty

- Where it lives: the plan's risks / unknowns section, and (after the build) its Deviations section.
- What good looks like: unverified assumptions are labelled as unknowns, and anything changed during the build is recorded under Deviations with the reason. False confidence states guesses as fact.

## Comms

- Where it lives: the plan comment, read against the issue's thread highlights (eval) or the live issue thread (live) for maintainer direction, linked PRs, or patches, and against the repo-facts contribution policy (eval) or the repo's CONTRIBUTING / AI policy files (live).
- What good looks like: the comment responds to what maintainers already said (follows their chosen approach or explains why not, engages existing PRs instead of racing them), names this issue's specifics, and includes an AI-use disclosure (tool and extent) when the repo's policy requires disclosure in comments or "any form" of contribution.
