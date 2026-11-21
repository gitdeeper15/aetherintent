# II-F3 — Decentralization Analysis

**Status:** 🟢 SURVIVES (interim, SIGNED)
**Date:** 2026-11-21
**Author of analysis:** Samir Baladi (with AI assistance)
**Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date of signature:** 2026-11-21
**Verdict:** SURVIVES (interim, subject to recorded residual risks RR1–RR8)

---

## 0. Scope and Method

This analysis addresses **II-F3 — Decentralizability**: the claim that the Integrity Engine (IE) can be specified and executed **without a central authority**, and without breaking the determinism that the Aether family requires (AetherMesh A6).

**Method.** For the claim to survive, we must show:
1. `IE(T, C, H)` is a **deterministic function** (given in D3).
2. Any node with `(T, C, H)` can compute `IE(T, C, H)` **independently**.
3. No central authority is needed to **evaluate** `IE`, only to **distribute** `C` and `H` (which are supplied by other dimensions).

**Falsification criterion (from FALSIFICATION_PLAN.md §3.3):**
> II-F3 is falsified if any design for IE requires a central authority for its **core evaluation**.

**Pass criterion:**
> II-F3 survives if IE can be evaluated locally, deterministically, without a central authority, and the remaining distribution challenges are engineering problems (not architectural requirements for centralization).

---

## 1. IE as a Decentralized Function

### 1.1 The Definition (from D3)

> **D3 — Integrity Engine.** "The Integrity Engine is a deterministic function:
> `Decision = IE(T, C, H)`
> Where: T is the proposed transaction; C is the intent contract; H is the behavioral history (**optional**, for anomaly detection). Decision ∈ {ALLOW, ASK, HOLD, DENY}. The Integrity Engine does not use an LLM. It uses hard rules (deterministic predicates) to enforce intent validity."

**Canonical treatment of H (added per review):** When `H` is absent, we define `H = ∅` (the empty history). When `H` is partial, we define `IE`'s handling of `H` deterministically: it evaluates the anomaly-detection predicates against the **available** portion of `H`, and records the partiality as an input to the decision (see §3.3). This preserves determinism: two nodes with the same `(T, C, H = ∅)` get the same result; two nodes with the same `(T, C, H)` get the same result.

**Key properties:**
- `IE` is **deterministic** (same inputs → same output).
- `IE` is **local** (no external calls, no LLM).
- `IE` has a **small input space** `(T, C, H)`.

### 1.2 What "Decentralized Evaluation" Means

For `IE` to be decentralized:

> For any node `n` that possesses `(T, C, H)`, `n` can compute `IE(T, C, H)` **without contacting any central server**.

This is satisfied by D3 because:
- `IE` is a deterministic function of its inputs.
- No step in `IE` requires a central authority (no external lookup, no LLM call, no network round-trip).

**Claim:** IE's **evaluation** is decentralized by construction.

---

## 2. What IE Needs From Other Dimensions

IE does not produce `C` or `H`; it consumes them.

| Input | Source | Distribution mechanism |
|---|---|---|
| `T` (transaction) | The agent proposing it | Direct (local) |
| `C` (Intent Contract) | The delegator (human) | Distributed via AetherMesh / AetherGovern |
| `H` (Behavioral History) | The agent's prior actions | Distributed via AetherAudit / AetherMesh |

**Key observation:** The distribution of `C` and `H` is **not IE's responsibility**. IE assumes they are available. This is how AetherPolicy treats its inputs (`auth(i, T, K)`, `obs(K)`) — they come from AetherGovern and AetherRuntime, respectively.

**Therefore:** IE's decentralization depends on:
1. Whether `C` can be distributed without a central authority (AetherMesh handles this).
2. Whether `H` can be distributed without a central authority (AetherAudit/AetherMesh handle this).
3. Whether IE's **evaluation** can be done locally (D3 confirms: yes).

---

## 3. Challenges to Decentralization

### 3.1 Challenge 1 — Synchronization of `C`

**Scenario:** Node `n1` has `C` version 1. Node `n2` has `C` version 2. Both propose the same `T`. They compute different `IE(T, C, H)` results.

**Analysis:**
- This is a **synchronization challenge**, not an evaluation challenge.
- AetherMesh already handles synchronization of `C` via CRDTs and gossip.
- AetherGovern (R6: Partition tolerance) handles disconnected nodes.
- IE **inherits** the synchronization properties of AetherMesh and AetherGovern.

**Conclusion:** This is not a falsification of II-F3. It is a dependency on AetherMesh/AetherGovern, which is exactly what II-F3 expects.

### 3.2 Challenge 2 — Completeness of `H`

**Scenario:** Node `n1` has full `H`. Node `n2` has partial `H` (e.g., a partition). They compute different `IE(T, C, H)` results.

**Analysis:**
- This is a **data availability challenge**, not an evaluation challenge.
- AetherAudit already handles evidence distribution and reconstruction (Fn1–Fn5).
- AetherMesh handles partition tolerance (A6).
- IE **inherits** the availability properties of AetherAudit and AetherMesh.

**Conclusion:** This is not a falsification of II-F3. It is a dependency on AetherAudit/AetherMesh.

### 3.3 Challenge 3 — Conflicting `C` versions

**Scenario:** `n1` has `C1` (revoked), `n2` has `C2` (new). Both compute IE for `T`.

**Analysis:**
- This is a **policy lifecycle challenge**, which AetherPolicy and AetherGovern already address (R3: revocation with dependents; AetherPolicy Def 1: status as function of K).
- IE **inherits** the lifecycle properties of AetherPolicy and AetherGovern.

