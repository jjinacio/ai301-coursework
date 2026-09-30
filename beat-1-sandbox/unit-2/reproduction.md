# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

jjinacio

---

## Posted upstream

**Claim comment**

I am investigating issue #54. The problem is that `_detect_sections()` in `resume_parser.py` uses regex patterns anchored at the start of a line. When PDF-extracted text has leading whitespace before section headers like "Education:" and "Skills:", the patterns do not match and `detected_sections` returns empty.

My plan: I will reproduce this on a clean setup of my fork, run the exact snippet from the issue, and verify the test failures. Then I will check the regex patterns and the section-header matching logic to understand what needs to change.

**Reproduction comment**

Reproduced on a clean setup of my fork.

## Environment

- PathReview commit: 2f4e82f (2026-09-16)
- Python: 3.13.5
- Operating System: Windows 11
- Installation method: `pip install -e .`

## Steps

Clone and set up:
```
git clone https://github.com/jjinacio/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3
python -m venv venv
venv\Scripts\activate
pip install -e .
pip install pytest
```

Create `repro.py`:
```python
from ingestion.parsers.resume_parser import ResumeParser

# Case 1: Text WITHOUT leading whitespace (works)
r = ResumeParser()
text_no_indent = 'John Smith\njohn@example.com\nEducation:\n- B.S. Computer Science\nSkills:\nPython'
res1 = r.parse(text_no_indent)
print("Case 1 (no indent):", res1.metadata['detected_sections'])

# Case 2: Text WITH leading whitespace (fails - this is issue #54)
text_with_indent = """
    John Smith
    john@example.com
    Education:
    - B.S. Computer Science
    Skills:
    Python"""
res2 = r.parse(text_with_indent)
print("Case 2 (with indent):", res2.metadata['detected_sections'])
```

Run it: `python repro.py`

Then run tests: `pytest tests/unit/test_resume_parser.py -v --runxfail`

## Observed Behavior

Direct reproduction output:
```
Case 1 (no indent): ['Education', 'Skills']
Case 2 (with indent): []
```

Unit test results: 5 failed, 5 passed

- `test_parse_single_column_resume_text` - FAILED
- `test_parse_resume_no_work_experience` - FAILED
- `test_parse_markdown_resume` - FAILED
- `test_detect_sections` - FAILED
- `test_strip_markdown_syntax` - FAILED

## Root Cause

The `_detect_sections()` function in `ingestion/parsers/resume_parser.py` uses regex patterns anchored to `^` (start of line). When PDF-extracted text has leading whitespace before section headers, the patterns do not match and `detected_sections` returns empty.

## Conclusion

Section detection works without leading whitespace but fails when text is indented. The regex patterns need to account for optional leading whitespace before section headers.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Evaluation run completed successfully with model Sonnet on all 20 packages. Initial Windows encoding issue with emoji in package bundles was resolved by enabling Python UTF-8 mode (`PYTHONUTF8=1`). Used personal pro account credentials for API access. Completed in one full pass with no partial runs or retries needed.

**Package analysis**

The skill evaluation achieved 19/20 agreement (bar: 18/20, PASS). One package (pkg-03) disagreed: the skill rejected a reproduction that was expected to pass, citing missing Environment recorded details. The other 19 packages matched their gold labels. All category floors were met: clear-accept (7/8), disclosure (1/1), no-evidence (4/4), unfollowable-comms (3/3), wrong-target (4/4).

**Check rationale**

From `tools/repro-check/rubric.md`, Check 5:

"P if the comment describes what the author has already run, states next steps only as intentions (checking a code path, running more tests, reporting findings back—not as promises to fix or with any deadline), and makes any disclosure the repo's policy requires. F if it promises a fix or timeline, demands the issue be assigned or reserved, rates its own reproduction instead of describing what was run, or skips a disclosure the repo's policy requires."

This check wording was refined from an earlier version that was too strict about investigation promises. The revision explicitly separates investigation intentions (allowed) from fix promises (not allowed), which matches the voice-guide's emphasis on distinguishing "work I can commit to" (investigation) from "work I cannot promise" (fixes).

**Trade-offs**

Check 5's revision trades clarity about investigation intentions for the cost of slightly longer wording. The trade allows legitimate exploration promises (which are necessary in claim comments where the author hasn't reproduced yet) while still blocking fix promises and deadlines. This shift revealed an earlier issue: the rubric was rejecting all investigation statements, not just fix promises. The revision corrects this while keeping the original intent.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/repro-check/`.
