# Contribution: Add static hashCode methods for Byte, Short, and Character

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Josh Inacio
**GitHub Username:** jjinacio
**Issue:** https://github.com/JackFurton/who-would-build-a-kernel-in-java/issues/250
**Pull Request:** https://github.com/JackFurton/who-would-build-a-kernel-in-java/pull/256

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout, graded sandbox candidates, and iterated the real-world scan**

I installed issue-scout and rewrote `search-strategy.md` with four real-world queries, each traced to my fit profile, and I had to allow `gh search issues` in `settings.json` before the scan could run. The first scan accepted five issues, but all five came from the Java query, because every top result had zero comments and the same update date. I then interleaved the four queries before taking the top five, and a rescan graded candidates from all three query families: four accepts and one reject. I skipped a star filter, because stars aren't in the search results, and the details are in `scan-notes.md`.

My second pick is MaterialX #2813. It is explained under Why I Chose This Issue.

**Unit 6: Voice guard, memory file, repro-check, and plan-check**

I installed the voice guard and fixed its Windows path; its first entries are checker failures, and its four passes came later, on the Unit 7 PR. I made `CLAUDE.local.md` in the clone and excluded it from git. repro-check and plan-check both returned accept (read-outs saved as repro-check-verdict.md and plan-check-verdict.md next to the drafts), but they ran after PR #256 was open, so they changed nothing. I posted the claim as the bare `@duke-bot claim`, which I'm leaving as a gap, and I didn't post the repro report.

**Unit 7: Ship-gate and manifest: a compile-only guard, empty-diff blocks, and a Windows path fix**

The ship-gate ran my manifest's compile guard and the pr-precheck on the fix branch. I passed the upstream commit with `--base` for the manual runs, and those passed. The hook blocked `gh pr create` at 05:49 and 05:54, because local master had my commits and the diff was empty. Moving local master back to 621c29b fixed that, and the hook honored the 05:54 pass at 05:59. The gate's summary crashed on a checkmark on Windows, so I ran it with `PYTHONIOENCODING=utf-8`, and I fixed the `claude` path in `check_gate.py`. The manifest's must-pass check is a compile guard, and `make conformance` moved to the excluded table because this machine has no make, QEMU, or JDK 25.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found #250 on goodfirstissue.dev, not the Issue Selection site; I asked the instructor, who confirmed on 2026-11-06 that this pick is okay. It's three small static methods plus one test, with files and lines named. The repo is a small kernel project with merges every few days. The scout first rejected it because of my own claim. I changed the rule so a sole assignee who is me passes, and it accepted on rerun, straightforward. I learned the scout treats any assignee as taken.

My second pick is MaterialX #2813. The scout rejected it because a seven-week-old comment from an outsider says they want to take it on, and nobody has followed up. I kept it anyway, because the comment has no reply and the issue is still unassigned. I'd ask the maintainers before starting.

**Claim comment:** https://github.com/JackFurton/who-would-build-a-kernel-in-java/issues/250#issuecomment-6678581537

**Toolkit**

- Tool: issue-scout (rubric and search strategy), goodfirstissue.dev, and manual grading
- What it produced: I found #250 on goodfirstissue.dev, not through the scout. The scout's first two grades rejected #250 only because of my own claim, since the Available check treats any assignee as taken. After I changed that rule, it accepted #250 twice, with a difficulty note of straightforward. My first real-world scan accepted five issues, all from the Java query; after I applied the interleave fix, the rescan graded candidates from all three query families (four accepts, one reject).
- Difficulties: The scan's ranking didn't separate anything. Every top candidate had 0 comments and was updated the same day, so search order picked the top five and the Python queries never reached grading. The accepts on #250 came after I'd claimed it and opened a PR, so they didn't help me choose it.
- What changed: Applied the Unit 6 scope-feasible patch and difficulty note, changed Available to pass a sole assignee who is me, and kept MaterialX #2813 after a reject for a stale claim comment. I applied the round-robin interleave and rescanned, and skipped the star filter because stars aren't in the search result fields.

---

## Understanding the Issue

**Problem Description**

