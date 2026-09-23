# Rubric: is this a good first issue?

## Evidence policy

In eval mode, use only evidence inside the snapshot bundle: the repo-facts block, the issue metadata, and the comment thread. Do not fetch live GitHub state during an eval run, and measure every recency threshold against the bundle's stated capture date.

In live mode, gather the same signals from the live repository at the locations named in `references/evidence-guide.md`, and measure recency thresholds against today.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Active repo | repo_facts.last push to any branch; repo_facts.latest release; repo_facts.last 5 default-branch commits | P if the repository had a push within 90 days of the bundle capture date. F otherwise. | required |
| Small clear fix | issue title and body; issue labels; comment thread; repo_facts.this issue.linked PRs | Grade the size of the work requested, not the polish of the write-up. A terse body, a bare checklist, or a bug report with no repro steps can still pass; a long, well-formatted issue can still fail. P if the issue asks for one bounded change a newcomer could finish in a single pull request, and the thread shows no unresolved disagreement about what that change should be. F if any of the following holds: the issue is an umbrella or tracking issue listing sub-items meant to be split into separate work; the change is codebase-wide by its nature; the thread shows the design still under debate with no maintainer decision; a maintainer states the fix requires changes to core internals; the issue is a feature wish with no specification and an unmade product decision inside it; the issue is a usage or support question rather than a request for a change; or the issue's history shows repeated abandoned attempts (two or more closed, unmerged PRs) indicating the work is harder than the label suggests. | required |
| Available | repo_facts.this issue.assignees; repo_facts.this issue.linked PRs; comment thread | P if the issue has no assignee, no linked open PR, and no credible signal in the comments that someone is actively implementing it. F otherwise. | required |
| AI contribution allowed | repo_facts.contribution policy line; any CONTRIBUTING / AI policy text quoted or summarized in the bundle; PR or issue template text in the bundle | F if the policy states that AI-generated, AI-assisted, or machine-generated code or documentation is not accepted, is prohibited, or will be closed. P if the policy is silent on AI, or if it permits AI-assisted work subject to conditions (disclosure, author understanding, testing, human review) rather than banning it. Silence is P, not unclear: most repos state nothing and that is not a restriction. | required |

## Verdict rule

Accept only if all required checks pass (are P). Reject if any required check is F or unclear.
