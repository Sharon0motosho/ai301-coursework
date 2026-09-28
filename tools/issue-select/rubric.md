# Rubric: is this a good first issue?

Five required checks, one per way a first contribution actually dies: the
repo is dead, the rules reject my workflow, somebody else is already on it,
the work is too big to be one pull request, or nobody has decided what
"done" means. Three preferred checks rank the issues that survive.

Every recency threshold is measured against the bundle's `captured:` date in
eval mode, and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `repo-alive` | Repo facts: the `archived:` flag on the repo line, the `last 5 default-branch commits` list, and `last push to any branch`. | `archived: no` AND the newest default-branch commit is dated within 180 days of the capture date. If no commit list is given, use `last push to any branch` against the same threshold. | required |
| `ai-policy-allows-me` | Repo facts: the `contribution policy` line. Live mode: `CONTRIBUTING.md`, `.github/CONTRIBUTING.md`, any AI policy file they link, per `references/evidence-guide.md`. | The policy does not refuse AI-assisted contributions. Silence passes: no `CONTRIBUTING.md`, or one with no statement on AI, is not a restriction. Stated *conditions* pass — disclosure, personally understanding the change, testing it, human-reviewing AI output are terms I can meet, and a policy that closes unreviewed AI PRs is stating a condition, not a ban. Only an outright refusal to accept AI-assisted work ("we do not accept AI-generated code or documentation") fails. | required |
| `nobody-on-it-now` | Repo facts: the `this issue:` line (`assignees:`, and `linked PRs:` with each PR's state). Plus every claim in the Comments section ("I'll take this", "can I work on this", "working on this"), read with its date. | No assignee, AND no linked PR in `open` state, AND no claim comment dated within 90 days of the capture date. A `closed` unmerged linked PR, a `merged` PR that did not close the issue, and a claim comment older than 90 days are all abandoned attempts rather than live claims, and pass. | required |
| `one-bounded-change` | The issue title and body, and the Comments section. | The issue asks for one change a newcomer could ship as a single pull request. It fails on exactly three shapes: an explicit umbrella, tracking, or "mega" issue that exists to be split into separate work; an open-ended standing invitation for repeated incremental contributions, with no state in which the issue is finished; or a usage/support question rather than a request for a change. Size alone never fails it. One change that edits many files is one change, a long prescriptive body is a spec rather than an umbrella, and one deliverable plus the edits that keep the surrounding files consistent with it — a new docs page plus the pages that must now point at it, a new function plus its call sites — is still one change. A terse body is not a fail on its own. | required |
| `outcome-is-settled` | The issue body, its labels, its opener's `author_association`, and any maintainer comment in the Comments section. | Someone has settled what "done" means, by either route. Route one, authority: the opener is an `OWNER`, `MEMBER`, or `COLLABORATOR`, or the issue carries a `good first issue` or `help wanted` label, or a maintainer comment endorses a concrete approach. Route two, specification: the body itself pins the work down — reproduction detail for a bug, or named files, named behaviour, or acceptance criteria for a feature or docs task — in which case it is settled whoever opened it. Extras the body marks optional or lower-priority do not unsettle the rest. It fails only when *neither* route holds: no maintainer signal, AND the work still hangs on an unresolved decision the body itself flags — an asset, API, or design left "TBD", or a thread still arguing the approach with no maintainer settling it. | required |
| `maintainer-answers-issues` | Repo facts: the `maintainer first-response sample` block. | At least one sampled issue drew a first owner/member/collaborator reply within 30 days. | preferred |
| `ships-releases` | Repo facts: the `latest release` line. | A release dated within 365 days of the capture date. | preferred |
| `signposted-for-newcomers` | The issue's labels and body. | The issue carries a `good first issue`, `help wanted`, `easy`, or `documentation` label, or its body states reproduction steps or acceptance criteria. | preferred |

## Verdict rule

`accept` if and only if all five required checks grade `pass`. Any required
check graded `fail` or `unclear` produces `reject` — evidence I could not
find is evidence I do not have, and a first issue I cannot verify is not one
I should take.

The three preferred checks never change a verdict. They rank the accepted
issues against each other: an accepted issue in a repo that answers its
issues, ships releases, and labels work for newcomers outranks an accepted
issue in a repo that does none of those.
