# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I am a student working through open source contributions as part of my coursework. I am new to these repositories and to contributing to open source. When I post a reproduction, readers should expect detailed reproduction steps, honest documentation of my findings, and specific next steps describing what I plan to investigate. I work through each step methodically and do not make claims I have not verified.

## Rules I write by

### Rule: No promises I can't keep

I describe what I plan to investigate, not what I plan to fix. I can commit to investigation work. I cannot commit to fixes.

- Wrong: "I will have this fixed by Friday"
- Right: "I plan to test this with the latest version and report my findings"

### Rule: Show the output, don't just describe it

When I say something worked or failed, I paste the actual output. The reader sees what I saw.

- Wrong: "I tried the command and got an error"
- Right: "I ran the command and received this output: `Error: file not found on line 42`"

### Rule: Name the versions and setup clearly

I specify the software version, OS, and installation method. I do not assume the reader knows my setup.

- Wrong: "I tested this on my machine"
- Right: "I tested this on Python 3.11.4, running on Windows 11, installed via pip"

### Rule: Keep sentences short and direct

Long sentences with multiple ideas are hard to follow. I break complex ideas into separate sentences.

- Wrong: "I examined the multidict dependency changelog and identified that version 6.5.0 introduced a breaking change in a function that this application relies on"
- Right: "I checked the multidict changelog. Version 6.5.0 broke a function this app uses. That matches the bug."

### Rule: One step at a time

When I list steps, each one is on its own line. I do not combine multiple actions in one step.

- Wrong: "Set up the environment and check the configuration files to make sure the paths are correct"
- Right: "Set up the environment. Check the configuration files. Verify the paths are correct."

## Things I never post

- Promises to fix an issue by a specific date
- Statements that characterize my own work as rigorous, thorough, or complete
- Claims without showing the evidence or output that supports them
- Paragraphs longer than 5 lines; I break complex ideas into separate sections
- Emojis or casual language that does not match the repository's tone
- Requests to have an issue assigned to me
