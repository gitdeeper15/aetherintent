# AetherIntent — Architectural Investigation Report

**Record ID:** AETHERINTENT-01
**Version:** 1.0.0
**Date:** 2026-11-21
**Author:** Samir Baladi
**ORCID:** [0009-0003-8903-0029](https://orcid.org/0009-0003-8903-0029)
**License:** CC-BY-4.0
**Position in the Aether family:** Thirteenth project (candidate)
**DOI:** 10.5281/zenodo.24417251

---

## 0. Summary

This report summarizes the current state of the AetherIntent architectural
investigation. It is a **status report**, not a result.

**Current state:** Phase 0 (Architectural Hypothesis) in progress.
**Decision:** (E) Undetermined (interim).
**All four failure conditions (II-F1 to II-F4):** untested.

---

## 1. The question

> Can an autonomous agent execute a financially valid transaction that
> violates the human's delegated intent, and can an independent deterministic
> enforcement layer prevent that execution?

The question has four clauses, each mapped to a failure condition:

| Clause | Condition |
|:-:|---|
| (i) Not already covered by Policy / Audit / Govern | II-F1 |
| (ii) Does not decompose into Policy + Audit + Govern | II-F2 |
| (iii) No central intent authority required | II-F3 |
| (iv) Observable benefit | II-F4 |

A positive answer requires all four. A negative answer on any one is
sufficient to reject the candidate as a separate dimension.

---

## 2. The hypothesis

A potentially independent architectural gap may exist between policy
enforcement, audit, and governance: the verification of **intent validity** —
the consistency of a transaction with the human's delegated intent — as
distinct from **financial validity** (the transaction is technically
possible).

The hypothesis is falsifiable. It fails if:

- AetherPolicy already provides all intent verification requirements without
  an additional layer;
- AetherPolicy plus AetherAudit plus AetherGovern cover the responsibilities
  with bounded integration;
- AetherGovern already expresses the intent rules as policies;
- AetherAudit already records all decision evidence;
- a central intent authority is required (II-F3 fails);
- no observable benefit is demonstrated (II-F4 fails).

---

## 3. The five candidate responsibilities (initial scope)

| ID | Responsibility | Statement (hypothesis) |
|:-:|---|---|
| **M1** | Intent Interpretation | Translating natural-language delegation into a structured intent contract. |
| **M2** | Intent Enforcement | Deterministically verifying that a transaction is consistent with the intent contract. |
| **M3** | Behavioural Monitoring | Detecting deviations from normal agent behaviour. |
| **M4** | Decision Semantics | Classifying decisions as ALLOW, ASK, HOLD, or DENY. |
| **M5** | Integrity Ledger | Recording every proposal, decision, and execution event. |

None is confirmed as an independent architectural responsibility.

---

## 4. Phase status

| Phase | Description | Status |
|:-:|---|---|
| 0 | Architectural Hypothesis | ✅ Complete (`ARCHITECTURAL_HYPOTHESIS.md`) |
| 1 | Responsibility Mapping | ✅ Complete (`research/RESPONSIBILITY_BOUNDARIES.md`) |
| 2 | Hypothesis Review (Independent) | ⏳ Protocol complete; review not started |
| 3 | Falsification Design | ✅ Complete (`FALSIFICATION/FALSIFICATION_PLAN.md`) |
| 4 | Independence Decision | ⚠️ Interim: (E) Undetermined (`DECISIONS/INDEPENDENCE_DECISION.md`) |
| 5 | Architectural Specification | ❌ Not permitted |
| 6 | Reference Implementation | ❌ Not permitted |

**The investigation has not passed Phase 4.** The Phase 4 decision is
interim. It cannot be final until Phase 2 (independent review) is completed.

---

## 5. Failure condition status

| ID | Claim under test | Status |
|:-:|---|:-:|
| II-F1 | Non-redundancy | 🔴 Untested |
| II-F2 | Distinction from Policy + Audit + Govern | 🔴 Untested |
| II-F3 | Decentralizability | 🔴 Untested |
| II-F4 | Observable benefit | 🔴 Untested |

"Untested" is the default state at Phase 0. No attempt has been made to
falsify these conditions.

---

## 6. Threats to validity

| # | Threat | Bearing |
|:-:|---|---|
| T1 | Author-on-author reasoning | Hypothesis and decision by the same author. |
| T2 | No independent review | Phase 2 not started. |
| T3 | Structural dependency on AetherPolicy and AetherAudit | Both paused at Phase 4. |
| T4 | LLM interpretability | The LLM may misinterpret the delegation. |
| T5 | Scope of intent validity | May be semantic, not architectural. |
| T6 | No measurement | II-F4 untested. |
| T7 | Related work may already cover the gap | Comparison with the field is open. |

---

## 7. Decision

**Classification: (E) Undetermined (interim).**

### 7.1 Why the evidence is insufficient to accept

- All four failure conditions are untested.
- No implementation or experiment has been run.
- Structural dependency on AetherPolicy and AetherAudit (both unresolved).
- AetherIntent is a candidate, not a confirmed dimension.

### 7.2 Why the evidence is insufficient to reject

- No failure condition has been falsified.
- The intent integrity question is coherent.
- Related work exists but does not settle the question.
- The boundary map is drawable.

### 7.3 What is missing

1. Independent review (Phase 2).
2. Execution of II-F1, II-F2, II-F3 analyses.
3. Measurement for II-F4 (requires Phase 5 and Phase 6).
4. Resolution of AetherPolicy and AetherAudit.

---

## 8. What the investigation has established

It has established only that:

1. A coherent central question can be stated.
2. A five-responsibility hypothesis can be stated and falsified.
3. A boundary map can be drawn against the twelve other projects.
4. A falsification plan can be designed.

It has **not** established that AetherIntent is an independent dimension,
that the gap is genuine, that any responsibility is independent, or that any
benefit exists.

---

## 9. What the investigation has not done

- No implementation.
- No test.
- No experiment.
- No measurement.
- No independent review.
- No final decision.
- No specification.

No `src/`, no `tests/`, no `experiments/`.

---

## 10. Next steps

In order:

1. **Phase 2** — assign an independent reviewer; complete
   `REVIEW/INDEPENDENT_REVIEW.md`.
2. **Phase 3 execution** — carry out the II-F1, II-F2, and II-F3 analyses.
3. **Phase 4 final** — revise the decision from interim to final once Phase 2
   is complete.
4. **Phase 5** — only if Phase 4 is positive.
5. **Phase 6** — only after Phase 5.

Stopping after Phase 4 is a legitimate outcome. So is a negative Phase 4
decision.

---

## 11. Companion artifacts

| Artifact | Status |
|---|---|
| `README.md` | ✅ Complete |
| `ARCHITECTURAL_HYPOTHESIS.md` | ✅ Complete |
| `research/RESPONSIBILITY_BOUNDARIES.md` | ✅ Complete |
| `FALSIFICATION/FALSIFICATION_PLAN.md` | ✅ Complete |
| `DECISIONS/INDEPENDENCE_DECISION.md` | ✅ Complete (interim) |
| `REVIEW/README.md` | ✅ Complete |
| `REVIEW/INTERNAL_REVIEW.md` | ⏳ Placeholder |
| `REVIEW/INDEPENDENT_REVIEW.md` | ⏳ Placeholder |
| `REVIEW/INDEPENDENT_REVIEW_PROTOCOL.md` | ✅ Complete |

---

**AetherIntent v1.0.0 — Architectural Investigation Report**
© 2026 Samir Baladi — Ronin Institute / Rite of Renaissance
DOI: 10.5281/zenodo.24417251

*AetherIntent is a candidate, not a result.*
