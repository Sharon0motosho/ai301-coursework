# Evidence guide: where proof lives in a reproduction package

This is the map for `rubric.md`. For each family of proof it says where
to look (in an eval bundle and in live mode) and what good looks like.

## Environment

- Where it lives:
  - Eval bundle: the "Environment:" line or block at the top of the
    candidate repro report; version notes sometimes sit inside the
    steps (e.g. `pip install foo==1.2`). The issue's own environment
    is in the issue body; "affected version" notes from maintainers are
    in the thread highlights. The latest release is in the repo-facts
    block.
  - Live mode: the environment section of the student's repro draft;
    the issue body and its thread on GitHub; the repo's releases page.
- What good looks like: the software under test is named with a
  version (and how it was installed, if that matters), plus the OS or
  platform. Every factor the issue calls out as relevant is named too:
  a driver for a Windows-only bug, a backend or shell, a debug vs.
  release build. If the version differs from the issue's, the report
  says so in words ("filed on X, tested on Y, same behavior"). An
  older version than the one the issue is confirmed on, with no
  comment, is a silent deviation, not an environment record.

## Steps

- Where it lives:
  - Eval bundle: the "Steps" part of the candidate repro report,
    including code blocks and any file contents quoted there.
  - Live mode: the steps in the repro draft, and any file or script it
    quotes. Files in the student's working directory that the draft
    does not quote do not count.
- What good looks like: from a clean starting state to the trigger, the
  exact commands, code, or inputs are shown (or the issue's script is
  named as run verbatim). A stranger could copy them and get to the
  same point. Private repos, "our internal config", and steps like
  "set up the project" or "do the usual" break this. Terse is fine;
  a short numbered list with the real commands is complete.

## Behavior shown

- Where it lives:
  - Eval bundle: the code blocks in the candidate repro report (output
    excerpts, tracebacks, logs, printed values, exit codes), plus the
    "Expected / Actual" lines. Compare them with the symptom in the
    issue body: the exact error text, exit code, wrong value, or
    missing output the issue reports, and the trigger it names.
  - Live mode: the output pasted in the repro draft, compared with the
    issue on GitHub.
- What good looks like: the input used is the issue's trigger (same
  syntax, flag, operator, range, or data shape), and the artifact shows
  the issue's symptom itself. Watch for adjacent symptoms: a graceful
  argument error where the issue has a panic, a compile error where
  the issue has a runtime error, a garbled-but-alive process where the
  issue has a crash, or output that only proves the tool started. If
  the input differs from the issue's even slightly, check whether the
  artifact is still the issue's behavior. A cannot-reproduce shows the
  output of a real attempt at the trigger.

## Honesty

- Where it lives:
  - Eval bundle: the claim comment's summary ("reproduced", "not
    platform-specific") and the report's "Actual" line and closing
    sentences, each set next to the artifact it points at.
  - Live mode: the same sentences in the drafts.
- What good looks like: every "confirmed", "the cause is", "verified",
  "guaranteed", or "on every setup" has a shown artifact behind it. The
  stated outcome matches the output: an honest cannot-reproduce says
  what was tried, what happened, and what differed from the issue's
  setup, and that is a good report. Red flags: a root cause asserted
  with no trace or experiment shown; certainty ("100% reproducible",
  "on two machines") on top of the wrong artifact; widening the bug to
  a version or platform the thread says does not show it.

## Comms

- Where it lives:
  - Eval bundle: the candidate claim comment (read against the issue's
    title and body), and the repo-facts block's "bug reports" and
    "contribution policy" lines (read against both comments).
  - Live mode: the claim draft; the repo's CONTRIBUTING.md, AI policy
    files (AI_POLICY.md, AGENTS.md, CONTRIBUTING sections), and issue
    templates on GitHub; for Path Review, the house rules in
    `scope.md`.
- What good looks like: the claim names this issue's specifics (the
  symptom, the component, the file, or the first thing the writer will
  investigate) and promises only an investigation or a next step,
  never a fix or a date. For AI use, read the policy for what it asks
  of *comments*: "disclose all AI usage" or "disclose in issues and
  comments" means the comments must say AI was used and how much.
  Treat eval packages as AI-assisted work. A policy that only covers
  pull requests or code, that only asks for comments in the
  contributor's own words, or no AI policy at all, asks no disclosure
  of a comment. Boilerplate like "Can I be assigned? I'll have a fix
  in 2 days" fails on specificity and honesty together.
