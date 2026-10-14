# Procedure: how this skill grades a plan package

## Read order

1. Read the Repro evidence section first, noting each step and the expected behavior at each point. Mark the key observation: what changed, what stayed the same, and what you would expect the code to do to produce that behavior.
2. Read the Issue section to understand the user's report of the problem.
3. Read the Candidate plan section, noting the diagnosis, scope (In/Out), change description, and test plan.
4. Read the Candidate plan comment to understand how the author frames the work and what they commit to.
5. Read the Repo facts block to understand the contribution policy and any required disclosures (AI use, etc.).

This order matters because the repro evidence is the ground truth: all claims in the plan must be verifiable against it, not the other way around.

## Evidence gathering

**Diagnosis evidence**: Extract the "Cause:" statement from the plan. Extract the behavior observations from the Repro evidence steps (what the user saw at each step, what changed and what did not). Note down: does the stated cause explain the observed behavior?

**Scope evidence**: Extract the "In:" and "Out:" statements from the plan. Note down: are they clear and explicit? Do they bound the change tightly, or leave wiggle room?

**Executability evidence**: Extract the file name(s), function name(s), and change description from the "Change:" section of the plan. Note down: could you or a stranger find the code and make the change from these instructions alone?

**Test evidence**: Extract the test plan from the "Test:" section. Extract the repro steps from the Repro evidence section. Note down: does the test plan re-run the repro steps or equivalent? Does it name the observable change to expect? Would it catch both success and failure?

**Honesty evidence**: Scan the entire plan for claims about code behavior (e.g., "the callback does X"), impact ("this will not affect Y"), or verification ("we tested Z"). Note down: are these claims backed by the repro evidence, or are they assumptions? Are assumptions labeled as such?

**Comms evidence**: Extract the Candidate plan comment. Read the Repo facts block's CONTRIBUTING note and contribution policy. Note down: does the comment match the repo's tone and conventions? Does it disclose AI use if required? Does it make promises beyond executing the plan?

## Check execution

Execute the checks in this order:

1. **Diagnosis grounded in repro evidence**: Compare the stated cause to the repro steps. Grade P if the cause explains the behavior; F if it contradicts or ignores the evidence; ? if the connection is unclear.

2. **Scope is bounded**: Look at the In/Out statements. Grade P if both are explicit and clear; F if either is vague or missing; ? if you would need to ask the author what they meant.

3. **Change targets the cause, not the symptom**: Compare the proposed change to the diagnosis. Does the change fix the root cause, or just mask the symptom? Grade P if it targets the cause; F if it only treats the symptom; ? if the connection is unclear without reading code.

4. **Executability: a stranger can start**: Read the file names and change description. Could you start the work right now with only what is written? Grade P if yes; F if you would have to hunt for files or guess at implementation; ? if you could start but might need to read code to finish.

5. **Test plan is decisive**: Compare the test plan to the repro steps. Does the test distinguish success from failure? Does it catch the case that was broken? Grade P if yes to both; F if the test is vague or does not connect to the repro; ? if you are unsure whether the test would catch the bug.

6. **Honesty: unknowns and risks acknowledged**: Scan for claims about code or impact that are not confirmed in the repro evidence. Grade P if assumptions are labeled or the plan acknowledges unknowns; F if the plan states confident claims about unverified code behavior; ? if you are unsure whether something is verified.

7. **Comment fits thread and repo**: Read the comment against the repo facts and thread context. Grade P if the comment is appropriate, discloses what is required, and makes no false promises; F if it breaks the repo's conventions or skips a required disclosure; ? if the context is unclear.

## Verdict assembly

Apply the verdict rule: accept if every required check passes; reject if any required check fails or is unclear. Quote the evidence for the deciding check (the one that changes the verdict, or if all pass, one that exemplifies why the plan is ready).

Do not grade a check without naming the fact that decided it. For example:
- "Diagnosis grounded: pass — the plan says the view's model is not refreshed after push, and the repro shows the color does not update until the view is re-entered, which matches."
- "Scope is bounded: fail — the plan says 'Out: no changes to other views' but does not name what views are in scope or what 'refresh logic' means."

Output a one-line summary per check, then the final JSON block.
