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
