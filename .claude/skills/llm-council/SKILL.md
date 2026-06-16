---
name: llm-council
description: >-
  Pressure-test a decision through a council of five independent AI advisors who
  analyze it from clashing angles, anonymously peer-review each other, and then
  have a chairman synthesize a single honest verdict. Use when the user presents
  a real decision with stakes and uncertainty — pricing, a pivot, a launch,
  positioning, a hire, an architecture choice, "should I…", "is this a good
  idea", "talk me out of this", "poke holes in this" — and wants it stress-tested
  instead of rubber-stamped. Do NOT use for factual lookups, simple how-to
  questions, or tasks with one objectively correct answer.
---

# LLM Council

Claude tends to agree with you. Ask "should I launch this?" and it finds reasons
to launch; frame the same thing as "is this a bad idea?" and it finds reasons
not to. Same facts, opposite verdict. The Council removes that framing bias by
forcing five independent advisors to argue *before* anyone agrees, then having a
chairman weigh the argument rather than your phrasing.

## When to convene the council

Convene when ALL of these hold:
- There is a genuine **decision** (not a fact to look up).
- It carries **stakes** — time, money, reputation, or hard-to-reverse commitment.
- There is **real uncertainty** or more than one defensible option.

Skip it (just answer normally) for factual questions, lookups, creative writing
with no decision, or anything with a single objectively correct answer. If you
are unsure whether it qualifies, ask the user one sentence: "Want me to run this
through the Council, or just give you my take?"

## The five advisors

Each advisor thinks from one fixed angle and does not hedge. Full persona briefs
are in `references/advisors.md` — read that file before spawning them.

1. **The Contrarian** — hunts for the fatal flaw, the failure mode, the reason
   this blows up. Assumes it's a bad idea and tries to prove it.
2. **The First-Principles Thinker** — ignores convention and re-derives the
   problem from scratch. Questions whether the framing itself is right.
3. **The Expansionist** — finds the asymmetric upside, the bigger version, the
   thing the user is under-reaching on.
4. **The Outsider** — fresh eyes with no domain baggage; names the obvious thing
   insiders rationalize away.
5. **The Executor** — ignores whether it's a good idea and asks only: can this
   actually be done, by whom, by when, and what breaks first in practice.

## Process

### Step 1 — Frame the question neutrally
Restate the user's decision in one or two neutral sentences, stripping loaded
framing ("obviously great" / "probably dumb"). Include the concrete context the
advisors need (constraints, resources, timeline, what success looks like). If a
critical fact is missing, ask the user before proceeding.

### Step 2 — Spawn all five advisors in parallel
Launch five `Agent` subagents **in a single message** (one tool block, five
calls) so they run concurrently and independently — no advisor sees another's
answer. Give each one: the neutral question + context, and its persona brief
from `references/advisors.md`. Each must return **150–300 words** of specific,
grounded analysis from its angle, ending with a one-line bottom-line stance.

### Step 3 — Anonymous peer review
Collect the five responses, strip the advisor names, and label them Response A–E.
Spawn five review subagents in parallel; give each reviewer all five anonymized
responses and ask it to name: (a) the single strongest response and why, (b) the
biggest blind spot across the set, and (c) anything every response missed.

### Step 4 — Chairman synthesis
You act as the Chairman. Using the advisor responses and the peer reviews,
produce a verdict with these sections:
- **Convergence** — what the advisors agreed on.
- **Real disagreements** — where they genuinely clashed, and the substance of
  each side (not "some said X").
- **Blind spots surfaced** — what the review round exposed.
- **Recommendation** — your clear, direct call. You MAY override the majority if
  the reasoning warrants it; say so explicitly and why.
- **Next step** — one concrete action the user can take now.

### Step 5 — Deliver
Present the verdict directly in chat as scannable markdown (the sections above as
headers). Lead with the recommendation in one sentence, then the detail. Be
direct over balanced — the user came here to be challenged, not flattered. Do not
write the verdict to a file unless the user asks.

## Cost note

A full run is ~10 subagent invocations. Mention this only if the user seems
sensitive to it; for a high-stakes decision it is cheap insurance.
