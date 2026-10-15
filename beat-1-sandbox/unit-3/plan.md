# Plan: issue #69, output parser crashes on a top-level JSON array

Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/69
Repro it builds on: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/69#issuecomment-5998719854
Branch: `fix/69-json-array-fallback` (on my fork)

## Diagnosis

My repro shows the crash at `rag/generator/output_parser.py:68`, inside
`_parse_json_output`, with `data = ['First feedback item', 'Second
feedback item']`. `parse_review_output` calls `json.loads` and passes
whatever comes back straight to `_parse_json_output`, which assumes a
dict and calls `.items()`. My control run (the same call with a
top-level object, `{"strengths": "clear summary"}`) parses fine, so
the JSON decoding and the dict path both work; what's missing is a
check on the type of the decoded value before it reaches the dict
parser. The fenced-JSON branch (lines 37-44) passes its decoded value
the same way, so a fenced array should hit the same crash.

## The change

One bounded change in `rag/generator/output_parser.py`, function
`parse_review_output`: after each `json.loads` (the fenced branch and
the raw branch), only call `_parse_json_output(data)` when `data` is a
dict. Anything else (a list, or a bare string/number) gets a
`logger.warning` and goes to the existing `_parse_plaintext_output(raw)`
fallback, which returns one `general_feedback` section holding the
original text.

Why the plaintext fallback and not one section per array item: the only
caller, `ReviewGenerator.generate_section` in
`rag/generator/review_generator.py` (lines 76-80), keeps only
`sections[0]`. If each array item became its own section, every item
after the first would be silently dropped. The fallback keeps all of
the model's text in the one section the caller uses. Other plans on the
thread chose the same fallback; I got there from that caller.

In scope:
- the type check and fallback in `parse_review_output` (both branches)
- `tests/unit/test_output_parser.py`: remove the `@pytest.mark.xfail`
  marker from `test_json_array_fallback` (it is `strict=True`, so it
  would turn into an `XPASS(strict)` failure once the bug is fixed), and
  add one test for an array inside a ```json fence

Not in scope:
- `_parse_json_output` itself and the dict path (my control shows it works)
- the `sections = []  # noqa: F841` accumulator at the top of
  `parse_review_output`: CONTRIBUTING says it's a separate seeded defect
  and not to touch annotated `# noqa` lines
- turning array items into structured sections (would need the caller
  changed too; I'd raise that separately if a maintainer wants it)
- `_consolidate_feedback` or anything else in `review_generator.py`

## Test plan

1. Re-run my Unit 2 repro step on the branch:
   `pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q`.
   Before: `1 failed` with `AttributeError: 'list' object has no attribute 'items'`.
   Expected after: `1 passed`.
2. Re-run my control from the repro: `parse_review_output` on
   `{"strengths": "clear summary"}` still returns the same
   `FeedbackSection(section_name='strengths', ...)`, and on the array it
   now returns one `general_feedback` section whose content contains
   both items instead of raising.
3. The new fenced-array test passes.
4. Whole file without `--runxfail`: before `18 passed, 1 xfailed`;
   expected after `20 passed` (no xfail left, one new test).
5. `make lint` / `make typecheck` on the changed file, since CI runs both.

## Risks and unknowns

- I haven't checked whether a maintainer would rather get structured
  per-item sections. I'm going with the smaller change and saying so in
  the comment.
- I'm only testing on Windows / Python 3.12, like the repro.
- A bare JSON string or number (e.g. raw output `"42"`) hits the same
  `.items()` crash today. My type check covers it because it routes
  every non-dict, but I'm not adding a separate test for it.

## Deviations

Nothing changed; the plan held. The commit on `fix/69-json-array-fallback`
touches only the two files listed in scope: the `isinstance(data, dict)`
check in both branches of `parse_review_output`, plus removing the xfail
marker and adding the fenced-array test. Every test-plan step came out the way the plan
expected (`1 failed` -> `1 passed`, object control unchanged,
`18 passed, 1 xfailed` -> `20 passed`), and ruff, black and mypy were
clean on the changed files.

One thing the build made concrete that the plan didn't spell out: the
fallback section's content is the raw JSON text,
`'["First feedback item", "Second feedback item"]'`, brackets and quotes
included, because `_parse_plaintext_output` gets the original string. That
is what "the original text" in the plan means, so I'm not counting it as a
deviation. If a reviewer wants the items joined into plain lines instead, that would
be a follow-up change, and I'd record it here.
