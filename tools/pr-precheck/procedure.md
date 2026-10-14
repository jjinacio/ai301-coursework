# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Read the Candidate plan section first, noting the In/Out scope pair and the stated test plan.
2. Read the Issue section to understand what the PR claims to fix.
3. Read the PR description to understand what the author says changed.
4. Read the unified diff to see what actually changed.
5. Read the test-evidence section to see the before/after output and check results.
6. Read the repo-facts block to understand the repository's contribution template and policies.

**Why this order**: the plan's scope is the ground truth for what should be inside the diff. 
Checking the diff against the plan's scope first reveals silent drift. Then the description's 
claims are checked against the diff, and finally test evidence is checked against both the 
plan's test plan and the repo's own checks.

## Evidence gathering

**Plan scope evidence**: Extract the plan's "In:" and "Out:" statements. List the files and 
functions the plan names. Note any deviation section in the plan that re-ties mismatches.

**Diff evidence**: Extract the list of changed files from the diff. For each file, note whether 
it is in the plan's stated scope or covered by a deviation note. Scan the diff for debug lines, 
commented-out code, unrelated edits, or formatting churn.

**Description evidence**: Extract the "Changes:" section from the PR description. List each 
bullet point and what it claims changed. Then compare it against the actual files and functions 
modified in the diff.

**Test evidence**: Extract the test plan from the plan.md. Extract the repro steps from the 
issue context. Extract the before/after output and check results from the test-evidence section. 
Note: do the test commands match the repro steps? Is the expected behavior named? Do the checks pass?

**Template evidence**: Extract the PR description's sections. Compare against the repo's 
PULL_REQUEST_TEMPLATE.md. Note whether each section is filled with real content or left as 
boilerplate. Check for any required AI-use disclosure.

**Branch name evidence**: Note the exact branch name. Check whether it follows the required 
format (type/issue-number-slug).

## Check execution

Execute the checks in this order:

1. **Plan fidelity: diff matches plan scope** — Compare files changed in diff against plan scope. 
   Grade P if all are within scope or covered by deviation notes; F if any file outside scope 
   with no note.

2. **Plan fidelity: description matches diff** — Compare description's Changes bullets against 
   actual diff contents. Grade P if they match; F if description omits material changes or 
   claims changes the diff does not contain.

3. **Test evidence is decisive** — Compare test-evidence against plan's test plan and repro steps. 
   Grade P if evidence re-runs repro or equivalent, names expected behavior, shows checks passing, 
   would catch success/failure; F if vague or disconnected from repro.

4. **Diff is clean: no debris** — Scan diff for debug lines, commented code, unrelated refactoring. 
   Grade P if only needed changes present; F if debris or unrelated edits found.

5. **Description follows the repo's template and policy** — Check each template section is filled, 
   required disclosures present. Grade P if compliant; F if boilerplate or disclosure missing.

6. **Branch name is correct** — Check format (type/issue-number-description). Grade P if correct; 
   F if format does not match or issue number wrong.

## Verdict assembly

Apply the verdict rule: accept if every required check passes; reject if any required check 
fails or is unclear. Quote the evidence for the first failing required check in rubric order. 
If all checks pass, quote evidence from one that exemplifies why the PR is ready.

Example output line:
- "Plan fidelity (diff scope): fail — the diff adds `test_resume_parser.py` but the plan's 
  Out says 'no test changes'"
