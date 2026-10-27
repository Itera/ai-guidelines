---
name: dev-foundation-setup
description: Plan and set up a repo's AI-assisted dev foundation: lean CLAUDE.md, docs/ADRs, dev rules, secret guardrails, and issue/PR-review skills. Also audits an existing setup.
---

# Dev Foundation Setup

Sets up (or audits) the documentation, architecture guidelines, development rules and reusable skills for one repository so AI-assisted development stays **secure**, **token-efficient** and **maintainable**.

Work in phases. Never create or modify files before the user approves the plan (Phase 2). After each build step in Phase 3, stop and ask for review.

## Non-negotiable principles

**Security**
- Never open, print, grep or summarize: `.env*` (except `.env.example`), `*.pem`, `*.key`, `*.pfx`, `id_rsa*`, `secrets/`, `credentials*`, `*.tfstate*`, `kubeconfig`, `appsettings.*.json` with connection strings, `.npmrc`/`.pypirc` with tokens. If you need the shape of config, read `.env.example` or ask.
- Never run `env`, `printenv`, `set`, or commands that echo secrets.
- Never write real secrets, tokens, internal credentialed URLs or customer data into docs, rules or skills. Use placeholders like `<API_KEY>`.
- If you stumble on what looks like a committed secret, do not repeat it. Tell the user the file path and recommend rotation.

**Token efficiency**
- Root `CLAUDE.md` should be at most ~100 lines: what the project is, key commands, hard rules, and pointers. Use links instead of copying content in.
- Progressive disclosure: details live in `docs/`, path-scoped rules and skills, which load only when relevant.
- Each fact lives in exactly one place. Link to it; don't duplicate it.
- Skills get precise trigger descriptions and short bodies; long reference material goes in separate files the skill points to.
- During discovery, sample representative files instead of reading whole trees. Use a subagent for broad exploration and keep only its conclusions.

## Phase 0 — Intake

Ask the user (use a multiple-choice question tool if available, at most 4 questions) for anything you can't infer:
1. Stack / repo type (monorepo? languages?)
2. Issue tracker and PR platform (GitHub, Azure DevOps, GitLab, Jira…)
3. Compliance or security constraints (GDPR, customer data, regulated domain)
4. Existing standards or docs to reuse

Always get an answer to question 2. It decides which skills to create in Step C.

If the repo has no source code yet, treat it as **greenfield**. Mark planned paths "(planned)" in CLAUDE.md, and add a maintenance step to remove the marks once the files exist.

If `CLAUDE.md` or `.claude/` already exists, switch to **Audit mode** (see the end of this file).

## Phase 1 — Discovery (read-only)

