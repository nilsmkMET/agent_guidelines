# Agent Guidelines & Instruction Placement

This repository contains the global engineering and AI agent operating instructions (`AGENTS.md`), as well as documentation on how and where to configure these guidelines so that AI agents (such as GitHub Copilot in VS Code) always load and apply them.

---

## Where to Place Instructions So They Are Always Used

How AI agents pick up guidelines depends on whether you work at the **workspace/repository level** or want **global user-level** instructions applied across every folder you open.

### 1. Global User Level in VS Code (Applies to *any* folder or repo)

If you open individual sub-repositories directly (e.g. `code ~/git/hindcast_eval`), a file located at the parent directory or in another project folder will **not** automatically be loaded. To enforce these guidelines globally across all VS Code sessions regardless of opened directory:

- **Target location**:
  ```text
  ~/.config/Code/User/prompts/copilot-instructions.md
  ```
- **Setup via symlink** (recommended so you maintain a single source of truth):
  ```bash
  mkdir -p ~/.config/Code/User/prompts
  ln -s ~/git/agent_guidelines/AGENTS.md ~/.config/Code/User/prompts/copilot-instructions.md
  ```
- **Setup via VS Code Settings**:
  In VS Code settings (`settings.json`):
  ```json
  {
    "github.copilot.chat.codeGeneration.instructions": [
      {
        "file": "~/git/agent_guidelines/AGENTS.md"
      }
    ]
  }
  ```

---

### 2. Standard Workspace / Repository Root (Per Project)

When opening a specific repository or umbrella directory as the VS Code workspace:

- **Root `AGENTS.md`**:
  Copilot and standard agent frameworks recognize `AGENTS.md` located directly at the workspace root (e.g. `~/git/AGENTS.md` when opening `~/git`).
- **Standard Copilot Workspace Instructions**:
  Within individual repositories, Copilot natively checks:
  ```text
  .github/copilot-instructions.md
  ```
  Placing or symlinking instructions here ensures that anyone opening that specific repository has the guidelines active automatically.

---

## Contents of this Repository

- [AGENTS.md](./AGENTS.md): Complete engineering guidelines, standards for package management (`mamba`), code quality (`ruff`, `mypy`), git conventions (`main` branch), documentation, Lustre locking workarounds, and domain skills.
- [README.md](./README.md): This guide explaining how and where to configure the instructions so they are always active.

---

## Related Repositories

- [skillcollection](https://github.com/nilsmkMET/skillcollection): Curated domain skills and standardized methodologies referenced by `AGENTS.md`.
