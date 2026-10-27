---
name: fjellvettregler-check
description: Check how well a developer and their project follow Itera's AI-Fjellvettregler, and help close the gaps. Use when asked to check AI guideline compliance, run a Fjellvettregler check, review AI use on a project, or at the start of a new customer engagement.
---

# Fjellvettregler Check

Checks how well one developer and one customer project follow the [AI-Fjellvettregler](https://github.com/Itera/ai-guidelines/blob/main/guidelines/fjellvettregler.md), then helps fix the gaps.

**This is a self-check for the developer, not an audit.** The report belongs to the developer, who decides whether to share it. Honest answers matter more than a clean report.

Most of the rules are about agreements and habits, not code. So the check is mostly an **interview**, with a **read-only scan** of the repo for the rules that leave traces there. Run it at project start and then every quarter.

Work in phases. Don't change any file before the user approves the fix (Phase 4).

## How to install

Install it in your own account, not in customer repos, so it works on every project:

- **Claude Code:** copy this folder to `~/.claude/skills/fjellvettregler-check/`.
- **Cowork or claude.ai:** add the skill to your own account.

Then start it from inside the project's repo with `/fjellvettregler-check`, or ask something like "run a Fjellvettregler check".

## Non-negotiable principles

- **No secrets.** Never open, print or summarize `.env*` (except `.env.example`), keys, certificates, `secrets/`, `credentials*`, `*.tfstate*` or config files with connection strings. Never run `env`, `printenv` or commands that show tokens. You may check *whether* a credential exists or which account is signed in, but never show its value.
- **Customer details stay local.** The report and any notes contain customer context. Show them in the session, or save them in the customer's repo if the user asks. Never send them anywhere else, and never add them to the `ai-guidelines` repo.
- **Unclear means "can't verify", not "OK".** If you can't tell, ask. If the user doesn't know, mark the rule ❓.

## The rules

This skill's questions and checks are written for these ten rules:

1 Know the rules before you head out · 2 Tell someone where you're going · 3 Use approved equipment · 4 Pack only what you can afford to lose · 5 Give agents their own rope · 6 Know your provisions · 7 You own what you bring home · 8 There's no shame in turning back · 9 Hike as a team · 10 Check that the trip was worth it

**Drift check:** at the start, try to fetch `https://raw.githubusercontent.com/Itera/ai-guidelines/main/guidelines/fjellvettregler.md` and compare its rule titles with the list above. If they differ, warn the user: "The guidelines have changed since this skill was written. Some checks may be out of date." Then continue. If the fetch fails or is blocked, continue without a warning.

## Phase 1 — Intake

Ask for specifics, not yes or no. Use a multiple-choice question tool if one is available (max 4 per round), with "Don't know" as an option.

Score each answer:
- **✅** the user can name specifics
- **⚠️** the answer is vague or partial ("I think so", "mostly")
- **❓** the user doesn't know

First round:

1. **Policy (rule 1):** Which regulations apply to this customer (for example GDPR, the AI Act, sector rules), and which part of Itera's AI Use Policy matters most for this project? If the user hasn't read the policy, give them the link to [AI Use Policy at Itera](https://iteracloud.sharepoint.com/sites/framework/Itera%20Framework/AI%20use%20Policy%20at%20Itera.aspx) to read themselves. Never try to fetch it, because it requires an Itera login.
2. **Agreement (rule 2):** What did you agree with the customer about AI tools, data, hosting and cost, and where is it written down? Options: written / verbal / none / don't know.
3. **Tools (rule 3):** Which AI tools and models do you use on this project, who hosts them, and where did you check that they're approved for the PC you use (Itera's or the customer's)?
4. **Data (rule 4):** Which kinds of sensitive data does the project handle (personal data, health data, financial data, trade secrets, none), and how do you keep them out of prompts and agent context?

Second round:

5. **Costs (rule 6):** About how much did the project's AI use cost last month, and where would you see an alert if it went over budget?
6. **Turning back (rule 8):** When did AI work last go in circles for you, and what did you do then?
7. **Team (rule 9):** Which setups, skills or lessons has the team shared in the last few months, and where?
8. **Effect (rule 10):** Which metric tells you whether AI improves delivery on this project (DORA metrics, cycle time or similar), and what does it show?

Skip questions the user has already answered.

## Phase 2 — Repo scan (read-only)

Only scan if you're inside the project's repo. Sample, don't read whole trees. These checks are written for Claude Code. For other tools, look for their matching config files, or ask.

- **Rule 3, approved equipment:** list MCP servers (`.mcp.json`, `.claude/settings*.json`), plugins and third-party skills in `.claude/skills/`. For each one from outside the team, ask whether it was vetted and where it comes from.
- **Rule 4, sensitive data:** does `.gitignore` cover secret files? Are there deny rules for secret files? Do `CLAUDE.md`, `.claude/rules/` or committed prompts contain customer data, personal data or real internal hostnames? Is secret scanning (gitleaks, platform scanning) set up?
- **Rule 5, own rope:** look at permission rules, hooks and sandbox settings. Is auto mode or `bypassPermissions` turned on without hard limits? Do agents use personal tokens (for example `gh auth status` shows the developer's own account with broad scopes) or their own service accounts? Do MCP servers use the developer's credentials?
- **Rule 6, provisions:** is a default model set? Is the most expensive model the default for everything?
- **Rule 7, ownership:** is there a PR review process (branch protection, required reviews, a `pr-review` skill)? Are architecture decisions recorded (ADRs) by people?
- **Rule 9, team:** is `.claude/` (settings, skills, rules) committed and shared, or does everything live in local files?

## Phase 3 — Report

Give one row per rule:

| # | Rule | Status | Evidence | Next step |
|---|------|--------|----------|-----------|

- **Status:** ✅ OK · ⚠️ Gap · ❓ Can't verify. Combine the interview score and the scan. If they disagree, use the worse one.
- **Evidence:** one line: what the user said or what you found (file path, setting). No secret values.
- **Next step:** one concrete action and a link to the rule's wiki page: `https://github.com/Itera/ai-guidelines/blob/main/wiki/fjellvettregler/<NN>-<slug>.md`. Fetch the page if you need its advice.

After the table, list the **top 3 gaps** to fix first. Put rules 2, 4 and 5 first, since they carry the highest risk for the customer.

## Phase 4 — Help fix the gaps (after approval)

Ask which gaps to work on. Then, one at a time, show the exact change and write it only after the user approves.

**Shared files:** `.claude/settings.json`, CI config, `.gitignore` and other committed config affect the whole team, possibly including the customer's developers. Make those changes on a branch, and tell the user to put them through the team's normal PR review.

- **Rules 4 and 5 (guardrails):** if the [`dev-foundation-setup`](https://github.com/Itera/ai-guidelines/blob/main/skills/dev-foundation-setup/SKILL.md) skill is available, use it for deny rules, hooks and secret scanning. If not, propose the change and show the exact config before writing it.
- **Rule 2 (agreement):** offer to write an AI use note (see below) for the developer to take to the customer.
- **Rule 3 (equipment):** for each unvetted MCP server, plugin or skill: read its source or description, sum up what access it has, and let the user decide whether to keep it.
- **Rules 1, 6, 8 and 10:** these are habits. Suggest one small, concrete step (for example "set a budget alert in the provider's console", "add a lead-time metric to the next retro") and link the wiki page.

Never message the customer, change tool accounts or install tools yourself.

## AI use note (rule 2)

A short note the developer and the customer can agree on. Draft it from the intake answers, mark anything unknown as `<to agree>`, and keep it to one page. Suggest saving it as `docs/ai-use.md` in the customer's repo if the customer is fine with that, or else wherever the team keeps its agreements.

```markdown
# AI use on <project>

Agreed between <customer contact> and <Itera contact>, <date>.

## Tools and models
- <tool>, <model>, hosted by <provider> in <region>

## Data
- The AI may see: <e.g. source code, test data, public docs>
- The AI may not see: <e.g. production data, personal data, contracts>

## Cost
- AI costs are <billed to the customer / included in Itera's delivery>. Budget: <to agree>

## Agents
- Agents run with: <own service account / scoped token>, with <permission limits>

## Changes
- New tools or wider data access are agreed with <contact> before use.
- We review this note when the team, tools or scope change, or at least every <6> months.
```

## Finish

Sum up what was checked, what was fixed, and what's still open. Suggest running the check again in three months, or sooner if the team, tools or customer agreement change.

Also tell the user they can share anonymized lessons in the `ai-guidelines` repo. A lesson about rule 8 or rule 10 helps the whole company.
