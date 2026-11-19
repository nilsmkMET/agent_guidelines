# AGENTS.md — Global Engineering & Agent Guidelines for Nils M. K.

This file serves as the definitive reference manual and operational standard for all AI agents working across projects and repositories in `/home/nilsmk/git/`. Every agent session must adhere to these directives.

---

## 1. Core Operating Principles & Workflow Standards

1. **Package & Environment Management (`mamba` / `micromamba` exclusively)**:
   - Use `mamba` (or `micromamba`) from `conda-forge` for all Python environments and dependencies.
   - Never use standalone `pip install` commands unless explicitly requested or when installing local editable development packages (`pip install -e .`).
   - Every project/repository **MUST** contain a pinned, reproducible `environment.yml` at its root (e.g. `numpy=1.26.4`).
   - Use `-y` in commands (`mamba env create -f environment.yml -y`, `mamba install -y ...`) to prevent interactive stalling in headless VM/CLI sessions.
   - Check if the environment exists at session startup; if not, create it as documented.
   - Do not add unapproved channels; use only `conda-forge` (avoid paid-ToS channels from Anaconda Inc).

2. **Full Reproducibility & Code-First Execution ("Do Not Guess — Calculate")**:
   - Write clean, executable code for **all** tasks so that every step can be inspected, rerun, and recreated by a human or another agent session.
   - Never perform complex manual transformations or one-off opaque calculations in the terminal; write dedicated Python scripts (e.g. in `scripts/`, `src/`, or `AI_calculations/`) and execute them with explicit logging.
   - When calculations, estimates, or checks are needed, do not guess or do mental math: implement a small calculation script in `AI_calculations/AI_CALCULATION_XX.py` (where `XX` is a short description of the computation), run it to obtain the guaranteed result, and archive it.
   - Use symbolic mathematics (e.g. `sympy`) to solve equations algebraically rather than attempting step-by-step token arithmetic.
   - All scripts in `AI_calculations/` must follow the same coding guidelines as production code (linting, docstrings, type annotations).

3. **Code Reuse ("Old Code First")**:
   - Before building new algorithms, utilities, or workflows, inspect related repositories in `/home/nilsmk/git/` (e.g., oceanographic routines in `hindcast_eval`, `inverse_tides`, `bias_correction_2026`, `roms_tools_in_python`, sailing analytics in `routing`, `DumbSail`, text standards in `skillcollection`).
   - Reuse existing, proven modules, helper functions, and coordinate/data transforms whenever available to prevent redundant reinvention.

4. **Documentation & Interactive Notebooks (`ipynb`)**:
   - Document all functions, modules, pipelines, and mathematical assumptions thoroughly.
   - Provide runnable Jupyter notebooks (`.ipynb`) in `notebooks/` (or repository root where conventional) for interactive exploration, methodology verification, data visualization, and step-by-step reporting.
   - In standalone Python scripts, use logical section markers (`# %%`) at the beginning, between logical steps, and at the end of each script so files remain IPython/Jupyter cell-friendly.

5. **Version Control & Repository Hygiene**:
   - Every project folder must be an initialized Git repository with an appropriate `.gitignore` (ignoring data caches, `.pyc`, large NetCDF/GRIB binaries, virtual environments, `.ipynb_checkpoints`, and LaTeX auxiliary files like `.aux`, `.log`, `.out`, `.toc`, `.fls`).
   - Use `main` as the default/primary branch name across all repositories (never `master`). When initializing new repositories (`git init -b main`) or maintaining existing ones, ensure the primary branch is set to `main`.
   - Commit working code, configurations, notebooks, and documentation frequently with clear, descriptive commit messages.
   - Never commit API keys, personal credentials, or confidential data paths.

6. **Project Documentation: `README.md`, `DOCUMENTATION.md`, `HISTORY.md`, `DISCUSSIONS_LOG.md` & `AI_REASONING.md`**:
   - **`README.md`**: Primary entry point. Must clearly outline the scientific/engineering objectives, architecture, directory tree, prerequisites, and explicit instructions to reproduce the work end-to-end.
   - **`DOCUMENTATION.md`**: Comprehensive developer guide covering architecture, API design, setup instructions, and deep technical context.
   - **`HISTORY.md`** (or `HISTORY.med`): Maintain an exhaustive audit trail of all user prompts, execution environments, Lustre/storage paths, key decisions, model configurations, and exact terminal commands needed to pause and resume work seamlessly across different machines or sessions.
   - **`DISCUSSIONS_LOG.md`**: Maintain a concise, scannable interaction log using this standard format:
     ```markdown
     ## YYYY-MM-DD — <one-line topic>
     **Prompt:** <one or two sentence summary of what the user asked>
     **Actions:** <bullet list of what was done>
     **Outcome:** <one sentence on result or decision reached>
     ```
   - **`AI_REASONING.md`**: Record non-trivial mathematical formulations, algorithmic trade-off analyses, and design decisions that should persist across sessions.
   - **`THOUGHTS.md`**: Optional scratchpad for insights, hypotheses, or pending ideas to carry over into future sessions.

