# Agent team

The custom agent team for Mona's Project Pulse dashboard is:

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| **Orchestrator** | Claude Opus 5.5 (copilot) | Coordinates the team, turns the plan into scoped phases, delegates work to specialists, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 5.5 (copilot) | Researches the repository and relevant documentation, then produces an implementation plan with file assignments, dependencies, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.7 Flash (copilot) | Shapes the dashboard's user experience, visual hierarchy, accessibility, responsive behavior, and Project Pulse styling. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements application logic and assigned runnable-app support, following repository patterns and validating changes. | `.github/agents/coder.agent.md` |

I am using GitHub Copilot CLI in a Codespace to orchestrate this work.
