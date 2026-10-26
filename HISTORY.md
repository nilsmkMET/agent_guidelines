# HISTORY.md — Session History & Audit Trail

## 2026-10-26 — Master AGENTS.md Harmonization

- **Execution Environment**: Headless Linux development environment (`/home/nilsmk/git`).
- **Primary Objectives**:
  1. Deeply inspect baseline agent guideline templates.
  2. Inventory and review all `AGENTS.md` files across `/home/nilsmk/git/`.
  3. Merge maximum non-contradictory content into `/home/nilsmk/git/agent_guidelines/AGENTS.md`.
  4. Adhere strictly to all newly established rules (good reasoning, explicit documentation, surgical changes, verification).

### Key Reconciliations
- **Git Operations**: Preserved Nils' rule (git repos initialized with `main` branch, regular commits with clear messages) while acknowledging cautious agent behavior.
- **Environment & Mamba**: Reinforced `mamba`/`micromamba` from `conda-forge`, pinned versions, `-y` non-interactive flags, and headless VM considerations.
- **Code-First & Calculation Verification**: Formalized the requirement that agents must write testable Python scripts in `AI_calculations/` or `sympy` solvers rather than doing manual back-of-the-envelope token math.
- **Quality Checkers**: Integrated formal check chain (`ruff`, `pylint`, `mypy`, `codespell`, `complexipy`, `pytest`).
- **Standard Project Files**: Standardized `README.md`, `DOCUMENTATION.md`, `HISTORY.md`, `DISCUSSIONS_LOG.md`, `AI_REASONING.md`, and `THOUGHTS.md`.
