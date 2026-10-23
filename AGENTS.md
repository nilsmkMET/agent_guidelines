# AGENTS.md — Global Engineering & Agent Guidelines for Nils M. K.

This file serves as the definitive reference manual and operational standard for all AI agents working across projects and repositories in `/home/nilsmk/git/`. Every agent session must adhere to these directives.

---

## 1. Core Operating Principles & Workflow Standards

1. **Package & Environment Management (`mamba` exclusively)**:
   - Use `mamba` (or `micromamba`) from `conda-forge` for all Python environments and dependencies.
   - Never use standalone `pip install` commands unless explicitly requested or when installing local editable development packages (`pip install -e .`).
   - Every project/repository **MUST** contain a pinned, reproducible `environment.yml` at its root.
   - Use `-y` in commands (`mamba env create -f environment.yml -y`, `mamba install -y ...`) to prevent interactive stalling in headless VM/CLI sessions.

2. **Full Reproducibility & Code-First Execution**:
   - Write clean, executable code for **all** tasks so that every step can be inspected, rerun, and recreated by a human or another agent session.
   - Never perform complex manual transformations or one-off opaque calculations in the terminal; write dedicated Python scripts (e.g. in `scripts/`, `src/`, or `AI_calculations/`) and execute them with explicit logging.
   - When calculations, estimates, or checks are needed, do not guess or do mental math: implement a small calculation script, execute it, and record the verified result.

3. **Code Reuse ("Old Code First")**:
   - Before building new algorithms, utilities, or workflows, inspect related repositories in `/home/nilsmk/git/` (e.g., oceanographic routines in `hindcast_eval`, `inverse_tides`, `bias_correction_2026`, `roms_tools_in_python`, sailing analytics in `routing`, `DumbSail`, text standards in `skillcollection`).
   - Reuse existing, proven modules, helper functions, and coordinate/data transforms whenever available to prevent redundant reinvention.

4. **Documentation & Interactive Notebooks (`ipynb`)**:
   - Document all functions, modules, pipelines, and mathematical assumptions thoroughly.
   - Provide runnable Jupyter notebooks (`.ipynb`) in `notebooks/` (or repository root where conventional) for interactive exploration, methodology verification, data visualization, and step-by-step reporting.
   - In standalone Python scripts, use logical section markers (`# %%`) so files remain IPython/Jupyter cell-friendly.

5. **Version Control & Repository Hygiene**:
   - Every project folder must be an initialized Git repository with an appropriate `.gitignore` (ignoring data caches, `.pyc`, large NetCDF/GRIB binaries, virtual environments, `.ipynb_checkpoints`, and LaTeX auxiliary files like `.aux`, `.log`, `.out`, `.toc`, `.fls`).
   - Use `main` as the default/primary branch name across all repositories (never `master`). When initializing new repositories (`git init -b main`) or maintaining existing ones, ensure the primary branch is set to `main`.
   - Commit working code, configurations, notebooks, and documentation frequently with clear, descriptive commit messages.
   - Never commit API keys, personal credentials, or confidential data paths.

6. **Project Documentation: `README.md`, `wiki/`, and `HISTORY.md`**:
   - **`README.md`**: Primary entry point. Must clearly outline the scientific/engineering objectives, architecture, directory tree, prerequisites, and explicit instructions to reproduce the work end-to-end.
   - **`wiki/`**: Maintain modular markdown documentation files for deep-dive technical topics, methodology derivations, data source specifications, and diagnostic findings.
   - **`HISTORY.md`** (or `HISTORY.med`): Maintain an exhaustive audit trail of all user prompts, execution environments, Lustre/storage paths, key decisions, model configurations, and exact terminal commands needed to pause and resume work seamlessly across different machines or sessions.

7. **Reports & Publications: LaTeX for PDF Deliverables**:
   - Use **LaTeX** (`pdflatex` / `xelatex` / `latexmk`) for all formal technical reports, white papers, conference submissions, and PDF deliverables.
   - Maintain LaTeX source code and figure assets cleanly in a `report/` or `docs/` subfolder.

---

## 2. Python Architecture & Engineering Best Practices

