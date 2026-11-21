# II-F1 — Coverage Analysis

**Status:** 🟢 SURVIVES (interim, SIGNED)
**Date:** 2026-11-21
**Author of analysis:** Samir Baladi (with AI assistance)
**Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date of signature:** 2026-11-21
**Verdict:** SURVIVES (interim, subject to recorded residual risks)

---

## 0. Scope and Method

This analysis addresses **II-F1 — Non-redundancy**: the claim that the intent verification performed by the Integrity Engine (IE) is not already covered by AetherPolicy, AetherAudit, or AetherGovern.

**Method.** For each of the three existing dimensions, we ask: can the dimension express the element of intent validity that is *irreducible* — namely, the **Purpose** predicate defined in D1/D2 of `ARCHITECTURAL_HYPOTHESIS.md`? Evidence is drawn only from the published preprints `AetherPolicy_Research_Paper.pdf`, `AetherAudit_Research_Paper_v1.0.0.pdf`, and `AetherGovern_Research_Paper.pdf` (v0.1.0 only). No functional attribution is made without a textual citation.

**Definitional constraint (from review, added post-hoc).** The Purpose predicate must be an **outcome predicate** (over the resulting state/effect), not an **attribute predicate** (over the transaction or its metadata). If a candidate Purpose can be rewritten as a static `(a, c, s)` rule, then it is not an outcome predicate, and II-F1 does not apply to it. See Residual Risk R1.

---

## 1. What the Irreducible Element Is

From `ARCHITECTURAL_HYPOTHESIS.md` §4:

> **D1 — Purpose (irreducible):** "a declarative statement of the goal-state the delegation is intended to achieve, expressed as a verifiable predicate over the **outcome** of the transaction (not over the transaction itself)."

> **D2:** "T contributes to the **Purpose** of C, as verified by the Integrity Engine against the outcome predicate."

> **D2 (key clause):** "**Intent validity is distinct from policy compliance.** A transaction can be policy-compliant (scope, budget, constraints satisfied) but intent-invalid (does not contribute to Purpose)."

The irreducible element is thus: **a predicate over the outcome of a transaction**, which is:
1. Evaluated **before** the transaction is executed (to produce ALLOW/ASK/HOLD/DENY).
2. Not expressible as a predicate over the transaction itself, its context, or its state.

---

## 2. Coverage by AetherPolicy

### 2.1 Policy Definition (AetherPolicy §3.2, Def 1)

> "A policy is a tuple π = (id, iss, T, φ, e, n), where ... φ is a total, terminating, side-effect-free **condition over (a, c, s)**..."

**Claim [evidence]:** AetherPolicy's condition φ is defined over `(a, c, s)` — action, context, state. It is **not** defined over the outcome of the action.

**Inference [inference]:** A predicate over `outcome` cannot be expressed as a predicate over `(a, c, s)` in AetherPolicy's framework, because the outcome is not part of the tuple π. (See Residual Risk R1 for the proxy-condition caveat.)

### 2.2 Enforcement Outcomes (AetherPolicy §3.2, Def 3)

> "An enforcement mechanism maps an action and a decision to an outcome: executed, blocked, restricted ..., or detected."

**Claim [evidence]:** AetherPolicy's enforcement outcomes are `{executed, blocked, restricted, detected}`. There is no `ASK` or `HOLD` outcome.

**Inference [inference]:** The Integrity Engine's decision set `{ALLOW, ASK, HOLD, DENY}` (from D3) includes two outcomes (`ASK`, `HOLD`) that AetherPolicy does not natively produce. Whether this is a *fundamental* gap or a *naming* difference is not resolved by the preprint. (See Residual Risk R5.)

### 2.3 Conflict Resolution (AetherPolicy §3.2, Def 4)

> "A resolution function ⊕ maps a finite multiset of tuples (e, n, iss) to a decision."

**Claim [evidence]:** AetherPolicy's conflict resolution uses `effect (e)`, `priority (n)`, and `issuer (iss)`. It does not use an outcome predicate.