Collect, briefly:
- Top-level layout (`git ls-files | head -200` or a directory listing to depth 2)
- Manifests: package.json, *.csproj/*.sln, pyproject, go.mod, pom.xml, Dockerfile, etc.
- Build/test/lint commands (from manifests, Makefile, CI files)
- CI/CD config (.github/workflows, azure-pipelines.yml, .gitlab-ci.yml)
- Existing docs (README, docs/, wiki links, CONTRIBUTING)
- Conventions visible in 3–5 representative source files per area
- `.gitignore` coverage of secret files; any existing secret scanning (gitleaks, pre-commit, GitHub secret scanning)

## Phase 2 — Plan (stop for approval)

Present:
1. **Findings**: stack, commands, areas/boundaries, gaps, risks (especially security)
2. **Proposed file tree** with each file's purpose and rough size
3. **Permission/deny rules** to add
4. **Skills to create**, with their exact `description` trigger text
5. **Open questions**
6. **Maintenance routine**: who updates what, and when

Default target structure (adapt to the repo):

```
CLAUDE.md                     # lean entry point
.claude/
  settings.json               # shared permissions (committed)
  rules/                      # path-scoped rules (frontmatter: paths)
    backend.md, frontend.md, infra.md ...
  skills/
    issue-to-plan/SKILL.md
    pr-review/SKILL.md
    adr-writer/SKILL.md       # optional
    docs-sync/SKILL.md        # optional
docs/
  architecture/overview.md    # context, components, boundaries, data flow
  adr/0000-template.md
  adr/0001-<existing-key-decision>.md
  development/standards.md    # coding standards
  development/testing.md
  development/workflow.md     # branching, commits, definition of done
```

Small repo (a single app or site, no team, fewer than ~10 decisions): one `docs/decisions.md` replaces `docs/adr/`, and `docs/architecture/` and `docs/development/` can be left out.

## Phase 3 — Build (one step at a time, review after each)

### Step A — Security guardrails (always first)

1. Create or update `.claude/settings.json` (committed, so the team shares it). Example to adapt; check current Claude Code permission syntax:

```json
{
  "permissions": {
    "deny": [
      "Read(**/.env)",
      "Read(**/.env.*)",
      "Read(**/*.pem)",
      "Read(**/*.key)",
      "Read(**/*.pfx)",
      "Read(**/secrets/**)",
      "Read(**/*.tfstate*)",
      "Read(**/credentials*)",
      "Bash(env)",
      "Bash(printenv *)",
      "Bash(cat .env*)",
      "Bash(git push --force *)",
      "Bash(git push -f *)"
    ],
    "ask": [
      "Bash(git push *)",
      "Bash(gh repo edit *)",
      "Bash(gh repo delete *)",
      "Bash(gh api *)",
      "Bash(gh release *)"
    ]
  }
}
```
   Keep the full deny list even if the repo has no secrets today. It costs nothing, and secrets show up later. Only remove an entry if you can state a reason.
   Add `ask` rules for every irreversible or outward-facing command: pushes, visibility or settings changes, deletes, releases and deploys. Adapt them to the platform CLI (gh / az / glab). After Step B, grep the docs you wrote for commands and make sure each such command is covered.
   Allow `.env.example` explicitly if the deny patterns catch it. Tell the user that Bash deny rules are a safety net, not a guarantee; for stricter control, add a PreToolUse hook or run in a sandbox.
2. Ensure `.gitignore` covers secret files and `.claude/settings.local.json`.
3. If there's no secret scanning, recommend gitleaks (pre-commit and/or CI) or the platform's built-in scanning. Propose the config; don't install tools without consent. If scanning can't be enabled yet (for example, the platform only offers it for public repos and the repo is still private), add it as a step in the repo's launch or deploy docs so the recommendation isn't lost.

### Step B — CLAUDE.md and docs

CLAUDE.md template:

```markdown
# <Project name>
<1–3 lines: what it is, who uses it>

## Commands
- Build: `<cmd>`  - Test: `<cmd>`  - Lint: `<cmd>`  - Run locally: `<cmd>`

## Hard rules
- Never read or print secret files (.env, keys, secrets/). Use .env.example.
- <3–7 project-critical rules, e.g. "All DB access goes through <layer>">
- Architecture or convention changes require updating docs/ or adding an ADR.

## Where to find more
- Architecture: docs/architecture/overview.md
- Decisions: docs/adr/
- Standards & workflow: docs/development/
- Area rules load automatically from .claude/rules/
```

Hard rules:
- Write each one as a single imperative line. Its reason and details live only in the linked ADR or decision; link to it instead of restating it.
- Make every rule checkable: give a threshold or a named standard (e.g. "WCAG 2.1 AA contrast"), not a judgement ("sufficient contrast").

ADR template (`docs/adr/0000-template.md`): Title, Status (proposed/accepted/superseded), Context, Decision, Consequences, Alternatives considered. Write 1–3 ADRs that capture key existing decisions found in discovery, and mark them "accepted (retroactive)".

Path-scoped rule example (`.claude/rules/backend.md`):

```markdown
---
paths:
  - "src/api/**"