- **Typing & Docstrings**: Use strict Python type annotations (`typing`) and Google-style docstrings for all public functions, classes, and methods.
- **Defensive Programming**: Assert all assumptions regarding input structures, shape dimensions, datetime indices, coordinate projections, and physical bounds. Do not assume incoming real-world data is clean.
- **Logging**: Use `loguru` for structured, single-line logging. Capture parameters, data ranges, execution benchmarks, warnings, and final metrics. Avoid unformatted standard print calls in production pipelines.
- **Long Loops**: Wrap long-running iterations with `tqdm` for live progress tracking, ensuring clean terminal display with `loguru`.
- **Quality & Static Analysis**:
  - Format and lint with `ruff`.
  - Type-check with `mypy`.
  - Verify spellings with `codespell`.
  - Keep cognitive complexity below 20 (`complexipy`).
  - Unit test critical algorithms and math with `pytest`.
- **Headless & GUI Display Awareness**:
  - Matplotlib scripts must handle headless environments gracefully (e.g. use `matplotlib.use("Agg")` when running in headless batch jobs, while allowing interactive display when a windowing system exists).
  - Save all figures at high resolution (300 dpi PNG/PDF) with descriptive names: `{script_idx}p{plot_idx}_{descriptive_name}.png`.

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
- **Meteorological & Oceanographic Tools**:
  - Standard libraries: `xarray`, `netCDF4`, `h5py`, `pandas`, `numpy`, `scipy`, `cartopy`, `cmocean`, `utide`.
  - External APIs: Use robust retry logic and session management when querying APIs like Kartverket Tide API (`tideapi.php`).

---

## 4. Specialized Domain Skills Repository

Curated domain skills and standardized methodologies are maintained in `/home/nilsmk/git/skillcollection/`. Refer to and adopt these specifications for relevant tasks:
- **`roms-ocean-eval`** ([SKILLS_roms_ocean_eval.md](/home/nilsmk/git/skillcollection/SKILLS_roms_ocean_eval.md)): ROMS model verification, CPP options, non-linear tide-surge interaction diagnostics ($\zeta_{\text{int}}$), and `utide` harmonic analysis.
- **`kartverket-tide-api`** ([SKILLS_kartverket_tide_api.md](/home/nilsmk/git/skillcollection/SKILLS_kartverket_tide_api.md)): Norwegian Mapping Authority water level and tide API client specifications, unit conversions (cm to m), and caching.
- **`scientific-latex-report`** ([SKILLS_scientific_latex_report.md](/home/nilsmk/git/skillcollection/SKILLS_scientific_latex_report.md)): Standard scientific LaTeX templates, `booktabs` typography, and two-pass non-interactive PDF build scripts.
- **`grib-netcdf-wrangler`** ([SKILLS_grib_netcdf_wrangler.md](/home/nilsmk/git/skillcollection/SKILLS_grib_netcdf_wrangler.md)): High-performance NetCDF/GRIB handling, Lustre storage locking workarounds, memory-safe hyperslabs, and grid coordinate rotations.
- **`sailing-polar-telemetry`** ([SKILLS_sailing_polar_telemetry.md](/home/nilsmk/git/skillcollection/SKILLS_sailing_polar_telemetry.md)): ORC polar parsing, 2D SciPy polar interpolations, VMG, tacking loss metrics, and Dash-Plotly dashboard conventions.
- **`klarsprak-norsk`** ([SKILLS_klarspraak_norsk.md](/home/nilsmk/git/skillcollection/SKILLS_klarspraak_norsk.md)): Norwegian plain language standards (*Språklova* § 9, ISO 24495-1).
- **`clear-academic-english`** ([SKILLS_clear_academic_english.md](/home/nilsmk/git/skillcollection/SKILLS_clear_academic_english.md)): Academic writing standards, nominalization reduction, and scholarly clarity.
- **`plain-english`** ([SKILLS_plain_english_broad_audience.md](/home/nilsmk/git/skillcollection/SKILLS_plain_english_broad_audience.md)): Broad audience readability and plain language.

---

## 5. Session Resume Checklist

When opening a new session in any project repository, execute this checklist:
1. Read `AGENTS.md` and `README.md`.
2. Read `HISTORY.md` to restore the exact prompt history, parameter states, and execution context.
3. Check and activate the project's mamba environment (`environment.yml`).
4. Inspect the directory tree and test suite (`pytest`) to verify baseline stability before applying changes.
