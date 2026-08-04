# Project Overrides & Extensions Directory

> [!TIP]
> **HOW OVERRIDES & APPENDS WORK**:
> 1. **To Append Rules/Workflows**: Add any new markdown file (e.g. `payment-gateway.md` or `custom-auth.md`) to `rules/` or `workflows/`. The AI agent will load it alongside universal core rules.
> 2. **To Override Core Rules/Workflows**: Add a file with the **exact same filename** as a file in `.agents/core/` (e.g. `rules/workflow.md`). The AI agent will give this local file highest precedence over `.agents/core/rules/workflow.md`.

## Subdirectories
- `rules/`: Custom or overridden project coding standards.
- `workflows/`: Custom or overridden step-by-step task checklists.
