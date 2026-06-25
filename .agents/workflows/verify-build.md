---
description: Steps and rules to follow when running project verification (build, lint, test) and fixing errors
---

# Workflow: Verify & Build

This workflow defines the process to run full project verification (linting, testing, and production builds) and autonomously troubleshoot and fix any errors that arise. Use this workflow when the user requests verification, if a pre-commit check fails, or before handoff when verifying project health.

---

## Phase 1: Run Verification Checks

Execute the verification steps in order.

### 1. Lint Verification
- Run the linter:
  ```bash
  bun run lint
  ```
- If the lint step fails, note the errors.

### 2. Integration Tests Verification
- Run the integration tests:
  ```bash
  bun run test:int
  ```
- If tests fail, note the failing assertions and logs.

### 3. Production Build Verification
- Run the production build to compile and catch type-checking/bundling errors:
  ```bash
  bun run build
  ```
- If the build fails, analyze the output to locate TypeScript, React compiler, or Next.js layout compilation issues.

---

## Phase 2: Autonomously Resolve Errors

If any of the verification steps fail, you MUST systematically resolve the errors:

### 1. Fix Lint Errors
- For auto-fixable ESLint errors, run:
  ```bash
  bun run lint:fix
  ```
- For remaining manual lint errors, open the offending files, identify the violation of style/formatting, and modify the code to comply.

### 2. Fix TypeScript & Build Errors
- Identify the exact files, line numbers, and error messages from the TypeScript or Next.js build output.
- Check imports, types, type definitions, or missing properties.
- Edit the source files to resolve the types or compile-time issues.
- Do NOT bypass TypeScript check issues with `any` unless absolutely necessary and documented.

### 3. Fix Broken Tests
- Locate the failing test cases.
- Determine if the failure is due to:
  - Code changes breaking expected behavior (bug). Fix the implementation.
  - Outdated assertions or modified selectors in the UI component. Update the test file to match the new markup/props structure.
- Re-run the tests:
  ```bash
  bun run test:int
  ```

---

## Phase 3: Final Re-Verification

- Once edits are complete, re-run the verification commands:
  ```bash
  bun run lint
  bun run test:int
  bun run build
  ```
- Repeat Phase 2 and 3 until all three commands complete successfully without errors.
- Document any fixes applied.
