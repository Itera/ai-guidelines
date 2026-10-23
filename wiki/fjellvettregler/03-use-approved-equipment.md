# 3. Use approved equipment

How to check that AI tools, models, skills and packages are safe to use.

Rule: [AI-Fjellvettregler, rule 3](../../guidelines/fjellvettregler.md#3-use-approved-equipment)

## How to follow it

- Use only AI tools approved for Itera or for the customer's PCs.
- Before you use a model, check who owns it, where it is hosted and whether your data may be used for training.
- Vet third-party skills, agents, MCP servers and packages before you install them:
  - Read the code or `SKILL.md` yourself. Skills are instructions the agent will follow.
  - Check who publishes it and whether it is maintained.
  - Note what access it asks for (shell, network, file system, credentials).
  - Prefer pinned versions over "latest".

## Recommended skills and tools

- [`fjellvettregler-check`](../../skills/fjellvettregler-check/SKILL.md) lists MCP servers, plugins and third-party skills, and helps you vet them.

## Examples and lessons learned

_None yet. Add short, anonymized examples._

## Related pages

- [2. Tell someone where you're going](02-tell-someone-where-youre-going.md) – agreeing on tools with the customer.
- [5. Give agents their own rope](05-give-agents-their-own-rope.md) – limiting what installed tools can do.

## Sources

- [AI-Fjellvettregler](../../guidelines/fjellvettregler.md)