7. **Reports & Publications: LaTeX for PDF Deliverables**:
   - Use **LaTeX** (`pdflatex` / `xelatex` / `latexmk`) for all formal technical reports, white papers, conference submissions, and PDF deliverables.
   - Maintain LaTeX source code and figure assets cleanly in a `report/` or `docs/` subfolder.
   - **Mandatory Executive Summary**: Major technical and evaluation reports should feature a structured, stand-alone Executive Summary (approx. 1 page) placed immediately after the Abstract, highlighting primary findings, quantitative metrics, and operational recommendations.
   - **AI Transparency & Authorship Disclosure**: Any manuscript or report prepared with generative AI assistance **MUST** include a formal `Declaration of Generative AI and AI-Assisted Technologies in the Scientific Process` section placed immediately before the references/bibliography, complying with international journal publishing guidelines (COPE, Elsevier, Nature Portfolio). Detail the specific tools used, workflows assisted, and confirm human author accountability.
   - **Zero-Warning Margin Standard**: PDF builds must be checked for overfull `\hbox` warnings protruding into margins. Wide multi-column tables must utilize `\footnotesize`, tuned `\tabcolsep`, or wrapped paragraph columns (`p{...}`).

---

## 2. Python Architecture & Engineering Best Practices

- **Functional Architecture Over Unnecessary OOP**:
  - Favor simple, pure functions and flat modular architecture over deep object-oriented hierarchies. Use classes primarily when state encapsulation is genuinely required (e.g. data streaming, model estimators).
- **Constants & Configuration**:
  - Define all magic constants as top-level `ALL_CAPS_CONSTANTS` at the start of modules.
  - Pass constants as default keyword arguments to functions to preserve configurability.
  - Use JSON files for experiment configurations, model hyperparameters, and benchmark metadata. Prefer JSON over YAML/TOML/CSV for machine-readable config consistency.
- **Typing & Docstrings**:
  - Use strict Python type annotations (`typing`, `numpy.typing`) for all function signatures.
  - Provide Google-style docstrings for all functions, modules, and classes.
- **Defensive Programming & Input Validation**:
  - Assert all assumptions regarding input structures, shape dimensions, datetime indices, coordinate projections, and physical bounds.
  - Real-world data is inherently noisy and prone to corrupt values; assert non-tautological invariants at module boundaries.
- **Logging with `loguru`**:
  - Use `loguru` for structured, single-line logging.
  - Capture inputs, parameters, intermediate milestones, timings, warnings, and final metrics.
  - Ensure logs are formatted as single lines to prevent terminal interleaving.
  - Name log files systematically: `{script_idx}_{descriptive_name}_YYYY_MM_DDTHH_MM_SS.log`.
  - For long-running commands logged to file, disable output buffering (e.g. `--no-capture-output` with `conda run`).
- **Reproducibility & Random Seeds**:
  - If a script utilizes random number generators (`random`, `numpy.random`, `torch`), set and log a seed at the very top of the script. Define the seed as an `ALL_CAPS_CONSTANT`.
- **Long Loops & Iterations**:
  - Wrap iterations running longer than a few seconds in `tqdm` for live progress tracking, ensuring clean terminal display alongside `loguru`.
- **Script Modularity & Task Separation**:
  - Guard all executable scripts with `if __name__ == "__main__":` so functions can be imported into test suites without side effects.
  - For multi-step batch pipelines, break workflows into numbered, self-contained scripts: `00_ingest_data.py`, `01_preprocess.py`, `02_train_or_evaluate.py`.
  - Dedicate subfolders (e.g. `00_task_name/`, `01_task_name/`) to intermediate caches and artifacts to keep the repository root clean. Final deliverables belong in designated export directories (`data/`, `figures/`, `report/`).
  - Use explicit exit codes: `sys.exit(0)` on success and `sys.exit(1)` on fatal errors.
- **Static Analysis & Formal Code Checkers**:
  - Format and lint with `ruff`.
  - Secondary linting with `pylint` and `flake8` when configured.
  - Verify syntax without execution using `py_compile`.
  - Type-check with `mypy`.
  - Verify spellings with `codespell`.
  - Keep cognitive complexity below 20 (`complexipy`). If complexity must exceed 20 due to inherent problem requirements, document the justification.
  - Unit test with `pytest` after every significant code change.
