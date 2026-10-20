---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is this
pull request ready to submit?** A PR package is a candidate pull
request (its title, description, commit list, diff, and test
evidence) read against three things: the accepted plan it claims to
implement (scope, files, test plan, and any recorded deviations), the
issue that plan belongs to (body and thread), and the repo's stated
standards (PR template, contribution guide, AI-use policy).

Do not grade whether the plan was a good plan (that was decided when it
was accepted), whether the issue is worth fixing, or whether the code
is the cleverest possible fix. Grade whether this PR, as written,
delivers the plan, proves it, can be reviewed, and meets the repo's
asks. Grade one package per run; if you are handed more than one,
stop and ask which one.

## Inputs and modes

Decide the mode first, and say which mode you are in at the top of
your reply.

**Eval mode** applies whenever you are given a package bundle (a
markdown file headed `# Eval package: <id>` with `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Plan context`, and
`## Candidate PR` sections). The bundle is the whole world: every fact
comes from the bundle text. Do not fetch anything, do not open other
files, do not run commands. Eval mode always grades a complete
package: every check in `rubric.md`, the full verdict rule. Use the
bundle id as `item`.

**Live mode** applies when a student asks you to check their own PR
before opening it. Gather these inputs, and name each one's source in
your summary:

- **The plan**: the student's `plan.md` (in their course repo under
  `beat-1-sandbox/unit-3/plan.md`), including its `## Deviations`
  section. A house-chain student reads the house plan and the house
  repro pack the instructor routed them to instead.
- **The diff**: everything the branch changes relative to the repo's
  default branch. Produce it by running `git diff main...HEAD` (three
  dots) from the student's working copy of the Path Review fork, on the
  fix branch; also run `git log --oneline main..HEAD` for the commit
  list. Do not grade a diff the student pasted when you can run the
  command.
- **The draft PR title and description**: the text the student is
  about to paste into GitHub, from wherever they give it to you (a file
  or the message itself). If there is no draft, stop and ask for it.
- **The test evidence**: the before/after output in the draft
  description. Output that exists only in the student's terminal or
  in another file is not evidence a reviewer will see; grade what the
  description contains.
- **The issue side**: the issue body and thread on GitHub, the repo's
  `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`
  (template asks, CI requirements, branch and commit conventions,
  AI-use policy). Read the template and CONTRIBUTING from the working
  copy's default branch; read the thread live.

Use the PR URL as `item` if the PR is already open, otherwise the
branch name.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. If its `Repo:` line still shows the placeholder
`<ORG>/<PATH-REVIEW-REPO>`, stop without grading and tell the student
to fill the `Repo:` line in `scope.md` with their section's Path
Review repo. Never guess a scope. If the PR's target repo (the
`origin` remote of the working copy, or the repo in the PR URL) is not
the repo on that line, refuse to grade and say why. Apply the house
rules listed in `scope.md` as part of the repo's standards: the PR
comes from a `fix/<issue-number>-<slug>` branch on the student's fork,
it delivers one plan for one issue, and every template section gets
real content. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after grading, read `voice-guide.md` and hold the draft
PR title and description against each of its rules and its "Things I
never post" list. In the summary, list every rule the draft breaks:
the rule's name, the sentence in the draft that breaks it, and a
suggested rewrite. Voice notes never change the verdict on their own,
because no check in `rubric.md` reads the voice guide; they are the
student's personal standard, not the repo's. In eval mode, ignore
`voice-guide.md` entirely.

## Component reads

Read the components in this order and use each for one job:

1. `rubric.md` defines the checks (name, evidence, pass condition,
   weight) and the verdict rule. It decides WHAT passes.
2. `references/evidence-guide.md` maps where each evidence family
   lives in a bundle and in live mode, and what good looks like. It
   decides WHERE to look.
3. `procedure.md` gives the read order, the gathering moves, how each
   check executes, and how the verdict is assembled. It decides HOW.
   Execute it as written, step by step.

If `procedure.md` is silent on a step you need, do the narrowest thing
the rubric's pass condition implies and name the gap in your summary
under "Procedure gaps"; never silently invent a step. If `rubric.md`
has no checks or no verdict rule, or `procedure.md` has no steps,
refuse to grade: say which file is empty and that this tool cannot
grade without it. Do not invent checks.

## Verdict and output

The verdict is binary: `accept` means ready to submit (open the PR as
it stands), `reject` means hold (fix what the failing checks name,
then re-run). There is no third verdict and no score; reservations go
in a check's evidence line, not in the verdict.

Before the JSON block, write a short readable summary: the mode, one
line per check (grade and the deciding fact), the deciding check for
the verdict, and in live mode the voice-guide notes and any procedure
gaps. Then end your reply with this fenced JSON block, valid, with
every check in rubric order, and nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quoting the text that decided it: a file path from the diff, a line
  of the plan, a sentence of the description, a line of test output.
  "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse PR that delivers the
  plan and shows the proof is ready; a long, confident, well-formatted
  one can be hiding drift. Never pass or fail a check on length, tone,
  or formatting; read the diff against the plan, the evidence against
  the test plan, and the description against the diff.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it passes; note the tension in the
  summary. Do not fail a check for a reason that belongs to a
  different check.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Honest shortfalls are not failures.** A PR that does less than
  everything but says so plainly in its description (a deferred edge,
  a check it could not run, a recorded plan deviation) is graded on
  what it discloses. A shortfall only the diff reveals is silent drift.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs; where it is silent, an unverifiable claim is a
  failing one. A PR whose readiness you cannot verify from the package
  is not ready to submit.
