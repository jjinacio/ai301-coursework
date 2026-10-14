# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives.** In an eval package, the diagnosis is in the Candidate plan section, under "Cause:". The repro evidence is in the Repro evidence section, under "Steps:". Compare them directly.

**What good looks like.** The stated cause names a mechanism (a missing refresh, a wrong check, an off-by-one, a missing case) that would produce the exact behavior the repro evidence shows. The diagnosis does not contradict any step or observation in the repro evidence. For example: if the repro shows a color not updating after an action, a good diagnosis explains why the color is not recalculated; if the repro shows the color updates on re-entry, the diagnosis should explain why re-entry would fix it. A diagnosis that ignores or contradicts the evidence (saying the cause is X when the repro clearly shows Y happened) is bad.

## Scope

**Where it lives.** In an eval package, the scope is in the Candidate plan section, under "Change:". Look for the "In:" statement and the "Out:" statement.

**What good looks like.** The "In:" statement names specific files, functions, or areas that will be changed. The "Out:" statement names things that explicitly will NOT be changed, to head off scope creep. A bounded plan says "we will change the post-push callback to refresh context X, not any other refresh logic" rather than leaving the reader to guess what is in scope. Ambiguous scope ("improve the refresh logic", "fix related issues") is bad. A plan that says "Out: no changes to how other views refresh" is good because it prevents readers from expecting broader fixes.

## Executability

**Where it lives.** In an eval package, the executability is in the Candidate plan section, under "Change:". Look at the approach and the description of what will actually be changed.

**What good looks like.** The change description names the specific file (e.g., `pkg/gui/controllers/sync_controller.go`), the specific function or section (e.g., "the push completion callback"), and the concrete change to make (e.g., "add the commits context to the refresh scope"). A stranger could read this and open the right file, find the right place, and understand what change to make without guessing. Bad executability: "fix the refresh logic", "update the controller", "add the necessary changes"—these force the reader to hunt through code and guess what "necessary" means.

## Test plan

**Where it lives.** In an eval package, the test plan is in the Candidate plan section, under "Test:". The repro evidence is in the Repro evidence section, under "Steps:". Read the test plan against the steps.

**What good looks like.** The test plan re-runs the repro steps (or equivalent ones) and names the changed behavior to observe. For example: "repro steps above; at step 3 the color must flip without leaving the view"—that pins down what success looks like and connects directly to the repro. The test plan should catch both success and failure: a vague plan like "run the steps and check the color" does not, because the reader would not know what color to expect or whether a partial color change counts. A good test also checks adjacent cases (the same scenario from a different code path, or with a different option) to make sure the fix does not break something nearby.

## Honesty

**Where it lives.** Unknowns and assumptions are scattered through the plan statement. Look for claims about what the code does (e.g., "the post-push callback updates context X"), claims about what the fix will do (e.g., "this will not affect other views"), and claims about what has been verified (e.g., "we tested force push").

**What good looks like.** When the plan makes an assumption about code behavior or impact on other views, it either confirms that assumption in the repro evidence, or it says clearly that it is an assumption to verify during the build. For example: "the post-push callback runs for both regular and force push (to verify during build)" is honest; "the callback runs for all push types" without any qualification is confident and fine if the code obviously backs it. Bad honesty: "this will not affect other views" without any reason given or code checked, or "the repro confirms all cases" when the repro only tested one branch, not force push or the main commits panel.

## Comms

**Where it lives.** In an eval package, the plan comment is in the Candidate plan comment section. The repo facts are in the Repo facts block, which states the contribution policy and any disclosure requirements.

**What good looks like.** The comment says what the plan will do in language matching the repo's tone. It discloses AI use if the repo requires it (check the CONTRIBUTING.md or the repo facts block). It makes no promises beyond "the fix is small and I will verify it works"; it does not ask to be assigned or reserved, and it does not overstate confidence ("this is obviously correct" instead of "I will verify"). The comment reads like a working engineer, not a salesperson. Bad comms: "I will fix this right away", "this is a trivial bug", "assign this to me", or skipping a required AI disclosure.
