# II-F1 — Non-Redundancy: Result

**Protocol:** Protocol 1 (see README §Falsification Protocols)
**Status:** 🟢 SURVIVES (interim, SIGNED)
**Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date of signature:** 2026-11-21
**Verdict:** SURVIVES (interim, subject to recorded residual risks)

---

## Claim under test

Intent verification is **not already covered** by Policy + Audit + Govern.

## Pass criterion (claim survives)

For at least one `(T, C)` pair, no composition `R_Policy ∘ R_Audit ∘ R_Govern` produces the same DENY decision as `IE(T, C, H)`, unless it presupposes an independent "intent" object.

## Fail criterion (claim falsified)

For **every** `(T, C)`, some composition `R_Policy ∘ R_Audit ∘ R_Govern` produces the same DENY decision as `IE(T, C, H)` — i.e., no independent "intent" is needed.

---

## 1. Definitional clarification (added post-review)

**Purpose is an outcome predicate, not an attribute predicate.**

To avoid circularity (a policy could name "gaming_pc" as an attribute constraint), the following clarification is normative for II-F1:

- A **policy predicate** (AetherPolicy Def 1) is a condition `φ(a, c, s)` over the transaction, its context, and the state *before* execution.
- A **Purpose predicate** (AetherIntent D1) is a condition over the **outcome** of the transaction — the state/effect *after* execution, or the goal-state the delegation intends to achieve.

**Consequence for II-F1:** If a candidate `Purpose` can be rewritten as an attribute predicate `φ(a, c, s)` that any AetherPolicy author could have written, then it is not an outcome predicate and II-F1 does not apply to it. The distinction is maintained only when the Purpose predicate refers to the resulting state, not to the transaction's metadata.

**This clarification is recorded as a Residual Risk (R1) below.**

---

## Test cases

| # | Transaction T | Intent Contract C | Policy verdict | Audit verdict | Govern verdict | Composition verdict | IE verdict | Same? |
|:-:|---|---|---|---|---|---|---|:-:|
| 1 | Office chair purchase within budget and scope, contributes to Purpose | Scope=office furniture; Budget=500; Purpose="equip home office" | ALLOW | N/A (post-hoc) | ALLOW | ALLOW | ALLOW | ✓ |
| 2 | Gaming PC purchase within budget and scope, violates Purpose | Scope=office furniture; Budget=2000; Purpose="equip home office" | ALLOW (within scope/budget) | N/A (post-hoc) | ALLOW | ALLOW | DENY | ✗ |
| 3 | Ambiguous purchase: standing desk with gaming features | Scope=office furniture; Budget=500; Purpose="equip home office" | ALLOW | N/A | ALLOW | ALLOW | ASK | ✗ |

**Reading of the test cases.**

- Case 1: All four dimensions agree; no conflict.
- Case 2 (**central case**): Policy, Audit, and Govern all return ALLOW (the transaction is within scope and budget). IE returns DENY because the transaction does not contribute to the Purpose **as an outcome predicate**. The composition cannot reproduce the DENY decision.
- Case 3 (**ambiguity case**): Policy, Audit, and Govern cannot express "uncertainty"; only IE produces ASK.

**Caveat on Case 2 (from review):** The DENY depends on `Purpose="equip home office"` being an outcome predicate (a goal-state), not an attribute predicate (e.g., "disallow gaming_pc"). If the latter, the case collapses into AetherPolicy. See Residual Risk R1.

---

## Verdict

**SURVIVES (interim, SIGNED).**

**Justification.** Cases 2 and 3 provide evidence that the composition cannot reproduce IE's decisions. AetherGovern v0.1.0 (§3.2, R1–R6) addresses authority, not outcome. AetherPolicy (Def 1: φ over (a,c,s), not outcome) and AetherAudit (§1.2: post-hoc, not pre-execution) also do not cover Purpose. No examined dimension covers Purpose.

**Scope of this verdict:**
- This is an **interim** verdict. It does not close II-F2, II-F3, or II-F4.
- The composition of *all* dimensions (including AetherMesh) is the subject of **II-F2**, not II-F1. II-F1 tests whether each dimension individually covers Purpose.

---

## Residual Risks (recorded explicitly)

**R1 — Predicate-encoding ambiguity (Outcome vs. Attribute/State Predicate).**
A policy rule in AetherPolicy over (a, c, s) could encode a proxy condition for an intended outcome if the outcome is observable in state `s`. II-F1 requires that the Purpose predicate **cannot** be reduced to a static `(a, c, s)` rule. If it can, the II-F1 argument collapses into AetherPolicy.

**R2 — Pre-execution outcome prediction vs. determinism (A2).**
Evaluating an outcome predicate **before** execution may require predicting downstream effects. This creates tension with A2 (Deterministic admission) or requires an external world/evaluator model whose error bounds are not specified. This is a recorded residual risk, not a falsification.

**R3 — Composition vs. single-dimension coverage.**
This file lists individual verdicts for Policy, Audit, and Govern alongside a combined "Composition verdict." The composition of **all** dimensions (including AetherMesh) is the subject of **II-F2**, not II-F1. II-F1's scope is single-dimension non-redundancy.

**R4 — AetherGovern itself is `(E) undetermined`.**
Its scope may evolve. If AetherGovern v0.1.0 is later revised to include an outcome predicate, II-F1 may need revision.

**R5 — `ASK`/`HOLD` vs. AetherPolicy outcomes.**
The Integrity Engine's `{ALLOW, ASK, HOLD, DENY}` has not been tested against AetherPolicy's `{executed, blocked, restricted, detected}`. Whether the distinction is fundamental or nominal remains open.

**R6 — AetherMesh not examined.**
AetherMesh may supply a predicate relevant to Purpose (unlikely, but not verified). This is deferred to II-F2.

---

## Reviewer Confirmation

**Reviewed by:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date:** 2026-11-21
**Status authorization:** SURVIVES (interim, SIGNED) — interim Phase 3 record, subject to the residual risks R1–R6 above.

**Methodological note (2026-11-21):** This analysis relies exclusively on AetherGovern v0.1.0 (the hypothesis paper). It does **not** use AetherGovern v2.0.0 (the reference implementation) or any experimental results, which are out of scope for II-F1.

---

**Signature:** Jonathan Rabe
**Date:** 2026-11-21
