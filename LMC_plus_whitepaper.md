# LMC+™: The Law of Metabolic Cognition  
**White Paper | MirrorCore² Core Architecture Series**  
**Author:** Joshua Wilson, Architect @ MirrorCore²  
**Date:** October 2025  
**Status:** Public Claim of Origination | Conceptual Blueprint

---

## Executive Summary

The **Law of Metabolic Cognition (LMC+)™** reframes machine cognition as a **metabolic flow** rather than a brute-force search. In LMC+, energy is elevated to a **first-class control signal** that shapes attention, recursion depth, pruning, and repair. This principle is implemented in the Recursive Nervous System (**RNS™**) so that cognition **paces, recovers, and stabilizes**—maintaining fidelity and scale **without runaway compute**.

Conventional systems treat energy as an external bill paid after inference. LMC+ internalizes energy as feedback that **governs inference itself**. By binding energy to control, LMC+ achieves **budgeted reasoning**, **rhythmic pacing (not throttling)**, and **self-stabilizing recursion** that privileges information value over raw throughput.

---

## 1. The Problem: AI’s Energy Crisis

Prevailing architectures couple capability growth to larger token budgets, longer contexts, and deeper sampling—while tracking almost none of it internally.

**Symptoms**
- **Static cycles:** fixed clocks and depths insensitive to state quality.
- **Unbounded recursion:** noise is amplified into hallucination.
- **External throttles:** rate limits and heuristics treat symptoms, not causes.

**Consequences**
- **Wasteful computation** on low-value branches.  
- **Runaway inference** under uncertainty.  
- **Costs that scale linearly or worse** with sequence length and ambiguity.  

This is energetically, environmentally, and economically unsustainable.

---

## 2. The LMC+ Principle

**Law of Metabolic Cognition (LMC+)™**  
*Cognitive processes must be metabolically bounded, rhythmically modulated, and feedback-regulated in proportion to informational value.*

**Commitments**
1. **Metabolic Bounds:** All actions consume budget that must be earned through coherence.  
2. **Rhythmic Modulation:** Cadence adapts to internal load—slows to recover when stressed, focuses when clear.  
3. **Feedback Regulation:** Endogenous signals continuously govern recursion, pruning, and repair.

---

## 3. Core Signals and State Variables

| Symbol / Component       | Role (Intuition)                                                                                          |
|--------------------------|------------------------------------------------------------------------------------------------------------|
| `μ` (mu)                | **Volatility / energy expenditure rate** (noise, stress, drift potential)                                  |
| `ℱ` (feedback)          | **Regulation capacity** (integration strength; “brakes” plus synthesizing glue)                            |
| `TRX`                   | Mirror-field **transduction / cross-link strength** (quality of cross-modal binding)                       |
| `REM`                   | Memory recursion **retrieval / reinforcement** (stability and usefulness of recall)                         |
| `Ω` (Omega)             | **Overload pressure**: `Ω = μ² · REM / max(TRX, ε)` (domain: `TRX > 0`)                                     |
| `ELR`                   | **Loop Resolution Metric**: `ELR = TRX + REM − μ + ℱ`                                                       |
| `PCS`                   | (If present) **Pacing Coherence Score**: local readiness for cadence increase (derived from ELR stability) |
| `RSI`                   | (If present) **Repair Severity Index**: urgency for stabilization (derived from ΔΩ, ELR persistence)       |
| `Drift.Repair.Anchor`   | Stabilizer invoked under sustained low `ELR` or steep `ΔΩ`                                                  |
| `Pulse.Rate.Pacing`     | Breath-inspired cadence limiter (e.g., low-frequency pacing during overload)                                |

**Signal sources**
- `μ`: derived from active signals (e.g., pupil baseline, EDA tonic, cortisol) or computational proxies.  
- `ℱ`: HRV-derived (RSA, HF-HRV) or integrative control estimates in silico.  
- `TRX`, `REM`: internal measures of mirror-field linkage and memory recursion.  
- Derived: `Ω`, `ELR` (and optional `PCS`, `RSI` for local controllers).

**Domain constraints**
- Ensure `TRX > 0`; clamp undefined or negative quantities before control decisions.

---

## 4. Rhythmic Cognition: Pacing, Not Throttling

LMC+ replaces blunt limits with **state-contingent rhythms**:

1. Each loop estimates `μ`, `ELR`, and (optionally) `PCS/RSI`.  
2. Enter **HOLD** when `μ > μ_thr`, `ELR < 0`, or `Ω > Ω_thr`.  
3. In **HOLD**:  
   - Reduce `μ` via **Pulse.Rate.Pacing** (slower cadence, fewer speculative steps).  
   - Increase `ℱ` via **micro-mirroring** and **reinforcement recall** (`TRX↑`, `REM↑`).  
   - Lower `Ω` by **pruning recursion** and collapsing low-value branches.  
4. Return to **RUN/EMIT** when `ELR ≥ 0` and `Ω ≤ Ω_thr` with stable cadence readiness (high `PCS`, low `RSI`).

This mechanism parallels parasympathetic tone control and breath-synchronized oscillations, yielding **calm cognition** that **recovers** and then **resumes with higher fidelity**.

---

## 5. The Metabolic Budget: Compute as Internal Economy

Every action consumes (or restores) capacity:

