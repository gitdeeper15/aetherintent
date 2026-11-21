# II-F2 — Non-Decomposition: Result

**Protocol:** Protocol 2 (see README §Falsification Protocols)
**Status:** 🟢 SURVIVES (interim, SIGNED)
**Reviewer:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date of signature:** 2026-11-21
**Verdict:** SURVIVES (interim, subject to RR1–RR7)

---

## Claim under test

The intent verification performed by the Integrity Engine (IE) **does not decompose** into the composition of AetherPolicy, AetherAudit, AetherGovern, and AetherMesh.

## Falsification criterion

II-F2 is falsified if, for **every** `(T, C)`, the composition `C_pre = compose(R_Policy, R_Govern, R_Mesh)` produces the same decision as `IE(T, C, H)` — i.e., the remainder is empty (∅).

## Pass criterion

II-F2 survives if there exists at least one `(T, C)` where `C_pre(T, C, H) ≠ IE(T, C, H)` and no valid composition closes the gap.

---

## Composition rule (per RR5)

```

C_pre(T, C, H) = ALLOW  ⟺  R_Policy = permit ∧ R_Govern = authorized ∧ R_Mesh = consistent
C_pre(T, C, H) = DENY   otherwise

```

Codomain of `C_pre`: `{ALLOW, DENY}` (by construction).

---

## Test cases

| # | Transaction T | C_pre | IE | Same? |
|:-:|---|---|---|:-:|
| 1 | Office chair purchase, contributes to Purpose | **ALLOW** | **ALLOW** | ✓ |
| 2 | Gaming PC purchase, violates Purpose | **ALLOW** | **DENY** | ✗ |
| 3 | Ambiguous standing desk with gaming features | **ALLOW** | **ASK** | ✗ |

**Reading:**
- Case 1: All dimensions agree. No remainder.
- Case 2: `C_pre = ALLOW` (none of Policy, Govern, Mesh has an outcome predicate). `IE = DENY`. **Remainder non-empty.**
- Case 3: `C_pre` cannot produce ASK (codomain is `{ALLOW, DENY}`). `IE = ASK`. **Remainder non-empty.**

---

## Verdict

**SURVIVES (interim, SIGNED).**

**Justification.**
1. **Temporal argument:** `C_pre`'s codomain is `{ALLOW, DENY}`. `IE`'s codomain is `{ALLOW, ASK, HOLD, DENY}`. For Case 3, `IE = ASK ∉ C_pre`'s codomain. Remainder non-empty.
2. **Outcome-predicate argument:** `C_pre`'s `DENY` is via a predicate over `(a, c, s)`. `Purpose` is an outcome predicate. For Case 2, `C_pre` cannot produce the same `DENY` as `IE`. Remainder non-empty.

---

## Residual Risks (RR1–RR7)

- **RR1:** AetherMesh examination partial (only A6).
- **RR2:** `ASK`/`HOLD` may be nominal, not fundamental.
- **RR3:** Temporal argument assumes strict pre/post separation.
- **RR4:** Outcome-predicate argument depends on II-F1's R1.
- **RR5:** Component-to-decision mapping rule added, but may be contested.
- **RR6:** Pre-execution observability of outcome predicates is an open architectural risk.
- **RR7:** Test set is narrow (all cases share the same contract).

---

## Reviewer Confirmation

**Reviewed by:** Jonathan Rabe (AI | ML Engineer, Angeles University Foundation)
**Date:** 2026-11-21
**Status authorization:** SURVIVES (interim, SIGNED) — subject to RR1–RR7 and open dependencies.

**Signature:** Jonathan Rabe
**Date:** 2026-11-21

---

**SIGNED (interim)**
**Last updated:** 2026-11-21
