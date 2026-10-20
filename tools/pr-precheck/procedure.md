# Procedure: how this tool grades a PR package

Follow these steps in order. Do not grade any check until the read
order is finished and the evidence for every check is gathered.

## Read order

1. **Mode and setup.** Decide eval or live mode (SKILL.md "Inputs and
   modes"). In live mode, read `scope.md` first and stop or refuse as
   SKILL.md's scope seam says. Then read `rubric.md` and
   `references/evidence-guide.md`, and write down the list of checks in
   table order and the verdict rule.
2. **Repo standards.** Read the repo-facts block (live: the PR
   template, `docs/CONTRIBUTING.md`, and the house rules in
   `scope.md`). Write down, as a numbered list: every template
   section, every checklist item, every required entry (changelog,
   whatsnew, release notes), any title or issue-reference convention,
   every gating precondition, every repo check (suite, linter,
   formatter, CI job), and one line for the AI policy: "PR disclosure
   required (tool + extent)", "comments only", or "none".
3. **Issue and thread.** Read the issue for the reported behavior and
   its failing cases. Read each thread highlight and write down every
   comment from a MEMBER, OWNER, or COLLABORATOR that gives direction
   (a requested or rejected approach, a restated rule, a pointer to
   code or a PR). If none, write "no direction".
4. **The plan, before the PR.** Read the plan context in full and
   write down: (a) the in-scope items as a numbered list, including
   every failure mode or case it says the fix covers; (b) the files
   list; (c) the "Not in scope" list; (d) the chosen mechanism in one
   sentence; (e) the test plan's cases as a numbered list, each with
   its expected-after result, and any repo check it names; (f) any
   recorded deviation.
5. **The diff, before the description.** Read `### Diff` (live: run
   `git diff main...HEAD` and `git log --oneline main..HEAD`) and
   write down every changed file and, under it, one line per hunk
   saying what the hunk does. Then read the commit list.
6. **Description and title, then evidence.** Read the title and
   description; write down each claim about what changed, each
   disclosed shortfall, and each claim about test outcomes. Then read
   the test evidence.

Why this order: the plan and the diff are each written down before the
description is read, so the description's confident summary cannot set
what you see in the diff. The side-by-side checks (diff against plan,
evidence against test plan, description against diff) compare two
lists you already wrote, rather than one text read through the other.

## Evidence gathering

Build these pairings from your notes (locations per the evidence
guide). Keep a short quote or file/hunk reference for each.

- **diff-within-plan:** for each hunk from step 5, label it (a) fix,
  (b) test, (c) asked-for changelog/docs entry, (d) covered by a
  disclosed shortfall/deviation, or "extra". Quote each "extra" hunk's
  file and what it does.
- **plan-delivered:** for each in-scope item from step 4(a), the hunk
  that delivers it, or the disclosure sentence that names it as not
  delivered, or "missing". Compare 4(d) to what the hunks actually do
  and note "same mechanism" or "different: <how>".
- **description-matches-diff:** for each change-claim from step 6, the
  hunk(s) that back it, or "contradicted by <hunk>" / "no hunk".
- **repro-re-run:** for each test-plan case from step 4(e), the
  evidence line that shows it re-run and its after-result, or the
  disclosure that names it as not run, or "missing". Mark whether each
  shown after-result equals the plan's expected-after.
- **repo-checks-run:** for each repo check from step 2 and 4(e), the
  evidence or description line that names it with its outcome, or
  "not mentioned".
- **reviewable-diff:** walk the `+` and `-` lines once looking only for
  the debris tells in the evidence guide; quote each hit with its file.
- **template-and-asks:** for each item on the step 2 list (other than
  the repo checks and AI policy), where the PR honors it (section text,
  file in the diff, title wording), or "ignored".
- **ai-disclosure-when-required:** the policy line from step 2 and the
  disclosure sentence from the description, or "none".
- **maintainer-direction-honored:** for each direction from step 3,
  the hunk or description sentence that follows or answers it, or "not
  mentioned".
- **title-says-the-change:** the title, next to a one-line summary of
  the diff.

In live mode, gather only what the draft description, the branch diff,
`plan.md`, and the repo files contain. Output that exists only in the
student's terminal is "missing" until it is pasted into the draft.

## Check execution

1. Run the required checks in rubric table order: diff-within-plan,
   plan-delivered, description-matches-diff, repro-re-run,
   repo-checks-run, reviewable-diff, template-and-asks,
   ai-disclosure-when-required, maintainer-direction-honored. Then the
   preferred check, title-says-the-change.
2. Apply each check's pass condition to the pairing gathered for it.
   Do not re-read the whole package for a check; go back only to the
   section the evidence guide names for that check's family.
3. Every check runs, even after an earlier check fails, so the author
   sees every problem in one pass.
4. Grade `pass`, `fail`, or `unclear`. A pairing marked "missing",
   "extra", "ignored", "not mentioned", or "contradicted" is a `fail`
   when the check's pass condition requires that item: absence of
   required evidence is a fail, not an unclear. Use `unclear` only
   when the package itself is broken for that check (for example, the
   plan context is absent, so there is nothing to compare the diff
   against).
5. Before failing a check for a missing item, look for a disclosed
   shortfall covering it (step 6 notes); where the check's pass
   condition accepts one, a disclosure turns that item into a pass.
6. Each grade gets one evidence line: the fact or quote that decided
   it. For a fail, quote the offending hunk, the missing case, or the
   ignored ask. For a pass, name the strongest supporting fact.
7. Fail a check only for a reason its own pass condition names. A
   problem that belongs to another check is graded there, not twice.

## Verdict assembly

1. Take the nine required grades. `unclear` counts as `fail`.
2. If all nine pass, the verdict is `accept`. If any fails, the
   verdict is `reject`. The preferred check never changes it.
3. Name the deciding check in the readable summary: for a reject, the
   first failing required check in rubric table order, with its
   evidence quote (if several failed, still quote that first one and
   list the rest by name); for an accept, say every required check
   passed and quote the repro-re-run evidence.
4. In live mode, then hold the draft title and description against
   `voice-guide.md` and list every broken rule by name with the
   offending sentence (this never changes the verdict). Also list any
   procedure gap you hit.
5. End the reply with the JSON block from SKILL.md, every check
   (required and preferred) in rubric table order, and nothing after
   it.
