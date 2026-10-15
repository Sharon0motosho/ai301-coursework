# Rubric: is this plan ready to post and build from?

Every check reads the package against the issue it belongs to and the
reproduction evidence the plan builds on. "The issue" means the issue
context (title, body, thread highlights); "the repro evidence" means
the accepted reproduction (its steps, control runs, artifacts, and its
expected/actual lines); "the plan" means the candidate plan; "the
comment" means the candidate plan comment. `references/evidence-guide.md`
says where each piece of evidence lives, and `procedure.md` says in what
order to gather it and run these checks.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (its diagnosis/cause line and whatever its approach assumes is broken), read against every step, control run, and artifact in the repro evidence | Pass when the stated cause explains what the repro evidence shows AND no step or control in the repro evidence rules it out. Fail when a control run or step shows the blamed component working correctly (the same input succeeding without the blamed piece, the symptom already present before the blamed step runs, the blamed feature working elsewhere in the same build), or when the plan blames something the repro evidence never touches while the evidence points at a different component. A cause taken from the issue or the thread still fails if the repro evidence contradicts it: the repro evidence outranks confident opinions in the thread. | required |
| one-bounded-change | The plan's in-scope work (scope statement, numbered approach steps, files/areas list), read against the behavior the issue reports | Pass when everything the plan commits to doing is needed to fix the reported behavior: the fix itself, regression tests for it, and docs/notes that describe the fix. Explicit deferrals ("not in scope: X, leave to a follow-up") are fine and count toward passing. Fail when the plan also commits to work the issue did not ask for: refactors or rewrites, dependency or framework migrations, new options/settings/UI, a retry or abstraction layer, unrelated bug fixes "while in the area", or CI/infra changes. It fails even when the core fix inside it is correct, and even when the extra work is offered as a "PR series". | required |
| stranger-can-start | The plan's approach and files/areas, read as if by someone who has never talked to the author | Pass when the plan names where the change goes (a file, function, module, or code site) AND commits to one concrete approach, so a stranger could open that location and start. Fail when the location is missing or vague ("somewhere in the input stack", "wherever it panics"), when the key decision is left open ("upstream or vendored, whichever is easier", "gocui? tcell? not sure"), or when the plan is only "investigate / profile / poke around" with no chosen change. | required |
| decisive-test | The plan's test plan, read against the repro evidence's steps and expected/actual lines | Pass when the test plan names an observable outcome that would show this specific fix worked: re-running the repro steps (or a named regression test built from them) with a stated expected result (an exit code, the right output, a test passing, a value changing). Fail when the only test is generic ("run the full test suite", "make sure nothing regresses", "CI is green") or subjective ("should feel fast", "nothing should feel broken"), because none of those would show whether the reported bug is gone. | required |
| thread-direction-engaged | The thread highlights (maintainer, owner, or collaborator comments: a located culprit, a proposed or rejected approach, a posted patch or test build, an open PR or prior art), read against the plan and the comment | If the thread contains no such direction, pass. If it does, pass when the plan or comment either follows that direction or names it and explains why it departs. Fail when the plan goes a different way and the comment never mentions the direction (for example, the owner pointed at a source file and posted a patched build, and the plan only documents a workaround), or when the plan takes an approach a maintainer already rejected without acknowledging it. | required |
| ai-disclosure-when-required | The repo-facts block's contribution policy (CONTRIBUTING / AI policy text), read against the comment and the plan. Treat every package as AI-assisted work. | First decide what the policy requires of issue comments. If the policy requires disclosing AI use in comments or issues, or requires disclosing "all AI usage in any form", pass only when the comment or plan discloses AI assistance (the tool or the fact of assistance, and its extent). If the policy has no AI rule, only regulates code or pull requests, only bans fully AI-generated work, or only asks that comments be in the contributor's own words, pass: no disclosure is required there. Fail only when a disclosure requirement that covers comments exists and neither the comment nor the plan discloses. | required |
| unknowns-stated | The plan's risk/unknowns lines and any certainty words ("guaranteed", "definitely", "will fix everything"), read against what the repro evidence actually covered | Pass when anything the plan has not verified (an untested platform, an unmeasured cost, an unchecked tool) is named as open rather than claimed as settled. | preferred |
| comment-matches-plan | The comment, read against the plan | Pass when the approach the comment announces is the approach the plan commits to, with no extra promises (dates, wider fixes) the plan does not contain. | preferred |

## Verdict rule

Accept (ready to post and build from) when every `required` check
passes. Reject (hold) when any `required` check fails. `unclear`
counts as fail: a plan the grader cannot verify from the package is a
plan a maintainer cannot verify either. `preferred` checks never change
the verdict; report them so the writer knows what would make the plan
stronger.
