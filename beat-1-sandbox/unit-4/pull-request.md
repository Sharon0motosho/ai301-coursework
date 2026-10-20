# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-howard/pull/86

**Branch**

fix/69-json-array-fallback

**My pr-precheck verdict on my draft**

I ran pr-precheck in live mode on my draft (`pr-draft.md` in this folder) against `plan.md`, `git diff main...HEAD` on the branch, the PR template and CONTRIBUTING, and the issue #69 thread. The verdict was **accept**, with all nine required checks passing. It flagged two voice-guide notes, both under my "Show the output, don't describe it" rule: the whole-file before/after was summarized instead of pasted, and the array control's "before" was described instead of quoted. I fixed both and ran the tool again on the revised draft before opening the PR.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Calibration check: `--include-calibration --only calib-01,calib-02,calib-03,calib-04`. My first try errored before grading anything because the `claude` CLI wasn't on my PATH. After I added the CLI from the VS Code extension, it agreed on all 4 calibration packages (calibration isn't scored, so this run has no agreement score).
2. Full run with `--save-run`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the run in `eval-run.txt`. I didn't change any tool file after it.

**Package analysis**

**pkg-20** (standards-wall, ghostty-org/ghostty#13604). My rubric decided **reject** and the gold label is **reject**.

The diff and evidence in pkg-20 are good. The fix matches the plan, the repro is re-run with `?997;2n` before and `?997;1n` after, and `zig build test` passes. What makes it not ready is the repo's standards. The repo facts say "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance", and the description has no AI disclosure at all. My `ai-disclosure-when-required` check treats every package as AI-assisted work, so it failed: "Policy requires disclosing all AI use with tool and extent; the description contains no AI disclosure."

Two other checks also failed, and I don't think either is as solid. `template-and-asks` failed because "The repo gates first-time contributors with a vouch flow, and nothing shows the precondition is met or does not apply". The package doesn't say whether this author is a first-time contributor, so that fail is really "unclear counted as fail". `description-matches-diff` failed because the description says "a second case covers the single-theme scheme flip", but the second added test checks `.fullscreen`. That is a real mismatch, but it's small, and the rewritten first test does cover the scheme flip. The verdict is right, and the disclosure check is the one I'd defend. If the description had disclosed AI use, those two weaker fails could still have rejected a PR the gold might accept.

**Check rationale**

> | ai-disclosure-when-required | The repo-facts block's contribution policy (live: CONTRIBUTING.md, AI_POLICY.md, the PR template, and the course rule in `scope.md`), read against the description. Treat every package as AI-assisted work. | First decide what the policy requires of a pull request. If it requires disclosing AI use for PRs or "all AI usage in any form" (live, in Path Review: always, because the course requires an AI-use disclosure on every PR), pass only when the description discloses AI assistance with the tool named and the extent of what it did; a vague mention ("had AI look at it") fails when the policy asks for tool and extent. If the policy has no AI rule, or regulates only issue comments, pass. | required |

This check comes from my Unit 3 plan-check rubric, where the same idea was about issue comments. I changed it to ask what the policy requires "of a pull request", because a PR is the thing being submitted now. Of the 20 packages, only one repo requires disclosure of all AI use (pkg-20); another only says comments must be in your own words.

What I rejected: requiring a disclosure on every package. That would fail the 18 packages whose repos have no AI policy, including all 7 clear-accepts. The other choice was to pass any mention of AI. I added "a vague mention ("had AI look at it") fails when the policy asks for tool and extent" because calib-02's description says "i had claude look at it", which names a tool but not what it did. "Treat every package as AI-assisted work" is there because the grader can't tell whether AI was used, so a missing disclosure in a strict-policy repo has to count as missing, not as "maybe no AI was used". The live-mode part is there because this course requires the disclosure on every Path Review PR, even though Path Review's CONTRIBUTING has no AI rule.

**Trade-offs**

Nothing changed after the run, and here is how I know: I did one full run and never revised a tool file after it. The four fingerprints in the `eval-run.txt` header (`SKILL.md sha256:fe1e14798e0f9166`, `rubric.md sha256:622b33337f4adad3`, `procedure.md sha256:e7e0f10a297333fa`, `evidence-guide.md sha256:344f70437bb2cc90`) match the files in `tools/pr-precheck/`, so no check was loosened and there was nothing to canary.

The trade-off I accept is in `repro-re-run`: a case that isn't shown still passes if the description names it as not run and says why, as long as one of the issue's failing cases is shown with a concrete result. That is what lets an honest shortfall be ready. But it means a PR could re-run only the easy failure mode, disclose that it skipped the hard one, and still pass. calib-04 shows the other side: it skipped repro 2 without saying so and failed. I accept that the check will pass a disclosed skip that a maintainer might still push back on.

`ai-disclosure-when-required` also gives something up. Because it treats every package as AI-assisted, a strict-policy PR written with no AI at all would still fail for not disclosing. In the eval set that is only pkg-20.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