**Conclusion:** This is not a falsification of II-F3. It is a dependency on AetherPolicy/AetherGovern.

### 3.4 Divergent Local Decisions (added per RR6)

Even with deterministic `IE`, nodes with divergent `C` or partial `H` can return **different decisions** on the same `T` until synchronization completes.

**Design question (RR6):** How are conflicting local outcomes reconciled? Two strategies:

1. **Fail-closed (conservative):** If `C` or `H` is stale or partial, return `HOLD` or `DENY`. This is safer but may over-block.
2. **Optimistic:** Allow the transaction, and reconcile post-hoc via AetherAudit. This is faster but may allow violations.

**Current AetherIntent position:** D3 does not specify which strategy. This is an **open design question**, recorded as RR6. The choice may depend on the domain (financial transactions favor fail-closed; low-stakes actions favor optimistic).

### 3.5 Anomaly/History Semantics (added per RR7)

D3 treats `H` as optional. §3.2 treats completeness of `H` as an availability challenge. The two must be reconciled.

**Canonical treatment:**
- `H = ∅` is well-defined: IE evaluates anomaly predicates against empty history (e.g., returns "no anomaly detected").
- `H` partial is well-defined: IE evaluates anomaly predicates against the available portion of `H`, and **records the partiality** in the decision (e.g., returns `ASK` if partiality prevents a confident anomaly determination).

This preserves determinism while acknowledging partiality.

### 3.6 Central Authority vs. Root of Trust (added per RR8)

**Clarification:** II-F3 addresses whether `IE` requires a **central authority for runtime evaluation**. It does not address whether the **delegator's authority** (the issuance of `C`) or the **root of trust** (key issuance, revocation roots) is decentralized.

- **Runtime evaluation:** Local (D3). No central authority required.
- **Issuance of `C`:** Handled by AetherGovern (R2: authorized policy change; R3: revocation with dependents). May or may not be decentralized.
- **Root of trust:** A design question for AetherGovern and AetherIdentity. Not a runtime bottleneck for IE.

**Consequence:** II-F3 survives because the **evaluation** step is decentralized. The **authority and trust root** may be centralized or decentralized; that is a separate question.

---

## 4. Why IE Is Decentralizable

**Claim:** IE can be evaluated without a central authority.

**Argument:**
1. `IE` is a deterministic function (D3).
2. `IE`'s evaluation does not require network access, external lookup, or an LLM (D3).
3. `IE`'s inputs `(T, C, H)` can be distributed without a central authority (AetherMesh, AetherAudit, AetherGovern).
4. The remaining challenges (synchronization, availability, lifecycle, divergent decisions) are **engineering problems** handled by other dimensions or by design choices (RR6), not **architectural requirements** for a central authority.

**Therefore:** IE is decentralizable. II-F3 SURVIVES (interim, SIGNED).

---

## 5. What This Analysis Does Not Claim

- That IE is **trivially** decentralized. Dependencies on AetherMesh/AetherAudit/AetherGovern are real and must be resolved for a full implementation.
- That the RR6 reconciliation strategy is decided. It is an open design question.
- That the three challenges in §3 are fully solved. They are **inherited** from other dimensions, which are themselves `(E) undetermined`.
- That the test set is comprehensive. This draft addresses only the evaluation aspect of decentralization, not multi-agent interactions.

---

## 6. Residual Risks (RR1–RR8)

**RR1 — Inherited dependencies on other dimensions.** IE's decentralization depends on AetherMesh, AetherAudit, and AetherGovern. All three are `(E) undetermined`. If any fails to decentralize, II-F3 is conditional.

**RR2 — Evaluation vs. distribution.** This analysis addresses only IE's evaluation (local). It does not address whether `C` and `H` are fully distributed. That is a problem for AetherMesh/AetherAudit/AetherGovern.

**RR3 — No implementation.** The claim is analytical, based on D3. No reference implementation of IE exists.

**RR4 — Test set is narrow.** No concrete multi-agent scenarios are tested.

**RR5 — Determinism assumes D3 holds.** If IE ever uses an LLM (contrary to D3), II-F3 would be falsified.

**RR6 — Divergent local decisions under stale C or partial H (added per review).** Nodes with divergent `C` or partial `H` can return different decisions on the same `T` until synchronization completes. The reconciliation strategy (fail-closed vs. optimistic) is an open design question.

**RR7 — Underspecified anomaly/history semantics (added per review).** D3 treats `H` as optional; §3.2 treats completeness of `H` as an availability challenge. The canonical treatment of `H = ∅` and partial `H` is recorded in §3.5, but the choice of anomaly thresholds and predicates remains open.

**RR8 — Scope of "central authority" vs. root of trust (added per review).** II-F3 addresses runtime evaluation, not the authority/trust root for issuing `C`. These are distinct questions, and the latter is deferred to AetherGovern and AetherIdentity.

---

## 7. Next Step

1. Submit to the independent reviewer (Jonathan Rabe). — **Done, SIGNED 2026-11-21.**
2. Address RR1–RR8 in later phases.
3. Move to II-F4 (Observable Benefit).

---

## 8. Reviewer Confirmation

**Reviewed by:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date:** 2026-11-21
**Status authorization:** SURVIVES (interim, SIGNED) — subject to RR1–RR8 and open upstream dependencies.

**Signature:** Jonathan Rabe
**Date:** 2026-11-21

---

**SIGNED (interim)**
**Last updated:** 2026-11-21
