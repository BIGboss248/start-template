---
description: Rules and steps to follow when trying to redesign a react component without changing its logic or core functionality
---

# Workflow: Redesign Component

This workflow defines the step-by-step process to follow when refactoring or redesigning the visual styling, theme, layout, or animations of an existing React component, without altering its core business logic or API signature.
**IMPORTANT:** Adhere to the standards defined in `.agents/rules/` for all implementations. Do not proceed without referencing the applicable rules.

---

## Phase 1: Pre-Flight & Planning

Before modifying any styling or markup:

1. **Chain of Thought Planning:** Plan the visual changes, styling strategy, and animation pipelines.
2. **Behavior Analysis:** Analyze the existing component's prop interface, callbacks, hooks, and internal state. The redesign **must not** break existing behavior.
3. **Shadcn Component Check:** Replace custom elements with standard Shadcn UI components where beneficial.
4. **GSAP Skills Check:** Check local GSAP skills for complex animations.
5. **RSC vs. Client Boundary Check:** Maintain the existing rendering strategy (`rules/architecture.md`). Isolate new interactivity into leaf Client Components rather than converting the entire component.

---

## Phase 2: Implementation & Styling

1. **Styling:** Exclusively use Shadcn CSS variables and Tailwind logical properties. Merge classes with `cn(...)` (`rules/ui-styling.md`).
2. **Loading Skeletons & CLS Prevention:** Update corresponding `<Skeleton>` fallbacks to match new dimensions exactly (`rules/ui-styling.md`).
3. **Responsive Layouts:** Ensure responsiveness across all devices.
4. **Animations:** Use GSAP for complex timelines, referencing GSAP skills when applicable.
5. **Asset Optimization:** Exclusively use Next.js `<Image>` components (`rules/ui-styling.md`).

---

## Phase 3: SEO & Semantic HTML Integrity

1. **Semantic HTML:** Do not break semantic HTML hierarchies or heading nesting (`rules/seo.md`).
2. **Metadata Integrity:** Do not alter or break existing JSON-LD schemas, alt texts, or meta tags embedded in the component (`rules/seo.md`).

---

## Phase 4: Documentation & Test Verification

1. **Documentation Preservation & Updates:** Do not corrupt existing TSDoc annotations. If new helpers or props are introduced, document them using strict English TSDoc (`rules/documentation.md`). Update `README.modules.md` or `ARCHITECTURE.md` if state flows or dependencies are impacted (`rules/documentation.md`).
2. **Test Verification (MANDATORY):** Do NOT run tests manually. If the redesign breaks testing selectors (e.g. `data-testid`), update the selectors in the tests so they align with the new markup while keeping assertions intact.

---

## Phase 5: Verification & Cleanup

1. **Verification Gate:** Do NOT manually run checks. Rely on `pre-commit` hooks, or use the `/verify-build` workflow if manual verification is requested.
2. **Code Cleanup:** Remove any temporary test styling or console logs (`// TODO: REMOVE BEFORE PRODUCTION`).

---

## Phase 6: Handoff & User Commits

1. **Handoff:** Present completed work. Do NOT run git commit commands. Leave staging and committing to the user.
