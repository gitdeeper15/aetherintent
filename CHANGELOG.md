# Changelog

All notable changes to AetherIntent are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] — 2026-11-21

### Added
- Architectural hypothesis (`ARCHITECTURAL_HYPOTHESIS.md`), with:
  - Central question and four clauses (II-F1 to II-F4)
  - Five candidate responsibilities (M1–M5)
  - Boundaries with the twelve existing dimensions
  - Structural dependency on AetherPolicy and AetherAudit (declared explicitly)
  - **Refinements (post-review):** T1/T2 labels corrected to Proof-1/Proof-2; D1/D2 sharpened with an irreducible "Purpose" predicate.
- Research paper (`paper/AetherIntent_Research_Paper_v1.0.0.pdf`) — TBD.
- Repository documentation:
  - `README.md` with full project overview
  - `CITATION.cff` with DOI and identifiers
  - `manifest.json` and `scientific_state.json`
  - `ROADMAP.md` with Phase 3 status
  - `INVENTORY.md` (complete file inventory)
  - `PROJECT_SUMMARY.txt` (one-page summary)
  - `AUTHORS.md` and `CONTRIBUTING.md`
- DOI: 10.5281/zenodo.24417251

### Phase 2 — Independent Review (Completed)
- **Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation).
- **Date:** 2026-11-21.
- **Outcome:** Accepted the interim classification (E) Undetermined (interim). Two refinements were requested and addressed: (1) clarify T1/T2 labels in A1–A6, (2) sharpen D1/D2 definitions.
- **Record:** `REVIEW/INDEPENDENT_REVIEW.md` (signed).

### Phase 3 — Falsification Execution (Completed)
- **II-F1 (Non-Redundancy):** SURVIVES (interim, SIGNED — Jonathan Rabe, 2026-11-21). Residual risks R1–R6 recorded. `FALSIFICATION/II-F1_RESULT.md`, `FALSIFICATION/II-F1_COVERAGE_ANALYSIS.md`, `REVIEW/II-F1_SIGNED.md`.
- **II-F2 (Non-Decomposition):** SURVIVES (interim, SIGNED — Jonathan Rabe, 2026-11-21). Residual risks RR1–RR7 recorded. `FALSIFICATION/II-F2_RESULT.md`, `FALSIFICATION/II-F2_COMPOSITION_ANALYSIS.md`, `REVIEW/II-F2_SIGNED.md`.
- **II-F3 (Decentralizability):** SURVIVES (interim, SIGNED — Jonathan Rabe, 2026-11-21). Residual risks RR1–RR8 recorded. `FALSIFICATION/II-F3_RESULT.md`, `FALSIFICATION/II-F3_DECENTRALIZATION_ANALYSIS.md`, `REVIEW/II-F3_SIGNED.md`.
- **II-F4 (Observable Benefit):** NOT EXECUTABLE UNDER CURRENT PHASE CONSTRAINTS. Status record reviewed by Jonathan Rabe (2026-11-21). `FALSIFICATION/II-F4_STATUS.md`, `REVIEW/II-F4_REVIEWED.md`.

### Decision
- Phase 4: **(E) Undetermined (interim)**.
- II-F1–II-F3 SURVIVE (interim, signed) on argument only. II-F4 is not executable under current constraints. AetherPolicy and AetherAudit remain paused at Phase 4.

### Structural Dependency
- AetherIntent is structurally dependent on the resolution of:
  - **AetherPolicy** (Phase 4: undetermined; paused)
  - **AetherAudit** (Phase 4: undetermined; paused)
- Any conclusion about AetherIntent is conditional on how those two investigations resolve.

### Not included
- No implementation
- No tests
- No experiments
- No specification

---

**AetherIntent v1.0.0**
© 2026 Samir Baladi — Ronin Institute / Rite of Renaissance
DOI: [10.5281/zenodo.24417251](https://doi.org/10.5281/zenodo.24417251)
