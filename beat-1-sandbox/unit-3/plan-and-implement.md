# Plan and Implement: Issue #54

## GitHub username
jjinacio

## Plan comment
**Link:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54#issuecomment-6029233195

**Text pasted from posted comment:**

Reproduced on commit 2f4e82f (Windows 11, Python 3.13.5).

Cause. The regex patterns in _detect_sections() and _strip_markdown() use `^` which only matches at the start of a line with no leading whitespace. When PDF extraction adds indentation, the patterns fail and you get an empty list instead of section names.

Fix. Change the four patterns from `^` to `^\s*` in _detect_sections() at lines 134-135. Same fix in _strip_markdown() around line 98. Remove @pytest.mark.xfail(strict=True) from test_detect_sections, test_parse_single_column_resume_text, test_parse_resume_no_work_experience, test_parse_markdown_resume, and test_strip_markdown_syntax. They're marked strict so a passing test shows as XPASS(strict) which fails CI.

Scope. Only changing resume_parser.py and test_resume_parser.py. Not touching anything else.

Test. Before fix: run the indented snippet from the issue (expect []). After fix: run the same (expect ['Education', 'Skills']). Run unindented snippet both ways (expect ['Education', 'Skills'] either way). Full test suite: expect all 5 tests to pass, no regressions.

Not confirmed. Haven't checked what else calls _detect_sections(). Haven't verified the two markdown tests are truly out of scope. Will check both before changing anything.

## Branch
fix/54-resume-section-whitespace

## Evidence
### Before fix
```
python -c "from ingestion.parsers.resume_parser import ResumeParser; r = ResumeParser(); res = r.parse('\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n'); print(res.metadata['detected_sections'])"
```
**Output:** `[]` (empty list - sections not detected)

### After fix
```
python -c "from ingestion.parsers.resume_parser import ResumeParser; r = ResumeParser(); res = r.parse('\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n'); print(res.metadata['detected_sections'])"
```
**Output:** `['Education', 'Skills']` (sections correctly detected)

### Unindented test (should pass both before and after)
```
python -c "from ingestion.parsers.resume_parser import ResumeParser; r = ResumeParser(); res = r.parse('Education:\n- B.S. Computer Science\n\nSkills: Python\n'); print(res.metadata['detected_sections'])"
```
**Output (before and after):** `['Education', 'Skills']` ✓

## Run history
- **Run 1 (partial, encoding error):** 19/19 scored items graded before unicode encoding failure on pkg-02
- **Run 2 (full, UTF-8 enabled):** All 20 packages successfully graded
  - Agreement: **19/20** (exceeds 18/20 requirement)
  - Only disagreement: pkg-14 (gold: accept, verdict: reject)
  - All categories matched: clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4
  - **Result: PASS**

## Package analysis
**Package:** pkg-03 (clear-accept category)

**Gold label:** accept

**Skill verdict:** accept

**Reasoning:** pkg-03 presents a plan to fix a concurrency bug in a cache invalidation routine. The plan correctly diagnoses a race condition where concurrent writes can leave stale data; scopes to only the affected function; proposes to add a lock around the read-check-write sequence; and specifies a test that would reproduce the original race with goroutines and verify the lock prevents it. The approach directly addresses the root cause (not a symptom), is executable (file, line, code pattern specified), and the test plan re-runs the repro with the expected outcome. The comment accurately states the work and keeps appropriate tone. Plan passes all required checks.

## Check rationale
**Check quoted from rubric.md:**

"Executability: a stranger can start" — P if someone who has never seen the codebase could start executing the plan without asking the author anything: file names/paths are stated, the approach is spelled out step-by-step, and the order of work is clear. F if critical details are missing, asserted rather than shown ("obvious fix", "standard pattern"), or would require reading unspecified code to understand what to do.

**Why this check reads this way:** This check enforces that a plan is complete enough to hand off. A plan that requires the reader to infer details ("obvious fix", "standard pattern", or hunting through code to understand what to do) fails because the builder (a classmate, a TF, or even Claude with your rubric and evidence guide) might infer wrongly and waste time or ship a different fix than intended. Requiring names/paths, step-by-step approach, and clear order prevents misinterpretation and ensures the plan is a real specification.

## Trade-offs
This check trades breadth for precision. A looser check ("the plan is mostly clear") would pass more plans and be faster to grade, but would miss plans that omit critical details like file names or the order of steps. By requiring "someone who has never seen the codebase" to be able to start, we ensure every plan is complete enough to execute — which is the whole point of grading plans this week before builds begin. The trade-off means plans need to be specific, which takes more effort to write but saves confusion and rework during the build phase.
