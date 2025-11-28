# Lite Workflows

The lite module ships only the workflows required for the multi-agent worktree agile model and intentionally omits legacy BMAD flows.

## Included workflows

- **lite-init-project**: Bootstraps a greenfield repo with docs, pnpm scaffolds, and worktrees for PM/frontend/backend/QA.
- **lite-feature-dev**: Picks up PM-authored tasks and executes feature delivery in isolated worktrees with feature branches.
- **lite-qa-regression**: Keeps QA-owned tests current with PRD/API/UI inputs and runs regression from the QA worktree.

## Explicitly out of scope

- Enterprise, game, or creative workflows from the BMAD distribution.
- Multi-phase planning/party-mode helpers that are not required for the worktree-first routine.

The lite team bundle should reference **only** these workflows to avoid loading unrelated behaviors.
