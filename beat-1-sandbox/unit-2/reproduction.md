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

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54#issuecomment-5903287208

I am investigating issue #54. The problem is that `_detect_sections()` in `resume_parser.py` uses regex patterns anchored at the start of a line. When PDF-extracted text has leading whitespace before section headers like "Education:" and "Skills:", the patterns do not match and `detected_sections` returns empty.

My plan: I will reproduce this on a clean setup of my fork, run the exact snippet from the issue, and verify the test failures. Then I will check the regex patterns and the section-header matching logic to understand what needs to change.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54#issuecomment-5903950724

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

The skill evaluation achieved 19/20 agreement (bar: 18/20, PASS). One package (pkg-03) disagreed: gold label accept, skill verdict reject. The Environment recorded check states: "P if the report states the version of the software under test, the operating system, and how it was installed or built, and any difference from the environment the issue targets is stated in the report rather than left for the reader to notice. F if any of those three is absent, or if the report's environment differs from the issue's target without the report saying so." The pkg-03 package's environment block was missing the OS specification, triggering the F condition. The skill correctly applied the rubric's mechanical check despite the gold label expecting accept. The other 19 packages matched their gold labels. All category floors were met: clear-accept (7/8), disclosure (1/1), no-evidence (4/4), unfollowable-comms (3/3), wrong-target (4/4).

**Check rationale**

From `tools/repro-check/rubric.md`, Check 1:

"P if the report states the version of the software under test, the operating system, and how it was installed or built, and any difference from the environment the issue targets is stated in the report rather than left for the reader to notice. F if any of those three is absent, or if the report's environment differs from the issue's target without the report saying so."

This reproduction states PathReview commit 2f4e82f, Python 3.13.5, Windows 11, and `pip install -e .` — all three required facts are present. The reproduction passes Check 1 (P).

Check 2: "P if a stranger could re-run the attempt without inventing anything that could change the failure: the starting state is pasted in full, or given verbatim in the issue itself and referenced exactly, or described where the parts left for the reader to write are either spelled out in the issue itself (inputs, options, ranges) or cannot change the failure being reproduced, and the command that triggers the failure appears exactly as it was run."

The reproduction provides the full `git clone` command, exact venv activation commands (`venv\Scripts\activate`), exact install commands (`pip install -e .`), and the full `repro.py` code to copy. A stranger could reproduce this without guessing. Passes Check 2 (P).

Check 3: "P if the output shows the same kind of failure as the issue, at the same point in the run. F if it shows a different kind of failure, or one that stops earlier than the issue's."

The reproduction shows both cases: Case 1 output `['Education', 'Skills']` (success) and Case 2 output `[]` (failure matching issue #54's reported empty detection). Passes Check 3 (P).

Check 4: "P if the output the conclusion rests on is shown, and every other run the report claims is either shown or stated in a line that agrees with the shown output (repeats of a shown run, controls, and side checks may be summarized rather than pasted)."

The conclusion "regex patterns need to account for optional leading whitespace" rests on the shown direct output and the 5 failed/5 passed pytest results. No claims rest on hidden runs. Passes Check 4 (P).

Check 5: "P if the comment describes what the author has already run, states next steps only as intentions (checking a code path, running more tests, reporting findings back—not as promises to fix or with any deadline), and makes any disclosure the repo's policy requires."

The claim comment describes the reproduction already run and states intention to "check the regex patterns and the section-header matching logic to understand what needs to change" — investigation, not fix promises. Passes Check 5 (P).

**Trade-offs**

The rubric and voice guide work together without conflicts. The rubric's emphasis on investigation intentions (not promises) aligns with the voice guide's distinction between "work I can commit to" and "work I cannot promise." The five rules in the voice guide are each concrete enough that the skill can evaluate them, and the rubric's five checks provide the grading criteria. This design means reproducibility and honesty are prioritized over speed or confidence self-assessment.

---

Related paths: `eval-run.txt` in this directory; your skill's files in `tools/repro-check/`.
