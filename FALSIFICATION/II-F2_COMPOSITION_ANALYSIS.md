# II-F2 — Composition Analysis

**Status:** 🟢 SURVIVES (interim, SIGNED)
**Date:** 2026-11-21
**Author of analysis:** Samir Baladi (with AI assistance)
**Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date of signature:** 2026-11-21
**Verdict:** SURVIVES (interim, subject to recorded residual risks RR1–RR7)

---

## 0. Scope and Method

This analysis addresses **II-F2 — Non-decomposition**: the claim that the intent verification performed by the Integrity Engine (IE) **does not decompose** into the composition of AetherPolicy, AetherAudit, AetherGovern, and AetherMesh. In other words: is there a remainder that survives after composing all four dimensions?

**Method.** For each test case, we compute:
1. The **IE decision** `IE(T, C, H)` — the reference.
2. The **composition decision** `C_pre(T, C, H) = compose(R_Policy, R_Govern, R_Mesh)` — the candidate (pre-execution only).
3. The **remainder** — the set of inputs where `C_pre ≠ IE`.

**Falsification criterion (from FALSIFICATION_PLAN.md §3.2):**
> II-F2 is falsified if, for every `(T, C)`, the composition produces the same decision as `IE(T, C, H)` — i.e., the remainder is empty (∅).

**Pass criterion:**
> II-F2 survives if there exists at least one `(T, C)` where `C_pre(T, C, H) ≠ IE(T, C, H)` and no valid composition closes the gap.

---

## 1. Composition Semantics

### 1.1 What Each Dimension Contributes to the Composition

| Dimension | Contribution to `C_pre` | Temporal position | Output codomain |
|---|---|---|---|
| **AetherPolicy** | `R_Policy(T, C) → {permit, deny}` via `φ(a, c, s)` | Pre-execution | `{permit, deny}` |
| **AetherGovern** | `R_Govern(T, C) → {authorized, unauthorized}` via R1–R6 | Pre-execution | `{authorized, unauthorized}` |
| **AetherMesh** | `R_Mesh(T, C) → {consistent, inconsistent}` via A6 | Pre-execution | `{consistent, inconsistent}` |
| **AetherAudit** | `R_Audit(E) → {verdict, no-verdict}` | Post-execution | `{verdict, no-verdict}` |

### 1.2 Formal Composition Rule (added per RR5)

The composition `C_pre` maps the three pre-execution outputs into `{ALLOW, DENY}` by the following rule:

```

C_pre(T, C, H) = ALLOW  ⟺  R_Policy(T, C) = permit
∧ R_Govern(T, C) = authorized
∧ R_Mesh(T, C) = consistent
C_pre(T, C, H) = DENY   otherwise

```

**Consequence:** `C_pre` has codomain `{ALLOW, DENY}` **by construction**. It cannot produce `ASK` or `HOLD`, because `ASK` and `HOLD` are not in the codomain of any pre-execution dimension (see §3.1).

### 1.3 Temporal Split

`C` has two phases:
1. **Pre-execution:** `C_pre = compose(R_Policy, R_Govern, R_Mesh) → {ALLOW, DENY}`.
2. **Post-execution:** `R_Audit(E) → {verdict, no-verdict}`.

`IE` produces a **single pre-execution decision** `IE(T, C, H) ∈ {ALLOW, ASK, HOLD, DENY}`.

This temporal split is **central** to II-F2.

---

## 2. Test Cases

### Case 1 — Office chair (consistent case)

| Step | Decision |
|---|---|
| `R_Policy` | permit (within scope, budget, constraints) |
| `R_Govern` | authorized |
| `R_Mesh` | consistent |
| `C_pre` | **ALLOW** |
| `IE(T, C, H)` | **ALLOW** |
| **Same?** | ✓ |

**Analysis:** No remainder. All dimensions agree.

---

### Case 2 — Gaming PC (central case)

| Step | Decision |
|---|---|
| `R_Policy` | permit (within scope=office furniture, budget=2000) |
| `R_Govern` | authorized |
| `R_Mesh` | consistent |
| `C_pre` | **ALLOW** |
| `IE(T, C, H)` | **DENY** (does not contribute to Purpose="equip home office") |
| **Same?** | ✗ |

**Analysis of the remainder:**
- `C_pre` returns ALLOW because none of `R_Policy`, `R_Govern`, `R_Mesh` has access to the **outcome predicate** (Purpose).
- `IE` returns DENY **before** execution.
- **The remainder is the pre-execution decision `DENY`**.

**Open question (RR6):** How does `IE` evaluate the outcome predicate **before** execution, without executing or relying on an unmodeled predictor? This is an architectural open risk, acknowledged below.

---

### Case 3 — Ambiguous purchase (standing desk with gaming features)

