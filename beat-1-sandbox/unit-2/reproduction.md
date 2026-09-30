# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Sharon0motosho

---

## Posted upstream

**Claim comment**

<!-- TODO: replace with the comment's permalink once posted, then make sure the text below matches what was posted. -->
[claim comment permalink — not yet posted]

> Hi, I'd like to work on this as my first contribution here. Running the xfail test in `tests/unit/test_output_parser.py` (`test_json_array_fallback`) with `--runxfail` on current `main` gives me the same `AttributeError: 'list' object has no attribute 'items'` from `_parse_json_output` in `rag/generator/output_parser.py`. I'll post the full repro below. My next step is working out what an array response should turn into (one section per item, or falling through to the plaintext parser) and I'll report back here before opening a PR.

**Reproduction comment**

<!-- TODO: replace with the comment's permalink once posted, then make sure the text below matches what was posted. -->
[reproduction comment permalink — not yet posted]

**Reproduction: reproduced on current `main`**

Environment:
- Windows 11 Home (10.0.26200), Python 3.12.2
- Repo at `main`, commit `99673c7` (2026-09-16)
- Fresh venv with only what the parser module needs: `pip install structlog pytest` (structlog 26.1.0, pytest 9.1.1). I did not install the full project or use Docker; this module doesn't import anything else from the repo.

Steps (from a fresh clone):

```
git clone https://github.com/codepath/pathreview-ai301-fa26-howard.git
cd pathreview-ai301-fa26-howard
python -m venv .venv
.venv\Scripts\activate          # source .venv/bin/activate on Linux/macOS
pip install structlog pytest
pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
```

Observed (trimmed):

```
data = ['First feedback item', 'Second feedback item']

    def _parse_json_output(data: dict) -> list[FeedbackSection]:
        ...
>       for key, value in data.items():
E       AttributeError: 'list' object has no attribute 'items'

rag\generator\output_parser.py:68: AttributeError
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
1 failed, 18 deselected in 0.30s
```

Control: the same call with a top-level object instead of an array parses fine:

```
>>> import json
>>> from rag.generator.output_parser import parse_review_output
>>> parse_review_output(json.dumps({"strengths": "clear summary"}))
[FeedbackSection(section_name='strengths', content='clear summary', confidence=0.85, suggestions=[])]
>>> parse_review_output(json.dumps(["First feedback item", "Second feedback item"]))
AttributeError: 'list' object has no attribute 'items'
```

Without `--runxfail`, the whole file gives `18 passed, 1 xfailed`, so the marker is currently hiding exactly this failure.

Expected: an array response returns a `list[FeedbackSection]` instead of raising (the test only asserts `isinstance(result, list)`).
Actual: `parse_review_output` sends the parsed list straight to `_parse_json_output`, which assumes a dict and calls `.items()`.

What I haven't checked: I only ran this on Windows and didn't try the fenced-JSON path (an array inside a ```json code fence), although it calls the same `_parse_json_output`, so I'd expect the same error there.

I used Claude Code (an AI assistant) to run these steps and draft this comment. I reviewed the commands and the output before posting.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Three runs, in order:

1. `--only calib-01,calib-03,calib-04,pkg-20,pkg-03,pkg-09 --include-calibration`: **3/3**
   scored items (all 3 calibration packages agreed too, so 6/6 overall). This was a smoke
   run to check the harness wiring. I also used it to test the check I was least sure of,
   `ai-disclosure-when-required`, against the disclosure-wall package (pkg-20) and the two
   packages whose AI policies look strict but ask no disclosure of comments (pkg-03, pkg-09).
2. Full run with `--save-run eval-run.txt`: **no score**. The harness crashed before
   grading anything: `UnicodeEncodeError: 'charmap' codec can't encode character
   '\U0001f64f'`. On Windows, Python was writing the prompt to `claude` in cp1252, and one
   package contains an emoji. No file was written.
3. The same full run again with `PYTHONUTF8=1` set and the rubric unchanged: **20/20**. This
   is the committed run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

**Package analysis**

`pkg-16` (pandas-dev/pandas#66656, `rename_axis` with a tuple name).

- My rubric decided: **reject**.
- Gold label: **reject**. The note says the report "tested pandas 1.5.3 against an issue
  confirmed on latest and main, without acknowledging the deviation; the shown ValueError
  is that old version's behavior, not evidence about the reported bug."

The only required check that failed was `target-matches-issue`. Its evidence line from the
run: "Report used pandas 1.5.3 while issue is confirmed on 'latest version and main branch'
and thread confirms on 2.3.3/current main, with no acknowledgment of the version gap."

The interesting part is what passed. `artifact-shows-issue-behavior` passed ("Traceback
shows `ValueError: Length of new names must be 1, got 3` using the issue's own trigger"),
and so did `claims-backed-by-evidence`. The report used the issue's exact code, and a
`ValueError` from `rename_axis` looks like the crash the issue describes. My artifact check
only compares the input against the issue's trigger and the output against its symptom, and
both matched. It has no idea that the same symptom on a two-year-old release says nothing
about today's bug. That is why the version question is its own check. If I had folded it
into "environment recorded", this package would have passed, because it does record its
environment, completely. The problem is that it records the wrong one and never says so.
It agrees with the gold label for the right reason, but only one check is holding it.

**Check rationale**

`ai-disclosure-when-required`, quoted from the `rubric.md` uploaded to `tools/repro-check/`:

> First decide what the policy requires of issue comments. If the policy requires disclosing AI use in comments or issues, or requires disclosing "all AI usage in any form", pass only when the claim or the report discloses AI assistance (the tool or the fact of assistance, and its extent). If the policy has no AI rule, only regulates code or pull requests, or only asks that comments be in the contributor's own words, pass: no disclosure is required there, and a comment that reads as the contributor's own words meets an own-words rule. Fail only when a disclosure requirement that covers comments exists and neither comment discloses.

Before the first run I read every package's repo-facts line. The first version I considered
was "if the repo has an AI policy, the comments must disclose AI use". I rejected it because
six of the twenty repos have an AI policy and they ask for very different things. ripgrep
(pkg-03) wants comments "written by humans in their own words", which is a rule about voice,
not about disclosure. fd (pkg-09) wants the tool named "in the pull request" and says "the
policy states no disclosure ask for issue comments". Both are gold accepts, and the simpler
check would have rejected both. So the check starts with "First decide what the policy
requires of issue comments" and names each shape that asks nothing of a comment. That way
the grader has to read the policy and cannot react to the word "AI". The package set is
treated as AI-assisted ("course packages are treated as AI-assisted work"), so the check
tells the grader to assume that. Otherwise ghostty's "all AI usage in any form must be
disclosed" could be dodged by guessing that no AI was used.

**Trade-offs**

The own-words clause gives something up. Under this check, a comment in a repo like
ripgrep's passes as long as it *reads* like the contributor's own words, and a grader can't
tell from the text whether a person actually wrote it. An AI-drafted comment that sounds
human would pass. I accept that miss, because the only other choice is failing every
comment in an own-words repo, and that would reject pkg-03, a gold accept.

I checked that this check didn't cost anything elsewhere before the full run: the smoke run
put pkg-03 and pkg-09 in as canaries next to pkg-20. pkg-20 rejected (ghostty's policy
covers comments, and neither comment discloses), and pkg-03 and pkg-09 accepted. The
confirming full run had disclosure 1/1 and clear-accept 8/8, and pkg-05 (conda's
"permissive-with-responsibility" policy), pkg-07 (p5.js, where the comment does
disclose), and pkg-12 (prettier's policy on AI code quality) also accepted. No package that should pass was held by this check.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
