# DISCUSSIONS_LOG.md — Interaction Log

## 2026-11-06 — Update AGENTS.md and domain skills from NorKyst tidal/surge evaluation
**Prompt:** Review AGENTS file in ../agent_instructions and skills in ../skillcollection, and make updates based on work done.
**Actions:**
- Updated `AGENTS.md` with mandatory stand-alone 1-page Executive Summaries, formal Generative AI transparency disclosures (COPE/Elsevier/Nature standards), zero overfull `\hbox` margin standards, and cross-references to expanded skills.
- Synchronized checklists and pre-response quality protocols.
**Outcome:** Global agent instructions updated with publishing and technical reporting standards derived from NorKyst-800m operational evaluation.

## 2026-10-26 — Consolidate master AGENTS.md guidelines
**Prompt:** Merge content from baseline guideline templates and existing project AGENTS.md files across ~/git/ into master AGENTS.md, preserving existing rules and following all operational standards.
**Actions:**
- Analyzed and compared `agent_guidelines/AGENTS.md` against baseline guideline templates and repository-specific AGENTS.md files (`DumbSail`, `routing`, `hindcast_eval`, `oddfjell`, `new_barents`, `roms2way`, `inverse_tides`).
- Identified and reconciled key divergence points (git policy: Nils maintains active version control with `main` branch convention; external templates restrict agent git commands).
- Incorporated all non-contradictory engineering principles, AI reasoning standards, defensive programming conventions, logging and file-naming rules, session checklists, and sub-agent cost controls into `agent_guidelines/AGENTS.md`.
- Documented findings and established this discussion log.
**Outcome:** Master `AGENTS.md` updated with comprehensive guidelines, fully consistent across repositories.
