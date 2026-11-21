# II-F4 — Observable Benefit: Status

**Protocol:** Protocol 4 (see README §Falsification Protocols)
**Status:** ⚫ NOT EXECUTABLE UNDER CURRENT PHASE CONSTRAINTS
**Record type:** Status record (not a test result or verdict)
**Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date of review:** 2026-11-21
**Classification:** NOT EXECUTABLE (interim Phase 3 record)

---

## 1. Scope of this Record

This document is a **status record**, not a test result. It records whether II-F4 (Observable Benefit) can be executed under the current phase constraints. It does **not** record a verdict of SURVIVES or FALSIFIED, and it does **not** claim that AetherIntent is or is not independent.

Three distinct things are kept separate here:
1. **Test result** (SURVIVES / FALSIFIED): not applicable — the test has not been run.
2. **Executability status** (NOT EXECUTABLE UNDER CURRENT CONSTRAINTS / EXECUTABLE): the subject of this record.
3. **Overall classification of AetherIntent's independence** (Phase 4 decision (E)): unchanged by this record.

---

## 2. Claim under test

A benefit can be observed from the use of AetherIntent (specifically, from the Integrity Engine) under a defined evaluation, compared to systems without it.

---

## 3. What II-F4 Requires (from FALSIFICATION_PLAN.md §3.4)

II-F4 requires a defined evaluation with four components:

1. **Scenarios**: concrete cases where the benefit would be observable.
2. **Metrics**: the evaluation metrics defined in `README.md` under "Evaluation Design" — EM1–EM5:
   - **EM1:** intent-violating transactions prevented
   - **EM2:** false positives (intent-valid transactions blocked)
   - **EM3:** decision determinism
   - **EM4:** latency overhead
   - **EM5:** auditability (whether a decision can be reconstructed and verified)
3. **Baselines**: systems without AetherIntent, for comparison.
4. **Mapping to the failure condition**: how the measurement would falsify the claim.

**Important distinction (per review):** Components 1–4 can be **designed** without an implementation. What cannot be done under the current constraints is the **execution** of the measurement itself. II-F4's status reflects the latter, not the former.

**Note on metric alignment (per review):** The metrics EM1–EM5 above are the canonical names used in `README.md` (Evaluation Design). Earlier drafts of this record referred to them informally; this version aligns with the README.

---

## 4. Why II-F4 Cannot Be Executed Now

### 4.1 Phase Constraints

`scientific_state.json` and `ARCHITECTURAL_HYPOTHESIS.md` both record:
- `implementation`: `none`
- `experiments`: `none`

Per `FALSIFICATION_PLAN.md` §6, the current plan is not a delegation to implement, experiment, or measure.

### 4.2 What Is Permitted vs. Not Permitted

| Activity | Permitted under current constraints? |
|---|---|
| Designing scenarios, metrics, baselines (Components 1–4) | ✅ Yes |
| Executing an experiment to measure benefit | ❌ No |
| Implementing IE in a reference system | ❌ No |
| Running a controlled evaluation | ❌ No |

### 4.3 Consequence

II-F4 cannot be **executed** under the current constraints. This is a statement about executability, not about the claim itself.

---

## 5. Why This Is Not Falsification and Not Survival

**II-F4 is not FALSIFIED.** Falsification would require showing that no benefit exists. No such showing has been made.

**II-F4 is not SURVIVES.** Survival would require showing that a benefit is observed. No such observation has been made.

**Therefore:** II-F4 is `NOT EXECUTABLE UNDER CURRENT PHASE CONSTRAINTS` — a status that acknowledges the test cannot be run within the current research delegation.

---

## 6. What Would Make II-F4 Executable

II-F4 would become executable after:
1. The phase authorization required for implementation and evaluation is granted.
2. The conditions for execution and measurement are in place (a reference implementation, a controlled environment, a pre-registered protocol).
3. The evaluation design (Components 1–4) is finalized and approved as pre-registered.

**This record does not assert that these steps must occur in Phase 5 or Phase 6 specifically.** It records only that they have not occurred, and that II-F4 is therefore not executable now.

---

## 7. Consequence for the Phase 4 Decision

The Phase 4 decision **(E) Undetermined (interim)** is **not changed** by II-F4's status.

Per `INDEPENDENCE_DECISION.md` §5, the classification can change when:
- II-F1, II-F2, or II-F3 is falsified (rejecting AetherIntent as a separate dimension), OR
- II-F4 becomes executable and shows observable benefit (accepting AetherIntent), OR
- The structural dependency on AetherPolicy / AetherAudit is resolved (changing the evidence base on which (E) rests).

**Important distinction (per review):** Resolving AetherPolicy or AetherAudit is a **prerequisite/input** for settling the structural dependency. It does **not** automatically resolve II-F4. The II-F4 measurement path remains separate: it requires its own execution and measurement, independent of AetherPolicy/AetherAudit resolution.

**This record does not narrow the transition paths listed in §5 of the decision.** It records only that II-F4 is not currently executable, and therefore that one of the paths (the II-F4 measurement path) is not available at this time.

---

## 8. Residual Risks

**RR1 — No measurement.** The absence of measurement is not evidence for or against AetherIntent. It is a statement about the current phase constraints.

**RR2 — Simulation is not a substitute.** A simulation could be designed and built, but it would not, by itself, satisfy II-F4's requirement for an observable benefit. A simulation is a design exercise, not an empirical test.

**RR3 — Future executability.** II-F4 may become executable after the phase authorization required for implementation and evaluation is granted. This status is interim.

**RR4 — Design vs. execution.** Components 1–4 (scenarios, metrics, baselines, mapping) can be designed now. This record should not be read as claiming they cannot. It claims only that the **execution** of the measurement cannot be done under current constraints.

**RR5 — Metric alignment.** The metrics EM1–EM5 referenced in §3 must remain aligned with the README's Evaluation Design section. If the README is updated, this record should be updated accordingly.

---

## 9. Reviewer Confirmation (Status Record Review, Not a Verdict)

**Reviewed by:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date:** 2026-11-21
**Record type reviewed:** II-F4 Status Record (NOT EXECUTABLE UNDER CURRENT PHASE CONSTRAINTS) — not a verdict signature.

**Reviewer statement:**
> "I confirm that I have reviewed II-F4_STATUS.md as a status record (not a test result or verdict), and that its classification as NOT EXECUTABLE UNDER CURRENT PHASE CONSTRAINTS accurately reflects the current non-execution state and leaves the overall Phase 4 classification at (E) Undetermined (interim)."

**Signature:** Jonathan Rabe
**Date:** 2026-11-21

---

## 10. Next Step

1. Update `DECISIONS/INDEPENDENCE_DECISION.md` to reflect this status record (see Section 7).
2. Update `scientific_state.json` only after verifying that the schema supports the chosen value.
3. II-F4 remains NOT EXECUTABLE until the phase authorization required for implementation and evaluation is granted.

---

**REVIEWED (Status Record) — NOT A VERDICT**
**Last updated:** 2026-11-21
