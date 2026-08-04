---
trigger: always_on
description: Rules for agent task planning (JSON Pre-Flight Plan & Source Audit), git branch management, commit naming conventions, pre-commit verification checks, and privacy consent. MANDATORY for all tasks.
---

# Workflow & Task Management (MANDATORY)

## Pre-Flight Planning (Structured JSON Plan & Source Audit)

Before writing any code or executing terminal commands for a non-trivial task or feature implementation, you MUST perform a source audit and output a structured JSON plan block for user review and approval.

### 1. Mandatory Source Audit
Prior to generating the JSON plan, you MUST inspect relevant workspace directives:
* Read `.agents/rules/` files applicable to the request domain.
* Read any matching `.agents/workflows/*.md` file for step-by-step phase checklists.
* Read `AGENTS.md` and user-specified documentation.

### 2. JSON Plan Schema Specification
Output the plan inside a fenced `json` code block formatted strictly to the following structure:

```json
{
  "goal": "Clear summary of the feature or task objective",
  "sources_audited": [
    {
      "path": ".agents/rules/workflow.md",
      "scope_summary": "Extracted planning format, self-reflection rules, and commit guidelines"
    },
    {
      "path": ".agents/workflows/create-component.md",
      "scope_summary": "Extracted component placement, skeleton, TSDoc, and reflection phases"
    }
  ],
  "subtasks": [
    {
      "id": 1,
      "description": "Atomic description of subtask 1",
      "workflow_phase_ref": "Phase 1: Pre-Flight & Planning",
      "tool_intents": ["view_file", "write_to_file"],
      "verification_criteria": "Clear testable criterion to mark task complete",
      "status": "pending"
    },
    {
      "id": 2,
      "description": "Perform Self-Reflection & Quality Audit",
      "workflow_phase_ref": "Phase 4: Self-Reflection & Quality Audit",
      "tool_intents": [],
      "verification_criteria": "Output mandatory Self-Reflection block with 3 adversarial edge-case checks",
      "status": "pending"
    }
  ],
  "eval_metrics": [
    "Objective criteria defining successful overall task completion"
  ],
  "risk_factors": [
    "Potential edge cases, breaking changes, or backup strategies"
  ]
}
```

### 3. Execution & Progress Policy
* **Approval Gate:** Wait for the user to approve the JSON plan before proceeding to Step 1.
* **Sequential Execution:** Execute subtasks one by one. Update the user on progress after completing each step.
* **Dynamic Re-planning:** If runtime findings or unexpected errors require modifying the plan, re-emit the updated JSON plan block with modified `subtasks` or `status` values for user review.

## Self-Reflection & Quality Audit (MANDATORY)

Before completing any non-trivial code implementation or presenting final work to the user, you MUST execute a dedicated **Self-Reflection Pass**.

### Anti-Bypass Directive
> [!CAUTION]
> You are strictly forbidden from skipping Self-Reflection or stating "No issues found" without explicitly outputting the required 3-part markdown critique block. Confirmation bias is forbidden; approach your own output as an aggressive third-party code reviewer.

### Mandatory Self-Critique Block Format
You MUST output a fenced Markdown section titled `### Self-Reflection & Quality Audit` containing:

1. **Adversarial Edge-Case Checks:** Test your implementation against at least 3 failure scenarios (e.g., null/undefined state, async race conditions, slow network streaming, empty arrays).
2. **Domain Rule Compliance Audit:** Explicitly verify compliance against loaded `.agents/rules/` (e.g., TSDoc `@param`/`@returns`/`@throws` completeness, Tailwind logical properties `ms-`/`pe-`, semantic theme tokens, RSC boundaries).
3. **Identified Bugs & Corrections:** Explicitly document any bugs, missing requirements, or edge-case oversights identified during self-critique and detail how they were fixed.

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

## Cleanup

* **TODO Flags:** Any placeholder data, mock API responses, or debug `console.log`s left in the codebase must be explicitly marked with a `// TODO: REMOVE BEFORE PRODUCTION` comment.

## Privacy Policy & Terms of Service

* Any change to the codebase involving telemetry, cookies, or user data collection requires an update to the Privacy Policy and Terms of Service.
* Implement a non-intrusive consent banner for users to accept these terms if a new data-collection feature is added.