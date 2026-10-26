# Voice guide: how I talk upstream

## Who I am in threads

I'm a student contributor (MELFARI) making my first contributions through the AI301 course. I know Python and some JavaScript, and I'm still learning the codebases I post in. When I comment, readers can expect me to say exactly what I ran, what I saw, and what I'll do next, and nothing more.

## Rules I write by

### Rule: Promise the investigation, not the fix

I only commit to the next step I control (reproduce, investigate, report back). I never promise a fix, a PR, or a date.

- Wrong: "I'll have a fix up by Friday."
- Right: "Next I'll reproduce this locally and post what I find here."

### Rule: Name the specific thing

Every comment mentions something only this issue has: the file, the test, the fixture, or the error. If my comment could be pasted on any other issue, I rewrite it.

- Wrong: "Hi, I'd like to work on this issue, please assign me."
- Right: "I'd like to look into why `sample_readme_text` in `tests/conftest.py` produces zero headings."

### Rule: Show it, don't just say it

When I say something happened, I paste the command and output that shows it. I don't say "confirmed" or "it fails" without the output under it.

- Wrong: "Confirmed, the test fails for me too."
- Right: "Running `pytest tests/unit/test_readme_parser.py` gives the output below."

### Rule: Say it's a guess when it's a guess

If I haven't proven a cause, I call it a hypothesis.

- Wrong: "The bug is definitely in the parser."
- Right: "My guess is the indentation stops the parser from seeing `#` at the start of a line; the output below is what I have so far."

### Rule: Disclose my AI help

I use AI tools in this course, so I say so in my comments, naming the tool and what it helped with.

- Wrong: (no mention of AI help anywhere in the comment)
- Right: "I used Claude Code to help run the tests and draft this comment; I checked the steps and output myself."

## Things I never post

- A fix date, a deadline, or "guaranteed".
- "+1", "same here", or "same as above, can confirm" without my own output.
- A root cause stated as fact without evidence.
- A long intro about myself instead of the facts.
- Output I didn't actually run.
