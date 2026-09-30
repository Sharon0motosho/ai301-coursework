# Rubric: is this reproduction package ready to post?

Every check reads the package against the issue it belongs to. "The
issue" means the issue context (title, body, thread highlights); "the
report" means the candidate repro report; "the claim" means the
candidate claim comment. `references/evidence-guide.md` says where each
piece of evidence lives.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The report's environment record (its environment line or block), read against the issue's own environment and against any factor the issue or thread says changes the failure (OS, driver, backend, shell, build profile, runtime version) | Pass when the report names the version of the software under test and the OS/platform it ran on, AND names every factor the issue itself singles out as relevant to the bug. Fail when there is no environment record, or when an issue-relevant factor is left out so a reader cannot tell whether the attempt was under the conditions the issue describes. | required |
| target-matches-issue | The version and setup named in the report's environment record and steps, read against the version/branch the issue says is affected (including thread notes like "confirmed on main" or "maintainers could not reproduce on X") | Pass when the attempt ran against a version and setup the issue says is affected, OR the report explicitly states the difference and says what it means (e.g. "filed on 3.8.4, still reproduces on 3.9.6"). Fail when the attempt used a version or setup outside what the issue targets and the report does not acknowledge the difference, or when it generalizes the bug to a setup the thread says does not show it. | required |
| steps-rerunnable | The report's steps and inputs: commands, code, config, and files, from starting state through the trigger | Pass when a stranger with only public resources could redo the attempt: the exact commands or code are shown (or the issue's own script is named as used verbatim), every input the trigger depends on is shown or unambiguously referenced, and nothing depends on private code, unshared config, or unstated manual setup. Fail when a step is vague ("set up the project", "run the usual command"), when the trigger step is missing, or when the repro lives in something the reader cannot get. | required |
| artifact-shows-issue-behavior | The report's artifacts (output excerpts, logs, tracebacks, exit codes, printed values) read against the symptom the issue describes, and the inputs the report used read against the issue's trigger | Pass when the attempt used the issue's trigger (same input shape, flag, syntax, or operation), AND the shown artifact displays the issue's symptom itself: the same error kind, crash, wrong value, or missing output. An honest cannot-reproduce also passes when the attempt used the issue's trigger and the artifact shows what actually happened instead. Fail when there is no artifact; when the artifact only shows the tool running, a version banner, or setup succeeding; or when the input was changed (different syntax, operator, argument, or a modified expression) so the artifact shows a different error than the issue's (e.g. a graceful validation or compile error where the issue reports a crash or a runtime error). | required |
| claims-backed-by-evidence | Every assertion in the claim and the report ("confirmed", "reproduced", "the cause is", "guaranteed", "on every machine", "not platform-specific"), each read against the artifacts actually shown | Pass when each assertion is supported by something shown in the package, and the stated outcome (reproduced / cannot reproduce) matches what the artifacts show. A cannot-reproduce passes when it says what differed from the issue's setup. Fail when the package asserts a reproduction, a root cause, certainty, or a wider scope that its artifacts do not show, or narrates an artifact as something it is not (a still-running process called a crash, an argument error called the reported panic). | required |
| claim-specific-and-honest | The claim comment, read against the issue's title and body | Pass when the claim names something specific to this issue (its symptom, component, file, or a concrete first investigation step) and commits only to investigating or to a stated next step. Fail when the claim could be pasted on any issue unchanged ("assign me please", "+1"), states no intent to work on it, or promises a fix, a guaranteed outcome, or a delivery date. | required |
| ai-disclosure-when-required | The repo-facts block's contribution policy (CONTRIBUTING / AI policy text), read against the claim and the report. Treat every package as AI-assisted work. | First decide what the policy requires of issue comments. If the policy requires disclosing AI use in comments or issues, or requires disclosing "all AI usage in any form", pass only when the claim or the report discloses AI assistance (the tool or the fact of assistance, and its extent). If the policy has no AI rule, only regulates code or pull requests, or only asks that comments be in the contributor's own words, pass: no disclosure is required there, and a comment that reads as the contributor's own words meets an own-words rule. Fail only when a disclosure requirement that covers comments exists and neither comment discloses. | required |
| control-run | The report's steps and artifacts | Pass when the report shows a contrasting run (the same steps without the trigger, a known-good input, or a previous version) whose output differs in the way the issue predicts. | preferred |
| template-asks-covered | The repo-facts block's bug-report template asks, read against the report's environment record | Pass when every item the repo's bug template asks for (versions, OS, config) appears in the report. | preferred |

## Verdict rule

Accept (ready to post) when every `required` check passes. Reject
(hold) when any `required` check fails. `unclear` counts as fail: proof
the grader cannot find in the package is proof a stranger cannot find
either. `preferred` checks never change the verdict; report them so the
writer knows what would make the package stronger.

The one exception to `unclear`-as-fail is live mode on a claim-only
draft: checks whose evidence is the repro report are graded `unclear`
with evidence `not yet applicable: claim-only draft` and are left out
of the verdict. The verdict then rests on `claim-specific-and-honest`
and `ai-disclosure-when-required`.