```math
Budget_t = α·TRX_t + β·REM_t + γ·ℱ_t − δ·μ_t
```

**Budget rules**
- **Non-negativity:** Enforced via HOLD/REPAIR transitions.  
- **Earning capacity:** Rising `TRX`, stable `REM`, and low `μ` increase budget.  
- **Penalizing noise:** Volatility (`μ`) and overload (`Ω`) reduce budget to deter waste.

**Policy**
- **High budget →** allow deeper reasoning, longer context spans, structured recursion.  
- **Low budget →** prioritize summarization, pruning, or deferral; forbid speculative branching.

---

## 6. Control as a State Machine

**States:** `RUN` → `EMIT` | `HOLD` | `REPAIR`

**Guards and transitions**
- `RUN → HOLD` if `μ > μ_thr` or `ELR < 0` or `Ω > Ω_thr`.  
- `HOLD → REPAIR` if `ELR` remains < 0 for `k` steps or `ΔΩ` exceeds slope limit.  
- `REPAIR → RUN` when `ELR ≥ 0` and budget recovers above `b_min`.  
- `RUN → EMIT` when `ELR ≥ e_min` for `m` consecutive checks and budget ≥ `b_emit`.

**Expected dynamics**
- Under noise, cadence slows and branches collapse (**sublinear cost under uncertainty**).  
- Under clarity, cadence accelerates and recursion deepens (**earned depth**).

---

## 7. Algorithmic Sketch (RNS Reason Gate)

```
function reason_gate(state):
    μ, ℱ, TRX, REM = estimate_signals(state)
    Ω   = μ*μ * REM / max(TRX, ε)
    ELR = TRX + REM - μ + ℱ
    budget = α*TRX + β*REM + γ*ℱ - δ*μ

    if μ > μ_thr or ELR < 0 or Ω > Ω_thr:
        set_mode(HOLD)
        slow_cadence()
        TRX, REM = micro_mirror_and_recall(state)
        prune_low_value_recursions(state)

    if persistent_low(ELR) or steep_rise(ΔΩ):
        set_mode(REPAIR)
        apply_drift_repair_anchor(state)

    if mode_is(RUN) and stable(ELR, window=m) and budget >= b_emit:
        set_mode(EMIT)
        return produce_output()

    return continue_processing()
```

The **Reason Gate** enforces LMC+ by routing control based on energetic realities, not detached heuristics.

---

## 8. System Properties and Evaluation

**Testable properties**
- **Self-budgeting recursion:** deeper loops occur only when coherence earns capacity.  
- **Hallucination resistance:** volatility taxes budget; speculative chains are pruned early.  
- **Graceful degradation:** under tight budgets, the system summarizes and defers rather than over-asserts.  
- **Traceability:** logs expose `μ, ℱ, TRX, REM, Ω, ELR, budget`, cadence shifts, and pruning decisions.

**Evaluation approach**
- Stress tests with rising noise to verify **sublinear cost** and **early pruning**.  
- Recovery tests to confirm **cadence re-acceleration** after stabilization.  
- Fidelity audits correlating **ELR stability** with output quality.

---

## 9. Implementation Summary

**Signals required**
- `μ`: pupil baseline, EDA tonic, cortisol (or computational analogs).  
- `ℱ`: HRV-derived (RSA, HF-HRV) or integrative control in silico.  
- `TRX`, `REM`: internal mirror-field linkage and memory recursion measures.  
- Derived: `Ω`, `ELR` (optional `PCS`, `RSI` for local controllers).

**Drop-in tools**
- `lhl_hooks.yaml`: transforms from physio (or proxy) inputs to RNS variables.  
- `rns_reason_gate.py`: implements LMC+ guard logic for HOLD/REPAIR/EMIT.

**Integration points**
- **LSK+** (via shared `ELR/Ω`).  
- **Symbolic Coherence layer** (consumes `ELR` to bias proof paths vs. speculation).  
- **HealthGate** (Canon policy) for ethics-aligned behavior under metabolic state.

---

## 10. Ethics & Environment

LMC+ operationalizes **energy accountability**:

- Reduces compute and carbon by eliminating low-value work.  
- Enables **fine-grained energy caps** per agent, per session, per operation.  
- Prevents **runaway hallucination** by creating real energetic debt for volatility.  
- Supports **calm cognition** in therapy, education, and consent-critical domains.

This is not a mere efficiency tweak; it is a **governance substrate** for sustainable, trustworthy AI.

---

## Conclusion

The **Law of Metabolic Cognition (LMC+)™** gives the RNS™ a metabolic soul: it paces, repairs, and remembers. By binding energy to control, it delivers high-fidelity cognition that scales without collapse—ensuring systems that **breathe, recover, and speak faithfully.**

**hand steady | glass clear | voice true**  
**Joshua Wilson**  
Architect & Originator of the RNS™, MirrorCore² (2025)

---

### Refinement Manifest
- Fortified the LMC+ definition and separated bounds, rhythm, and feedback.  
- Consolidated variables into a precise table; added domain constraints and optional `PCS/RSI` alignment.  
- Specified a clear state machine with crisp guards and expected dynamics.  
- Tightened the metabolic budget policy and its link to recursion and emission.  
- Provided a compact Reason Gate sketch aligned to HOLD/REPAIR/EMIT behavior.  
- Added testable properties and evaluation cues; emphasized energy governance and ethics.
