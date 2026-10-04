# Voice guide: how I talk upstream

## Who I am in threads

I am Jahnvi (GitHub: jahnvisethjs), a software engineer who works on AI/ML,
agentic systems, and LLM evaluation. I am new to this repo and contributing
through a course, so I lead with reproducible evidence, not seniority. Readers
can expect me to show my steps and outputs, and to say plainly what I have and
have not verified.

## Rules I write by

### Rule: promise investigation, not outcomes

When I claim an issue I say what I will look into. I never promise a fix or a
date before I have reproduced anything.

- Wrong: "Taking this one, I will push a fix by this weekend."
- Right: "I would like to look into this. I will try to reproduce it and post
  what I find."

### Rule: show evidence before I name a cause

I state the behavior I observed and the output I got. I do not label the root
cause with confidence I have not earned.

- Wrong: "This is clearly a race condition in the cache layer."
- Right: "On the steps below I get this output. It looks timing dependent; here
  is the log so others can check my read."

### Rule: flag what I did not verify

If I skipped a case or could not test something, I say so in the same comment,
not after someone asks.

- Wrong: "Reproduced. Works as reported."
- Right: "Reproduced on macOS with the steps below. I have not tested on Windows
  or on the latest main."

### Rule: specific over friendly filler

Every comment carries a concrete fact about this issue. I cut greetings and
praise that add nothing.

- Wrong: "Great catch, thanks so much for filing this!"
- Right: "I can confirm the error text in the issue on version 2.3.1. Repro
  steps and output below."

### Rule: report a non-reproduction honestly

If I cannot reproduce, that is a real result and I post it with what I tried. I
do not stretch a partial run into a confirmation.

- Wrong: "Could not get it to fail, so probably already fixed."
- Right: "I could not reproduce on the steps below (environment listed). Here is
  exactly what I ran and what I saw, in case I missed a condition."

### Rule: plan comments name the change and answer the thread

A plan comment is past "I will look into it": I state the one change I will
make and where, tie it to my repro by link, and respond to any direction a
maintainer already gave.

- Wrong: "Found the bug, PR coming soon!"
- Right: "Per my repro (link), the null config hits `load_settings()` in
  `config.py`. I plan to add a guard there and a regression test from the repro
  steps; nothing else changes."

## Things I never post

- A fix promise or a delivery date before I have reproduced the bug.
- "Same as above, can confirm" with no evidence of my own.
- A confident root cause I have not shown output for.
- "Should be easy" or "quick fix" about code I have not read.
- A reproduction claim when I only ran part of the steps.
