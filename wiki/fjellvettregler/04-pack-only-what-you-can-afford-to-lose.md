# 4. Pack only what you can afford to lose

How to keep sensitive data out of prompts and agent context.

Rule: [AI-Fjellvettregler, rule 4](../../guidelines/fjellvettregler.md#4-pack-only-what-you-can-afford-to-lose)

## How to follow it

- Never paste personal data, credentials, competitively sensitive information or data that must not be used for training or made public.
- Remember that agents read files on their own. Keep secrets out of the repo, and block agents from reading files like `.env`.
- Use synthetic or anonymized test data instead of production data.
- Check logs, stack traces and screenshots for personal data before you share them.

## Recommended skills and tools

- [`dev-foundation-setup`](../../skills/dev-foundation-setup/SKILL.md) sets up deny rules so agents can't read secret files. See [Project setup](../project-setup.md).

## Examples and lessons learned

_None yet. Add short, anonymized examples._

## Related pages

- [1. Know the rules before you head out](01-know-the-rules.md)
- [5. Give agents their own rope](05-give-agents-their-own-rope.md)

## Sources

- [AI-Fjellvettregler](../../guidelines/fjellvettregler.md)
