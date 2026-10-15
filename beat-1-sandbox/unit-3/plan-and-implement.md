# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Sharon0motosho

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/69#issuecomment-6203820838

> **Plan for #69**
>
> Building on my repro above: the crash is at `output_parser.py:68`, where `_parse_json_output` calls `.items()` on the list that `json.loads` returned. My control (the same call with a top-level object) parses fine, so the dict path works. What's missing is a type check before the decoded value gets there.
>
> Change (one function, `parse_review_output` in `rag/generator/output_parser.py`): after `json.loads` in both the fenced and the raw branch, only call `_parse_json_output` when the result is a dict. Anything else gets a warning log and goes to the existing `_parse_plaintext_output(raw)` fallback, so the array comes back as one `general_feedback` section with the original text.
>
> I know other plans here also use the plaintext fallback. I picked it over one-section-per-item for my own reason: `ReviewGenerator.generate_section` only keeps `sections[0]`, so per-item sections would silently drop every item after the first.
>
> Tests: remove the `strict=True` xfail marker from `test_json_array_fallback` and add a test for an array inside a ```json fence. To check it, I'll re-run my repro command, which should go from `1 failed` (the `AttributeError`) to `1 passed`, and re-run the object control to confirm it's unchanged. The whole file should go from `18 passed, 1 xfailed` to `20 passed`.
>
> Not touching: `_parse_json_output`, the `sections = []  # noqa: F841` line (a separate seeded defect per CONTRIBUTING), or the caller in `review_generator.py`. If maintainers would rather have structured per-item sections, that needs the caller changed too, so I'd keep it as a separate issue.
>
> Open question: I've only run this on Windows / Python 3.12.
>
> I used Claude Code to help trace the code and draft this plan. I checked the caller and the line numbers myself.

---

## Your branch

**Branch**

fix/69-json-array-fallback

**Evidence**

Re-ran my Unit 2 repro step (Windows 11, Python 3.12.2, structlog 26.1.0, pytest 9.1.1) and the object/array control.

**Before** (on `main` at `99673c7`):

```
$ pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
data = ['First feedback item', 'Second feedback item']

    def _parse_json_output(data: dict) -> list[FeedbackSection]:
        ...
>       for key, value in data.items():
E       AttributeError: 'list' object has no attribute 'items'

rag\generator\output_parser.py:68: AttributeError
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
1 failed, 18 deselected in 0.76s

$ pytest tests/unit/test_output_parser.py -q
18 passed, 1 xfailed in 0.72s
```

**After** (on `fix/69-json-array-fallback` at `82093cd`):

```
$ pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
.                                                                        [100%]
1 passed, 19 deselected in 0.65s

$ pytest tests/unit/test_output_parser.py -q
....................                                                     [100%]
20 passed in 0.35s
```

Control from the repro, re-run on the branch:

```
>>> parse_review_output(json.dumps({"strengths": "clear summary"}))
[FeedbackSection(section_name='strengths', content='clear summary', confidence=0.85, suggestions=[])]
>>> parse_review_output(json.dumps(["First feedback item", "Second feedback item"]))
[warning  ] raw_json_not_an_object         json_type=list
[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]
```

The object case is unchanged, and the array now comes back as one section instead of raising. `ruff check`, `black --check`, and `mypy` on the changed files all pass.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

There were four runs. The first one did not get a score because the CLI was not found. The second run used `--only` on 6 packages and scored 6/6. The third run was a full run, but it crashed with a `UnicodeEncodeError` and did not get a score. The fourth run completed successfully and scored 20/20.

`agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

For pkg-04, my rubric's verdict was **reject**, and the gold label was also **reject**. The plan only involves documentation and passes the other plan-quality checks. However, the owner had already pointed out `src/tui/light_windows.go` and posted a patched build, but the comment did not mention any of that. Because it did not address the direction already given by the owner, it failed the `thread-direction-engaged` check. The run's evidence for that check:

> "Owner said 'This seems to be the culprit' (src/tui/light_windows.go lines 70-84) and posted patched binary 8916cbc; plan is docs-only and the comment never mentions the owner's finding or patch."

**Check rationale**

> | thread-direction-engaged | The thread highlights (maintainer, owner, or collaborator comments: a located culprit, a proposed or rejected approach, a posted patch or test build, an open PR or prior art), read against the plan and the comment | If the thread contains no such direction, pass. If it does, pass when the plan or comment either follows that direction or names it and explains why it departs. Fail when the plan goes a different way and the comment never mentions the direction (for example, the owner pointed at a source file and posted a patched build, and the plan only documents a workaround), or when the plan takes an approach a maintainer already rejected without acknowledging it. | required |

I only count direction from a maintainer, owner, or collaborator because not every comment in a thread is necessarily correct. For example, calib-03 shows that someone in the thread can be confidently wrong, so following any comment without considering who said it would not always make sense.

The rule is to follow the direction given or explain why you are choosing a different approach, not that you must always obey it. Packages like pkg-03 and pkg-09 engage with open PRs and should still pass because they address the situation instead of ignoring it.

**Trade-offs**

The downside is that even a weak explanation for going against the direction can still pass, as long as a reason is given. This means the check does not necessarily confirm that the reason is a good one.

I checked that the change did not break other parts of the rubric by using pkg-04, pkg-09, pkg-14, and pkg-20 as canaries in the 6/6 run. The full run also scored 7/7 on clear-accept and 2/2 on thread-convention, which shows those checks were still working as expected.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
