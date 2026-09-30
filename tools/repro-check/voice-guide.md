# Voice guide: how I talk upstream

## Who I am in threads

I'm a student making my first open-source contributions, coming from
Python, SQL, and REST API work. In Path Review I'm here to reproduce a
bug, fix it, and learn how review works. Readers can expect me to say
what I ran and what I saw, and to say plainly when I don't know yet.

## Rules I write by

### Rule: Name the issue's specifics

Every claim or repro comment names something only this issue has: the
file, the function, the input, or the error text. If the comment could
be pasted on another issue unchanged, it isn't ready.

- Wrong: "Hi, can I work on this? I'd love to help!"
- Right: "I'd like to look into why `output_parser.py` calls `.items()` on a top-level JSON array; I'll start by running the xfail test in `test_output_parser.py`."

### Rule: Promise the investigation, not the fix

I commit to the next step I can actually take. No fix promises, no
dates, no "should be easy".

- Wrong: "I'll have a PR up for this by Friday."
- Right: "Next I'm going to trace where the fallback path hands the parsed value to `.items()` and report what I find here."

### Rule: Show the output, don't describe it

If I say something happened, the comment has the command and its output
pasted, not my summary of it. "It crashes" without the traceback is a
claim, not evidence.

- Wrong: "I ran it and got the same error as the issue."
- Right: "Running `pytest tests/unit/test_output_parser.py -k array --runxfail` gives: `AttributeError: 'list' object has no attribute 'items'` (full trace below)."

### Rule: Say what I didn't check

If my environment differs from the issue's or there's something I
haven't tried, I write that down instead of letting the reader assume.

- Wrong: "Confirmed on all platforms."
- Right: "I only ran this on Windows 11 with Python 3.12; I haven't tried Linux or the Docker setup."

### Rule: Disclose AI help where it's asked for

When the repo's policy asks for AI disclosure, I say which tool I used
and what it did, in one plain sentence.

- Wrong: (no mention, after an AI assistant drafted the report)
- Right: "I used Claude Code to help run the steps and draft this report; I re-ran the commands myself and checked the output."

## Things I never post

- A fix promise or a delivery date.
- "+1", "same here", or "can confirm" with nothing of my own under it.
- A root cause I haven't shown with output or a test.
- "Guaranteed", "100%", or "works everywhere".
- Exclamation-heavy enthusiasm in place of information.
- A comment I haven't run through repro-check first.