**Inference [inference]:** A `Purpose` conflict (two policies both permit the transaction but only one contributes to the Purpose) cannot be resolved by ⊕, because ⊕ has no access to the outcome.

### 2.4 Finding for AetherPolicy

| Element of D2 | Expressible in AetherPolicy? | Evidence |
|---|:-:|---|
| T within scope of C | ✓ (target predicate T) | Def 1 |
| T within budget of C | ✓ (part of φ) | Def 1 |
| T satisfies constraints in C | ✓ (part of φ) | Def 1 |
| **T contributes to Purpose of C** | **✗ (no outcome predicate)** | Def 1, Def 3, Def 4 |

**Status for AetherPolicy: NOT COVERED (by argument).**

---

## 3. Coverage by AetherAudit

### 3.1 Temporal Placement (AetherAudit §1.2)

> "Policy versus audit. Policy decides, **before** an action, whether it is allowed. Audit examines, **after** the action, whether the evidence supports a stated answer about what occurred."

**Claim [evidence]:** AetherAudit operates **after** the action. It produces an `audit verdict` on evidence, not a `pre-execution decision`.

**Inference [inference]:** A `Purpose` predicate that must produce a decision **before** execution (ALLOW/ASK/HOLD/DENY) cannot be performed by AetherAudit, because AetherAudit's role is post-hoc examination.

### 3.2 Audit Verdict vs. Enforcement Decision (AetherAudit §4.2, Def 5)

> "An audit verdict is a statement by a verifier that, given an evidence set E, a stated audit question has a stated answer."

**Claim [evidence]:** An audit verdict is a statement *about evidence*, not a decision *on a transaction*.

**Inference [inference]:** The two output types are not substitutable: one is post-hoc and epistemic, the other is pre-execution and action-guiding.

### 3.3 Finding for AetherAudit

| Element of D2 | Expressible in AetherAudit? | Evidence |
|---|:-:|---|
| T within scope of C | ✗ (not a pre-execution check) | §1.2 |
| T within budget of C | ✗ (not a pre-execution check) | §1.2 |
| T satisfies constraints in C | ✗ (not a pre-execution check) | §1.2 |
| **T contributes to Purpose of C** | **✗ (post-hoc, not pre-execution)** | §1.2, §4.2 Def 5 |

**Status for AetherAudit: NOT COVERED (by argument).**

---

## 4. Coverage by AetherGovern (v0.1.0 only)

AetherGovern v0.1.0 §1.3 states its central question as:
> "Is there a body of **authorization, policy, accountability, revocation, and dispute-resolution** requirements...?"

§4.2 defines its scope as:
> "which agents are **authorized** to perform which actions, under which policies..."

Its six requirements (R1–R6, §3.2) are:
- R1: Scoped, attenuating delegation
- R2: Authorized policy change
- R3: Revocation with dependents
- R4: Attribution
- R5: Dispute handling
- R6: Partition tolerance

### 4.1 Finding for AetherGovern (v0.1.0)

**Claim [evidence]:** AetherGovern v0.1.0's requirements (R1–R6) address **authority** (who may act), **revocation** (withdrawal of authority), and **attribution** (chain of responsibility). None addresses an **outcome predicate** (whether a transaction contributes to a delegator's purpose).

**Inference [inference]:** AetherGovern v0.1.0 does not cover `Purpose` as defined in D1/D2. A transaction can be authorized (R1–R6 satisfied) but intent-invalid (does not contribute to Purpose).

**Status for AetherGovern v0.1.0: NOT COVERED (by argument).**

### 4.2 Residual Risk (R4)

AetherGovern v0.1.0 itself is `(E) undetermined` (its §3.2 F1–F5 are not yet closed). This means:
1. The finding "AetherGovern v0.1.0 does not cover Purpose" is conditional on the current specification.
2. If AetherGovern v0.1.0 is later revised to include an outcome predicate, II-F1 may need revision.

---

## 5. [SUPERSEDED BY §8–§9] Interim Verdict (pre-AetherGovern)

