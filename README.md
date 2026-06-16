# LLM Council — a Claude Code skill

Stop your AI from being a yes-man. The **LLM Council** runs a decision through
five independent AI advisors who argue from clashing angles, anonymously
peer-review each other, and then have a chairman synthesize one honest verdict —
so the answer reflects the *argument*, not how you happened to phrase the
question.

Based on Andrej Karpathy's "LLM Council" idea.

## Install

Copy the skill into your skills directory:

```bash
# Project-level (this repo's Claude Code sessions)
mkdir -p .claude/skills
cp -r skills/llm-council .claude/skills/

# Or user-level (all your projects)
mkdir -p ~/.claude/skills
cp -r skills/llm-council ~/.claude/skills/
```

Restart Claude Code (or start a new session) and it will auto-load. The skill
triggers when you bring a real decision with stakes — e.g. "should I launch
this?", "poke holes in this plan", "talk me out of this pivot".

## Files

```
skills/llm-council/
├── SKILL.md                 # entry point: when to convene + the 5-step process
└── references/
    └── advisors.md          # the five advisor persona briefs
```

## What was fixed

The original copy of this skill had no YAML frontmatter, so Claude Code never
registered it as a skill, and its body was descriptive prose rather than
instructions Claude could execute. This version adds valid `name`/`description`
frontmatter with explicit trigger conditions and rewrites the body as concrete,
step-by-step operating instructions (parallel advisor spawn, anonymous peer
review, chairman synthesis).
