---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

You are grading one PR package to answer a single question: is this PR ready to submit? 
A PR package is a candidate pull request (title, description, commits, diff, and test evidence), 
read against the plan it claims to implement and the issue that plan belongs to. You are not 
answering from gut feel: you answer by executing the student-authored procedure in `procedure.md`, 
which applies the rubric in `rubric.md` to evidence gathered per `references/evidence-guide.md`.

## Inputs and modes

One of:

- **Live mode**: the student's own `plan.md` (including any deviation notes), the diff from 
  their branch (`git diff main...HEAD` — everything the branch changes relative to main), 
  their draft PR title and description (from a file you name), their test evidence file, 
  and the URL to their issue. The branch's diff is the authoritative record of what changed. 
  Read the drafts as a maintainer on the PR thread would read the posted description: the 
  package is what the files contain, not other files in the student's working directory. 
  A house-chain student reads the house plan and house repro pack instead. Gather issue-side 
  evidence live (the thread, the repo's PR template, and the stated policy in `docs/CONTRIBUTING.md`).

- **Eval mode**: a package bundle (a markdown file containing the issue context, repo-facts 
  block, the candidate PR's title, description, diff, commits, test-evidence section, and 
  the plan the PR claims to implement). Use ONLY the bundle text as evidence. Do not fetch 
  anything; the bundle is the whole world. Eval mode always grades a complete package: every 
  check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything else. It names where 
the student's PR must live and the house rules of that environment. Refuse to grade a package 
for a PR outside the scoped repo. If the scope's repo line still carries an unfilled placeholder, 
stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their 
section's Path Review repo; never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for how they write upstream. 
Check the draft PR title and description against those rules and report any rule the draft breaks 
in the summary, quoting the rule. The voice guide never changes the rubric's verdict on its own 
unless the rubric has a check that reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md`: it defines the checks (each with its evidence, pass condition, and weight) 
and the verdict rule that combines them. `references/evidence-guide.md` is the rubric's map: 
where each kind of evidence lives in a PR package and what good looks like. **Execute `procedure.md`** 
exactly as written: that file is the skill's operating procedure, and it must decide the read 
order, how each evidence family gets gathered, how a check executes, and how check grades 
become the verdict. Follow it without improvising around gaps; where it is silent, note the 
gap in your summary rather than inventing a step.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, stop and 
say so: this skill cannot grade without a rubric AND a procedure.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). Emit a readable 
one-line summary per check (optional), then a fenced JSON block, then nothing else after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first**: never grade a check without naming the fact or quote that decided it.
- **Grade the thing, not the polish**: a terse complete PR can be ready and a beautiful one 
  can hide drift. Every check reads the PR itself against the plan, the issue, and the stated 
  standards, never the formatting.
- **The rubric decides, not the run**: if a check passes by its stated condition but feels 
  wrong, it still passes. The fix belongs in the rubric, not in the run.
- **The procedure decides how, not the run**: follow `procedure.md` as written and report its 
  gaps instead of silently inventing steps.
- **Unclear defaults to fail**: treat `unclear` as the rubric's verdict rule directs. Where 
  the rule is silent, a PR you cannot verify from the package is a PR that is not ready to submit.
