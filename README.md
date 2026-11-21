# Pocket Skills

A collection of practical skills for coding agents. Pick the one that fits the work and keep its instructions and references close to your agent.

## Skills

| Skill | What it helps with |
| --- | --- |
| [maintainable-changes](skills/maintainable-changes/SKILL.md) | Make changes that stay easy to understand and revise, with judgment around comments, tests, compatibility and abstractions. |
| [writing-good-tests](skills/writing-good-tests/SKILL.md) | Write tests that catch meaningful regressions without unnecessary mocks and test bloat. |
| [test-audit](skills/test-audit/SKILL.md) | Audit a test suite for low-value, duplicate and implementation-coupled tests while preserving useful coverage. |
| [prune-tests](skills/prune-tests/SKILL.md) | Find and remove tests that do not protect behavior. |
| [build-portfolio-site](skills/build-portfolio-site/SKILL.md) | Plan, build and deploy a personal portfolio, with guidance for design, search and AI visibility. |

## Install

Run this in your project and choose the skills and agents you want.

```sh
npx skills add fenil210/pocket-skills
```

To install a single skill, use its name.

```sh
npx skills add fenil210/pocket-skills --skill maintainable-changes
```

Add `-g` to install across projects. The [skills CLI](https://github.com/vercel-labs/skills) supports Codex, Claude Code and other agents.

You can also copy a complete skill directory from `skills/` into your agent's skills directory. For project-specific installations, Codex uses `.agents/skills/` and Claude Code uses `.claude/skills/`. Keep all references and license files together with `SKILL.md`.

## Origins and licenses

`maintainable-changes` is part of this collection. `build-portfolio-site`, `test-audit`, and `writing-good-tests` come from [shipshape-skills](https://github.com/fenil210-cactus/shipshape-skills). `prune-tests` comes from [caldoai/skills](https://github.com/caldoai/skills/tree/main/skills/prune-tests). The imported skills retain their original license notices in their directories. `test-audit` includes its upstream OpenClaw Foundation license and the shipshape repository license.