- **Shell & Bash Script Standards**:
  - Run `shellcheck` on all shell scripts.
  - Always include a strict error preamble in bash scripts (`set -euo pipefail`).
- **Headless & GUI Display Awareness**:
  - Matplotlib scripts must automatically detect whether a graphical display is available. In headless environments, default to `matplotlib.use("Agg")` and save figures without attempting interactive popups.
  - Save all figures at high resolution (300 dpi PNG/PDF) using the standard naming scheme:
    `{script_idx}p{plot_idx}_{descriptive_name}.png` (e.g. `00p01_sea_surface_height_anomalies.png`).
  - Print compact metric summaries in stdout/logs alongside figure generation so both humans and AI agents can inspect key results without needing image rendering.

---

## 3. HPC, Data Storage & Oceanographic Domain Protocols

- **Lustre Storage File Locking**:
  - When accessing read-only Lustre mount points (such as `/lustre/storeB/...`) with NetCDF4, HDF5, or `xarray`, always ensure file locking is disabled:
    ```bash
    export HDF5_USE_FILE_LOCKING=FALSE
    ```
    or in Python:
    ```python
    import os
    os.environ["HDF5_USE_FILE_LOCKING"] = "FALSE"
    ```
- **Remote OPeNDAP & THREDDS Access**:
  - When reading NetCDF datasets served via THREDDS or OPeNDAP endpoints, stream data directly with `xarray` via the remote URL. Do not download massive raw NetCDF files locally unless explicitly requested.
- **Meteorological & Oceanographic Tools**:
  - Standard numerical libraries: `xarray`, `netCDF4`, `h5py`, `pandas`, `numpy`, `scipy`, `cartopy`, `cmocean`, `utide`.
  - External APIs: Use robust retry logic and session management when querying APIs like Kartverket Tide API (`tideapi.php`).
- **RAM and Host Resource Safety**:
  - Run memory-heavy scripts sequentially. Monitor memory footprint to prevent out-of-memory (OOM) host lockups.
  - When running long background jobs, use a `.sh` wrapper with `tee` to preserve terminal and file logging.

---

## 4. Sub-Agent & Resource Management

- **Model Escalation Guard**:
  - When delegating tasks to sub-agents, do **not** use a more expensive model (e.g. Opus) than the active agent without explicit user approval.
  - By default, use the same model tier as the parent session.

---

## 5. Follow the Rules of Good AI Development

### 1. Think Before Coding
**Don't assume. Don't hide confusion. Surface tradeoffs.**
- State assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them rather than picking silently.
- If a simpler approach exists, propose it.
- If an instruction is unclear, stop and ask the user for clarification.

### 2. Simplicity First
**Minimum code that solves the problem. Nothing speculative.**
- No features beyond what was asked.
- No abstractions for single-use code.
- No speculative configurability that wasn't requested.
- No defensive error handling for impossible internal scenarios.
- If 200 lines could be 50, rewrite it.

### 3. Surgical Changes
**Touch only what you must. Clean up only your own mess.**
- Do not modify adjacent unrelated code, comments, or formatting.
- Do not refactor functional code outside the request scope.
- Match existing repository style and conventions.
- Remove imports, variables, and functions made orphaned by your edits. Do not remove pre-existing dead code unless requested.
- Every changed line must trace directly to the user's request.

### 4. Goal-Driven Execution
**Define success criteria. Loop until verified.**
- Transform tasks into verifiable goals:
  - "Add validation" → "Write tests for invalid inputs, then make them pass."
  - "Fix the bug" → "Write a test that reproduces it, then make it pass."
  - "Refactor module" → "Ensure tests pass before and after."
- For multi-step tasks, execute a clear plan:
  1. `[Step]` → verify: `[check]`
  2. `[Step]` → verify: `[check]`
  3. `[Step]` → verify: `[check]`

---

## 6. Specialized Domain Skills Repository

