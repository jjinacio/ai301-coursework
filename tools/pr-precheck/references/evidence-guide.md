# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**In an eval bundle**: the plan-context block's In/Out scope pair, any Deviations section, 
and the candidate PR's diff (list of changed files and the unified diff).

**In live mode**: `plan.md` (the In/Out statements and any Deviations section), your branch's 
diff (`git diff main...HEAD`), and your draft PR description's Changes section.

**What good looks like**: every file in the diff falls inside the plan's stated In scope, or 
is explained in a deviation note. The PR description's Changes bullets match the actual 
changed files and functions in the diff, with nothing material left out or added silently. 
If the diff is smaller than the plan promised, a deviation note explains why. Silent drift 
is when the diff does more or less than the plan said with no note, or when the description 
claims changes the diff does not contain.

## Test evidence (harness category: not-tested)

**In an eval bundle**: the plan-context block's test plan, the test-evidence section's 
before/after output, the check results (repo's own `make test`, `make lint`, `make typecheck` 
or equivalent), and the reproduction's own steps from the issue context.

**In live mode**: the test plan in `plan.md`, your `test_evidence.md` file (before/after 
output from re-running the repro steps), and the output of running the repo's checks 
(`make test-unit`, `make test-integration`, `make lint`, `make typecheck` or the repo's 
equivalents).

**What good looks like**: the test evidence re-runs the reproduction steps from the issue 
or their equivalent, the expected behavior after the fix is named explicitly (not just 
"tests pass"), the before and after outputs are visible and differ as expected, and the 
repo's own checks pass. Decisive evidence would catch both success (tests fail before the 
fix, pass after) and failure (tests still fail, or new breakage appears). Vague evidence 
says only "tests pass" with no before/after, or does not connect to the reproduction at all.

## Diff quality (harness category: unreviewable)

**In an eval bundle**: the candidate PR's unified diff and commit list.

**In live mode**: your branch's diff (`git diff main...HEAD`), the commit list on your 
branch (`git log main...HEAD --oneline`), and a scan for debug lines or commented code in 
the actual changes.

**What good looks like**: the diff contains only the code changes needed to fix the issue. 
The fix is visible in the unified diff: the lines that address the plan are shown, not 
buried in refactoring or cleanup. No debug output (`print()` statements, `console.log()`, 
logging at debug level with commented notes), no commented-out code blocks, no unrelated 
refactoring (renaming variables in unrelated functions, reformatting, drive-by cleanup), 
and no formatting churn. Each commit message describes the change it contains. Unreviewable 
diffs carry commented code, debug output, unrelated refactoring, or cleanup the issue did 
not ask for.

## Standards and comms (harness category: standards-wall)

**In an eval bundle**: the repo-facts block's PULL_REQUEST_TEMPLATE.md sections and 
contribution policy, and the candidate PR's title and description with each template 
section filled in.

**In live mode**: the repo's `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`, 
your draft PR title and description in `pr_draft.md`, and any required disclosures per the 
repo's policy.

**What good looks like**: the PR description fills every section of the repo's template with 
real content (not boilerplate placeholders like "[describe the change]"). The summary 
clearly states what changed and why. The issue number is named after "Closes #". The 
testing section is honestly filled (ticking only boxes that are true). If the repo's policy 
requires an AI-use disclosure, that disclosure is present and specific about what the AI 
worked on. The PR follows the repo's stated contribution conventions (tone, length, level 
of detail). Non-compliant PRs skip template sections, leave placeholders unfilled, omit a 
required disclosure, or ignore the repo's stated policies and conventions.
