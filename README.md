# Pocket Skills

I collect skills that help coding agents build and maintain software. Each skill includes instructions and any supporting references it needs.

## Skills

| Skill | Use it to |
| --- | --- |
| [maintainable-changes](skills/maintainable-changes/SKILL.md) | Make code changes that stay readable and easy to maintain. |
| [writing-good-tests](skills/writing-good-tests/SKILL.md) | Write tests that catch meaningful regressions. |
| [test-audit](skills/test-audit/SKILL.md) | Review a test suite for weak, duplicate or brittle coverage. |
| [prune-tests](skills/prune-tests/SKILL.md) | Remove tests that don't protect behavior. |
| [build-portfolio-site](skills/build-portfolio-site/SKILL.md) | Build and deploy a personal portfolio site. |

## Install

Give your coding agent the repository link and ask it to install the skills you want:

```text
Install all skills from https://github.com/fenil210/pocket-skills for this project.
```

Or ask for a specific skill:

```text
Install maintainable-changes from https://github.com/fenil210/pocket-skills for this project.
```

You can also run the [skills CLI](https://github.com/vercel-labs/skills) yourself. To install all skills, then choose your agent:

```sh
npx skills add fenil210/pocket-skills --skill '*'
```

To install one skill, replace the name with any skill from the table:

```sh
npx skills add fenil210/pocket-skills --skill maintainable-changes
```

Add `-g` to install for your user across projects. For manual installation, copy the entire skill folder into your agent's skills directory. Codex uses `.agents/skills/` for a project, and Claude Code uses `.claude/skills/`.

