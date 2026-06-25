# VSCode Extension Guidelines

This guide classifies the extensions recommended in `extensions.json` based on their CPU/RAM overhead to prevent editor lag.

## 1. Essential / Zero-Overhead Extensions
*These extensions have minimal to no background processes and should always be active for JavaScript/TypeScript web projects:*

* **Linter & Formatter**:
  * `dbaeumer.vscode-eslint` (Crucial for JS/TS static analysis)
  * `esbenp.prettier-vscode` (Code formatter)
* **Configuration & Formats**:
  * `redhat.vscode-yaml` (YAML highlighting, essential for GitHub Actions)
  * `tamasfe.even-better-toml` (TOML parser)
  * `remcohaszing.schemastore` (Autocompletes JSON schema files)
* **Visual Helpers**:
  * `pkief.material-icon-theme` (Explorer theme icons, zero CPU overhead)
  * `aaron-bond.better-comments` (Highlight tags in comments, zero overhead)
  * `bierner.markdown-mermaid` (Renders flowcharts in markdown files)

---

## 2. Dynamic Language Support (Disable unless needed)
*These extensions launch dedicated background Language Server Protocols (LSP) or compiler runtimes. **Disable them unless working on a project in that specific language to save RAM/CPU:***

* **Go Support**:
  * `golang.go` (Spawns a heavy `gopls` compiler analysis server)
* **Python Support**:
  * `ms-python.python` / `ms-python.autopep8` / `ms-python.flake8` (Launches Python helper processes and file watchers)
* **Java Support**:
  * `redhat.java` (**CRITICAL OVERHEAD**: Starts a local JVM process and full eclipse-language-server. Disable unless editing Java code!)
* **Niche Syntaxes**:
  * `ms-vscode.powershell` (Launches a PowerShell backend runhost)
  * `ahmadalli.vscode-nginx-conf` (Nginx configurations)
  * `l-i-v.mql-tools` (MetaQuotes language highlighting)
  * `cperezabo.routeros-syntax` (RouterOS Mikrotik configurations)

---

## 3. General Code Quality (Use with caution)
* **`sonarsource.sonarlint-vscode`**: Performs heavy code-quality scans. If editing large files, this can cause significant typing lag. Keep disabled by default and activate only for final QA review phases.
* **`tabnine.tabnine-vscode`**: Launches a local AI tab-completion node engine. If you are already utilizing an active AI coding assistant (like Antigravity/Gemini), disable Tabnine to avoid keyboard resource contention and redundant code suggestion noise.
