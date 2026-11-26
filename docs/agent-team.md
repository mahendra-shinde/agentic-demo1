# Mona's Project Pulse agent team

The custom agents in `.github/agents/` divide planning, implementation, and design responsibilities for Mona's Project Pulse dashboard.

| Agent | Assigned model | Definition | Responsibility |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | `.github/agents/orchestrator.agent.md` | Coordinates specialists without implementing code itself. Turns the plan into phases, assigns explicit file scopes, manages dependencies, checks the integrated result, and reports outcomes and blockers. |
| Planner | Claude Opus 5.5 (copilot) | `.github/agents/planner.agent.md` | Researches the repository and relevant documentation without writing code. Produces ordered implementation steps, file assignments, dependencies, edge cases, risks, and validation expectations. |
| Coder | GPT-5.5 (copilot) | `.github/agents/coder.agent.md` | Implements dashboard logic and assigned static app files with explicit errors and deterministic, testable behavior. Creates assigned runnable-app support and validates changes before reporting completion. |
| Designer | Gemini 3.7 Flash (copilot) | `.github/agents/designer.agent.md` | Shapes and implements assigned UI/UX and styling: accessible information hierarchy, interaction flow, responsive layout, readable spacing, project cards, status badges, and clear priority treatment. |

## How the team will build Project Pulse

1. The Orchestrator asks the Planner to translate the dashboard brief into phases with clear file ownership, dependencies, and validation expectations.
2. The Orchestrator assigns the Coder implementation work for `app/index.html` and `app/project-data.json`, and the Designer dashboard styling in `app/styles.css`. The dashboard will show active projects, owners, statuses, recent activity, priorities or risks, and contributor-friendly summaries. The data file will use a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority` for each project.
3. The Designer delivers a polished, accessible, responsive dashboard with visible cards, badges, and deterministic CSS hooks such as `.dashboard` and `.project-card`. The Coder creates the assigned `.vscode/launch.json` using strict JSON, with the **Run Project Pulse Dashboard** configuration serving from `${workspaceFolder}/app` and opening `index.html`, not a directory listing.
4. The Orchestrator runs work in parallel only when file scopes do not overlap and there are no data dependencies. Shared-file changes or work that depends on earlier output run sequentially, including coordination of HTML structure and CSS hooks.
5. The Coder validates implementation changes; the Designer reports design decisions and validation recommendations. The Orchestrator verifies that the integrated dashboard hangs together and reports the final outcome and any blockers.

All agents stay within their assigned scopes and do not stage, commit, or push changes. The learner controls Git operations through Copilot CLI prompts.