The kernel's Byte, Short, and Character wrapper classes are missing static `hashCode(value)` methods that exist in the JDK (since Java 8) and are already implemented in the kernel's Integer, Long, Float, Double, and Boolean wrappers. Code that calls these static methods doesn't compile against the kernel because they're not implemented. This breaks compatibility with standard JDK APIs.

**Expected Behavior**

Each class (Byte, Short, Character) should have a public static `hashCode(primitive_type value)` method that returns an int hash code for the given primitive value. These should match the existing pattern used in Integer.hashCode(), Long.hashCode(), etc., allowing code to call `Byte.hashCode(b)`, `Short.hashCode(s)`, and `Character.hashCode(c)` and have them compile and run correctly.

**Current Behavior**

The static hashCode methods are missing from Byte, Short, and Character classes. Attempting to call `Byte.hashCode(byte)`, `Short.hashCode(short)`, or `Character.hashCode(char)` results in a compilation error because these methods don't exist, even though they exist in the JDK and in other wrapper classes in this kernel.

**Affected Components**

- `Byte.java` wrapper class (needs static hashCode method)
- `Short.java` wrapper class (needs static hashCode method)
- `Character.java` wrapper class (needs static hashCode method)
- Existing: Integer.java, Long.java, Float.java, Double.java, Boolean.java (reference implementations)

**Toolkit**

- Tool: GitHub issue inspection + JDK API documentation + existing code patterns
- What it produced: Clear understanding that three static methods need to be added following existing patterns in Integer, Long, etc.
- Difficulties: None — issue is well-documented; pattern is already established
- What changed: Nothing — clear scope, no misunderstandings

---

## Reproduction Process

**Environment Setup**

Operating System: Windows 11 (cross-platform compatible with Linux/Mac). Required: Java Development Kit 25, Git, text editor or IDE. Project setup: Fork the repo at https://github.com/JackFurton/who-would-build-a-kernel-in-java, clone locally. The kernel builds with `make` and the Makefile (not Gradle), which invokes `javac --system none -XDstringConcat=inline` to compile the kernel's own java.base module. Verify setup: `java -version` confirms JDK 25 installed; `make --version` confirms make is available.

Setup process was straightforward — the Makefile automates the complex javac flags needed to compile the kernel's module system.

**Steps to Reproduce**

1. Clone forked repo: `git clone https://github.com/YOUR-USERNAME/who-would-build-a-kernel-in-java`
2. Locate target classes: Find Byte.java, Short.java, Character.java in the source tree
3. Examine existing pattern: Review Integer.java, Long.java, Float.java, Double.java, Boolean.java for their static hashCode(primitive) method implementations
4. Note the pattern: Each static hashCode method takes a primitive value and returns an int hash code
5. Verify the gap: Confirm Byte, Short, Character lack these methods
6. Observed result: Code attempting to call `Byte.hashCode(byte)`, `Short.hashCode(short)`, or `Character.hashCode(char)` will not compile

**Reproduction Evidence**

