# Resources

Curated external resources on AI-assisted development: research, frameworks, Norwegian regulation and places to stay up to date.

Prices, product features and regulations change fast. Links were checked as of 2026-10.

## Practices and maturity

- **AI Use Policy at Itera** – the internal policy. Use it together with this repo; see rule 1 in the [AI-Fjellvettregler](../guidelines/fjellvettregler.md).
- **[DORA: State of AI-assisted Software Development (2025)](https://cloud.google.com/resources/content/2025-dora-ai-assisted-software-development-report)** – research on how AI affects productivity, code quality, delivery flow and stability.
- **[DORA AI Capabilities Model (2025)](https://cloud.google.com/resources/content/2025-dora-ai-capabilities-model-report)** – seven organizational and technical capabilities that amplify the effect of AI, such as a clear AI stance, healthy data, AI-accessible internal data, working in small batches and quality internal platforms.
- **[DevClarity AI SDLC Maturity Model](https://www.devclarity.ai/resources/ai-sdlc-maturity-model)** – a matrix for assessing how mature an organization's AI use is across the development lifecycle.

## Project context and agents

- **[Karpathy's "LLM Wiki"](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)** (April 2026) – a "second brain" pattern: an agent keeps meeting notes, designs, user stories and other project knowledge as a linked markdown wiki that both people and AI can use as context. This repo follows the same pattern.
- **[bradygaster/squad](https://github.com/bradygaster/squad)** – orchestrates specialized development agents. In a customer project it was combined with a project wiki to give the agents better context.
- **[AGENTS.md](https://agents.md/)** – a project file holding architecture, rules and context, so you don't have to explain the same things again. That also saves tokens. See [Project setup](project-setup.md) for how to keep it lean.

## Privacy, regulation and Norwegian context

- **[KI-Norge](https://ki.norge.no/)** – entry point for information about AI use in Norway.
- **[Datatilsynet: DPIA guidance](https://www.datatilsynet.no/rettigheter-og-plikter/virksomhetenes-plikter/vurdering-av-personvernkonsekvenser/)** (Norwegian) – relevant when an AI solution processes personal data or may carry a high privacy risk. See also rule 4 in the [AI-Fjellvettregler](../guidelines/fjellvettregler.md).
- **[Nkom: Who does the AI Act apply to?](https://nkom.no/ki/hvem-gjelder-ki-loven)** (Norwegian) – introduction to the EU AI Act and which actors it covers.

## Staying up to date

- **[The Batch](https://www.deeplearning.ai/the-batch)** – weekly newsletter from DeepLearning.AI.
- **[Simon Willison's weblog](https://simonwillison.net/)** – ongoing technical posts and experiments with language models and AI tools.
- **[Ethan Mollick – One Useful Thing](https://www.oneusefulthing.org/)** – writing on how AI changes work and organizations.
- **[Practical AI](https://practicalai.show/)** – podcast on applying AI in practice.

## Costs and benefits

- **[DORA: ROI of AI-assisted Software Development](https://cloud.google.com/resources/content/dora-roi-of-ai-assisted-software-development)** – a framework and example model for weighing licenses, API and token costs, training, infrastructure and verification work against the expected gains. Plug in your own organization's numbers; the report's examples are not a benchmark.
- Lessons from projects:
  - Tight token budgets can hold back experimentation. Plan for a learning period (see rule 6 in the [AI-Fjellvettregler](../guidelines/fjellvettregler.md)).
  - Sharing context across teams, such as wikis, AGENTS.md files and skills, keeps the same analysis from being done several times.
- This repo has no up-to-date comparison of token prices yet.

## Sources

- Curated list shared by trifri, 2026-10-21, compiled from internal project material.
