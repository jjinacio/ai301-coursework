# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval package, look at the opening of the Candidate repro report. Compare it against what the Issue body says (the version, OS, how it was installed) and what the Repo facts block shows from the bug report template. In live mode, find the environment section in your draft repro comment and compare it to the issue on GitHub and the template in `.github/`.

**What good looks like.** You name three things clearly: the version of the software, the OS, and how it was installed. Each should be a straight fact, not something someone has to infer from a command prompt. If any of those three is different from what the issue describes, you say it out loud instead of letting the reader find out later. Matching the issue is good. Differing but saying so is good. Differing without saying anything is bad.

## Steps

**Where it lives.** In an eval package, the steps are in the preparation and execution sections of the Candidate repro report. In live mode, they are the same parts of your draft repro comment.

**What good looks like.** Someone who has never seen this repo could follow your report and reproduce it on their own machine without inventing anything that could change the failure. The starting state reaches the reader in one of three ways: the file contents pasted in full, the contents quoted verbatim in the issue itself and pointed to exactly, or a description where the parts left for the reader to write are either spelled out in the issue itself (inputs, options, ranges) or cannot change the failure being reproduced (a boilerplate wrapper around parameters the issue pins down, a config where only the one relevant line matters). The command you ran appears exactly as you typed it, with all the flags and options. If the reader would have to guess a file's contents, a flag, a driver, or a command that could change what the run does, that is a fail. Pasting everything is always safe; describing is fine only when the parts you skip cannot matter or are already spelled out in the issue.

## Behavior shown

**Where it lives.** In an eval package, look at the pasted output blocks in the Candidate repro report. Read them against what the Issue says should happen (the error, the crash, the wrong value). In live mode, look at the output in your draft repro comment and read it against the issue on GitHub.

**What good looks like.** The output you show matches the kind of failure the issue describes, happening at the same point in the run. A crash should answer a crash report, a wrong value should answer a wrong value report. A handled error message is not the same as a crash, and a failure that stops early is not the same as one that reaches the broken code. The output itself is the truth. The report's words about it do not change what the output actually shows.

## Honesty

**Where it lives.** Claims are everywhere in the Candidate repro report and claim comment, any time you say what you did or what you found. Find the output that backs each claim. Read them together.

**What good looks like.** The output your conclusion rests on is shown in the report. Runs beyond that one, like repeats of a shown run, a control, or a side check, can be stated in a line instead of pasted, as long as what you say about them agrees with the output you did show. What you conclude should match what your own output actually shows. A report that says "I ran it and it did not crash" with output to prove it is honest and good. A report whose key claim rests on a run with no output anywhere is not honest no matter how carefully written, and a conclusion that contradicts the shown output is not honest either.

## Comms

**Where it lives.** Your Candidate claim comment is the text to look at. Read it against the Issue body, the Repo facts block (which shows the bug template and contribution policy including any AI disclosure requirement), and in live mode against the issue thread and the repo's `CONTRIBUTING.md` file.

**What good looks like.** The comment says what you have done and mentions this issue's specific details. A next step is fine when it is an intention to investigate: a code path to read, more attempts to run, findings to report back. What is not fine is promising a fix, giving a fix a deadline, or asking to have the issue assigned or reserved to you. Do not use words like complete or rigorous to describe your own reproduction, because that is for the reader to judge. If the repo's policy says you must disclose AI use, disclose it. Silence from the repo is fine, but if the repo states a requirement, meet it. A comment that skips a stated requirement is a blocker no matter what else is good about it.
