# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity: diff matches plan scope | the plan's In/Out statements, the diff, and any deviation notes in plan.md | P if every file changed in the diff falls inside the plan's stated In scope, or is explained in a deviation note. F if the diff changes files the plan said would stay out, with no deviation note, or if the diff is smaller than the plan promised without a note explaining why. | required |
| Plan fidelity: description matches diff | the PR description's Changes section and the unified diff | P if the description's bullet points match what the diff actually changed (files, functions, logic), with nothing material left out or added silently. F if the description claims changes the diff does not contain, or if the diff has material changes the description does not mention. | required |
| Test evidence is decisive | the test plan from plan.md, the reproduction steps from the issue, and the test-evidence section's before/after output and check results | P if the test evidence re-runs the repro steps or their equivalent, names the observable changed behavior expected, shows the repo's own checks passing, and would catch both success and failure. F if the test evidence is vague, does not connect to the repro, or would not distinguish success from failure. | required |
| Diff is clean: no debris | the unified diff content line by line | P if the diff contains only the code changes needed to fix the issue (no debug lines, commented-out code, unrelated edits, formatting churn, or drive-by cleanups). F if the diff carries commented code, debug output, unrelated refactoring, or formatting churn. | required |
| Description follows the repo's template and policy | the PR description, the repo's PULL_REQUEST_TEMPLATE.md, and the stated contribution policy | P if the description fills every section of the repo's template with real content (not boilerplate), includes any required AI-use disclosure, and follows the repo's stated contribution policies and conventions. F if any template section is skipped, left as boilerplate, or if a required disclosure is missing. | required |
| Branch name is correct | the branch name as it appears in the PR metadata | P if the branch name follows the required format: type prefix (fix/feat/docs/test/refactor/perf/chore/), issue number, and short description, like `fix/54-section-detection`. F if the naming format does not match or the issue number is wrong. In eval mode, this check is preferred (not verifiable from frozen packages); in live mode, this check is required. | preferred (eval) |

## Verdict rule

Accept (`ready to submit`) if and only if every required check passes. Reject (`hold`) if any required check fails or is unclear. Treat `unclear` as `fail`: a PR you cannot verify from the package is a PR that is not ready to submit.