---
# Backend rules
- <conventions specific to this area, max ~30 lines>
```

### Step C — Skills

Create these in `.claude/skills/`. Adapt the platform commands (gh / az repos / glab) to the user's tooling. Scale them to the repo; don't skip them:
- Always create a review skill that checks a diff against **this repo's hard rules** and the security checklist in pr-review step 3. For a small repo, one short skill is enough, but it must still include the security checklist.
- Create issue-to-plan only if Phase 0 confirmed an issue tracker.

**issue-to-plan/SKILL.md**

```markdown
---
name: issue-to-plan
description: Turn an issue or user story into clarified acceptance criteria and an implementation plan. Use when given an issue/US number, link or text to start work on.
---
1. Fetch the issue (e.g. `gh issue view <n>`). Read only what's needed; don't paste secrets or customer data from it.
2. Restate the goal. List acceptance criteria as testable statements. Ask about anything ambiguous before planning.
3. Find affected areas using docs/architecture and targeted search, not broad reads.
4. Produce a plan: steps, files likely touched, tests to add, risks, and whether an ADR or docs update is needed.
5. Optionally suggest a branch name following docs/development/workflow.md.
Stop and wait for approval before writing code.
```

**pr-review/SKILL.md**

```markdown
---
name: pr-review
description: Review a pull request or local diff against project standards, security and docs impact. Use when asked to review a PR, a branch or staged changes.
---
1. Get the diff only (e.g. `gh pr diff <n>` or `git diff main...HEAD`). Don't read the whole repo.
2. Check against docs/development/standards.md and relevant .claude/rules/.
3. Security checklist: secrets or credentials in the diff, authn/authz, input validation, injection, unsafe deserialization, logging of sensitive data, new dependencies (known vulnerabilities, license).
4. Tests: are new behaviors covered? Do the changed tests still assert something meaningful?
5. Docs impact: does this change architecture, a convention or an API? If so, require a docs/ADR update.
6. Output findings ranked Blocker / Should fix / Nit, each with file:line, the problem and a suggested fix. Keep it short; no praise padding.
Never post comments to the PR unless the user asks.
```

Optional: **adr-writer** (draft an ADR from a discussion or PR) and **docs-sync** (given a diff, list docs that are now stale and propose updates).

### Step D — Verify

- Run the **Quality checklist** below against what you created, and fix any failures before you finish.
- Confirm deny rules work: ask the user to try having Claude read `.env` in a new session (it should be refused).
- Dry-run the review skill on a recent PR or diff, and `issue-to-plan` (if created) on a real issue. Tighten their descriptions if they trigger wrongly.
- Summarize what was created and the maintenance routine.

## Quality checklist (Step D runs it; Audit mode reports against it)

- CLAUDE.md is at most ~100 lines. Every pointer resolves to a real file. "(planned)" marks are allowed only at the end of a greenfield setup; Audit mode reports any that are still there, and any whose path doesn't match the planned file tree.
- No fact is duplicated. Each hard rule is one checkable line that links to its reason in docs instead of restating it.
- The full baseline deny list from Step A is present. Any entry that was removed has a stated reason.
- Every irreversible or outward-facing command named in the docs is covered by an `ask` rule.
- Secret scanning is enabled, or the launch/deploy docs have a step to enable it.
- A review skill exists that enforces this repo's hard rules and includes the security checklist. Every skill has a precise trigger description and a short body.
- No file contains secrets, credentialed hostnames or customer data.
- Docs match the code.

## Audit mode (existing setup)

Report against the Quality checklist without editing anything. Propose concrete trims and fixes, then apply them step by step after approval.

## Maintenance routine (include in the final summary)

- Every PR: the review skill checks hard rules and docs impact.
- Greenfield: once planned files exist, remove the "(planned)" marks.
- New significant decision: add an ADR.
- Quarterly: run this skill in Audit mode to trim bloat and fix drift.
