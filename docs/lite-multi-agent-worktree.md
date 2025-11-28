# Multi-Agent Worktree Agile Framework (Lite)

This document captures the design for a streamlined BMAD fork that uses git worktrees and specialized agents to enable parallel agile execution. The focus is on greenfield projects with predictable defaults (pnpm, React/Node) and multi-window chat control.

## Objectives

- Keep BMAD installation/build foundations while replacing complex enterprise workflows with a lean, multi-agent flow.
- Deliver a one-command bootstrap (`lite-init-project`) that generates docs, worktree scripts, and pnpm-ready frontend/backend scaffolds.
- Support multiple chat windows (PM, frontend, backend, QA) mapped to isolated worktrees and feature branches.
- Preserve BMAD agent packaging so the new module can be bundled as `team-lite` without touching legacy teams.

## Module layout

- **src/modules/lite/agents/** – PM, frontend-dev, backend-dev, and QA agent definitions tailored for the worktree model.
- **src/modules/lite/workflows/** – `lite-init-project`, `lite-feature-dev`, and `lite-qa-regression` workflows that cover initialization, implementation, and regression.
- **src/modules/lite/docs/** – Templates for PRD, Tech Design, UI Design, API contracts, Release Notes, and `AGENT.md` guidance.

## Standard project structure

```
project-root/
  AGENT.md
  docs/
    PRD.md
    TechDesignDoc.md
    API_Contracts.json
    UI_Design.md
    ReleaseNotes.md
  design/
    figma-links.md
    components-guideline.md
  tasks/
    frontend/
    backend/
  frontend/
  backend/
  tests/
    integration/api/generated/
    integration/api/manual/
    ui/puppeteer/
    e2e/scenarios/
  scripts/
    init-worktrees.sh
    generate-api-tests.ts
    puppeteer-runner.ts
  pnpm-workspace.yaml
```

## Worktree-driven agent model

- **PM Agent (main worktree)**: Maintains `docs/PRD.md`, breaks work into tasks, and updates `docs/ReleaseNotes.md`.
- **Frontend Dev Agent (frontend worktree)**: Implements UI/logic with pnpm, writes unit tests, and commits to `feature/front-*` branches.
- **Backend Dev Agent (backend worktree)**: Delivers APIs, migrations, and tests from `feature/back-*` branches.
- **QA Agent (qa worktree)**: Owns `tests/**`, executes regression (Puppeteer/Playwright friendly), and reports results without changing product code.

## Key workflows

- **lite-init-project**
  - Collect project name/description from the user.
  - Generate docs (`PRD.md`, `TechDesignDoc.md`, `UI_Design.md`, `API_Contracts.json`, `ReleaseNotes.md`, `AGENT.md`).
  - Create `scripts/init-worktrees.sh` to add frontend, backend, and QA worktrees on top of `main`.
  - Initialize pnpm-based frontend/backend templates.

- **lite-feature-dev**
  - PM updates `docs/PRD.md` and drops task files into `tasks/frontend/F-xxx.md` and `tasks/backend/B-xxx.md`.
  - Frontend/backend agents implement from their worktrees, run pnpm tests, and push `feature/front-*` or `feature/back-*` branches.

- **lite-qa-regression**
  - QA agent aligns tests with `docs/PRD.md`, `docs/API_Contracts.json`, and `docs/UI_Design.md`.
  - Maintains generated API tests under `tests/integration/api/generated/` and manual/complex cases under `tests/integration/api/manual/`.
  - Executes integration/E2E suites from the QA worktree and reports outcomes.

## Integration with multi-window chat

- Map each agent to a dedicated chat window (PM, frontend-dev, backend-dev, QA).
- Provide prompts so agents can "pull tasks and run routine" from their worktrees with minimal user input.
- Keep merging/release decisions on the main worktree, with PM/integrator reviewing feature branches before merge.

## Pruning legacy workflows

- Keep legacy BMAD modules installed for compatibility but **do not reference** their workflows in lite bundles.
- Comment out or remove BMM/BMGD/CIS workflow triggers in default teams so only `lite-init-project`, `lite-feature-dev`, and `lite-qa-regression` remain active.
- Treat additional workflows as opt-in and document any explicit additions in the team file to avoid surprise prompts.

## Naming and branding alignment

- Use the VibeCode-branded "Lite Worktree Agile" naming in prompts, docs, and team files instead of generic BMAD labels.
- Prefer agent names that match the worktree roles (PM, frontend-dev, backend-dev, QA) to reduce ambiguity during multi-window conversations.
- When publishing bundles, ensure the bundle name/description clearly communicates the worktree-first workflow and omits enterprise/creative language.

## Migration stance

This framework intentionally targets new projects. Brownfield migrations are out of scope for the initial release.
