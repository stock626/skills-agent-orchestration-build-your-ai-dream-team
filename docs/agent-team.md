Agent team for Mona's Project Pulse

Overview

Four custom specialist agents coordinate to build Project Pulse:

- Orchestrator
  - Role: Coordinate Planner, Coder, Designer; break work into phases and file-scoped tasks.
  - Notes: Ensures non-overlapping parallel work, verifies integration, surfaces blockers.
  - Model/tools: Claude Opus 4.7; tools: read, agent, memory.

- Planner
  - Role: Research repo, produce ordered implementation steps, file assignments, dependencies, edge cases, and validation criteria.
  - Model/tools: Claude Opus 4.7; tools: read, search, web, memory, todo.

- Coder
  - Role: Implement code and runnable app support within Orchestrator-assigned files. Validate changes.
  - Project Pulse specifics: create .vscode/launch.json when assigned; set cwd to ${workspaceFolder}/app; open index.html; follow repository patterns.
  - Model/tools: GPT-5.5; tools: read, edit, search, execute, web, memory, todo.

- Designer
  - Role: UI/UX, accessibility, responsive dashboard styling for Project Pulse.
  - Project Pulse specifics: produce polished dashboard UI with visible project cards, status badges, responsive layout, and deterministic CSS hooks (.dashboard, .project-card).
  - Model/tools: Gemini 3.1 Pro; tools: read, edit, search, web, memory, todo.

Usage guidance

- Orchestrator delegates clear file scopes and phases.
- Planner delivers a phase-by-phase plan with explicit file ownership.
- Coder and Designer only modify files assigned to them; report validation steps and remaining risks.

(Replace text updated with this summary.)
