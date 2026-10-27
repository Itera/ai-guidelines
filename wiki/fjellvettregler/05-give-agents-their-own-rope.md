# 5. Give agents their own rope

How to give agents their own, limited access and enforce hard limits instead of relying on prompt instructions.

Rule: [AI-Fjellvettregler, rule 5](../../guidelines/fjellvettregler.md#5-give-agents-their-own-rope)

## How to follow it

- **Own accounts:** give agents their own service accounts, tokens or API keys with only the access the task needs. Don't let them run as you.
- **Least privilege:** prefer read-only access, short-lived tokens and narrow scopes.
- **Hard limits:** use permission rules, sandboxes, containers and approval steps. A line in `CLAUDE.md` saying "don't do X" is not a guardrail.
- **Auto mode:** be strictest when the agent runs without approval for each step.
- **Test the limits:** try to make the agent do something it shouldn't, and check that it is blocked.

## Recommended skills and tools

- [`dev-foundation-setup`](../../skills/dev-foundation-setup/SKILL.md) sets up permission deny rules and suggests hooks or sandboxes for sensitive repos. See [Project setup](../project-setup.md).
- [`fjellvettregler-developer-check`](../../skills/fjellvettregler-developer-check/SKILL.md) checks permission rules, auto mode and whether agents run with your own credentials.

## Examples and lessons learned

_None yet. Add short, anonymized examples._

## Related pages

- [3. Use approved equipment](03-use-approved-equipment.md)
- [4. Pack only what you can afford to lose](04-pack-only-what-you-can-afford-to-lose.md)

## Sources

- [AI-Fjellvettregler](../../guidelines/fjellvettregler.md)
