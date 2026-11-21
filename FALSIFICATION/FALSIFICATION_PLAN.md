# AetherIntent — Falsification Plan

**Status:** Phase 3 — Falsification Design (Initial Draft)
**Version:** 1.0.0
**Date:** 2026-11-21
**Author:** Samir Baladi
**ORCID:** [0009-0003-8903-0029](https://orcid.org/0009-0003-8903-0029)
**License:** CC-BY-4.0
**Position in the Aether family:** Thirteenth project (candidate)
**Decision:** (E) Undetermined (interim)

---

## 0. Status of this document

This document specifies **how** the four failure conditions (II-F1 to II-F4)
would be tested. It is a **Phase 3 deliverable**.

It does **not**:
- run any test;
- report any result;
- claim that any condition has been examined;
- authorize implementation or experimentation.

**All four failure conditions remain untested.** This document only describes
the procedure by which they could be tested, and the decision rule that would
apply once they are.

---

## 1. Principles

### 1.1 State failure before success

Each condition is stated as a **failure condition**: a result that would
count against AetherIntent. The investigation does not look for confirmation;
it looks for the conditions under which the candidate fails.

### 1.2 One condition, one question

Each failure condition answers exactly one clause of the central question
(Section 2 of the hypothesis). A condition that answers more than one clause
is poorly stated and is revised.

### 1.3 No condition is tested by argument alone

A failure condition is **tested** only when there is a procedure, a result,
and a record. Reading the literature is not a test.

### 1.4 A negative result is a result

If any failure condition is falsified — that is, if the candidate fails it —
the candidate is rejected as a separate dimension, and the content may survive
as a feature of another dimension. This is a legitimate outcome, not a failure
of the investigation.

---

## 2. The four failure conditions

The four conditions are restated from the hypothesis (Section 6):

| ID | Clause of the central question | Condition under test | Current status |
|:-:|---|---|:-:|
| **II-F1** | (i) Non-redundancy | The intent verification is not already covered by AetherPolicy, AetherAudit, or AetherGovern. | 🔴 Untested |
| **II-F2** | (ii) Non-decomposition | The intent verification does not decompose into Policy + Audit + Govern; a remainder survives. | 🔴 Untested |
| **II-F3** | (iii) Decentralizability | No central intent authority is required. | 🔴 Untested |
| **II-F4** | (iv) Observable benefit | A benefit can be observed under a defined evaluation. | 🔴 Untested |

The **decision rule**: a positive answer requires all four to be tested with
none falsified, with II-F4 supported by measured results. A negative answer on
any one is sufficient to reject the candidate as a separate dimension.

---

## 3. How each condition would be examined

### 3.1 II-F1 — Non-redundancy

**Question.** Is the intent verification (M1–M5) already covered by
AetherPolicy, AetherAudit, or AetherGovern?

**Artifact.** A responsibility-by-responsibility coverage analysis: for each
of M1–M5, a record of which existing dimension (if any) provides the
responsibility, with a citation to the relevant Aether paper.

**Falsification.** II-F1 is falsified if, for every responsibility M1–M5, an
existing dimension is shown to provide the responsibility **without an
additional layer**.

**What remains open.** Even if II-F1 holds (no single dimension covers every
responsibility), the intent verification may still decompose — which is II-F2.

**Dependency.** This analysis depends on the resolution of AetherPolicy and
AetherAudit (T3).

### 3.2 II-F2 — Non-decomposition

**Question.** Does the intent verification decompose into Policy + Audit +
Govern?

**Artifact.** A composition argument: a construction that combines the three
dimensions and shows whether a remainder survives.

**Falsification.** II-F2 is falsified if the composition covers M1–M5 with
**no remainder** — that is, if every responsibility is provided by the
composition.

**What remains open.** If a remainder survives, it must be characterized:
is it a new responsibility, or is it an interaction effect that belongs to the
composition?

**Dependency.** This is the most fragile condition (threat T3).

### 3.3 II-F3 — Decentralizability

**Question.** Does intent verification require a central intent authority?

**Artifact.** A design argument or a construction showing whether the
responsibilities M1–M5 can be discharged without a central authority, relying
on AetherMesh for distribution.

**Falsification.** II-F3 is falsified if at least one of M1–M5 **requires** a
central authority (for example, a global clock, a single index, or a central
arbiter of intent).

**What remains open.** If II-F3 holds, the result does not show that
decentralization is efficient — only that no central authority is required.

### 3.4 II-F4 — Observable benefit

**Question.** Can a benefit be observed under a defined evaluation?

**Artifact.** A defined evaluation with scenarios (e.g., ES1–ES5), metrics
(e.g., EM1 and onward), and a mapping to the failure conditions. The
evaluation is **proposed, not executed**.

**Falsification.** II-F4 is falsified if, under the defined evaluation, no
measurable benefit is observed over the existing dimensions.

**What remains open.** II-F4 is the only condition that requires measurement.
Without an experiment, II-F4 cannot be tested. Phase 5 and Phase 6 are not
permitted until Phase 4 is positive.

---

## 4. Decision rule and outcomes

| Outcome | Condition | Consequence |
|:-:|---|---|
| **(A) Accepted** | All four tested, none falsified, II-F4 supported by measured results. | AetherIntent proceeds to Phase 5 (specification). |
| **(R) Rejected** | At least one condition falsified. | The candidate is rejected as a separate dimension. |
| **(F) Folded** | The remainder is shown to belong to another dimension. | The candidate is folded; no separate dimension. |
| **(D) Deferred** | A condition cannot be tested because a dependency is unresolved. | The candidate remains at Phase 4 (interim). |
| **(E) Undetermined** | Evidence is insufficient to accept and insufficient to reject. | **Current state.** Phase 0. |

The current classification is **(E) Undetermined (interim)**. No condition has
been tested.

---

## 5. What would change the classification

To move from (E) to another outcome, one of the following is needed:

1. **II-F1 analysis** — a responsibility-by-responsibility coverage analysis.
2. **II-F2 analysis** — a composition argument.
3. **II-F3 analysis** — a decentralization argument.
4. **II-F4 measurement** — requires Phase 5 and Phase 6, which are not permitted.
5. **Resolution of AetherPolicy or AetherAudit** — the structural dependency (T3).

Without one of these, the project remains at Phase 4 (interim), classified
(E) Undetermined.

---

## 6. What this plan does not do

- It does not test any condition.
- It does not report any result.
- It does not claim that any condition is likely to hold or fail.
- It does not authorize implementation, experimentation, or measurement.
- It does not resolve the structural dependency on AetherPolicy and AetherAudit.

---

## 7. Next step

Phase 4 — Independence Decision. The decision record
(`DECISIONS/INDEPENDENCE_DECISION.md`) restates the current classification
(E) Undetermined (interim), names the evidence that is missing, and states
that no condition has been tested.

---

**AetherIntent v1.0.0 — Falsification Plan**
© 2026 Samir Baladi — Ronin Institute / Rite of Renaissance
DOI: 10.5281/zenodo.24417251

*Phase 3 deliverable. The plan describes how to test; it does not test.*
