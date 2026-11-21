# AetherIntent — Responsibility Boundaries

**Status:** Phase 1 — Responsibility Mapping (Initial Draft)
**Version:** 1.0.0
**Date:** 2026-11-21
**Author:** Samir Baladi
**ORCID:** [0009-0003-8903-0029](https://orcid.org/0009-0003-8903-0029)
**License:** CC-BY-4.0
**Position in the Aether family:** Thirteenth project (candidate)
**Decision:** (E) Undetermined (interim)

---

## 0. Status of this document

This document maps the **initial boundaries** between the five candidate
responsibilities of AetherIntent (M1–M5) and the twelve other projects of the
Aether family. It is a **Phase 1 deliverable**.

It does **not**:

- claim that the boundaries are sufficient;
- claim that any responsibility is independent;
- claim that the mapping is complete;
- claim that AetherIntent is a confirmed dimension;
- authorize specification or implementation.

Every boundary below is a **hypothesis**, not a result.

---

## 1. Purpose and method

### 1.1 Purpose

The central question of AetherIntent (Section 2 of the hypothesis)
presupposes that five responsibilities (M1–M5) exist and that they are not
fully carried by the existing dimensions. Phase 1 does not test that
presupposition. It only records, dimension by dimension, where the contact
surfaces are and what kind of overlap is suspected.

The output is a boundary map, not a decision.

### 1.2 Method

For each of the five responsibilities (M1–M5) and for each of the twelve other
projects, the map records one of four relations:

| Relation | Meaning |
|:-:|---|
| **Consumes** | AetherIntent depends on this dimension but does not provide it. |
| **Overlaps (suspected)** | The dimension may already provide part or all of the responsibility; the overlap is a reduction risk. |
| **Distinct (hypothesis)** | The responsibility is not carried by the dimension, as far as the author's present reading goes. |
| **Open** | The relation cannot be classified without resolving a structural dependency. |

Every relation marked **Overlaps (suspected)** or **Open** is a candidate
reduction.

---

## 2. The five responsibilities (restated)

| ID | Responsibility | Statement (hypothesis) | Principal reduction risk |
|:-:|---|---|---|
| **M1** | Intent Interpretation | Translating natural-language delegation into a structured intent contract. | May reduce to AetherPolicy (rules). |
| **M2** | Intent Enforcement | Deterministically verifying that a transaction is consistent with the intent contract. | May reduce to AetherPolicy (enforcement). |
| **M3** | Behavioural Monitoring | Detecting deviations from normal agent behaviour. | May be a runtime assurance concern (AetherRuntime). |
| **M4** | Decision Semantics | Classifying decisions as ALLOW, ASK, HOLD, or DENY. | May be a policy decision (AetherPolicy). |
| **M5** | Integrity Ledger | Recording every proposal, decision, and execution event. | May reduce to AetherAudit. |

---

## 3. Boundary map: M1–M5 against the foundational dimensions

### 3.1 AetherGit (Storage)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Consumes | The intent contract is stored as a file; Git does not interpret it. |
| **M2** | Consumes | Git stores the ledger; it does not enforce intent. |
| **M3** | Consumes | Git may store behavioural history; it does not analyze it. |
| **M4** | Distinct (hypothesis) | Git does not classify decisions. |
| **M5** | Consumes | Git stores the ledger; it does not define its semantics. |

### 3.2 AetherFlow (Integration)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Distinct (hypothesis) | Flow is transport, not interpretation. |
| **M2** | Distinct (hypothesis) | Flow does not enforce. |
| **M3** | Distinct (hypothesis) | Flow does not monitor behaviour. |
| **M4** | Distinct (hypothesis) | Flow does not classify. |
| **M5** | Consumes | Flow may carry the ledger; it does not define it. |

### 3.3 AetherProof (Verification)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Consumes | Proof may verify the signature of the intent contract. |
| **M2** | Distinct (hypothesis) | Proof verifies claims, not intent. |
| **M3** | Distinct (hypothesis) | Proof does not monitor behaviour. |
| **M4** | Overlaps (suspected) | A proof may attest to a decision; whether it is a decision record is open. |
| **M5** | Consumes | Proof may attest to the integrity of the ledger. |

### 3.4 AetherMesh (Decentralization)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Open | M-F3 (decentralizability) asks whether intent enforcement survives distribution. |
| **M2** | Open | Decentralized enforcement is an open question. |
| **M3** | Distinct (hypothesis) | Mesh distributes state; it does not monitor behaviour. |
| **M4** | Open | Decentralized decision-making is an open question. |
| **M5** | Consumes | Mesh distributes the ledger; it does not define it. |

### 3.5 AetherGovern (Governance)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Distinct (hypothesis) | Governance issues authority; it does not interpret intent. |
| **M2** | **Overlaps (suspected)** | This is threat **T5**. If enforcement is a governance decision, M2 may reduce to AetherGovern. |
| **M3** | Distinct (hypothesis) | Governance does not monitor behaviour. |
| **M4** | **Overlaps (suspected)** | Decision classification may be a governance decision. |
| **M5** | Consumes | Governance may issue rules; it does not define the ledger. |

### 3.6 AetherRuntime (Runtime Assurance)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Distinct (hypothesis) | Runtime does not interpret intent. |
| **M2** | Consumes | Runtime may enforce at use time; it does not define enforcement. |
| **M3** | **Overlaps (suspected)** | Behavioural monitoring is a runtime concern. |
| **M4** | Consumes | Runtime may check decisions; it does not define them. |
| **M5** | Distinct (hypothesis) | Runtime does not define the ledger. |

### 3.7 AetherOrchestrate (Orchestration)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Distinct (hypothesis) | Orchestration does not interpret intent. |
| **M2** | Consumes | Orchestration may sequence enforcement; it does not define it. |
| **M3** | Consumes | Orchestration may schedule monitoring; it does not define it. |
| **M4** | Distinct (hypothesis) | Orchestration does not classify. |
| **M5** | Consumes | Orchestration may write to the ledger; it does not define it. |

---

## 4. Boundary map: M1–M5 against the candidates

### 4.1 AetherIdentity (Identity — independence unresolved)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Consumes | The intent contract is attributed to a principal; identity does not interpret it. |
| **M2** | Consumes | Identity may be part of enforcement; it does not define it. |
| **M3** | Distinct (hypothesis) | Identity does not monitor behaviour. |
| **M4** | Distinct (hypothesis) | Identity does not classify. |
| **M5** | Consumes | Identity may be part of the ledger; it does not define it. |

### 4.2 AetherPolicy (Policy Lifecycle — Phase 4 paused)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | **Overlaps (suspected)** | **Structural dependency.** Intent interpretation may be a policy matter. |
| **M2** | **Overlaps (suspected)** | Enforcement may be a policy matter. |
| **M3** | Distinct (hypothesis) | Policy does not monitor behaviour. |
| **M4** | **Overlaps (suspected)** | Decision classification may be a policy decision. |
| **M5** | Open | The ledger may be a policy record. |

### 4.3 AetherAudit (Audit and Evidence — Phase 4 paused)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Consumes | Audit may record the intent contract; it does not interpret it. |
| **M2** | Consumes | Audit may record enforcement; it does not define it. |
| **M3** | Consumes | Audit may record behavioural history; it does not analyze it. |
| **M4** | Consumes | Audit may record decisions; it does not classify them. |
| **M5** | **Overlaps (suspected)** | The ledger may reduce to Audit. This is threat **T3**. |

### 4.4 AetherHuman (Human Participation — Phase 4 paused)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Consumes | The human issues the delegation; AetherHuman does not interpret it. |
| **M2** | Distinct (hypothesis) | AetherHuman does not enforce. |
| **M3** | Distinct (hypothesis) | AetherHuman does not monitor behaviour. |
| **M4** | Distinct (hypothesis) | AetherHuman does not classify. |
| **M5** | Consumes | AetherHuman may be part of the ledger; it does not define it. |

### 4.5 AetherMemory (Memory Lifecycle — Phase 0)

| Responsibility | Relation | Note |
|:-:|:-:|---|
| **M1** | Consumes | Memory may store the intent contract; it does not interpret it. |
| **M2** | Distinct (hypothesis) | Memory does not enforce. |
| **M3** | Consumes | Memory may store behavioural history; it does not analyze it. |
| **M4** | Distinct (hypothesis) | Memory does not classify. |
| **M5** | Consumes | Memory may store the ledger; it does not define it. |

---

## 5. Summary matrix

Legend: **C** = Consumes · **O** = Overlaps (suspected) · **D** = Distinct (hypothesis) · **?** = Open

| Dimension | M1 | M2 | M3 | M4 | M5 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| AetherGit | C | C | C | D | C |
| AetherFlow | D | D | D | D | C |
| AetherProof | C | D | D | O | C |
| AetherMesh | ? | ? | D | ? | C |
| AetherGovern | D | **O** | D | **O** | C |
| AetherRuntime | D | C | **O** | C | D |
| AetherOrchestrate | D | C | C | D | C |
| AetherIdentity | C | C | D | D | C |
| **AetherPolicy** | **O** | **O** | D | **O** | ? |
| **AetherAudit** | C | C | C | C | **O** |
| AetherHuman | C | D | D | D | C |
| AetherMemory | C | D | C | D | C |

The matrix is the author's reading. It is not a verified assessment.

---

## 6. The four high-risk overlaps

### 6.1 M2 vs AetherGovern (T5)

M2 is stated in terms of "authorized scope". If authorization is issued by
AetherGovern, then M2 may be a consumer of AetherGovern rather than an
independent responsibility.

### 6.2 M2 vs AetherPolicy

Expiration rules may belong to AetherPolicy. If so, M2 is a consumer of
AetherPolicy. **This is the most direct consequence of the structural
dependency on AetherPolicy (T3).**

### 6.3 M5 vs AetherAudit

The Integrity Ledger may reduce to Audit. If Audit already records decisions,
M5 is a consumer of Audit. **This is threat T3.**

### 6.4 M3 vs AetherRuntime

Behavioural monitoring may reduce to AetherRuntime. If Runtime already
monitors behaviour, M3 is a consumer of Runtime.

---

## 7. Open questions carried forward

- **Q2.** Can intent validity be expressed as a policy in AetherPolicy, or does it require a new primitive?
- **Q3.** Can the Integrity Ledger be expressed as an audit record in AetherAudit, or does it require a new primitive?
- **Q4.** Does the LLM layer introduce a new attack surface (e.g., prompt injection) that the Integrity Engine must mitigate?
- **Q5.** What is the minimal interface between AetherIntent and AetherPolicy?
- **Q6.** What is the minimal interface between AetherIntent and AetherAudit?
- **Q7.** Can intent validity be decentralized, or does it require a central authority?

---

## 8. What this document establishes

It establishes only that:

1. A boundary map can be drawn for M1–M5 against the twelve projects.
2. Four overlaps are the most likely reductions (Section 6).
3. The mapping is sufficient to **design** falsification conditions (Phase 3), not to **decide** independence (Phase 4).

It does not establish that the boundaries are sufficient, that any
responsibility is independent, or that the gap is genuine.

---

## 9. Next step

Phase 2 — Hypothesis Review. An independent reviewer is asked to assess
whether the boundary map is (a) complete enough to review, (b) fair to the
existing dimensions, and (c) honest about the open relations.

---

**AetherIntent v1.0.0 — Responsibility Boundaries**
© 2026 Samir Baladi — Ronin Institute / Rite of Renaissance
DOI: 10.5281/zenodo.24417251

*Phase 1 deliverable. Boundaries are hypotheses, not results.*
