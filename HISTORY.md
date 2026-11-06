# HISTORY.md — Session History & Audit Trail

## 2026-11-06 — Publishing, AI Transparency, and Tidal/Residual Modeling Enhancements

- **Execution Environment**: Linux VM, Conda environment `roms_tide_optimizer` (Python 3.11.15).
- **Primary Objectives**:
  1. Review and update `AGENTS.md` and domain skills in `/home/nilsmk/git/skillcollection/`.
  2. Embed operational lessons learned from 14,376-hour NorKyst-800m ROMS evaluation against Kartverket tide gauges.
- **Key Updates to AGENTS.md**:
  - Added requirement for a 1-page stand-alone **Executive Summary** in all major evaluation and validation reports.
  - Added mandatory **Declaration of Generative AI and AI-Assisted Technologies in the Scientific Process** conforming to COPE/Elsevier/Nature standards.
  - Added zero overfull `\hbox` margin standards to pre-response quality checklists.
  - Linked updated domain skills (`roms-ocean-eval`, `kartverket-tide-api`, `scientific-latex-report`, `agentic-coding-optimizer`).

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
