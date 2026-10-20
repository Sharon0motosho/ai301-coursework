# Evidence guide: where evidence lives in a PR package

**In an eval bundle** the sections are: the header (`source:`,
`captured:`), `## Repo facts` (its `pull requests:` line holds the
template and contribution asks; its `contribution policy` line holds
the AI policy and any gating rules), `## Issue`, `## Thread highlights`
(each line starts with a date, a name, and a role in parentheses such
as `(MEMBER)`, `(OWNER)`, `(COLLABORATOR)`, `(NONE)`), `## Plan
context` (the repro evidence the plan built on, then the plan itself:
its cause, "Scope", "Not in scope", "Files", "Test plan", and any
deviation note), and `## Candidate PR` with `### Title`,
`### Description`, `### Commits`, `### Diff`, and `### Test evidence`.

**In live mode** the same families come from: `plan.md` in the
student's course repo (`beat-1-sandbox/unit-3/plan.md`: its "The
change", "In scope", "Not in scope", "Test plan", and "Deviations"
sections); the branch diff from `git diff main...HEAD` and the commit
list from `git log --oneline main..HEAD`, run in the working copy on
the fix branch; the student's draft PR title and description; the
issue page and thread on GitHub; `.github/PULL_REQUEST_TEMPLATE.md`
and `docs/CONTRIBUTING.md` in the working copy; and `scope.md` in
this skill directory for the house rules.

## Plan fidelity (harness category: silent-drift)

Where it lives:
- Eval: the plan's in-scope steps, "Not in scope" list, and "Files"
  line in `## Plan context`; each `--- a/` / `+++ b/` file header and
  `@@` hunk in `### Diff`; the claims in `### Description` ("exactly",
  "no changes beyond", "tests added", "changelog added", the list of
  what changed).
- Live: `plan.md`'s "The change", "In scope", "Not in scope", and
  "Deviations" sections; `git diff main...HEAD --stat` for the file
  list and the full diff for hunks; the draft description.

What good looks like: every changed file appears in the plan's files
list or is a test/changelog/docs entry for the fix, and every hunk
inside those files does the plan's change; nothing touches a "Not in
scope" item. Every in-scope step and every failure mode the plan names
has a matching hunk. The description's account of the change is a
true summary of the diff. A mismatch in either direction is re-tied
only by a sentence that names it and says why (a deviation in the plan
or a stated shortfall in the description).

Silent drift looks like: a dependency bump, refactor, or second fix
riding along; the plan's second failure mode with no hunk and no
mention; the diff catching an error the plan said would be prevented;
a description saying "no changes beyond the plan" above a diff that
has them.

## Test evidence (harness category: not-tested)

Where it lives:
- Eval: `### Test evidence` (commands, `# before:` / `# after:`
  lines, pasted output, exit codes, named tests), read against the
  plan's "Test plan" and the repro evidence's failing cases and
  controls in `## Plan context`. Statements about the repo's suite
  ("`cargo test` passes (614 tests)") may sit in the evidence or the
  description.
- Live: the before/after output and check results pasted into the
  draft description, read against `plan.md`'s "Test plan". The repo's
  checks are named in `docs/CONTRIBUTING.md` ("CI must be green": lint,
  typecheck, test-unit, test-integration, frontend; locally `make
  lint`, `make typecheck`, `make test-unit`) and in the PR template's
  Testing checklist.

What good looks like: one entry per case the test plan names. Each
failing case shows the command, what it did before (or a reference to
the repro), and a concrete after-result equal to the plan's
expected-after. Controls are reported specifically. Each repo check
is named with its outcome. A case or check that was not run is named
as not run, with the reason.

Not decisive: "tests pass", "works now", "CI green" with no command;
"(none provided)"; output for a different input than the repro; only
one of two named failure modes shown; a suite never mentioned when the
repo asks for it.

## Diff quality (harness category: unreviewable)

Where it lives:
- Eval: the `+` and `-` lines of `### Diff`, the file headers (what
  files exist in the change), and `### Commits` (a commit named
  "gofmt", "cleanup", "formatting", or "bump" is a pointer to look for
  churn).
- Live: `git diff main...HEAD` (look at each hunk), and `git diff
  main...HEAD --stat` for unexpected files.

What good looks like: a reviewer can read the diff top to bottom and
every line is part of the fix or its test. Removed lines are removed
because the fix replaces them.

Debris tells: `print(`, `console.log`, `dbg!`, `fmt.Println`, or
temporary log lines added; lines starting with a comment marker that
hold old code; new `TODO`/`FIXME`/`XXX`; `<<<<<<<`/`>>>>>>>`; files
such as `.orig`, `.bak`, `.DS_Store`, `.env`, `.vscode/`, build or
cache output; hunks where the removed and added lines do the same
thing in a different layout (two assignments joined into one, an
`if` block collapsed onto one line, reflowed whitespace, reordered
imports).

## Standards and comms (harness category: standards-wall)

Where it lives:
- Eval: `## Repo facts`'s `pull requests:` line (template sections,
  checklist, title convention, changelog/whatsnew entry, issue
  reference, gating rules such as "not accepting new features", "new
  linters need an approved discussion", "vouch flow") and its
  `contribution policy` line (AI policy wording, CLA, rules a
  maintainer restated); `## Thread highlights` for maintainer
  direction; read against `### Title`, `### Description`, and the
  diff (for required entry files like `CHANGELOG.md`).
- Live: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with
  `Closes #`, Changes, Testing checklist, Screenshots / Demo, Notes for
  Reviewers), `docs/CONTRIBUTING.md` (branch name
  `<type>/<issue-number>-<description>`, Conventional Commits, seeded
  bug xfail-marker removal, do not touch `# noqa` lines), the house
  rules in `scope.md`, and the course's AI-use disclosure requirement.

What good looks like: every template section has text about this PR
(an inapplicable one says "n/a" and why); every asked-for entry
exists; ticked checklist boxes are backed by the diff or evidence; the
AI disclosure, where required, names the tool and what it did; any
maintainer direction in the thread is followed or answered by name.

Visibly ignored looks like: no `Closes #` when the template asks for
it; a "User-facing changes" or "How Has This Been Tested" section
missing; a bug fix with no CHANGELOG entry when the guide asks for
one; a feature PR to a repo that says it is not accepting features;
an AI-policy repo with no disclosure, or "had claude look at it" where
the policy asks for tool and extent. Whether the description's claims
match the diff is plan fidelity, above, not this family.
