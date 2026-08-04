# Agent Rules Management, Git Submodule Sync & Exclamation Override Guide

This guide explains how to manage, update, and synchronize AI agent rules and workflows across your repositories using **Git Submodules**, while keeping project-specific data (`PROJECT.md`) completely isolated and allowing local rule overrides via the **Exclamation (`!`) Prefix**.

---

## 1. Directory & Layer Architecture

Your workspace uses a 4-level precedence cascade. Rules defined at higher levels automatically override lower levels.

To ensure AI tools and IDEs auto-index all directives natively, rules and workflows are stored as flat files directly under `rules/` and `workflows/` without nested subdirectories.

```
📁 project-root/
├── 📄 AGENTS.md                          <-- AI Entry Point & Precedence Router (Do not edit per project)
├── 📁 docs/
│   └── 📄 agent-rules-management.md      <-- This documentation file
└── 📁 .agents/
    ├── 📄 PROJECT.md                     <-- Level 3: Project Context & Metadata (NEVER overwritten)
    ├── 📁 rules/       [SUBMODULE] ──────► Flat Rules Directory
    │   ├── 📄 workflow.md                 <-- Base universal rule
    │   ├── 📄 !workflow.md                <-- Level 4: Project Override (Takes highest priority!)
    │   ├── 📄 framework-nextjs.md         <-- Flat Framework Rule
    │   └── 📄 custom-project-rule.md      <-- Appended Project Rule
    └── 📁 workflows/   [SUBMODULE] ──────► Flat Workflows Directory
        ├── 📄 create-component.md         <-- Base universal workflow
        ├── 📄 !create-component.md        <-- Level 4: Project Override Workflow (Takes highest priority!)
        └── 📄 nextjs-create-component.md  <-- Flat Framework Workflow
```

### Precedence Order (Lowest to Highest)
1. **Level 1: System Base** (`~/.gemini/config/AGENTS.md`) - System-wide user settings.
2. **Level 2: Core Directives** (`.agents/core/rules/` & `.agents/core/workflows/`) - Universal rules synced from central GitHub repo.
3. **Level 3: Project Context** (`.agents/PROJECT.md`) - Active DB, framework version, client info, site map.
4. **Level 4: Exclamation Prefix Overrides** (`.agents/rules/!<file>.md` & `.agents/workflows/!<file>.md`) - **HIGHEST PRECEDENCE**: Local overrides prefixed with `!`.

---

## 2. Exclamation (`!`) Prefix Override Mechanism

Instead of placing local overrides in a separate directory, override files are created directly inside `.agents/rules/` or `.agents/workflows/` prefixed with an exclamation mark (`!`):

- **Overriding a Rule**: To override `workflow.md` or `framework-nextjs.md`, create `.agents/rules/!workflow.md` or `.agents/rules/!framework-nextjs.md`.
- **Overriding a Workflow**: To override `create-component.md`, create `.agents/workflows/!create-component.md`.
- **Appending a Rule/Workflow**: Create any file without a `!` prefix or matching base filename (e.g. `stripe-payments.md`). It will be loaded alongside core rules.

**AI Evaluation Rule**: When reading directives, the AI agent checks for a `!` prefixed file matching the target file name. If found, the AI agent uses the `!` version **instead** of the base version.

---

## 3. Framework Rule & Workflow Naming Conventions (Prefix Format)

Do NOT place rules or workflows in subdirectories (like `frameworks/nextjs.md` or `nextjs/create-component.md`), because IDE indexers may miss them.

Instead, store all files in the root of `rules/` and `workflows/` using standard prefixes:

- **Framework Rules**: Prefix with `framework-<name>.md`
  - Example: `.agents/rules/framework-nextjs.md` (Override: `.agents/rules/!framework-nextjs.md`)
- **Framework Workflows**: Prefix with `<name>-<workflow>.md`
  - Example: `.agents/workflows/nextjs-create-component.md` (Override: `.agents/workflows/!nextjs-create-component.md`)

AI agents dynamically inspect `Framework & Version` in `.agents/PROJECT.md` and look up files matching `framework-<name>.md` and `<name>-<workflow>.md`.

---

## 4. Setting Up `.agents/core` as a Git Submodule

### Step A: Create Your Central Rules Repository
1. Create a new GitHub repository for your core rules, e.g., `github.com/bigboss248/start-template`.
2. Push your universal `rules/` and `workflows/` folders to this repository.

### Step B: Add Submodule to Template or Project Repo
In your template or project root directory, link `.agents/core` as a Git Submodule:

```bash
# If .agents/core exists as a standard folder, clean or remove it before adding submodule
git submodule add git@github.com:bigboss248/start-template.git .agents/core
```

---

## 5. How to Update & Sync Rules Across Projects

### When You Learn Something New & Update Universal Rules
1. Make your improvements to the rules inside your central repo (`start-template`) or inside `.agents/core/` of any project.
2. Commit and push from the central repo:
   ```bash
   git add .
   git commit -m "feat: enhance pre-flight planning schema and self-reflection checks"
   git push origin main
   ```

### Syncing Updates in Any Project or Downstream Repo
To pull the latest universal rules into any project, run:

```bash
git submodule update --remote --merge
```

> [!NOTE]
> **WHY YOUR PROJECT DATA IS SAFE**: Running `git submodule update --remote` strictly updates the core base rules. Your project configuration (`.agents/PROJECT.md`) and local exclamation overrides (`!*.md`) live safely alongside your project.

---

## 6. Cloning a Repo with Submodules for New Projects

When initializing a new repository from your template or cloning an existing project:

```bash
# Clone repository and initialize submodules in one step:
git clone --recurse-submodules git@github.com:bigboss248/my-project.ok

# Or if cloned without --recurse-submodules:
git submodule update --init --recursive
```
