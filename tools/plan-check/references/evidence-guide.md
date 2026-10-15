# Evidence guide: where evidence lives in a plan package

In an eval bundle the sections are: `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Repro evidence`, `## Candidate plan`, and
`## Candidate plan comment`. In live mode the same families come from
the GitHub issue page, the student's posted repro comment on that
issue, the repo's `docs/CONTRIBUTING.md`, and the student's draft
`plan.md` and draft plan comment.

## Diagnosis and grounding

Where it lives:
- Eval: the plan's cause/diagnosis line (often the first paragraph of
  `## Candidate plan`), plus any approach step that assumes a cause.
  The behavior that cause must explain is in `## Repro evidence`:
  numbered steps, control runs ("Control:", "same steps with/without
  X"), artifacts (tracebacks, printed values, exit codes), and the
  `Actual:` line.
- Live: the cause line in the draft `plan.md`, read against the
  student's posted repro comment (steps, control, observed output).

What good looks like: the stated cause names the component that the
repro's artifact points at (the frame in the traceback, the step where
the value first goes wrong), and every control run is consistent with
it. A control that makes the symptom disappear while the blamed part
still runs, or that shows the bad value already present before the
blamed part runs, rules the diagnosis out.

## Scope

Where it lives:
- Eval: the plan's scope / "in scope" / "not in scope" lines, its
  numbered approach or "proposed changes" list, and its files/areas
  list. The comment often repeats the scope ("I plan to ... and also
  ...").
- Live: the same sections of the draft `plan.md` and the draft
  comment.

What good looks like: every item in the approach list fixes the
reported behavior or tests that fix; extra ideas appear only under
"not in scope" or "follow-up". A bounded plan for a one-line bug names
one or two files; a drive-by rewrite lists migrations, new settings,
new UI, framework or harness changes, or says "while in the area".

## Executability

Where it lives:
- Eval: the plan's "Change", "Approach", and "Files" lines: named
  paths (`src/walk.rs`), functions (`Terminal.print`), or code sites
  (the push completion callback).
- Live: the same lines in the draft `plan.md`.

What good looks like: a stranger could open the named file, find the
named function or site, and start the stated change without asking a
question. Words like "somewhere", "maybe", "or", "whichever", "not
sure", "investigate", "profile" in the approach are signs a decision
has not been made yet.

## Test plan

Where it lives:
- Eval: the plan's "Test" / "Test plan" line, read against the repro
  evidence's steps and `Expected:` line.
- Live: the test plan in the draft `plan.md`, read against the steps
  in the student's posted repro comment.

What good looks like: the test re-runs the repro (or a named test
built from it) and says what the result must be (exit 0, the match is
printed, the color flips at step 3, the xfail test passes). "Run the
full suite", "nothing regresses", and "feels fast" are not decisive:
they could all pass with the bug still there.

## Honesty

Where it lives:
- Eval: the plan's "Risk", "Unknowns", or "open question" lines; any
  certainty word in the plan or comment; stated deferrals.
- Live: the same in the draft, plus the `## Deviations` section of
  `plan.md` after the build (where a mid-build change is recorded and
  explained).

What good looks like: what was not checked is named as unchecked
("verified gzip, xz, zstd; will check the other three"), not dressed
up as settled. A deviation is recorded in the plan with the reason,
not left to show up only in the diff.

## Comms

Where it lives:
- Eval: `## Candidate plan comment`, read against `## Thread
  highlights` (look for OWNER / MEMBER / COLLABORATOR comments that
  locate a culprit, propose or reject an approach, post a patch or
  test build, or point at an open PR) and against the `## Repo facts`
  block's `contribution policy` line (AI policy: does it require
  disclosure for comments, only for PRs, or only own words?).
- Live: the draft comment, read against the live issue thread
  (maintainer comments and other contributors' plans) and the repo's
  `docs/CONTRIBUTING.md`.

What good looks like: the comment names the direction the thread
already gave and says it follows it (or why not), mentions any open PR
it would otherwise race, and carries an AI-use disclosure when the
repo's policy requires one for comments. Boilerplate that would read
the same on any issue, or a plan that silently goes a different way
from the owner's posted patch, is not thread-aware.
