# Agent team

The Mona's Project Pulse dashboard will be built by a coordinated team of four custom agents. GitHub Copilot CLI in a Codespace orchestrates the work, assigning planning, implementation, and design tasks to the appropriate specialist.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, breaks requests into phases, delegates scoped work, manages dependencies, and verifies that the integrated result works together. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, risks, dependencies, edge cases, and validation needs, then produces an actionable implementation plan without writing code. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements dashboard logic and assigned support configuration, follows repository patterns, keeps behavior explicit and testable, and validates completed code. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the dashboard's UI/UX, accessibility, information hierarchy, interaction flow, responsive behavior, visual clarity, and polished Project Pulse styling. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |

The Orchestrator starts with the Planner's research, then assigns non-overlapping implementation and design work to the Coder and Designer before integrating and checking the result. All agents leave Git operations to the learner.
