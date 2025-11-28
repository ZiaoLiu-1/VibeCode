# Lite Module: Multi-Agent Worktree Agile Framework

The **lite** module is a streamlined fork of the BMAD Method focused on a pragmatic, multi-agent agile workflow. It keeps the BMAD installation/build foundations while replacing heavyweight enterprise workflows with a standardized worktree-first process.

## Goals

- Provide a one-command project bootstrap (`lite-init-project`) that lays down docs, worktree scripts, and pnpm-based frontend/backend scaffolds.
- Enable parallel development across PM, frontend, backend, and QA agents using git worktrees and feature branches.
- Ship ready-to-run workflow definitions and templates that can plug into Codex/ChatGPT multi-window sessions.
- Keep greenfield projects opinionated (pnpm, React/Node-first) without forcing brownfield migrations.

## Scope and directory layout

- `agents/`: Definitions for the PM, frontend-dev, backend-dev, and QA agents.
- `workflows/`: Workflows such as `lite-init-project`, `lite-feature-dev`, and `lite-qa-regression` that orchestrate initialization, implementation, and regression.
- `docs/`: Templates for PRD, Tech Design, UI Design, API contracts, Release Notes, and `AGENT.md` guidance.
- `teams/`: VibeCode-branded bundles (see `team-lite.yaml`) that isolate lite agents/workflows from legacy BMAD teams.

Each file follows BMAD's module packaging rules so it can be bundled into `dist/teams` outputs alongside the existing modules.

## Workflow highlights

- **lite-init-project**: Initializes a new repository with standard docs, pnpm workspaces, and a `scripts/init-worktrees.sh` helper to add frontend, backend, and QA worktrees on top of `main`.
- **lite-feature-dev**: Reads PM-authored tasks (e.g., `tasks/frontend/F-xxx.md`, `tasks/backend/B-xxx.md`), lets frontend/backend agents implement in their isolated worktrees, and pushes `feature/front-*` or `feature/back-*` branches.
- **lite-qa-regression**: Keeps QA-owned integration/E2E suites aligned with `docs/PRD.md`, `docs/API_Contracts.json`, and `docs/UI_Design.md`, running regressions from the QA worktree.

## Multi-window agent model

- **PM Agent**: Curates `docs/PRD.md`, splits work into tasks, and updates `docs/ReleaseNotes.md`.
- **Frontend Dev Agent**: Works in the frontend worktree, implements UI/logic with pnpm, and commits to `feature/front-*` branches.
- **Backend Dev Agent**: Works in the backend worktree, delivers APIs/migrations/tests, and commits to `feature/back-*` branches.
- **QA Agent**: Owns `tests/**`, executes regressions (Puppeteer/Playwright friendly), and reports results without touching product code.

## Pruning legacy workflows

- Lite intentionally **excludes** enterprise, creative, and game workflows from the default bundle; keep those modules installed but not referenced in lite teams.
- Default team definitions should comment out or remove BMM/BMGD/CIS workflow references unless explicitly needed for brownfield/enterprise use.
- Worktree routines should only surface `lite-init-project`, `lite-feature-dev`, and `lite-qa-regression` to avoid cross-talk with older flows.

## Integration notes

- Team bundles should reference a dedicated `team-lite` definition that only pulls in lite agents/workflows.
- Keep legacy BMAD modules intact but disabled in team configs so the lite pipeline remains focused and predictable.
- Workflows should surface clear prompts so each agent can be driven from separate chat windows (PM, frontend, backend, QA) while sharing the same git repo via worktrees.
