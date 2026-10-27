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

**Language:** before anything else, ask the user whether to run the check in English or Norwegian (bokmål). Use that language for every question, reaction, the report and the AI use note. Use Norwegian names for Norwegian laws and bodies (for example *sikkerhetsloven*, *personvernombud*). The rule titles have no official Norwegian version, so give them in English. The linked wiki pages are in English.

**Links:** whenever you give the user a link, write the full URL as plain text, not as a Markdown link (`[text](url)`) or in backticks. Terminals like Claude Code don't make Markdown links clickable, but they do make bare URLs clickable.

## Phase 1 — Intake

Ask one thing per question, and ask for specifics, not yes or no. Keep it conversational: ask **one question at a time**, in the order below, and wait for the answer before you ask the next. Never batch several questions in one message or tool call. React briefly to each answer (one sentence, for example a short tip or a note that it's a gap) before moving on. A multiple-choice question tool is fine for questions with options, one question per call, with "Don't know" as an option. Skip questions the user has already answered, and follow-ups that don't apply. The rule headings below are for your scoring only. Don't tell the user which rule a question belongs to.

Score each rule from its answers:
- **✅** the user can name specifics for every question
- **⚠️** some answers are vague or partial ("I think so", "mostly")
- **❓** the user doesn't know

**Rule 1, policy**

1. Which industry is the application in? Options: health / finance / public sector / other.
2. Ask only if the answer to 1 was not "other": do you know whether the project follows the rules for that industry, on top of GDPR? Options: yes / partly / no / don't know. Name the rules for the chosen industry in the question:
   - **Health:** *Normen*, the Norwegian code of conduct for information security in healthcare.
   - **Finance:** rules from Finanstilsynet and the EU's Digital Operational Resilience Act (DORA). Not the same as the DORA metrics in rule 10.
   - **Public sector:** the Security Act and Digdir's guidance.
3. Has the application had a GDPR review, for example by the customer's data protection officer? Options: yes / partly / no / not needed, no personal data / don't know. Give these common ways applications break GDPR as examples:
   - **No legal basis:** processing personal data without valid consent or another legal basis, for example tracking cookies set before the user agrees.
   - **Weak security:** missing access control, unencrypted personal data, or personal data in logs and test environments.
   - **Unsafe transfers:** sending personal data outside the EU/EEA, for example to a cloud or AI service, without a valid transfer basis.
4. Have you read Itera's AI Use Policy? Options: yes / partly / no. If not "yes", give them the link to read themselves. Give the full URL on its own line (see **Links** above):
   https://iteracloud.sharepoint.com/sites/framework/Itera%20Framework/AI%20use%20Policy%20at%20Itera.aspx
   Never try to fetch it, because it requires an Itera login.

**Rule 1, EU AI Act**

- **A1.** Does the application you build have AI functionality, such as a chatbot, recommendations, scoring of people, or generated text or images? Options: yes / no / don't know.
- **A2.** Ask only if A1 was "yes": has the customer approved the AI functionality, for example through its legal or compliance department? Options: yes / no / don't know.

If A1 was "yes" or "don't know", tell the user that some AI functionality falls under the EU AI Act, which Norway is adopting through the EEA Agreement. Give these examples (give the link https://nkom.no/ki/hvem-gjelder-ki-loven to Nkom's AI Act page for more):
- **Prohibited:** manipulating people, social scoring, or recognizing emotions at work or in schools.
- **High-risk, with strict requirements:** AI used in hiring, education and exams, credit or public benefits, critical infrastructure, or biometric identification.
- **Transparency:** chatbots must tell people they are talking to an AI, and some AI-generated content must be marked.

Scoring:
- **✅** A1 is "no", or A2 is "yes".
- **⚠️** A2 is "no" or "don't know".
- **❓** A1 is "don't know".

**Rules 2 and 3, tools and agreement** (agreed with the customer before you started)

5. Which AI tools do you use on this project? Ask this as an open question, not multiple choice, so the user can write out all their tools.
6. Through which subscription do you use each tool? Options: the customer's / Itera's / your own personal one / don't know.
7. Who approved each tool? Options: the customer / only Itera / nobody / don't know.
8. What data did you agree with the customer that the AI may see?
9. Ask only if a tool runs on Itera's or a personal subscription: are those AI costs billed to the customer or included in Itera's delivery? Options: billed to the customer / included in Itera's delivery / not agreed / don't know.
10. Do you use any models outside those tools (for example through a direct API, or a local model)? If yes, ask who owns each model, then where it runs, as separate questions.

The developer doesn't need to know where the models in an approved tool are hosted. That's checked once, by whoever approves the tool. For example, with GitHub Copilot on the customer's business subscription, GitHub is the provider, and its terms cover the models it offers.

Scoring:
- **Rule 2:** ✅ if the customer approved every tool (7), the data is agreed (8), and the costs are settled (the customer's subscription in 6, or an answer in 9).
- **Rule 3:** ✅ if every tool is approved by the customer or Itera, no tool runs on a personal subscription without approval, and any model from 10 has a known owner and location.
- "Only Itera" in 7 is OK for rule 3 but a gap for rule 2, because the customer hasn't agreed to the tool.

**Rule 4, sensitive data**

11. Which kinds of sensitive data does the project handle? Let the user choose several options at once, or write them out if the tool doesn't allow that. Options: personal data / health data / financial data / trade secrets / none / don't know.
12. How do you keep that data out of prompts and agent context? Skip if the answer to 11 was "none".

**Rule 6, provisions**

13. How much AI use do you have available, for example tokens, credits or premium requests per month? Options include "no limit" and "don't know".
14. When do you get more? Options: every month / when someone tops it up / when I ask for it / don't know.
15. How can you see how much you've used so far? Options: in the tool / in a dashboard / I get an alert near the limit / no way to see it / don't know.
16. How do you choose the model and effort level for a task?

**Rule 9, team**

17. Which setups, skills or lessons has the team shared in the last few months?
18. Where does the team share them?

**Rule 10, effect**

19. Which metric tells you whether AI improves delivery on this project (DORA metrics, cycle time or similar)?
20. What does that metric show so far? Skip if the answer to 19 was "none" or "don't know".

## Phase 2 — Repo scan (read-only)

Only scan if you're inside the project's repo. **Ask the user first**, and say in one sentence what you'll look at (settings, permissions, MCP servers, `.gitignore`, review setup), and that you won't open secret files. If they say no, skip this phase and score from the interview only. Sample, don't read whole trees. These checks are written for Claude Code. For other tools, look for their matching config files, or ask.

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

- **Status:** ✅ OK · ⚠️ Gap · ❓ Can't verify · 💡 Reminder. Rule 8 is a judgment call in the moment, so it's not asked about or scored. Give it 💡 and this next step: "Stop if the AI goes in circles, or you no longer understand what it does. Rethink the task or do it yourself." Combine the interview score and the scan. If they disagree, use the worse one.
- **Evidence:** one line: what the user said or what you found (file path, setting). No secret values.
- **Next step:** one concrete action and a link to the rule's wiki page: `https://github.com/Itera/ai-guidelines/blob/main/wiki/fjellvettregler/<NN>-<slug>.md`. Fetch the page if you need its advice.

After the table, list the **top 3 gaps** to fix first. Put AI functionality the customer hasn't approved first (see Rule 1, EU AI Act), then rules 2, 4 and 5, since they carry the highest risk for the customer.

## Phase 4 — Help fix the gaps (after approval)

Ask which gaps to work on. Then, one at a time, show the exact change and write it only after the user approves.

**Shared files:** `.claude/settings.json`, CI config, `.gitignore` and other committed config affect the whole team, possibly including the customer's developers. Make those changes on a branch, and tell the user to put them through the team's normal PR review.

- **Rules 4 and 5 (guardrails):** if the [`dev-foundation-setup`](https://github.com/Itera/ai-guidelines/blob/main/skills/dev-foundation-setup/SKILL.md) skill is available, use it for deny rules, hooks and secret scanning. If not, propose the change and show the exact config before writing it.
- **Rule 2 (agreement):** offer to write an AI use note (see below) for the developer to take to the customer.
- **Rule 3 (equipment):** for each unvetted MCP server, plugin or skill: read its source or description, sum up what access it has, and let the user decide whether to keep it.
- **Rule 1, GDPR:** never conclude that the application does or doesn't follow GDPR. If it hasn't had a GDPR review, suggest that the developer asks the customer's data protection officer for one.
- **Rule 1, AI Act:** never conclude that a use is or isn't legal. If the AI functionality isn't approved, suggest that the developer asks the customer's legal or compliance contact to look at it.
- **Rules 1, 6 and 10:** these are habits. Suggest one small, concrete step (for example "set a budget alert in the provider's console", "add a lead-time metric to the next retro") and link the wiki page.

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