Curated domain skills and standardized methodologies are maintained in [nilsmkMET/skillcollection](https://github.com/nilsmkMET/skillcollection) (local clone: `/home/nilsmk/git/skillcollection/`). Refer to and adopt these specifications for relevant tasks:
- **`roms-ocean-eval`** ([SKILLS_roms_ocean_eval.md](/home/nilsmk/git/skillcollection/SKILLS_roms_ocean_eval.md)): ROMS model verification, CPP options, non-linear tide-surge interaction diagnostics ($\zeta_{\text{int}}$), `utide` harmonic analysis, total tidal error vectors ($D_{\mathrm{total}}, D_4$), 1D circular phasors vs 2D current ellipses, and Inverse Barometer (IB) domain-scale under-response dynamics and blending.
- **`kartverket-tide-api`** ([SKILLS_kartverket_tide_api.md](/home/nilsmk/git/skillcollection/SKILLS_kartverket_tide_api.md)): Norwegian Mapping Authority water level and tide API client specifications, unit conversions (cm to m), multi-year timeseries ingestion, station availability, and caching.
- **`scientific-latex-report`** ([SKILLS_scientific_latex_report.md](/home/nilsmk/git/skillcollection/SKILLS_scientific_latex_report.md)): Standard scientific LaTeX templates, `booktabs` typography, 1-page executive summaries, AI transparency disclosures, margin-safe table layout, and two-pass non-interactive PDF build scripts.
- **`agentic-coding-optimizer`** ([SKILLS_agentic_coding_optimizer.md](/home/nilsmk/git/skillcollection/SKILLS_agentic_coding_optimizer.md)): Token budget management, context compaction strategies, caching optimization, and autonomous agent engineering workflows.
- **`grib-netcdf-wrangler`** ([SKILLS_grib_netcdf_wrangler.md](/home/nilsmk/git/skillcollection/SKILLS_grib_netcdf_wrangler.md)): High-performance NetCDF/GRIB handling, Lustre storage locking workarounds, memory-safe hyperslabs, and grid coordinate rotations.
- **`sailing-polar-telemetry`** ([SKILLS_sailing_polar_telemetry.md](/home/nilsmk/git/skillcollection/SKILLS_sailing_polar_telemetry.md)): ORC polar parsing, 2D SciPy polar interpolations, VMG, tacking loss metrics, and Dash-Plotly dashboard conventions.
- **`klarsprak-norsk`** ([SKILLS_klarspraak_norsk.md](/home/nilsmk/git/skillcollection/SKILLS_klarspraak_norsk.md)): Norwegian plain language standards (*Språklova* § 9, ISO 24495-1).
- **`clear-academic-english`** ([SKILLS_clear_academic_english.md](/home/nilsmk/git/skillcollection/SKILLS_clear_academic_english.md)): Academic writing standards, nominalization reduction, and scholarly clarity.
- **`plain-english`** ([SKILLS_plain_english_broad_audience.md](/home/nilsmk/git/skillcollection/SKILLS_plain_english_broad_audience.md)): Broad audience readability and plain language.

---

## 7. Session Startup & Resume Checklist

When opening a new session in any project repository, execute this checklist:
1. **Read Core Files**:
   - Read `AGENTS.md` and `README.md`.
   - Read existing project tracking files: `DOCUMENTATION.md`, `HISTORY.md`, `DISCUSSIONS_LOG.md`, `AI_REASONING.md`, `THOUGHTS.md` (skip those not yet created).
2. **Inspect Workspace**:
   - Inspect the directory tree and existing code to understand current status.
3. **Environment Verification**:
   - Check and activate the project's mamba environment specified in `environment.yml`. If missing, create it using `mamba env create -f environment.yml -y`.
4. **Baseline Testing**:
   - Run existing unit tests (`pytest`) to establish baseline stability before applying changes.

---

## 8. Pre-Response Quality Checklist

After completing any code changes, verify each item before returning control to the user:
- [ ] All modified files pass static checks: `mypy`, `ruff`, `pylint`, `codespell`, `complexipy` (< 20 or documented).
- [ ] Any calculation scripts in `AI_calculations/` pass static checks and tests.
- [ ] All unit tests pass (`pytest`).
- [ ] Generated figures are correctly named (`{script_idx}p{plot_idx}_{descriptive_name}.png`) and saved at 300 dpi; headless execution verified.
- [ ] LaTeX documents compile cleanly with zero fatal errors and zero overfull \hbox margin warnings.
- [ ] Reports prepared with generative AI include a formal Generative AI transparency disclosure section.
- [ ] Relevant documentation files (`README.md`, `DOCUMENTATION.md`, `HISTORY.md`) are kept up to date.
- [ ] `DISCUSSIONS_LOG.md` is updated with a concise record of the interaction.
- [ ] `AI_REASONING.md` is updated if new non-trivial mathematical formulations or algorithmic decisions were reached.
- [ ] Code is reviewed for clean architecture, removing any orphan variables, imports, or dead code introduced by changes.
- [ ] No API keys, secrets, credentials, or confidential data paths are committed or logged.
- [ ] Code is secure, safe, and robust.

