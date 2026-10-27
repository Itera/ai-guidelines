# AGENTS.md

This repo is Itera's shared knowledge base for AI-assisted work. People and agents read it to find guidance; people add to it through an agent that follows this file. You (the agent) maintain the structure: you file new knowledge in the right place, keep pages linked and keep `index.md` current.

**This repo is public.** Never write customer names, project code names, internal URLs, personal data, credentials, prices from customer contracts or code from customer repos. Generalize instead ("a public-sector customer", "a .NET monorepo"). If a contribution can't be made safe, say so and stop.

## Layout

- `guidelines/` – normative rules owned by people (e.g. the AI-Fjellvettregler). Do **not** edit unless the user explicitly asks to change a guideline. Never move content from `wiki/` into here on your own.
- `wiki/` – everything else: practices, tool notes, lessons learned, explanations. Descriptive, not policy. Flat until it gets crowded; propose subfolders before creating them.
- `skills/` – reusable agent skills, one folder per skill with a `SKILL.md`. When a guideline changes, update the skills that check it (e.g. `skills/fjellvettregler-check`) in the same PR.
- `index.md` – the catalog. One line per page. Read it first.

## Finding information

1. Read `index.md`, then open only the pages that look relevant.
2. Answer with links to the pages you used. If guidelines and wiki pages disagree, the guideline wins; point out the conflict.
3. If the answer isn't in the repo, say so before using general knowledge.

## Adding knowledge

When the user wants to add something (an experience, a tip, a tool review, a link):

1. Read `index.md` and search `wiki/` for related pages. **Extend an existing page rather than creating a new one** whenever the topic overlaps.
2. Strip anything confidential (see above). Show the user what you removed.
3. Write or update the page:
   - Title as `# Heading`, then a one-line summary.
   - Short sections, concrete advice, date-sensitive claims marked with the month (e.g. "as of 2026-10").
   - Link related pages with relative links. Link to `guidelines/` where a rule applies.
   - If new information contradicts a page, don't silently overwrite. Update it and keep the old view under `## Open questions` with both sides.
   - End with `## Sources` (links, or "Experience shared by <GitHub user>, <date>").
4. Update `index.md`: add or adjust the line for every page you created or changed significantly.
5. Open a PR (or ask the user to). One topic per PR. Summarize which pages changed and why.

## Writing style

- English, plain and short. Lists over long prose.
- File names: lowercase-kebab-case, e.g. `wiki/agent-permissions.md`.
- No duplicated facts: each fact lives on one page; others link to it.

## Maintenance

When asked to "lint" or "health-check" the repo, report (don't fix silently): broken links, pages missing from `index.md`, near-duplicate pages, contradictions, skills whose rule list no longer matches `guidelines/`, and claims older than ~6 months about tools, prices or models.