| Step | Decision |
|---|---|
| `R_Policy` | permit |
| `R_Govern` | authorized |
| `R_Mesh` | consistent |
| `C_pre` | **ALLOW** |
| `IE(T, C, H)` | **ASK** (uncertain whether Purpose is satisfied) |
| **Same?** | ✗ |

**Analysis of the remainder:**
- `C_pre` returns ALLOW because no dimension has an outcome predicate, and `C_pre`'s codomain is `{ALLOW, DENY}`.
- `IE` returns ASK (request human clarification) **before** execution.
- **The remainder is the pre-execution decision `ASK`**.

**Caveat (RR2):** If `ASK`/`HOLD` are nominal (reducible to `permit with flag`), Case 3 weakens.

---

## 3. Why the Remainder Cannot Be Closed

### 3.1 The Temporal Argument (formal)

By §1.2, `C_pre`'s codomain is `{ALLOW, DENY}`. `ASK` and `HOLD` are not in this codomain.

`IE`'s codomain is `{ALLOW, ASK, HOLD, DENY}`.

**Therefore:** For any `(T, C)` where `IE(T, C, H) ∈ {ASK, HOLD}`, no composition can produce the same decision. The remainder is non-empty for Case 3.

### 3.2 The Outcome-Predicate Argument

Even when `C_pre` produces `DENY`, it does so via a predicate over `(a, c, s)` (via `R_Policy`). But `Purpose` (D1) is an **outcome predicate** — a predicate over the state/effect **after** execution. It is not expressible as `φ(a, c, s)` without collapsing into an attribute predicate (II-F1's R1).

**Therefore:** `C_pre` cannot produce the same `DENY` as `IE` for Case 2. The remainder is non-empty for Case 2.

---

## 4. The Remainder (Formal)

Let `D_IE = IE(T, C, H)` and `D_C = C_pre(T, C, H)`.

**Remainder** `R = {(T, C) : D_IE ≠ D_C and no valid composition closes the gap}`.

From Cases 2 and 3:
- Case 2: `D_IE = DENY`, `D_C = ALLOW` → remainder contains Case 2.
- Case 3: `D_IE = ASK`, `D_C = ALLOW` → remainder contains Case 3.

**Therefore:** `R ≠ ∅`. II-F2 SURVIVES (interim, SIGNED).

---

## 5. What This Analysis Does Not Claim

- That II-F2 SURVIVES **finally**. The verdict is interim.
- That AetherMesh has been examined in full. Only A6 was used (RR1).
- That `ASK`/`HOLD` are fundamentally distinct from Policy's outcomes (RR2).
- That the remainder is "large". Only that it is **non-empty**.
- That the test set is representative. All three cases share the same contract (RR7).

---

## 6. Residual Risks (RR1–RR7)

**RR1 — AetherMesh examination is partial.** Only A6 was used. A full AetherMesh analysis may reveal a predicate relevant to Purpose.

**RR2 — `ASK`/`HOLD` question.** If `ASK`/`HOLD` are nominal (reducible to `permit with flag`), Case 3 weakens.

**RR3 — Temporal argument assumes strict pre/post separation.** If AetherAudit evolves to include pre-execution checks, the argument weakens.

**RR4 — Outcome-predicate argument depends on II-F1's R1.** If Purpose can be reduced to `(a, c, s)`, Case 2 collapses.

**RR5 — Component-to-decision mapping gap (added per review).** The composition rule in §1.2 must be made explicit. It has been added, but the mapping from `{permit, deny} × {authorized, unauthorized} × {consistent, inconsistent}` to `{ALLOW, DENY}` is a design choice that could be contested (e.g., should a "consistent but unauthorized" transaction be ALLOW or DENY?).

**RR6 — Pre-execution observability of post-execution outcomes (added per review).** Case 2 assumes IE denies before execution based on an outcome predicate. How IE evaluates the outcome without executing or relying on an unmodeled predictor is an open architectural risk. This is a residual risk, not a falsification — but it is the most consequential one.

**RR7 — Narrow test set (added per review).** All three cases use the same "equip home office" contract. Broader test cases (multi-agent, cross-dimension) are needed before any non-interim conclusion.

---

## 7. Next Step

1. II-F3 (Decentralizability).
2. Broaden the test set (RR7) in later phases.
3. Address RR6 (pre-execution observability) as a design question, not a falsification question.

---

## 8. Reviewer Confirmation

**Reviewed by:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date:** 2026-11-21
**Status authorization:** SURVIVES (interim, SIGNED) — interim Phase 3 record, subject to the residual risks RR1–RR7 above and to the open dependencies (AetherPolicy and AetherAudit paused at Phase 4; AetherGovern v0.1.0 undetermined).

**Signature:** Jonathan Rabe
**Date:** 2026-11-21

---

**SIGNED (interim)**
**Last updated:** 2026-11-21
