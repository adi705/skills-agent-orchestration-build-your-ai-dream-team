# Agent team

For Mona's Project Pulse dashboard, I will use a four-agent custom team defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 5.5 (copilot). Responsible for researching the codebase and docs, identifying edge cases, and creating the implementation plan with file ownership, dependencies, validation steps, and open questions. Definition: `.github/agents/planner.agent.md`
- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsible for breaking the work into phases, delegating tasks to specialist agents, sequencing parallel vs. dependent work, and verifying that the final result integrates cleanly. Definition: `.github/agents/orchestrator.agent.md`
- Designer — Model: Gemini 3.7 Flash (copilot). Responsible for UI/UX, accessibility, information hierarchy, interaction flow, and the visual design of the dashboard experience. Definition: `.github/agents/designer.agent.md`
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing the code changes in the assigned file scope, handling app support such as launch configuration, and validating behavior before reporting completion. Definition: `.github/agents/coder.agent.md`

This team is intentionally specialized: the Planner researches and plans, the Orchestrator coordinates, the Designer shapes the product experience, and the Coder turns the approved plan into working implementation. The work is managed through GitHub Copilot CLI in a Codespace, with the orchestrator coordinating the specialist agents across the repository.
