Title: fix(rag): fall back to plaintext when parsed JSON is not an object

## Summary
`parse_review_output` in `rag/generator/output_parser.py` passed whatever `json.loads` returned straight to `_parse_json_output`, which calls `.items()`, so a top-level JSON array (raw or inside a ```json fence) crashed with `AttributeError: 'list' object has no attribute 'items'`. This PR implements my posted plan on #69: after each `json.loads`, only a dict goes to the JSON parser; any other JSON value is logged with a warning and goes to the existing `_parse_plaintext_output(raw)` fallback, so the array comes back as one `general_feedback` section holding the original text. I used the plaintext fallback rather than one section per array item because the only caller, `ReviewGenerator.generate_section`, keeps only `sections[0]`.

## Issue
Closes #69

## Changes
- `rag/generator/output_parser.py`: `isinstance(data, dict)` check after `json.loads` in both the fenced branch and the raw branch of `parse_review_output`; non-dict values log `json_not_an_object_in_fence` / `raw_json_not_an_object` and return `_parse_plaintext_output(raw)`.
- `tests/unit/test_output_parser.py`: removed the `strict=True` xfail marker from `test_json_array_fallback`, and added `test_json_array_in_code_fence_falls_back_to_plaintext`.

Nothing else changed. Not touched, as the plan says: `_parse_json_output` and the dict path, the `sections = []  # noqa: F841` line (a separate seeded defect per CONTRIBUTING), and `review_generator.py`.

## Testing
Windows 11, Python 3.12.2. Before = `main` at `99673c7`, after = this branch at `82093cd`.

Repro from my #69 comment:
```
$ pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
# before:
E       AttributeError: 'list' object has no attribute 'items'
rag\generator\output_parser.py:68: AttributeError
1 failed, 18 deselected in 0.76s
# after:
1 passed, 19 deselected in 0.65s
```

Control from the repro (object unchanged, array no longer raises):
```
>>> parse_review_output(json.dumps({"strengths": "clear summary"}))
[FeedbackSection(section_name='strengths', content='clear summary', confidence=0.85, suggestions=[])]
>>> parse_review_output(json.dumps(["First feedback item", "Second feedback item"]))
[warning  ] raw_json_not_an_object         json_type=list
[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]
```
Before (on `99673c7`), the second call gave `AttributeError: 'list' object has no attribute 'items'` (full trace in my #69 repro comment).

Whole test file, marker gone and one new fenced-array test:
```
$ pytest tests/unit/test_output_parser.py -q
# before:
18 passed, 1 xfailed in 0.72s
# after:
20 passed in 0.35s
```

Repo checks, run locally on the branch:
```
$ ruff check .                      -> All checks passed!
$ black --check .                   -> 110 files would be left unchanged.
$ mypy api/ core/ ingestion/ rag/ agent/ safety/ --ignore-missing-imports
                                    -> Success: no issues found in 76 source files
$ LLM_PROVIDER=mock pytest tests/unit -q
                                    -> 377 passed, 52 xfailed in 28.26s
```
(The 52 xfails are other seeded bugs; none are for #69 any more.)

- [ ] **CI is green on this PR (all five jobs)** — not yet: this is my first PR from this fork, so CI may wait for a maintainer to approve the workflow run. I'll update this box once it has run.
- [x] Unit tests pass (`make test-unit`) — 377 passed, 52 xfailed (output above)
- [ ] Integration tests pass (`make test-integration`) — not run locally; `tests/integration/` has no tests yet, and CI treats an empty collection as success
- [x] Linter passes (`make lint`) — ruff and black clean
- [x] Type checker passes (`make typecheck`) — mypy clean
- [x] New/updated tests cover the changes — `test_json_array_fallback` (raw array) and the new fenced-array test
- [x] If this fixes a seeded bug: removed its `@pytest.mark.xfail` marker (and any matching suppression in `pyproject.toml`) — marker removed; there is no `pyproject.toml` suppression for #69

Not run locally: the `frontend` job (`npm test`). This PR doesn't touch `frontend/`, so I'm relying on CI for it.

## Screenshots / Demo
n/a: backend parser change with no UI; the before/after output is above.

## Notes for Reviewers
- The fallback section's content is the raw JSON text, brackets and quotes included (`'["First feedback item", "Second feedback item"]'`), because `_parse_plaintext_output` gets the original string. If you'd rather have structured per-item sections, that needs `ReviewGenerator.generate_section` changed too, so I'd raise it as a separate issue.
- A bare JSON string or number also goes to the fallback now (it crashed the same way before); I didn't add a separate test for it.
- I've only run this on Windows / Python 3.12; CI covers Linux / 3.11.

AI use: I used Claude Code (Claude Opus) to help trace the parser code, run the tests and checks, and draft this description. I reviewed the diff and re-checked the output above myself.
