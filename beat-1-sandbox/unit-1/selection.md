# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/69

**Verdict output**

Live-mode run of the installed skill on three candidates (#62, #69, #54). The
skill ranked #69 first. Output pasted verbatim:

```
Ran all three candidates through the rubric against live GitHub data (repo facts, issue bodies, comments, linked-PR search). All three sit in-scope (`codepath/pathreview-ai301-fa26-howard`), so the Path Review claim-comment house rule applies throughout.

**Repo-level facts (shared by all three):** not archived, newest commit 2026-09-16 (well within 180 days) → `repo-alive` pass. No `CONTRIBUTING.md`/AI-policy file anywhere in root or `.github/` → silence passes `ai-policy-allows-me`. No releases exist → `ships-releases` fails (preferred only). Sampled issue #43 got a COLLABORATOR reply within 2 days → `maintainer-answers-issues` passes.

**Ranked read-out (all three accepted):**

1. **#69 – Output parser crashes on top-level JSON array fallback** — best fit: zero infra dependency (pure Python logic in `rag/generator/output_parser.py`), exact file paths and an existing `xfail` test to un-mark, which is a near-perfect match for practicing "fix + test" review cycles with no Docker/DB/Redis in the way.
2. **#62 – Health check references `settings.redis_host`** — strong backend/API fit given FastAPI/Flask experience, very concrete repro (`GET /health` → 503 + logged `AttributeError`), small diff (fix attribute name mismatch in `Settings`).
3. **#54 – Resume section detection fails on leading whitespace** — good fit too (self-contained regex fix in `resume_parser.py` with a worked code example and 3 failing tests), ranked last only because "ingestion" is a step further from the user's REST-API background than the other two.

No candidate failed a required check: none unassigned/unclaimed by scope rules, each is one bounded change with a settled definition of done (opener is a `COLLABORATOR` and/or carries `good first issue` + concrete repro).

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/69",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; newest default-branch commit 2026-09-16, within 180 days of 2026-09-28"},
      {"name": "ai-policy-allows-me", "grade": "pass", "evidence": "No CONTRIBUTING.md, AI_POLICY.md, or AGENTS.md found in repo root or .github/ — silence passes"},
      {"name": "nobody-on-it-now", "grade": "pass", "evidence": "assignees: none; 0 comments; PR search for 'is:pr 69' returned total_count:0"},
      {"name": "one-bounded-change", "grade": "pass", "evidence": "Body names exact files (rag/generator/output_parser.py, tests/unit/test_output_parser.py), one fix, effort estimate 2-4 hours"},
      {"name": "outcome-is-settled", "grade": "pass", "evidence": "Opener Aburke225 has author_association Collaborator; issue carries 'good first issue' label and specifies the exact xfail marker to remove"},
      {"name": "maintainer-answers-issues", "grade": "pass", "evidence": "Sampled issue #43 drew a COLLABORATOR reply from Aburke225 2 days after opening"},
      {"name": "ships-releases", "grade": "fail", "evidence": "Releases endpoint returned an empty list — no releases exist"},
      {"name": "signposted-for-newcomers", "grade": "pass", "evidence": "Labels include 'good first issue'; body gives exact repro (top-level JSON array causes .items() AttributeError)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/62",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; newest default-branch commit 2026-09-16, within 180 days of 2026-09-28"},
      {"name": "ai-policy-allows-me", "grade": "pass", "evidence": "No CONTRIBUTING.md/AI policy file found anywhere in the repo — silence passes"},
      {"name": "nobody-on-it-now", "grade": "pass", "evidence": "assignees: none; 0 comments; PR search for 'is:pr 62' returned total_count:0"},
      {"name": "one-bounded-change", "grade": "pass", "evidence": "Single mismatch between settings.redis_host/redis_port and the Settings class's redis_url field, causing a 503 on GET /health"},
      {"name": "outcome-is-settled", "grade": "pass", "evidence": "Opener Aburke225 has author_association Collaborator; issue carries 'good first issue' label"},
      {"name": "maintainer-answers-issues", "grade": "pass", "evidence": "Sampled issue #43 drew a COLLABORATOR reply from Aburke225 2 days after opening"},
      {"name": "ships-releases", "grade": "fail", "evidence": "Releases endpoint returned an empty list — no releases exist"},
      {"name": "signposted-for-newcomers", "grade": "pass", "evidence": "Labels include 'good first issue'; body gives exact repro (GET /health returns 503, AttributeError logged)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/54",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "archived: false; newest default-branch commit 2026-09-16, within 180 days of 2026-09-28"},
      {"name": "ai-policy-allows-me", "grade": "pass", "evidence": "No CONTRIBUTING.md/AI policy file found anywhere in the repo — silence passes"},
      {"name": "nobody-on-it-now", "grade": "pass", "evidence": "assignees: none; no linked/mentioned PR found; one claim comment (williamnelson170708-dotcom, 2026-09-18) exists but the Path Review house rule in scope.md says classmate claim comments do not block"},
      {"name": "one-bounded-change", "grade": "pass", "evidence": "Single regex-anchoring bug in _detect_sections() with a worked code example and 3 named failing tests"},
      {"name": "outcome-is-settled", "grade": "pass", "evidence": "Opener Aburke225 has author_association COLLABORATOR; issue carries 'good first issue' label"},
      {"name": "maintainer-answers-issues", "grade": "pass", "evidence": "Sampled issue #43 drew a COLLABORATOR reply from Aburke225 2 days after opening"},
      {"name": "ships-releases", "grade": "fail", "evidence": "Releases endpoint returned an empty list — no releases exist"},
      {"name": "signposted-for-newcomers", "grade": "pass", "evidence": "Labels include 'good first issue'; body gives a worked code example showing the bug"}
    ],
    "verdict": "accept"
  }
]
```
```

The verdict for issue #69 is `accept`.

---

## Eval iterations

**Run history**

Four runs, in order:

1. `--only issue-01,issue-14,issue-20` — **2/3**. A three-issue smoke run to check
   the harness wiring and the two checks I was least sure of. `issue-01` disagreed.
2. `--only issue-01,issue-05,issue-10,issue-20` — **4/4**. Re-run of the whole
   scope family after rewriting two checks, to confirm the fix for `issue-01`
   had not leaked the umbrella cases.
3. `--only issue-17,issue-12,issue-08,issue-09,issue-15` — **5/5**. Canaries, one
   from each category the first three runs had not touched (dead-repo, policy,
   claimed) plus the two accepts I judged riskiest.
4. Full run, `--save-run eval-run.txt` — **20/20**. This is the committed run:
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.

**Issue analysis**

`issue-01` (conda/conda#16475, "Add permanent docs for installing PyPI packages
with `conda install`").

- Gold label: **accept**.
- My rubric's first decision: **reject**. Current decision after revision:
  **accept**.

The first version of `one-bounded-change` failed an issue whose body "enumerates
deliverables that would naturally ship as separate pull requests (authoring a new
document *and* restructuring other documents, for instance)." `issue-01` is
exactly that shape on its face: it asks for a new task page, plus edits to
`manage-pkgs.rst`, plus edits to `pip-interoperability.rst`, plus edits to
`new-features.md`, plus an optional `troubleshooting.rst` entry. My check counted
five bullets and read an umbrella. `outcome-is-settled` failed it too, because the
opener is a `CONTRIBUTOR` rather than a maintainer, the only label is
`type::documentation` rather than `good first issue`, and the thread is empty — so
neither of that check's authority signals fired.

Both reads were wrong for the same underlying reason: I had written checks that
counted surface features instead of asking the question the check exists to ask.
A docs page and the pages that must now point at it are not separate pull
requests; they are one change plus the edits that keep the rest of the docs
consistent with it. No maintainer would want those split. And the long
prescriptive body that made the issue *look* like an umbrella is the thing that
makes it safe — it is a specification, and a specification settles what "done"
means no matter who typed it. Both checks were treating detail as a warning sign
when detail is the opposite.

**Check rationale**

`one-bounded-change`, quoted from the `rubric.md` uploaded to
`tools/issue-select/`:

> The issue asks for one change a newcomer could ship as a single pull request. It
> fails on exactly three shapes: an explicit umbrella, tracking, or "mega" issue
> that exists to be split into separate work; an open-ended standing invitation for
> repeated incremental contributions, with no state in which the issue is finished;
> or a usage/support question rather than a request for a change. Size alone never
> fails it. One change that edits many files is one change, a long prescriptive body
> is a spec rather than an umbrella, and one deliverable plus the edits that keep the
> surrounding files consistent with it — a new docs page plus the pages that must now
> point at it, a new function plus its call sites — is still one change. A terse body
> is not a fail on its own.

It is written as a closed list of three failing shapes rather than as a judgement
about size because the `issue-01` disagreement showed me that every proxy I
reached for — bullet count, file count, body length — was measuring effort rather
than separability, and those two come apart constantly. The three shapes that
survived are the ones where the issue genuinely has no single finish line: an
umbrella exists to be split, a standing invitation is never done, and a support
question was never a change request. The sentences after the list exist to close
the specific wrong readings I had already made once, because a threshold I only
hold in my head is one the model does not have.

**Trade-offs**

It gives up the ability to reject a large issue. "Size alone never fails it" means
a single well-specified deliverable that is simply too big for a newcomer — a
"rewrite the caching layer" issue with named files and acceptance criteria — is
one change, ships as one pull request, and passes this check cleanly. I accept
that miss. `outcome-is-settled` and the preferred `signposted-for-newcomers` catch
some of those, but a maintainer-authored, well-specified, enormous refactor would
get through, and nothing in my rubric currently measures diff size or blast
radius.

What loosening it did *not* cost is measured rather than assumed: run 2 re-ran the
whole scope family with `--only issue-01,issue-05,issue-10,issue-20` immediately
after the rewrite. `issue-01` flipped to accept as intended, and `issue-05`
(sympy's open-ended "incrementally adding more type annotations"), `issue-10`
(tldr's literal "megaissue"), and `issue-20` all still rejected — 4/4. The two
umbrella cases are caught by the first and second shapes in the list, not by size,
so removing the size proxy left them where they were.

---

## Selection rationale

**Selection rationale**

*1. Fit to my interests and the time available.* I picked #69, the output parser
crashing on a top-level JSON array. I have built REST APIs in Python and what I
actually want out of this course is practice at the fix-plus-test-plus-review
loop, not at fighting an environment. #69 is pure Python logic in
`rag/generator/output_parser.py` with no database, no Redis, and no Docker in the
way, and it already ships a test marked `xfail` that my fix should un-mark. That
means the "did I fix it?" question has an objective answer I can run locally in
seconds, which is the right shape for the time I have this unit. I also liked that
it sits in the RAG side of the codebase, which is the part I know least and most
want to read.

*2. What the verdict identified correctly, and what I weighed that the rubric could
not.* The rubric was right that all three candidates were live, unclaimed, and
bounded — and it was right to notice that #54 already has a classmate's claim
comment on it while #69 has none, even though the Path Review house rule means
that does not block me. What the rubric could not weigh is the thing that decided
it: #69's failing test already exists. My rubric has no check for "is there an
existing test that will tell me when I am done," and that was the single most
important property for a first contribution where I am still learning the review
loop. The rubric also ranked #62 second on my API background, which is a fair read
of my fit profile, but #62 is a one-line attribute rename — I would learn less from
it even though it is closer to what I already know. Fit-to-background and
fit-to-learning are not the same thing, and the rubric only sees the first.

*3. The anticipated difficulty in claiming it.* Low, with one caveat. #69 has no
assignee, no linked PR, and zero comments, so nobody is visibly on it, and the
Path Review house rule means a classmate arriving later does not cost me anything
— credit attaches to the pull request I open. The caveat is that it is labelled
`good first issue` and `tier-1` in a classroom repo where twenty-odd people are
picking at the same time, so I should expect company. The real difficulty is not
claiming it but writing the claim comment well, which is Unit 2's work.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
