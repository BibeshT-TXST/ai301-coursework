# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-commits | "last 5 default-branch commits" under Repo facts | At least one commit authored by a human (username not ending in `[bot]`) dated within 90 days of the capture date. A bot commit merging a human's PR counts as human. | required |
| maintainer-answers | "maintainer first-response sample" under Repo facts; author_association in the Comments section | Pass unless the sample shows maintainers systematically ignoring threads a maintainer has engaged with. A backlog with no replies on unanswered issues does not fail this check when the commit list shows maintainers merging contributor PRs within 30 days of the capture date. | preferred |
| repo-alive | `archived:` and "last push to any branch" on the repo line | `archived: false` AND last push within 180 days of the capture date. | required |
| repo-used | "latest release" under Repo facts; stars on the repo line | A release dated within 12 months of the capture date, or 500+ stars if the project ships no releases. | preferred |
| scope-bounded | Issue body and the full comment thread | Fail only if one of these is present: the issue is an umbrella or tracking issue whose sub-items are meant to become separate issues; the thread shows an unsettled design debate no maintainer has closed; a maintainer says the fix touches core internals; or the issue is a usage or support question. Otherwise pass. A detailed multi-step plan confined to one area is one piece of work — sub-headings are not sub-issues. A terse body, a missing reproduction, or no checklist never fails this check. | required |
| unclaimed | "assignees:" and "linked PRs:" under Repo facts; the Comments section | No assignee, no open linked PR, and no claim comment ("I'll take this", "working on this") dated within 60 days of the capture date. A claim older than 60 days with no linked PR is abandoned and passes. | required |
| ai-policy-open | "contribution policy" line under Repo facts | No outright ban on AI-generated or AI-assisted contributions. Disclosure, testing, personal-understanding and human-review conditions pass. Silence passes. | required |

## Verdict rule

Accept when every `required` check passes. A single `required` fail rejects the issue.

`unclear` on a required check counts as fail. Rationale: every required check maps to a field the bundle actually carries, so missing evidence means the signal is genuinely absent, not merely unrecorded.

`preferred` checks never change the verdict. They rank accepted issues in live mode: an accepted issue passing `repo-used` outranks one that does not.

All recency thresholds measure against the bundle's stated capture date in eval mode, and against today's date in live mode.
