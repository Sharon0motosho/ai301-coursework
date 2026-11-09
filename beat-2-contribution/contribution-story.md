# Contribution: [FEATURE] improve API documentation

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]

**Student:** Sharon Omotosho

**GitHub Username:** Sharon0motosho

**Issue:** https://github.com/PowerGridModel/power-grid-model-io/issues/243

**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 9: Building and improving my issue scouting tool**

I built a fit profile, created search queries, and worked on ranking rules to help find open-source issues that match my skills and interests. My first scan accepted 0 out of 5 issues because my ranking rules prioritized issues with comments, which often meant they had already been claimed. I changed the ranking to prioritize issues with no comments, checked for claims before selecting issues, and expanded the response sample to 100 issues. After reviewing 38 issues, I also noticed that Python issues tagged as good first issues tend to get claimed within a few days, but the website does not always show those claims.

**Unit 10: Upstream voice rules, a Windows fix to the voice guard, and my repo memory file**

I added an Upstream section to my voice guide with four rules: read the thread and linked PRs before claiming, name a first step and a report-back day, ask one question with what I already checked, and nudge only once after 7 quiet days. On Windows the guard stopped every post with "claude CLI is not on PATH" even though Claude was installed, because Python's `subprocess` can't launch `claude.cmd` by its short name; I changed one line in `check_voice.py` to look up the full path with `shutil.which`, and after that it passed my claim through the hook (guard log, 2026-11-08 22:21). On my report-back comment it stopped me three times: twice fairly (I summarized output instead of pasting it, and stated a cause I hadn't shown), and once wrongly, when my claims rule fired on a plain status line, so I reworded the line instead of promising a date I hadn't picked.

**Unit 11: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 12: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

This issue came from the AI301 Issue Selection site, not from my scan. My scout's first scan graded five issues from my own search queries but didn't accept any of them. I decided to use the site to look for issues myself. After reviewing 38 issues, I found two that were still available and matched my skills: this one and common-voice/common-voice#3101. My scout accepted both. After making changes to the scout in Unit 10, I graded them again, and both were still accepted, but with different difficulty ratings.

This issue was rated straightforward because it clearly explains the problem, identifies the exact generated page, and provides an example of what needs to be fixed (`functions._functions.has_value` should be `functions.has_value`). I also found an earlier PR, #491, that made the same fix by changing just one file, which showed me the change was small. The common-voice issue was rated ambitious because the cause of the problem was unknown and the report was from 2021 on Edge 90. Its maintainers also hadn't responded to any of their five most recent outside issues, while this organization had responded to an outside issue on the same day.

I chose this issue because it involves Python, and it seemed like a small change that I could complete within two weeks. The organization's CONTRIBUTING.md also allows AI-assisted work as long as I disclose it using an `Assisted-by:` line. I also wanted to learn how Sphinx autodoc decides which module path to show in the documentation and how to work with signed commits and DCO sign-offs, since the organization requires them.

Before claiming the issue, I read through the discussion and the earlier PR. I noticed that PR #491 had received a positive response from a maintainer, who said it "looks good to go," but it was eventually closed because the commits weren't signed. My scout didn't catch this, so I made sure to mention it in my claim and stated that I would sign my commits.

I also noticed a few problems when using my scout to find issues. My original ranking rules prioritized issues with 1-5 comments, which often meant that the issues had already been claimed. I changed the ranking to prioritize issues with zero comments. I also found that neither the scout's search results nor the Issue Selection site showed claim comments, so I still had to check each issue myself. The scout did get some things right, though. It correctly rejected issues that were already claimed and a large 567-feature tracker. Both issues it accepted were also still available when I checked them again before claiming.

Claim comment: https://github.com/PowerGridModel/power-grid-model-io/issues/243#issuecomment-6731437884

**Toolkit**

- Tool: issue-scout. I used it to scan issues from my search queries. I also used the Issue Selection site to find and grade two issues, then graded them again after making changes to the scout in Unit 10.
- What it produced: It accepted #243 with all eight checks passing and rated it straightforward. It also accepted common-voice#3101 but rated it ambitious and flagged its maintainer response history.
- Difficulties: My first scan accepted 0 out of 5 issues because my ranking rules prioritized issues that already had comments. The search results also didn't show whether an issue had been claimed. My scout also missed that the earlier PR #491 was closed because its commits weren't signed.
- What changed: I changed the ranking to prioritize issues with zero comments and updated my checks to consider whether an issue was manageable. I also started reading issue discussions and checking linked PRs myself before claiming an issue.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Windows 11 Home (10.0.26300), Python 3.14.7, uv 0.13.7, Sphinx 9.1.1, and power-grid-model-io at `main` commit `accd97e` (2026-11-05). I forked the repo with `gh repo fork PowerGridModel/power-grid-model-io --clone` and installed the docs dependencies with the same command the repo's `.readthedocs.yaml` uses, `uv sync --frozen --group doc`.

Challenges: `uv` wasn't installed, so I installed it with `pip install --user uv` and run it as `python3 -m uv`. I also cloned outside my OneDrive folder (`C:\Users\kanyi\code`) so OneDrive wouldn't try to sync the virtual environment. Read the Docs builds on Linux, and I only built on Windows.

**Steps to Reproduce**

1. `uv sync --frozen --group doc`
2. `uv run sphinx-build -b html docs docs/_build/html -q -E`
3. `grep -o 'id="power_grid_model_io[^"]*\._[a-z_]*\.[A-Za-z_]*"' docs/_build/html/power_grid_model_io.html | sort`
4. Observed result: the API page documents 10 public objects under a private module path, including the issue's own example.

**Reproduction Evidence**

- **Commit showing reproduction:** none (built on unchanged `main`, `accd97e`)
- **Screenshots/logs:**

```
id="power_grid_model_io.data_types._data_types.StructuredData"
id="power_grid_model_io.functions._functions.both_zeros_to_nan"
id="power_grid_model_io.functions._functions.complex_inverse_imaginary_part"
id="power_grid_model_io.functions._functions.complex_inverse_real_part"
id="power_grid_model_io.functions._functions.degrees_to_clock"
id="power_grid_model_io.functions._functions.get_winding"
id="power_grid_model_io.functions._functions.has_value"
id="power_grid_model_io.functions._functions.is_greater_than"
id="power_grid_model_io.functions._functions.value_or_default"
id="power_grid_model_io.functions._functions.value_or_zero"
```

- **My findings:** the issue is real on today's `main`. All 10 objects are exported publicly from `functions/__init__.py` and `data_types/__init__.py`, but `docs/power_grid_model_io.md` runs `automodule` on the private modules (lines 39 and 46). The same build showed three related problems the issue doesn't mention: `functions.filters` is in `__all__` but never documented, and the docs still list `data_stores.csv_file_store` and `utils.regex`, which fail with `No module named ...` because those modules no longer exist. I posted these findings on the issue and asked whether the extra three belong in this PR or a separate one: https://github.com/PowerGridModel/power-grid-model-io/issues/243#issuecomment-6731668518

**Toolkit**

- Tool: repro-check, first on my claim alone and then on my claim plus repro report; the voice guard on both comments before they posted.
- What it produced: accept both times. The only miss was the preferred `control-run` check, because I had no contrasting build yet.
- Difficulties: the voice guard stopped my report-back comment three times. Twice it was right (I described the output instead of pasting it, and named a cause without showing it). Once it applied my claims rule to a line that wasn't a claim.
- What changed: I pasted the real `grep` output and the two directive lines into the comment and worded the cause as unconfirmed. My build with the fix will be the control run.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
