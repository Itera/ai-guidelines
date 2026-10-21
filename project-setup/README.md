# Project setup

Guidance and tools for setting up a repository for AI-assisted development so it stays **secure**, **token-efficient** and **maintainable**.

## Dev Foundation Setup skill

[`dev-foundation-setup`](skills/dev-foundation-setup/SKILL.md) is a skill that plans and sets up a repo's AI-assisted dev foundation: a lean `CLAUDE.md`, architecture docs and ADRs, dev standards, secret guardrails, and skills for planning issues and reviewing PRs. It can also audit an existing setup.

### What it does

It runs in four phases and stops for review between them:

1. **Intake:** asks up to 4 questions (stack, issue tracker, compliance needs, existing docs).
2. **Discovery:** looks around the repo without changing anything, and skips secret files.
3. **Plan:** proposes a file tree, permission rules, skill trigger texts and a maintenance routine. Nothing is written until the developer approves it.
4. **Build:** done in steps, each reviewed before the next:
   - **A:** security guardrails (`.claude/settings.json` deny rules, `.gitignore`, a secret-scanning recommendation)
   - **B:** a lean `CLAUDE.md`, `docs/architecture`, ADRs and dev standards
   - **C:** the `issue-to-plan` and `pr-review` skills, plus optional ADR and docs-sync skills
   - **D:** verification

If a repo already has a setup, it switches to an **audit mode** that finds bloat, duplicated content and missing guardrails.

### How to use it

1. **Get the skill into your tool.**
   - **Claude Code:** commit it once to your repo as `.claude/skills/dev-foundation-setup/SKILL.md`. Everyone who clones the repo then has it, and the skills it creates end up in the same `.claude/skills/` folder.
   - **Cowork or claude.ai:** add it to your own account.
   - **Shared centrally:** if your organization shares skills centrally, publish it there.
2. **Start it inside the repo.** Type `/dev-foundation-setup`, or ask something like "set up our dev foundation for this repo". Starting in plan mode is a good extra safety step.
3. **Answer the intake questions, review the plan and approve each step.** The most important check is step A: open `settings.json`, then start a new session and ask Claude to read `.env`. It should be refused.
4. **Use the skills it created in daily work:**
   - "Plan issue #123" runs `issue-to-plan`.
   - "Review PR #45" or "review my branch" runs `pr-review`, which flags security problems and missing doc updates. It never posts comments unless asked.
5. **Maintain it.** Once a quarter, run `/dev-foundation-setup` again on a repo that already has a setup. It will audit and trim instead of starting over.

### Two things to know

- The deny rules for shell commands are a safety net, not a guarantee. For sensitive repos, add a hook or a sandbox, as the skill points out.
- The permission syntax in the example config may need small adjustments for your Claude Code version. Step D checks this by having developers test the rules.
