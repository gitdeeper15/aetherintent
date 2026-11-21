# AetherIntent — Architectural Hypothesis

**Status:** Phase 0 — Architectural Hypothesis (In Progress)
**Version:** 1.0.0
**Date:** 2026-11-21
**Author:** Samir Baladi
**ORCID:** [0009-0003-8903-0029](https://orcid.org/0009-0003-8903-0029)
**License:** CC-BY-4.0
**Position in the Aether family:** Thirteenth project (candidate)
**Decision:** (E) Undetermined (interim)

---

## 0. Status of this document

This document states a **hypothesis**. It does not state a result.

- It does **not** claim that AetherIntent is an independent architectural dimension.
- It does **not** claim that the gap it describes is genuine.
- It does **not** claim that the boundaries with the existing Aether dimensions are sufficient.
- It does **not** authorize implementation.

The next steps are: review of this hypothesis, review of the responsibility boundaries, definition of falsification conditions, and a decision on architectural independence. Only after that decision may specification or implementation begin.

---

## 1. Position in the Aether family

The Aether family currently has **seven foundational dimensions**, **four candidates** (Identity, Policy, Audit, Human), and **two candidates under investigation** (Memory, Intent).

| # | Project | Status |
|---|---|---|
| 1 | AetherGit | Foundational |
| 2 | AetherFlow | Foundational |
| 3 | AetherProof | Foundational |
| 4 | AetherMesh | Foundational |
| 5 | AetherGovern | Foundational |
| 6 | AetherRuntime | Foundational |
| 7 | AetherOrchestrate | Foundational |
| 8 | AetherIdentity | Candidate (independence unresolved) |
| 9 | AetherPolicy | Candidate (Phase 4: undetermined; paused) |
| 10 | AetherAudit | Candidate (Phase 4: undetermined; paused) |
| 11 | AetherHuman | Candidate (Phase 4: undetermined; paused) |
| 12 | AetherMemory | Candidate (Phase 0: in progress) |
| 13 | **AetherIntent** | **Candidate (this document; Phase 0)** |

AetherIntent is the **thirteenth project**. It is **not** a replacement for AetherPolicy, AetherAudit, or any other dimension.

### 1.1 Structural dependency

AetherIntent is **structurally dependent** on the resolution of AetherPolicy and AetherAudit. Both are currently paused at Phase 4 with an (E) Undetermined (interim) decision.

- If AetherPolicy is folded into another dimension, the boundary between policy rules and intent enforcement will need to be redrawn.
- If AetherAudit is folded, the boundary between audit records and the integrity ledger will need to be redrawn.

This dependency is treated as a threat to validity (T3), not as a detail.

---

## 2. Central question

> **Can an autonomous agent execute a financially valid transaction that violates the human's delegated intent, and can an independent deterministic enforcement layer prevent that execution?**

The question is structural, not accusatory or normative. It asks whether a distinction exists between:
- **Financial validity:** the transaction is technically possible (sufficient funds, valid credentials, correct amount).
- **Intent validity:** the transaction is consistent with what the human actually authorized.

The hypothesis is that these are **different properties**, and that the distinction requires an architectural layer.

The question has four clauses, each mapped to a failure condition:

| Clause | Condition |
|---|---|
| (i) Not already covered by Policy / Audit / Govern | **II-F1** |
| (ii) Does not decompose into Policy + Audit + Govern | **II-F2** |
| (iii) No central intent authority required | **II-F3** |
| (iv) Observable benefit | **II-F4** |

A positive answer needs all four. A negative answer on any one is sufficient to reject the candidate as a separate dimension.

---

## 3. Assumptions (A1–A6)

Every theorem below is conditional on these assumptions.

| ID | Assumption | Used by |
|---|---|---|
| **A1** | **Finite delegation universe.** The set of transactions that could ever be proposed is finite (or bounded for the duration of the delegation). | Proof-1 |
| **A2** | **Deterministic admission.** The integrity engine evaluates intent consistency using deterministic predicates over the transaction and the intent contract. | Proof-1, Proof-2 |
| **A3** | **LLM non-authority.** The LLM layer interprets natural language and explains decisions, but does not authorize execution. | Proof-2 |
| **A4** | **Signature soundness.** The intent contract is signed by the human delegator and is unforgeable. | Proof-1 |
| **A5** | **Authority precedes autonomy.** The agent cannot propose a transaction that exceeds the authority granted in the intent contract. | Proof-1 |
| **A6** | **Trusted execution gateway.** The payment rail (e.g., Razorpay) accepts only transactions that have passed the integrity engine. | Proof-2 |

---

## 4. Formal definitions (D1–D3)

### D1 — Intent Contract

An **Intent Contract** is a structured representation of a human delegation, consisting of:

- **Scope:** the set of transaction types the agent may propose (e.g., "office chairs", "preferred suppliers").
- **Budget:** the maximum amount the agent may spend.
- **Constraints:** additional conditions (e.g., "no gaming equipment", "only trusted suppliers").
- **Purpose (irreducible):** a declarative statement of the goal-state the delegation is intended to achieve, expressed as a verifiable predicate over the **outcome** of the transaction (not over the transaction itself). Purpose is not a policy rule; it specifies what the delegator would consider a successful outcome, which may be violated even when Scope, Budget, and Constraints are all satisfied.
- **Authority:** the principal (human) who issued the delegation.

### D2 — Intent Validity

A transaction **T** is **intent-valid** with respect to an intent contract **C** if:

- **T** is within the scope of **C**.
- **T** is within the budget of **C**.
- **T** satisfies all constraints in **C**.
- **T** contributes to the **Purpose** of **C**, as verified by the Integrity Engine against the outcome predicate.

**Intent validity is distinct from policy compliance.** A transaction can be policy-compliant (scope, budget, constraints satisfied) but intent-invalid (does not contribute to Purpose). This distinction is the sole justification for a separate architectural dimension. If a verifiable distinction cannot be demonstrated, AetherIntent must be folded into AetherPolicy (failure condition II-F2).

### D3 — Integrity Engine

The **Integrity Engine** is a deterministic function:

**Decision = IE(T, C, H)**

Where:
- **T** is the proposed transaction.
- **C** is the intent contract.
- **H** is the behavioral history (optional, for anomaly detection).
- **Decision** ∈ {ALLOW, ASK, HOLD, DENY}.

The Integrity Engine **does not** use an LLM. It uses hard rules (deterministic predicates) to enforce intent validity.

---

## 5. Propositions (P1–P2)

### P1 — The LLM cannot authorize

**Statement:** The LLM layer cannot directly authorize a transaction. It can only propose and explain.

**Proof sketch:** By A3, the LLM does not have write access to the payment rail. By A6, the payment rail accepts only transactions that have passed the Integrity Engine. Therefore, the LLM cannot authorize.

### P2 — Intent validity is distinct from financial validity

**Statement:** A transaction can be financially valid but intent-invalid.

**Proof sketch:** Consider a transaction T that is within the budget (financial validity) but out of scope (intent invalidity). By D2, T is intent-invalid. By D3, the Integrity Engine will DENY T. The DENY decision is independent of financial validity.

---

## 6. Falsification conditions (II-F1 to II-F4)

| ID | Condition under test | Status |
|:-:|---|---|
| **II-F1** | Non-redundancy: the intent verification is not already covered by AetherPolicy, AetherAudit, or AetherGovern. | 🔴 Untested |
| **II-F2** | Non-decomposition: the intent verification does not decompose into Policy + Audit + Govern; a remainder survives. | 🔴 Untested |
| **II-F3** | Decentralizability: no central intent authority is required. | 🔴 Untested |
| **II-F4** | Observable benefit: a benefit can be observed under a defined evaluation. | 🔴 Untested |

All four conditions are **untested**. The decision rule: a positive answer requires all four to be tested with none falsified, with II-F4 supported by measured results.

---

**AetherIntent v1.0.0 — Architectural Hypothesis (Part 1)**
© 2026 Samir Baladi — Ronin Institute / Rite of Renaissance
DOI: 10.5281/zenodo.24417251

---

## 7. Relation to existing Aether dimensions

AetherIntent is the **thirteenth candidate**. It is not assumed to be independent of the following dimensions:

### 7.1 With the foundational dimensions

| Dimension | Responsibility | AetherIntent's relation |
|---|---|---|
| **AetherGit** | Storage and versioning | AetherIntent consumes storage for the Intent Contract and the Integrity Ledger. |
| **AetherFlow** | Integration | AetherIntent does not integrate. |
| **AetherProof** | Verification | AetherIntent consumes verification primitives for signature validation (A4). |
| **AetherMesh** | Decentralization | AetherIntent relies on Mesh for distribution; it does not distribute itself. |
| **AetherGovern** | Governance authority | AetherIntent consumes authority for the delegation; it does not issue it. |
| **AetherRuntime** | Runtime assurance | AetherIntent does not monitor execution. |
| **AetherOrchestrate** | Orchestration | AetherIntent does not orchestrate. |

### 7.2 With the candidates

| Dimension | Responsibility | AetherIntent's relation |
|---|---|---|
| **AetherIdentity** | Determination of agent identity | AetherIntent uses identity to attribute the delegation; it does not determine identity. |
| **AetherPolicy** | Policy evaluation and enforcement | **Structural dependency.** If lifecycle rules belong to Policy, the boundary between policy and intent must be redrawn. |
| **AetherAudit** | Evidence and accountability | **Structural dependency.** If the Integrity Ledger is an audit record, the boundary must be redrawn. |
| **AetherHuman** | Human participation | AetherIntent consumes the human's delegation; it does not address human participation itself. |
| **AetherMemory** | Memory lifecycle | AetherIntent does not manage memory; it consumes memory for the Intent Contract. |

### 7.3 Boundaries are not assumed sufficient

The boundaries are **not assumed to be sufficient**. A later phase must analyze whether:

- AetherPolicy's rules already cover intent enforcement.
- AetherAudit's records already cover the Integrity Ledger.
- AetherGovern's authority already covers delegation.
- The boundaries overlap in ways that make AetherIntent a feature, not a dimension.

---

## 8. What this document does not claim

- That AetherIntent is an independent architectural dimension.
- That the five candidate responsibilities (M1–M5) are complete.
- That the boundaries with the existing dimensions are sufficient.
- That the gap is genuine.
- That implementation should begin.

This document claims only that the hypothesis is worth investigating under the stated methodology.

---

## 9. Threats to validity

The following threats apply to the investigation:

| # | Threat | Description |
|---|---|---|
| **T1** | Author-on-author reasoning | The hypothesis and the decision will be written by the same author. |
| **T2** | No independent review | Phase 2 (independent review) has not started. |
| **T3** | Structural dependency on AetherPolicy and AetherAudit | Both are paused at Phase 4; any conclusion is conditional. |
| **T4** | LLM interpretability | The LLM may misinterpret the delegation; the integrity engine cannot fix a bad interpretation. |
| **T5** | Scope of intent validity | "Intent validity" may be a semantic notion, not an architectural one. |
| **T6** | No measurement | II-F4 is untested; no evaluation has been run. |
| **T7** | Related work may already cover the gap | The comparison with the field is open. |

---

## 10. Falsification-first methodology

AetherIntent follows the same falsification-first methodology as AetherMesh and AetherMemory:

1. **Architectural Hypothesis** (this document).
2. **Responsibility Mapping** — boundaries with the existing dimensions.
3. **Hypothesis Review** — independent review of the hypothesis and the boundaries.
4. **Falsification Design** — specific conditions under which AetherIntent does not deserve to be a separate dimension.
5. **Independence Decision** — a decision on whether the candidate is independent.
6. **Specification** — only if the independence decision is positive.
7. **Implementation** — only after specification.
8. **Evaluation** — only after implementation.

**No implementation begins before step 5.**

Stopping after step 4 is a legitimate outcome. So is a negative decision.

---

## 11. Next steps

In order:

1. **Responsibility mapping** of the five candidate responsibilities (M1–M5) against the existing dimensions.
2. **Independent review** of this hypothesis and the boundaries.
3. **Falsification conditions** designed for AetherIntent (II-F1 to II-F4).
4. **Independence decision**: is AetherIntent a separate dimension, a feature of AetherPolicy, a feature of AetherAudit, or a bounded extension of one of them?
5. **If independent**: specification (`SPECIFICATION.md`).
6. **If not independent**: report the result, and do not proceed to implementation.

---

## 12. Open questions (initial)

- **Q1.** Is "intent validity" a property of the transaction, the agent, or the delegation?
- **Q2.** Can intent validity be expressed as a policy in AetherPolicy, or does it require a new primitive?
- **Q3.** Can the Integrity Ledger be expressed as an audit record in AetherAudit, or does it require a new primitive?
- **Q4.** Does the LLM layer introduce a new attack surface (e.g., prompt injection) that the Integrity Engine must mitigate?
- **Q5.** What is the minimal interface between AetherIntent and AetherPolicy?
- **Q6.** What is the minimal interface between AetherIntent and AetherAudit?
- **Q7.** Can intent validity be decentralized, or does it require a central authority?
- **Q8.** How is behavioral anomaly detected without violating the agent's autonomy?
- **Q9.** What is the difference between "intent validity" and "policy compliance"?
- **Q10.** Is AetherIntent a dimension, a feature, or a bounded extension of an existing dimension?

These are initial questions. They are not final.

---

**AetherIntent v1.0.0 — Architectural Hypothesis (Complete)**
© 2026 Samir Baladi — Ronin Institute / Rite of Renaissance
DOI: 10.5281/zenodo.24417251
