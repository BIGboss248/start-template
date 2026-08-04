# Agent Rules Management, Git Submodule Sync & Exclamation Override Guide

This guide explains how to manage, update, and synchronize AI agent rules and workflows across your repositories using **Git Submodules**, while keeping project-specific data (`PROJECT.md`) completely isolated and allowing local rule overrides via the **Exclamation (`!`) Prefix**.

---

## 1. Directory & Submodule Architecture

When using Git Submodules to synchronize rules across projects, the Git submodule is cloned directly to **`.agents`** (or **`.agents/core`**).

```
📁 project-root/ (Parent Project Git Repo)
├── 📄 AGENTS.md                          <-- AI Entry Point & Precedence Router (Tracked in project repo)
├── 📁 docs/
│   └── 📄 agent-rules-management.md      <-- This documentation file
└── 📁 .agents/       [GIT SUBMODULE] ───► Cloned from git@github.com:bigboss248/start-template.git
    ├── 📄 PROJECT.md                     <-- Project Context & Metadata (Untracked by central submodule)
    ├── 📁 rules/                         <-- Flat Rules Directory
    │   ├── 📄 workflow.md                 <-- Tracked Core Rule (Synced via Git)
    │   ├── 📄 !workflow.md                <-- Project Override (Untracked local file — SAFE)
    │   └── 📄 framework-nextjs.md         <-- Tracked Core Rule (Synced via Git)
    └── 📁 workflows/                     <-- Flat Workflows Directory
        ├── 📄 create-component.md         <-- Tracked Core Workflow (Synced via Git)
        └── 📄 !create-component.md        <-- Project Override Workflow (Untracked local file — SAFE)
```

---

## 2. Git Submodule Sync & Untracked Local Files Policy

### How Submodule Updates Work

When you clone `.agents` as a Git Submodule:
- **Tracked Files**: Files originating from the central rules repository (`rules/workflow.md`, `rules/framework-nextjs.md`, `workflows/create-component.md`) are tracked by the submodule.
- **Untracked Local Files**: Any project-specific files added locally in your repository—such as `.agents/PROJECT.md`, local override files (`.agents/rules/!workflow.md`), or custom app rules—are **untracked by the central submodule repository**.

### Why Updates Will Never Break Sync

When you pull updated rules from the central repository using:

```bash
git submodule update --remote --merge
```

1. **Only Tracked Files Update**: Git updates ONLY the tracked core rules and workflows coming from the central repository.
2. **Untracked Files Remain Safe**: Git **does NOT delete or overwrite untracked local files**. Your `.agents/PROJECT.md` and local `!*.md` override files will stay completely untouched and intact.
3. **Zero Sync Conflicts**: Because local project files are untracked by the central submodule remote, there are no merge conflicts or stashing problems when updating rules.

---

## 3. Exclamation (`!`) Prefix Override Mechanism

Instead of placing local overrides in a separate directory, override files are created directly inside `.agents/rules/` or `.agents/workflows/` prefixed with an exclamation mark (`!`):

- **Overriding a Rule**: To override `workflow.md` or `framework-nextjs.md`, create `.agents/rules/!workflow.md` or `.agents/rules/!framework-nextjs.md`.
- **Overriding a Workflow**: To override `create-component.md`, create `.agents/workflows/!create-component.md`.
- **Appending a Rule/Workflow**: Create any file without a `!` prefix or matching base filename (e.g. `.agents/rules/stripe-payments.md`). It will be loaded alongside core rules.

**AI Evaluation Rule**: When reading directives, the AI agent checks for a `!` prefixed file matching the target file name. If found, the AI agent uses the `!` version **instead** of the base version.

---

## 4. Framework Rule & Workflow Naming Conventions (Prefix Format)

Do NOT place rules or workflows in subdirectories (like `frameworks/nextjs.md` or `nextjs/create-component.md`), because IDE indexers may miss them.

Instead, store all files in the root of `rules/` and `workflows/` using standard prefixes:

- **Framework Rules**: Prefix with `framework-<name>.md`
  - Example: `.agents/rules/framework-nextjs.md` (Override: `.agents/rules/!framework-nextjs.md`)
- **Framework Workflows**: Prefix with `<name>-<workflow>.md`
  - Example: `.agents/workflows/nextjs-create-component.md` (Override: `.agents/workflows/!nextjs-create-component.md`)

AI agents dynamically inspect `Framework & Version` in `.agents/PROJECT.md` and look up files matching `framework-<name>.md` and `<name>-<workflow>.md`.

---

## 5. Setting Up `.agents` as a Git Submodule in a New Project

### Step A: Link Submodule to Your New Repository
In your project root directory, run:

```bash
git submodule add git@github.com:bigboss248/start-template.git .agents
```

### Step B: Create Your Project Context File
Create `.agents/PROJECT.md` locally to store project-specific details (database, framework version, client info). This file is untracked by the central rules repo and remains local to your project.

---

## 6. How to Update & Sync Rules Across Projects

### When You Learn Something New & Update Central Rules
1. Edit universal rules inside your central repo (`start-template`).
2. Commit and push:
   ```bash
   git add .
   git commit -m "feat: update Next.js RSC caching rules and workflow checklists"
   git push origin main
   ```

### Syncing Updates in Downstream Repositories
To pull the latest rules into any project, run:

```bash
git submodule update --remote --merge
```

---

## 7. Cloning a Project Repository with Submodules

When cloning a project repository containing the `.agents` submodule:

```bash
# Clone repository and initialize submodules in one step:
git clone --recurse-submodules git@github.com:bigboss248/my-project.git

# Or if cloned without --recurse-submodules:
git submodule update --init --recursive
```
