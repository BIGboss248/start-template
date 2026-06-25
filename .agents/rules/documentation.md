---
trigger: model_decision
description: Rules for inline TSDoc annotations (MANDATORY for all code changes), unit test generation, Mermaid formatting, and documentation.
---

# Code Documentation & Testing

## Inline Documentation & TSDoc (MANDATORY)

> [!IMPORTANT]
> This section is **MANDATORY** and must be strictly followed for all code generation and modifications.

* **TSDoc Standard:** You MUST use strict `TSDoc` syntax (not legacy JSDoc) for all exported functions, classes, Next.js components, and Payload CMS configurations.
* **English Language Requirement:** All inline comments, TSDoc blocks, system diagrams, and documentation files (such as `ARCHITECTURE.md` and module-specific `README.modules.md` files) MUST be written exclusively in **English**, regardless of the application's target language, locale (e.g., Persian/fa), or translation settings.
* **Individual Tag Requirements:**
  * **Generics (`@typeParam`):** You MUST write a separate `@typeParam` tag for *every single* generic type parameter. Explain its constraints, what it represents, and how it is used.
  * **Inputs (`@param`):** You MUST write a separate `@param` tag for *every single* input parameter. Grouping parameters or omitting any parameter is strictly prohibited. Explain the parameter name, expected data type, restrictions/constraints (e.g., non-empty, positive number, specific string format), and its semantic role in the function.
  * **Defaults (`@defaultValue`):** You MUST use the `@defaultValue` tag to specify default values for optional parameters, optional configuration options, or component props.
  * **Outputs (`@returns`):** You MUST write a detailed `@returns` tag describing the return value. Do not just state the return type. Describe the exact meaning of the return value, any range of possible values, and conditional return scenarios (e.g., "returns `null` if the user is not found, otherwise returns the fully populated `User` object").
  * **Exceptions (`@throws`):** You MUST write a separate `@throws` tag for *every* exception class or error type that the function can throw. Describe the specific logical condition or input state that triggers the exception, and how the consumer should handle it.
* **The "Why" over the "What":** Do not simply repeat the function name. Explain *why* the logic exists and what specific edge cases it handles.
* **Descriptive Comments:** Write clear, descriptive inline comments on all code you generate to explain the rationale, complex logic blocks, and any non-trivial execution flows.

### Detailed TSDoc Example

```typescript
/**
 * Processes a financial transaction for a user account, applying fees and verifying funds.
 *
 * @typeParam TReceipt - The specific type of transaction receipt returned, extending `TransactionReceipt`.
 * 
 * @param accountId - The unique identifier of the target account (must be a valid 24-character hex ID).
 * @param amount - The transaction amount in cents (must be a strictly positive integer, greater than 0).
 * @param isExpress - An optional flag to apply express processing fees.
 *                    @defaultValue `false`
 * 
 * @returns A promise resolving to a detailed receipt of type `TReceipt` containing the `transactionId`,
 * the `finalBalance` (in cents) after fees, and the `timestamp` of processing.
 *
 * @throws {@link AccountNotFoundError} Thrown if the `accountId` does not match any record.
 * @throws {@link InsufficientFundsError} Thrown if the account balance is less than `amount` plus processing fees.
 * @throws {@link ValidationError} Thrown if the `amount` is non-positive or `accountId` is malformed.
 */
export async function processTransaction<TReceipt extends TransactionReceipt>(
  accountId: string,
  amount: number,
  isExpress: boolean = false
): Promise<TReceipt> {
  // Implementation details...
}
```

## Context-Aware Testing

* **Test Generation:** For every complex utility, component, or Server Action generated, you MUST autonomously generate a corresponding unit test file (e.g., `action.test.ts`).
* **Framework Selection:** Inspect `package.json` to identify if a testing framework is already established (e.g., Jest, Vitest, Cypress). If found, strictly use that framework's syntax and test runner. If no testing framework is installed, default to the native test runner provided by the project's package manager (`bun test`).
* **Execution:** Do NOT run tests manually during standard workflow execution. Test execution is handled automatically via `pre-commit` hooks. If manual testing/verification is requested, follow the `/verify-build` workflow.

## Architecture Mapping & Localized Docs

### Global System Architecture (`ARCHITECTURE.md`)

* Maintain a single `ARCHITECTURE.md` file at the root of the workspace.
* This file must contain a high-level **Mermaid.js sequence or flowchart diagram** illustrating how the system modules interconnect and interact.
* **Trigger:** Evaluate the codebase layout on *every code generation cycle*. If a change alters data flow, module boundaries, or dependencies, immediately update this diagram.
* **Modular Updates:** ONLY append or modify the specific section relating to your current task to prevent context loss.
* **Mermaid Syntax Validation:** Double-check your Mermaid syntax for strict compliance (e.g., quote labels containing special characters) before saving the file to prevent errors.
* **Payload CMS ERDs:** Any time you modify a Payload CMS Collection or Global, you MUST update the Entity-Relationship Diagram (ERD) inside `ARCHITECTURE.md` to reflect the new database schema.

### Localized Algorithmic Documentation (`README.modules.md`)

* Create or append to a `README.modules.md` (or a dedicated `.md` file matching the module name) within the *immediate directory* of the modified/generated code.
* **Requirements:**
  1. **Algorithmic Breakdown:** A step-by-step prose explanation of how the algorithm functions.
  2. **Execution Flow / Internal Data Flow:** A localized Mermaid diagram visualizing state changes, conditional branches, or internal control loops of that specific file/directory (e.g., Zustand state, Server Action mutations).
  3. **Consumer Tracking (File References):** Explicitly list the exact file paths of every file that imports or consumes this module. Update this list if implemented in new files.
  4. **Integration Graph (Mermaid):** A Mermaid flowchart that visually connects this module to the files/routes (`page.tsx`, `layout.tsx`) that ultimately render/use it.
  5. **Dependencies:** Note any external packages or specific Payload CMS API endpoints this module relies on.
* **Scope:** Apply this rule retroactively when refactoring, and mandatorily for all brand-new code implementations.
