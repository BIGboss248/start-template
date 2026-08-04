# Agent Rules Management, Git Submodule Sync & Override Guide

This guide explains how to manage, update, and synchronize AI agent rules and workflows across your repositories using **Git Submodules**, while keeping project-specific data (`PROJECT.md`) completely isolated and allowing local rule overrides/appends.

---

## 1. Directory & Layer Architecture

Your workspace uses a 4-level precedence cascade. Rules defined at higher levels automatically override lower levels.

```
📁 project-root/
├── 📄 AGENTS.md                          <-- AI Entry Point & Precedence Router (Do not edit per project)
├── 📁 docs/
│   └── 📄 agent-rules-management.md      <-- This documentation file
└── 📁 .agents/
    ├── 📄 PROJECT.md                     <-- Level 3: Project Context & Metadata (NEVER overwritten)
    ├── 📁 overrides/                     <-- Level 4: Project-Specific Overrides & Appends (NEVER overwritten)
    │   ├── 📄 README.md                  <-- Explanation of local overrides
    │   ├── 📁 rules/                     <-- Add/override rules for this project
    │   └── 📁 workflows/                 <-- Add/override workflows for this project
    └── 📁 core/  [GIT SUBMODULE] ───────► Level 2: Central Universal Rules (Syncable via Git)
        ├── 📁 rules/                     <-- Universal coding standards (workflow.md, seo.md, i18n.md)
        └── 📁 workflows/                 <-- Universal task checklists (/create-component, /verify-build)
```

### Precedence Order (Lowest to Highest)
1. **Level 1: System Base** (`~/.gemini/config/AGENTS.md`) - System-wide user settings.
2. **Level 2: Core Directives** (`.agents/core/`) - Universal rules synced from central GitHub repo.
3. **Level 3: Project Context** (`.agents/PROJECT.md`) - Active DB, framework version, client info, site map.
4. **Level 4: Project Overrides** (`.agents/overrides/`) - **HIGHEST PRECEDENCE**: Local overrides and appends for this repo.

---

## 2. Setting Up `.agents/core` as a Git Submodule

### Step A: Create Your Central Rules Repository
1. Create a new GitHub repository for your core rules, e.g., `github.com/bigboss248/start-template`.
2. Push your universal `rules/` and `workflows/` folders to this repository.

### Step B: Add Submodule to Template or Project Repo
In your template or project root directory, link `.agents/core` as a Git Submodule:

```bash
# If .agents/core exists as a standard folder, clean or remove it before adding submodule
git submodule add git@github.com:bigboss248/start-template.git .agents/core
```

This creates a `.gitmodules` file in your project root:
```ini
[submodule ".agents/core"]
	path = .agents/core
	url = git@github.com:bigboss248/start-template.git
```

---

## 3. How to Update & Sync Rules Across Projects

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
> **WHY YOUR PROJECT DATA IS SAFE**: Running `git submodule update --remote` strictly updates the contents of `.agents/core/`. Your project configuration (`.agents/PROJECT.md`) and local overrides (`.agents/overrides/`) live outside the submodule and are **never modified**.

---

## 4. How to Override or Append Rules Per Project

### Scenario A: Appending a Custom Rule for a Specific Project
If a project needs a rule that only applies to it (e.g., Stripe Payments or WebSockets):
1. Create a new markdown file in `.agents/overrides/rules/` (e.g., `stripe-payments.md`).
2. The AI agent will automatically load this rule alongside all core rules.

### Scenario B: Overriding a Universal Core Rule
If a project needs a different workflow or rule than the universal core:
1. Copy the filename of the core rule you want to modify (e.g., `workflow.md`).
2. Create a file with the **exact same name** inside `.agents/overrides/rules/` (`.agents/overrides/rules/workflow.md`).
3. Modify the file to fit your project's custom requirements.
4. The AI agent's precedence policy will prioritize `.agents/overrides/rules/workflow.md` over `.agents/core/rules/workflow.md`.

---

## 5. Cloning a Repo with Submodules for New Projects

When initializing a new repository from your template or cloning an existing project:

```bash
# Clone repository and initialize submodules in one step:
git clone --recurse-submodules git@github.com:bigboss248/my-project.ok

# Or if cloned without --recurse-submodules:
git submodule update --init --recursive
```
