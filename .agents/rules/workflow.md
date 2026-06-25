---
trigger: always
description: Rules for agent task planning (Chain of Thought), git branch management, commit naming conventions, pre-commit verification checks, semantic versioning, and privacy consent. MANDATORY for all tasks.
---

# Workflow & Task Management (MANDATORY)

## Pre-Flight Planning (The "Chain of Thought" Rule)

* Before writing any code or executing terminal commands for a new feature, you MUST output a numbered list detailing the exact steps you plan to take.
* Wait for the user to approve the plan before proceeding to Step 1.
* Execute ONE logical step at a time to maintain context.

## Task Segmentation

* Break down every feature implementation into smaller, atomic tasks. Execute them one by one.
* Following Git operations (staging, committing, and pushing) are left entirely to the user. Do not execute Git commit or stage commands yourself, nor should you block progress waiting for commits between tasks. Simply present the completed work and let the user handle commits.

## Git Branching & Commits

On tasks:

1. git branch accordingly:
   * `feat/short-description` (for new features)
   * `fix/short-description` (for bug fixes)
   * `refactor/short-description` (for structural changes)
2. Implement the feature code, but **do not stage or commit**.
3. Leave all git staging and committing entirely to the user. You may suggest appropriate conventional commit messages for their reference at the end of a task (e.g., `git commit -m "feat: add localization to header navigation"`), but do not execute them, ask to run them, or wait/block progress for them.

## Verification Gate

* Do NOT run verification commands (e.g. `bun run build`, `bun run lint`, or testing suites) during standard tasks/workflows (creating pages, utilities, components, etc.) because these checks are handled automatically by `pre-commit` hooks.
* If manual verification, project-wide testing, or build-fixing is explicitly requested by the user, follow the `/verify-build` workflow.

## Versioning & Cleanup

* On every confirmed change or completed feature, modify the version number in `package.json` accordingly (e.g., utilizing [package manager versioning](../../AGENTS.md#project-context--metadata) where appropriate) and let the user include this modification in their commit.
* **TODO Flags:** Any placeholder data, mock API responses, or debug `console.log`s left in the codebase must be explicitly marked with a `// TODO: REMOVE BEFORE PRODUCTION` comment.

## Privacy Policy & Terms of Service

* Any change to the codebase involving telemetry, cookies, or user data collection requires an update to the Privacy Policy and Terms of Service.
* Implement a non-intrusive consent banner for users to accept these terms if a new data-collection feature is added.