**Note.** This section was the working verdict before the AetherGovern v0.1.0 examination. It is kept here for traceability. The authoritative verdict is §9.

**Interim verdict (superseded):** INCONCLUSIVE (interim) — pending AetherGovern examination.

---

## 6. What This Analysis Does Not Claim

- That AetherIntent is an independent dimension.
- That Purpose is *uniquely* expressible by AetherIntent (only that it is *not* expressible by the three examined dimensions, on the evidence available).
- That the open questions in §2.2 and §3.2 are resolved.
- That `ASK` and `HOLD` are fundamentally distinct from AetherPolicy's outcomes (this is recorded as open).

---

## 7. Next Step

1. II-F2 (Non-decomposition) is the next failure condition. It tests whether the composition of *all* dimensions (including AetherMesh) can produce the same DENY decision as IE.
2. II-F3 (Decentralizability) and II-F4 (Observable Benefit) follow.
3. The final decision (Phase 4) is deferred until all four conditions are examined.

---

## 8. AetherGovern Examination (v0.1.0 only)

**Methodological constraint:** This section relies exclusively on AetherGovern v0.1.0 (the hypothesis paper). It does **not** use AetherGovern v2.0.0 (the reference implementation) or any experimental results.

See §4 above for the full analysis.

---

## 9. Final Verdict (Revised, Signed)

**Verdict: SURVIVES (interim, SIGNED).**

**Summary of coverage findings:**

| Dimension | Covers Purpose? | Evidence (version) | Status |
|---|:-:|---|---|
| AetherPolicy | ❌ | Def 1 (φ over (a,c,s), not outcome) | NOT COVERED |
| AetherAudit | ❌ | §1.2 (post-hoc, not pre-execution) | NOT COVERED |
| AetherGovern | ❌ | §3.2 R1–R6 (authority predicates) | NOT COVERED (v0.1.0 only) |

**Residual risks (recorded explicitly):**

**R1 — Predicate-encoding ambiguity (Outcome vs. Attribute/State Predicate).**
A policy rule in AetherPolicy over (a, c, s) could encode a proxy condition for an intended outcome if the outcome is observable in state `s`. II-F1 requires that the Purpose predicate **cannot** be reduced to a static `(a, c, s)` rule. If it can, the II-F1 argument collapses into AetherPolicy.

**R2 — Pre-execution outcome prediction vs. determinism (A2).**
Evaluating an outcome predicate **before** execution may require predicting downstream effects. This creates tension with A2 (Deterministic admission) or requires an external world/evaluator model whose error bounds are not specified.

**R3 — Composition vs. single-dimension coverage.**
This analysis evaluates each dimension individually. The composition of **all** dimensions (including AetherMesh) is the subject of **II-F2**, not II-F1.

**R4 — AetherGovern itself is `(E) undetermined`.**
Its scope may evolve. If AetherGovern v0.1.0 is later revised to include an outcome predicate, II-F1 may need revision.

**R5 — `ASK`/`HOLD` vs. AetherPolicy outcomes.**
The Integrity Engine's `{ALLOW, ASK, HOLD, DENY}` has not been tested against AetherPolicy's `{executed, blocked, restricted, detected}`. Whether the distinction is fundamental or nominal remains open.

**R6 — AetherMesh not examined.**
AetherMesh may supply a predicate relevant to Purpose (unlikely, but not verified). This is deferred to II-F2.

**Conclusion:** On the evidence currently available, no examined dimension covers Purpose. II-F1 SURVIVES (interim, SIGNED). A final verdict requires the residuals above to be resolved.

---

## 10. Reviewer Confirmation

**Reviewed by:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date:** 2026-11-21
**Status authorization:** SURVIVES (interim, SIGNED) — interim Phase 3 record, subject to the residual risks R1–R6 above.

**Signature:** Jonathan Rabe
**Date:** 2026-11-21

---

**DRAFT — Now SIGNED (interim)**
**Last updated:** 2026-11-21
