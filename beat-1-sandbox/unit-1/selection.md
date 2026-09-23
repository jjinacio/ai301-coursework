# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54

**Verdict output**

Live-mode run of the installed `issue-select` skill on three candidates
(#54, #65, #56), pasted verbatim. Verdict for #54 is `accept`, ranked first on fit.

````
﻿I have all the data. All three issues are in scope and fully evidenced. Let me now apply the rubric.

---

## Grading Summary

**Repo baseline (applies to all three):**
- Last push: 2026-09-16 (6 days ago) → **Active repo: PASS**
- AI/contribution policy: No CONTRIBUTING.md; PR template is silent on AI → **AI contribution allowed: PASS**
- Recent commits by human author "Andrew Burke" → live, healthy repo

---

### Issue #54 — "Resume section detection fails on text with leading whitespace"

| Check | Grade | Evidence |
|---|---|---|
| Active repo | **PASS** | Last push 2026-09-16 — 6 days before today, well within 90 days |
| Small clear fix | **PASS** | Bounded to `_detect_sections()` in `resume_parser.py`; exact failing tests named; single regex/whitespace fix; no debate in thread |
| Available | **PASS** | No assignees, no linked PRs, no comments at all |
| AI contribution allowed | **PASS** | No CONTRIBUTING.md; PR template silent on AI; silence = pass |

**Verdict: ACCEPT**

---

### Issue #65 — "review_service unit tests misconfigure async mocks — 13 of 19 tests fail"

| Check | Grade | Evidence |
|---|---|---|
| Active repo | **PASS** | Last push 2026-09-16 — 6 days before today |
| Small clear fix | **PASS** | Fix confined to mock setup in one test file (`tests/unit/test_review_service.py`); repro command given; no design debate |
| Available | **PASS** | No assignees, no linked PRs, no comments |
| AI contribution allowed | **PASS** | Policy silent on AI |

**Verdict: ACCEPT**

---

### Issue #56 — "Structural chunker silently drops documents that contain no headings"

| Check | Grade | Evidence |
|---|---|---|
| Active repo | **PASS** | Last push 2026-09-16 — 6 days before today |
| Small clear fix | **PASS** | Bounded to `StructuralChunker.chunk()` fallback logic; Python repro snippet provided; one named failing test; no design debate |
| Available | **PASS** | No assignees, no linked PRs, no comments |
| AI contribution allowed | **PASS** | Policy silent on AI |

**Verdict: ACCEPT**

---

## Fit Ranking (all three accepted)

**1. #54** — Best fit. Python regex/parsing bug in production code (`resume_parser.py`), labeled `good first issue` + `tier-1`. The fix is extremely precise (strip leading whitespace before pattern matching) and the verification path is crystal-clear: three named failing tests go green. Aligns with your preference for Python backend debugging with obvious test confirmation.

**2. #56** — Close second. Python ingestion bug also labeled `good first issue` + `tier-1`. Slightly less prescriptive about the exact fix (the body gestures at "single block or fallback" without deciding), but still a small, bounded addition to one method with one named failing test. Good for someone who wants a bit more design latitude.

**3. #65** — Solid but narrowest. Fix is test-code only (AsyncMock setup in one test file), so you won't touch production logic. The `tier-1` label is there but no `good first issue`. Great learning opportunity for Python async mock patterns, but the surface is narrower than the other two.

---

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/54",
    "checks": [
      {"name": "Active repo", "grade": "pass", "evidence": "Last push 2026-09-16 — 6 days before today (2026-09-22), well within 90-day threshold"},
      {"name": "Small clear fix", "grade": "pass", "evidence": "Fix scoped to _detect_sections() in resume_parser.py; exact failing tests named; no comments or debate"},
      {"name": "Available", "grade": "pass", "evidence": "assignees: [], closedByPullRequestsReferences: [], comments: [] — nothing in flight"},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md; PR template silent on AI; silence passes per rubric"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56",
    "checks": [
      {"name": "Active repo", "grade": "pass", "evidence": "Last push 2026-09-16 — 6 days before today, well within 90 days"},
      {"name": "Small clear fix", "grade": "pass", "evidence": "Fix scoped to StructuralChunker.chunk() fallback; one named failing test; no design debate in thread"},
      {"name": "Available", "grade": "pass", "evidence": "assignees: [], closedByPullRequestsReferences: [], comments: [] — nothing in flight"},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md; PR template silent on AI; silence passes per rubric"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/65",
    "checks": [
      {"name": "Active repo", "grade": "pass", "evidence": "Last push 2026-09-16 — 6 days before today, well within 90 days"},
      {"name": "Small clear fix", "grade": "pass", "evidence": "Fix confined to mock setup in tests/unit/test_review_service.py; exact repro command given; no design debate"},
      {"name": "Available", "grade": "pass", "evidence": "assignees: [], closedByPullRequestsReferences: [], comments: [] — nothing in flight"},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md; PR template silent on AI; silence passes per rubric"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

Four runs, in order:

1. `--only issue-12` — **1/1**. A single-issue canary, run first to confirm that a
   newly added policy check fired on the one bundle in the `policy` category before
   spending credit on a full run.
2. Full 20-issue run — **8/13 scored items**. Incomplete: 7 of the 20 bundles errored
   before they were graded. The harness pipes each prompt to the `claude` CLI over
   stdin, and on this Windows machine Python encoded that stream as cp1252, which
   cannot represent characters present in several bundles (`U+2728`, `U+2192`). Those
   7 items raised `UnicodeEncodeError` and never reached the model. The harness
   correctly refused to write `eval-run.txt`. Fixed by setting `PYTHONUTF8=1` in the
   environment; `run_eval.py` itself was not modified, so the file fingerprints in the
   saved run are the shipped ones.
3. `--only issue-01,issue-04,issue-11,issue-19,issue-15,issue-05,issue-09` — **6/7**.
   The five disagreements from run 2, plus `issue-05` and `issue-09` as canaries.
4. Full 20-issue run, saved with `--save-run eval-run.txt` — **19/20, bar PASS**,
   categories `claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1  scope 4/4`.

The final score, 19/20, matches the agreement line in the committed `eval-run.txt`.

**Issue analysis**

`issue-19` (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the
UI"). My rubric graded it **reject**; the gold label is **accept**. It is the single
disagreement in the committed run.

My `Small clear fix` check sank it. The issue body opens "There are two potential
causes which should be fixed:" and lists two, then adds a second list headed
"Additional suggestions:" with three more items. My check fails an issue when it "is
an umbrella or tracking issue listing sub-items meant to be split into separate work,"
and five enumerated work items across two lists is exactly the shape that clause
matches. The check has no way to tell that shape apart from a genuine umbrella issue
like `issue-05`, which it correctly rejects.

The gold note reads it the other way: "maintainer-diagnosed performance bug with named
causes, unclaimed." Under that reading there is one bug — the UI freezes — and the
lists are a collaborator's diagnosis of why, not a backlog. The suggestions are
optimizations a contributor may take or leave, not required deliverables. This is one
of the four scope calls the assignment describes as genuinely arguable, and I think
both readings are defensible from the bundle text alone.

**Check rationale**

The check, quoted as currently written in the uploaded `tools/issue-select/rubric.md`:

> | Small clear fix | issue title and body; issue labels; comment thread; repo_facts.this issue.linked PRs | Grade the size of the work requested, not the polish of the write-up. A terse body, a bare checklist, or a bug report with no repro steps can still pass; a long, well-formatted issue can still fail. P if the issue asks for one bounded change a newcomer could finish in a single pull request, and the thread shows no unresolved disagreement about what that change should be. F if any of the following holds: the issue is an umbrella or tracking issue listing sub-items meant to be split into separate work; the change is codebase-wide by its nature; the thread shows the design still under debate with no maintainer decision; a maintainer states the fix requires changes to core internals; the issue is a feature wish with no specification and an unmade product decision inside it; the issue is a usage or support question rather than a request for a change; or the issue's history shows repeated abandoned attempts (two or more closed, unmerged PRs) indicating the work is harder than the label suggests. | required |

This check began much stricter. Its first form required the issue to name "at least one
specific code location or implementation target" **and** give "at least one concrete
verification signal (repro steps, expected behavior, sample input/output, test
instruction, or acceptance condition)." That version scored badly in both directions at
once. It rejected `issue-01`, `issue-04`, `issue-11` and `issue-19` — all gold accepts —
because those are bounded pieces of work written up informally, and it accepted
`issue-15`, a gold reject, because that one is written up beautifully despite carrying
years of unresolved design debate and two abandoned pull requests behind a friendly
label.

Both failures have one cause: the check was grading how well the issue was *written*
rather than how large the work *is*. The evidence guide warns about precisely this —
"Short is not the same as unscoped... Grade the size of the work being asked for, not
the polish of the writeup." The rewrite makes that instruction the check's first
sentence, then replaces the two documentation requirements with a list of disqualifying
*shapes*: umbrella issues, codebase-wide changes, unsettled design debate, maintainer
statements that core internals are involved, unspecified feature wishes, support
questions, and histories of repeated abandoned attempts. Those are properties of the
work, and they are visible whether or not the reporter wrote a careful bug report.

**Trade-offs**

The rewrite gives up `issue-19`, and that loss is deliberate rather than accidental. The
"umbrella or tracking issue listing sub-items" clause is what rejects `issue-05`
(a codebase-wide type-annotation umbrella), `issue-10` (a self-described megaissue) and
`issue-20` (an unspecified feature wish) — three clear rejects the eval set was built to
force. Loosening it far enough to let `issue-19`'s five-item diagnosis through would put
all three at risk. Trading one arguable miss for three clear ones is a bad trade, and
the bar is set at 18/20 specifically to permit splitting on up to two arguable calls,
so I stopped tuning at 19/20 rather than chase the twentieth.

I also checked that the rewrite did not regress anything by re-running two canaries in
run 3. `issue-05` had to stay rejected despite the looser documentation requirements,
and it did. `issue-09` had to stay accepted despite my new "repeated abandoned attempts"
clause, since it carries exactly one closed unmerged PR; I set that threshold at "two or
more" for this reason, and it held. Run 4 confirmed both across the full set.

The clause I am least sure of is that same abandoned-attempts threshold. Two closed
unmerged PRs is a guess, not a measured number, and it is the clause most likely to
misfire on a repo where contributors routinely open and close drafts.

---

## Selection rationale

**Selection rationale**

I picked #54 because the fix is clearly scoped and it is purely Python, which is the
language I am strongest in. The issue is a good match for what I want to practice this
term: reading an unfamiliar codebase and debugging behavior that is wrong without being
obviously broken. The regex patterns in `_detect_sections()` are doing exactly what they
say, they are just anchored too strictly, so finding the fault means reading the code
carefully rather than following a stack trace. On time, I expect to have roughly a
weekend for Units 2 through 4, and a one-function fix with three named failing tests
fits inside that comfortably.

My skill got the mechanical checks right. It confirmed the repo is alive (last push six
days before my run, well inside my 90-day threshold), that nothing blocks contributing
with AI assistance (no CONTRIBUTING.md and a PR template that says nothing about it),
that the work is bounded to one function, and that nobody has assigned, claimed, or
opened a PR against it.

What I weighed that my rubric did not is that there is a clear way to know when I am
finished: I only need to make three failing tests pass. None of my four checks count
tests or ask how knowable "done" is, but for a first contribution that mattered more to
me than anything else on the list. The other thing I weighed is the trade I made on
subject matter. I said in my profile that I was interested in APIs, and #54 is not an
API issue, so I gave that up. I was willing to because the project is still machine
learning focused: PathReview is a RAG system, and `_detect_sections()` sits in the
ingestion path that feeds its index, so a resume this parser mishandles is a resume the
retrieval side never sees properly. My rubric has no way to weigh either of those. All
four of its checks ask questions about the issue and the repo; none of them ask anything
about the person taking it, and fit only ever reorders issues the checks have already
accepted.

I expect claiming to be straightforward. The issue has no assignee, no linked pull
requests, and no comments at all, so I am not working alongside anyone or picking up
someone else's half-finished attempt. Path Review's house rule also means classmate
claim comments would not block me even if some appeared later, since credit attaches to
the pull request I open rather than to whether it merges. The real difficulty is not
claiming it but the work after: deciding whether to loosen the patterns with `\s*` or
`[ \t]*`, since `\s` also matches newlines and could match a section name that merely
starts a continuation line.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
