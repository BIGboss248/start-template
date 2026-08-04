---
description: Steps and rules to follow when creating utility functions including API and server functions
---

# Workflow: Create Utility

## Phase 1: Pre-Flight Planning & Source Audit

Before writing any code:

1. **JSON Pre-Flight Plan:** Output the JSON plan schema as required by `rules/workflow.md`. Audit relevant sources (`AGENTS.md`, `rules/core-stack.md`, `rules/documentation.md`).
2. **Avoid Duplication:** Scan existing utilities under [Utility dir](@/AGENTS.md#project-context--metadata) to see if a similar function exists.
3. **Purity Design:** Design the utility to be a pure function (deterministic, free of side-effects) whenever possible, facilitating testing and cacheability.

---

## Phase 2: Architectural Placement & Types

1. **Directory Placement:** Store global utilities under [Utility dir](@/AGENTS.md#project-context--metadata). Store module-specific utilities within the immediate folder.
2. **Type Safety:** Write fully typed TypeScript functions avoiding `any` or `@ts-ignore` as dictated by `rules/core-stack.md`. Ensure E2E type safety with generated backend/CMS interfaces if applicable.
3. **Imports Standard:** Always import packages and files relative to the project root using `@/` path aliases.

---

## Phase 3: Documentation (MANDATORY)

1. **TSDoc Standard (MANDATORY):** Document the utility using strict English TSDoc syntax (`rules/documentation.md`). Focus on the "Why", generics (`@typeParam`), inputs (`@param`), default values (`@defaultValue`), return values (`@returns`), and exceptions (`@throws`).
2. **Algorithmic Documentation:** Update `README.modules.md` and `ARCHITECTURE.md` as necessary (`rules/documentation.md`).

---

## Phase 4: Unit Testing (MANDATORY)

1. **Test Generation:** Every utility must have a corresponding unit test file (e.g., `[utility-name].test.ts`) generated according to `rules/documentation.md`.

---

## Phase 5: Self-Reflection & Quality Audit (MANDATORY)

Execute an explicit Self-Reflection Pass and output the `### Self-Reflection & Quality Audit` block (`rules/workflow.md`) evaluating:

1. **Adversarial Checks:** Test boundary values (0, negative numbers, empty strings/arrays, `null`, `undefined`), unexpected API rejection formats, and floating-point precision issues.
2. **Utility Rule Checklist:**
   - [ ] Type Safety: Are there any `any` casts or unhandled implicit types?
   - [ ] Pure Function: Is the utility deterministic and side-effect free?
   - [ ] TSDoc Completeness: Are `@param`, `@returns`, and `@throws` fully annotated in English?
3. **Identified Bugs & Fixes:** Document any unhandled exceptions or edge cases found and corrected.

---

## Phase 6: Verification & Cleanup

1. **Verification Gate:** Lint the code and run tests.
2. **Code Cleanup:** Mark temporary structures with `// TODO: REMOVE BEFORE PRODUCTION`.

---

## Phase 7: Handoff & User Commits

1. **Handoff:** Present completed work to the user. Do NOT run git commit commands.

In the output provide these details in a list

1. Name of the function
2. where the file is saved
3. Why this function was created
4. inputs
5. Outputs
6. dependencies
