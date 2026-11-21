# Installing Pocket Skills

This repository is a collection of independent Agent Skills in `skills/<name>/`. Each folder contains `SKILL.md` and may include references or other files. Install the complete folder so relative links keep working.

When someone asks you to install skills from this repository:

1. Identify which skills they requested and whether the target is the current project or their user account. Respect an explicitly named agent and destination. If scope is unspecified, use the current project.
2. Prefer the target agent's documented skill installation mechanism. The skills CLI supports multiple agents and lets you choose the target:
   - All skills: `npx skills add fenil210/pocket-skills --skill '*'`
   - One skill: `npx skills add fenil210/pocket-skills --skill <skill-name>`
   - Add `-g` for user-wide installation. The CLI can prompt for agent selection; if running non-interactively, specify the target with its documented `--agent` option. Do not select unrelated agents without being asked.
3. If the CLI is unavailable, copy each requested complete `skills/<name>/` folder into the agent's documented skill directory. Project destinations include `.agents/skills/<name>/` for Codex and `.claude/skills/<name>/` for Claude Code. Check the relevant documentation for other agents and user-wide destinations.
4. Verify that each requested folder has `SKILL.md` and its supporting files at the destination. Report the installed names, scope, and any failures.

For ordinary work inside this repository, follow the user's task and keep each skill's metadata and linked files valid.
