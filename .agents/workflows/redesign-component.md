---
description: Rules and steps to follow when trying to redesign a react component without changing its logic or core functionality
---

# Workflow: Redesign Component

This workflow defines the step-by-step process to follow when refactoring or redesigning the visual styling, theme, layout, or animations of an existing React component, without altering its core business logic or API signature.
**IMPORTANT:** Adhere to the standards defined in `.agents/rules/` for all implementations. Do not proceed without referencing the applicable rules.

---

## Phase 1: Pre-Flight Planning & Source Audit

Before modifying any styling or markup:

1. **JSON Pre-Flight Plan:** Output the JSON plan schema as required by `rules/workflow.md`. Audit relevant sources (`AGENTS.md`, `rules/new-react-components.md`, `rules/seo.md`).
2. **Behavior Analysis:** Analyze the existing component's prop interface, callbacks, hooks, and internal state. The redesign **must not** break existing behavior or consumer props.
3. **Component Library Check:** Replace custom elements with standard `PROJECT["project_context_and_metadata"]["component_library"]` UI components where beneficial.
4. **Animation Skills Check:** Check `PROJECT["project_context_and_metadata"]["animation_library"]` skills for complex animations.
5. **Rendering Strategy Check:** Maintain existing component rendering boundaries. Isolate interactive elements into separate client/leaf modules if required by the framework.

---

## Phase 2: Implementation & Styling

1. **Styling:** Exclusively use CSS variables specified in `PROJECT["project_context_and_metadata"]["style_file_dir"]` and Tailwind logical properties. Merge classes with `cn(...)` (`rules/new-react-components.md`).
2. **Loading Skeletons & CLS Prevention:** Update corresponding `<Skeleton>` fallbacks to match new dimensions exactly.
3. **Responsive Layouts:** Ensure responsiveness across mobile, tablet, and desktop layout variants.
4. **Animations:** Use `PROJECT["project_context_and_metadata"]["animation_library"]` for complex timelines, referencing animation skills when applicable.
5. **Asset Optimization:** Use framework-optimized image components or standard optimized assets.

---

## Phase 3: SEO & Semantic HTML Integrity

1. **Semantic HTML:** Do not break semantic HTML hierarchies or heading nesting (`rules/seo.md`).
2. **Metadata Integrity:** Do not alter or break existing JSON-LD schemas, alt texts, or meta tags embedded in the component (`rules/seo.md`).

---

## Phase 4: Documentation & Test Verification

1. **Documentation Preservation & Updates:** Do not corrupt existing TSDoc annotations. If new helpers or props are introduced, document them using strict English TSDoc (`rules/documentation.md`). Update `README.modules.md` or `ARCHITECTURE.md` if state flows or dependencies are impacted (`rules/documentation.md`).
2. **Test Verification (MANDATORY):** Do NOT run tests manually. If the redesign breaks testing selectors (e.g. `data-testid`), update the selectors in the tests so they align with the new markup while keeping assertions intact.

---

## Phase 5: Self-Reflection & Quality Audit (MANDATORY)

Execute an explicit Self-Reflection Pass and output the `### Self-Reflection & Quality Audit` block (`rules/workflow.md`) evaluating:

1. **Adversarial Checks:** Test visual regressions, check that no props were renamed/broken for existing consumers, and verify skeleton sizing during loading states.
2. **Redesign Rule Checklist:**
   - [ ] Props & Logic Preservation: Did the component redesign preserve 100% of existing behavior and callbacks?
   - [ ] Styling Tokens: Are semantic tokens (`bg-background`, `text-foreground`) used exclusively?
   - [ ] RTL/i18n: Are Tailwind logical properties (`ms-`, `pe-`) preserved across all modified classes?
   - [ ] Tests: Are existing `data-testid` attributes preserved or updated in test files?
3. **Identified Bugs & Fixes:** Document any broken styles, layout shifts, or lost props identified and corrected.

---

## Phase 6: Verification & Cleanup

1. **Verification Gate:** Do NOT manually run checks. Rely on `pre-commit` hooks, or use the `/verify-build` workflow if manual verification is requested.
2. **Code Cleanup:** Remove any temporary test styling or console logs (`// TODO: REMOVE BEFORE PRODUCTION`).

---

## Phase 6: Handoff & User Commits

1. **Handoff:** Present completed work. Do NOT run git commit commands. Leave staging and committing to the user.
