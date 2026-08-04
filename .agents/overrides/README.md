# Project Overrides & Extensions Directory

> [!TIP]
> **HOW OVERRIDES & APPENDS WORK**:
> 1. **To Append Rules/Workflows**: Add any new markdown file (e.g. `payment-gateway.md`, `framework-vue.md`, or `custom-auth.md`) to `rules/` or `workflows/`. The AI agent will load it alongside universal core rules.
> 2. **To Override Core Rules/Workflows**: Add a file with the **exact same filename** as a file in `.agents/core/` (e.g. `rules/workflow.md` or `rules/framework-nextjs.md`). The AI agent will give this local file highest precedence over `.agents/core/rules/`.

## Naming Conventions
- Universal Rules: `<concern>.md` (e.g. `workflow.md`, `seo.md`)
- Framework Rules: `framework-<framework_name>.md` (e.g. `framework-nextjs.md`)
- Framework Workflows: `<framework_name>-<workflow_name>.md` (e.g. `nextjs-create-component.md`)

## Subdirectories
- `rules/`: Custom or overridden project coding standards.
- `workflows/`: Custom or overridden step-by-step task checklists.