- **Setup complete:** Git clone successful on Windows. This machine has JDK 21 and no `make`, so I compiled the kernel with the Makefile's javac flags locally. The full toolchain ran in GitHub Codespaces and upstream CI: `make conformance` passed 512/512 in Codespaces (that run's log wasn't saved, but upstream CI printed the same 512/512 in its log), and CI passed every make test target on this commit (https://github.com/JackFurton/who-would-build-a-kernel-in-java/actions/runs/42187544270).
- **Pattern identified:** Integer.hashCode(int), Long.hashCode(long), Float.hashCode(float), Double.hashCode(double), Boolean.hashCode(boolean) all exist and follow consistent pattern (all return `value;`)
- **Gap confirmed with compilation error:** On upstream master, attempting to call `Byte.hashCode(byte)`, `Short.hashCode(short)`, or `Character.hashCode(char)` produces (condensed from javac's three errors, one each for Byte, Short, and Character): `error: method hashCode in class Byte cannot be applied to given types`, then `required: no arguments, found: byte`
- **My findings:** Issue is clear and well-scoped. The pattern to follow is already established. The error message proves the methods are missing (they exist as instance methods, not static). No blockers to implementation.

**Toolkit**

- Tool: javac with the kernel's flags, a git worktree for upstream master, GitHub Codespaces for `make conformance`, and repro-check, run on claim-draft.md and repro-draft.md after PR #256 was open
- What it produced: Three compile errors on master (Byte, Short, Character), a compile-guard pass on the fix branch in the 05:54 ship-gate run, and `conformance: 512/512 passed` in Codespaces, corroborated by upstream CI's log (run 42207683990). repro-check returned accept.
- Difficulties: No make or QEMU on this machine. The first Codespace wouldn't accept SSH, so I added an sshd devcontainer feature on a branch and deleted that branch on GitHub. I grepped the local CONTRIBUTING.md myself and found no AI rule. My claim comment is the bare `@duke-bot claim`, with no first step or report-back date.
- What changed: I didn't post the reproduction report. repro-check ran after PR #256 was open, so it changed nothing and caught nothing. I deleted the remote codespace-sshd branch, but a local copy is still on my computer.

---

## Solution Approach

**Analysis**

Root cause: Three wrapper classes (Byte.java, Short.java, Character.java) are missing static `hashCode(primitive_type)` methods that are part of the standard JDK API (since Java 8) and are already implemented in five other wrapper classes in this kernel (Integer, Long, Float, Double, Boolean). The missing methods prevent code from compiling when it calls these static hashCode methods on Byte, Short, or Character.

**Proposed Solution**

Add three public static methods (one each to Byte, Short, Character) following the exact pattern already established in Integer, Long, Float, Double, and Boolean. Each method takes a primitive value and returns an int hash code: `return value;`. This matches the JDK implementation and is consistent with how the other five wrapper classes in this kernel compute hash codes. No changes to instance methods, constructors, or other code — only adding the three static hashCode methods and a conformance test.

**Implementation Plan**

1. Open Byte.java: Add public static `int hashCode(byte value)` method following Integer.hashCode(int) pattern
2. Open Short.java: Add public static `int hashCode(short value)` method following Integer.hashCode(int) pattern  
3. Open Character.java: Add public static `int hashCode(char value)` method following Integer.hashCode(int) pattern
4. Add conformance test to tests/conformance/duke/conformance/Boxing.java (required by issue #250) with negative values and boundary values
5. Build with `make conformance` to verify compilation and test passing (the kernel builds with Makefile and JDK 25, not gradlew)
6. Commit changes with message referencing issue #250
7. Push to fork and create PR

Deviations: None anticipated — pattern is clear and consistent across existing classes.

**Toolkit**

- Tool: plan-check, run on plan.md and plan-comment.md after PR #256 was open
- What it produced: An accept verdict, with all seven required checks passing (read-out saved as plan-check-verdict.md next to the drafts).
- Difficulties: It ran after the PR was open, so it changed nothing and caught nothing. It couldn't find a voice guide in its skill folder, so my voice rules weren't applied.
- What changed: Nothing in the plan. Before the run I changed one line in plan-comment.md from "I'll also add" to "The change also adds," since the test already existed. I didn't post the comment.

---

## Testing Strategy

**Compilation Verification:**
- Run `make conformance` to compile the kernel and run the conformance tests
- Before fix: Classes lack static hashCode methods; code calling them fails with "cannot be applied to given types"
- After fix: Compilation succeeds, three new methods are present and callable

**Method Functionality Verification:**
- Verify each method accepts the correct primitive type (byte, short, char)
- Verify each method returns an int value
- Test with sample values: call `Byte.hashCode((byte) 5)`, `Short.hashCode((short) 100)`, `Character.hashCode('A')` and confirm each returns an int value
- Compare hash values: calling the method twice with same value should return same hash (consistency)

**Conformance Test Verification:**
- Issue #250 requires a conformance test in tests/conformance/duke/conformance/Boxing.java
- Test must call the three static methods with negative values (e.g., -1, Byte.MIN_VALUE, Short.MIN_VALUE) and boundary values (e.g., MAX_VALUE, Character.MAX_VALUE)
- Test must verify hash codes match JDK behavior by running on HotSpot and in the kernel, comparing results
- Hash values should be deterministic and follow the pattern from Integer.hashCode(), Long.hashCode(), etc.

**Observable Success Criteria:**
1. The kernel compiles with the Makefile's javac flags, and upstream CI passed every make test target on this commit (https://github.com/JackFurton/who-would-build-a-kernel-in-java/actions/runs/42187544270)
2. Expected: three new public static methods exist and are accessible (the compile guard and the diff show this)
3. Expected: each method accepts its primitive type (not separately tested)
4. Expected: each method returns an int (not separately tested)
5. Expected: repeated calls return the same value (not separately tested)
6. Expected: no existing functionality broken (CI test steps passed on this commit)

**Toolkit**

- Tool: javac with the kernel's flags (compilation), GitHub Codespaces (`make conformance`, reported 512/512), GitHub Actions CI (every make test target)
- What it produced: Clear testing checklist with observable success criteria
- Difficulties: None — testing is straightforward code compilation and method verification
- What changed: Nothing — testing approach is standard for adding new methods

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-06**

Added the three static methods to Byte, Short, and Character. My first commit had no conformance test, even though issue #250 asks for one, so I added `staticHashCodes()` in a second commit.

**2026-11-06**

I opened PR #255 before I had verified the evidence. Its before/after output didn't come from a real run, so I closed #255 and opened #256 with output from real runs.

**2026-11-06**

The ship-gate's empty-diff check compared against local master, which had my two commits on it. I moved local master back to upstream (621c29b) and passed `--base 621c29b` to the gate.

**2026-11-06**

The voice guard blocked the PR body at 05:51 on a sentence-length rule, so I shortened the long sentences in "What changed and why." The guard passed at 05:56.

**Toolkit**

- Tool: git worktrees and branch moves for the base comparison; ship-gate's empty-diff check and voice guard for the blocks
- What it produced: The gate's diff blocks at 05:49 and 05:54 and the voice-guard block at 05:51, and the resolved base and PR body
- Difficulties: Local master had my commits on it, so the gate saw no diff. The voice guard rejected long sentences.
- What changed: Moved local master back to upstream, passed the base commit, and shortened the long sentences.

---

## Pull Request

**PR Link:** https://github.com/JackFurton/who-would-build-a-kernel-in-java/pull/256

**PR Description:** The fix adds three public static `hashCode` methods to Byte, Short, and Character, plus a conformance test. It closes #250, and it replaces #255, which I closed because its evidence was wrong.

**Maintainer Feedback:**

- 2026-11-06: none yet. #256 had no reviews or comments when I wrote this entry.

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate, run on fix/250-static-hashcode against upstream 621c29b before `gh pr create`
- What it produced: A manual PASS at 05:54 on the final PR text, which the hook honored at 05:59 and 06:02. The pr-precheck passed all six checks.
- Difficulties: Blocked on an empty diff, on a body-file path from the wrong folder, and on a Windows encoding crash. The voice guard also blocked the long sentences.
- What changed: Fixed the `claude` path, passed the base commit, ran with `PYTHONIOENCODING=utf-8`, and shortened the long sentences.

---

## Learnings and Reflections

**Technical Skills Gained**

Building a Makefile-driven kernel with its own javac flags, git worktrees, CI logs, and `gh` queries.

**Challenges Overcome**

The ship-gate saw no diff because my commits were on local master, so I moved master back and passed the base. Its Windows `claude` path and a checkmark crash needed fixes. PR #255 had output that never came from a real run, so I closed it and opened #256 with real output.

**What I'd Do Differently Next Time**

Only put evidence in a PR from runs I actually did. Check which branch a commit is on before pushing. Post a claim with a first step and a report-back date.

---

## Resources Used

- CONTRIBUTING.md and the Makefile in the kernel repo: the test targets, the javac flags, and the conformance rule.
- Issue #250: the description names the three files and their lines, and asks for the conformance test.
- CI run for 7af8fa9: https://github.com/JackFurton/who-would-build-a-kernel-in-java/actions/runs/42187544270
