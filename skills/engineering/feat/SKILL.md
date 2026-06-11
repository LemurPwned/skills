---
name: feature
description: Feature skill for new feature development
---

# Feature skill

## When to use 

Whenever you need to add a new feature to the codebase.


## Core prompt
Follow the below steps:

> Before implementing your change, run the skill `skills/grill-me` to ensure that you understand the user's intention clearly.
> After each change, make sure you have a small working test for this feature.
> After implementing your change, run `skills/engineering/thermo-nuclear-code-quality-review` to ensure that your change does not introduce any regressions.
> Fix the issues indicated by the `thermo-nuclear-code-quality-review`.
> If applicable to the codebase, update `AGENTS.md` or `ARCHITECTURE.md` if your changes are relevant.
