# Procedure: how this skill grades a plan package

Follow these steps in order. Do not grade any check until the read
order is finished.

## Read order

1. **Mode and setup.** Decide eval or live mode (SKILL.md "Inputs").
   In live mode, read `scope.md` and confirm the issue is in the scoped
   repo; stop if it is not. Then read `rubric.md` and
   `references/evidence-guide.md` and list the checks and the verdict
   rule.
2. **Repo facts first.** Read the repo-facts block (live: the repo's
   `docs/CONTRIBUTING.md`). Write down one line: what the AI policy
   requires of issue comments (disclosure / own words only / PR only /
   nothing).
3. **Issue, then thread.** Read the issue body for the reported
   behavior. Then read every thread highlight and write down each
   piece of maintainer/owner/collaborator direction: a located culprit,
   a proposed or rejected approach, a patch or test build, an open PR
   or prior art. If there is none, write "no direction".
4. **Repro evidence before the plan.** Read the repro evidence and
   write down: what the artifact shows, what each control run shows
   (what changed and whether the symptom went away), and the
   `Expected:` line. Do this before reading the plan so the plan's
   diagnosis cannot set what you see in the evidence.
5. **Plan, then comment.** Read the plan in full, then the comment.

Why this order: the wrong-cause check depends on having the controls
written down before the plan's confident diagnosis is read, and the
thread-direction check depends on having the maintainer's direction
written down before reading a comment that may leave it out.

## Evidence gathering

For each check, pull exactly these facts (locations per the evidence
guide) and keep a short quote of each:

- **diagnosis-grounded:** the plan's cause sentence; the list of
  control runs and artifacts from read-order step 4. For each control,
  ask: does it show the blamed component working, or the symptom
  present before the blamed component runs?
- **one-bounded-change:** every item the plan commits to (approach
  steps, "proposed changes", files list, and anything the comment adds
  like "and also..."). Mark each item "fixes/tests the bug" or "extra".
  Items under "not in scope" or "follow-up" are not commitments.
- **stranger-can-start:** the named file/function/site (or "none"),
  and the chosen approach in one sentence (or "undecided", quoting the
  hedge words).
- **decisive-test:** the test plan sentence, and the observable result
  it names (or "none named").
- **thread-direction-engaged:** the direction list from read-order step
  3, and for each item the sentence in the plan or comment that follows
  it or explains departing from it (or "not mentioned").
- **ai-disclosure-when-required:** the policy line from read-order
  step 2, and any disclosure sentence in the comment or plan (or
  "none").
- **unknowns-stated / comment-matches-plan (preferred):** the plan's
  risk/unknowns lines; the comment's announced approach next to the
  plan's.

In live mode, the repro evidence is the student's posted repro comment
on the issue, and the plan/comment are the drafts. The package is only
what the drafts contain and quote.

## Check execution

1. Run the required checks in rubric order: diagnosis-grounded,
   one-bounded-change, stranger-can-start, decisive-test,
   thread-direction-engaged, ai-disclosure-when-required. Then run the
   preferred checks.
2. Apply each check's pass condition to the facts gathered for it.
   Do not re-read the whole package for a check; go back only to the
   section the evidence guide names for that check.
3. Every check runs, even after an earlier check fails, so the writer
   sees every problem at once.
4. Grade `pass`, `fail`, or `unclear`. Use `unclear` only when the
   fact the check needs is genuinely missing from the package (for
   example, no test plan section at all is a `fail` for decisive-test,
   not `unclear`; `unclear` is for a repro evidence block that is
   absent so grounding cannot be judged).
5. Each grade gets one evidence line: the quote or fact that decided
   it. For a fail, quote the offending text (the extra scope item, the
   hedge, the control that rules out the cause).
6. When a check's pass condition is met, it passes even if the plan
   feels weak in another way. Note that tension in the summary; do not
   fail a check for a reason that belongs to a different check.

## Verdict assembly

1. Take the six required grades. `unclear` counts as `fail`.
2. If all six pass, the verdict is `accept`. If any fails, it is
   `reject`. Preferred checks never change it.
3. In the readable summary, name the deciding check: for a reject, the
   first failing required check in rubric order, with its evidence
   quote; for an accept, say all required checks passed.
4. In live mode, also hold the draft comment against `voice-guide.md`
   and list any broken rule by name (this never changes the verdict).
5. Emit the JSON block from SKILL.md last, with every check (required
   and preferred) in rubric order.
