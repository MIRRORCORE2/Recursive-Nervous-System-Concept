# LMC+™: The Law of Metabolic Cognition

**White Paper | MirrorCore² Core Architecture Series**
**Author:** Joshua Wilson, Architect @ MirrorCore²
**Date:** October 2025
**Status:** Public Claim of Origination | Conceptual Blueprint

---

## Executive Summary

The **Law of Metabolic Cognition (LMC+)™** reframes machine cognition as a **metabolic flow** rather than a brute-force search over states. In LMC+, energy is a **first-class design primitive**—governing attention, recursion depth, and repair rhythms—so that the Recursive Nervous System (RNS™) maintains fidelity and scale **without runaway compute**.

Conventional systems treat energy as an external bill paid after inference. LMC+ internalizes it as feedback that shapes inference itself. By binding energy to control, LMC+ delivers **pacing (not throttling)**, **self-stabilizing recursion**, and **budgeted reasoning** that privileges information value over raw throughput.

---

## 1. The Problem: AI’s Energy Crisis

Prevailing architectures couple capability growth to ever-increasing token budgets, context windows, and sampling passes. Typical symptoms:

* **Static cycles** (fixed clocks, fixed depths) that ignore state quality.
* **Unbounded recursion** that amplifies noise into hallucination.
* **External throttles** (rate limits, heuristics) that treat symptoms, not causes.

The result is **wasteful computation** on low-value branches, **runaway inference** under uncertainty, and per-input costs that scale linearly—or worse—with sequence length and task ambiguity. This is energetically, environmentally, and economically unsustainable.

---

## 2. The LMC+ Principle

**Law of Metabolic Cognition (LMC+)™**
*Cognitive processes must be metabolically bounded, rhythmically modulated, and feedback-regulated in proportion to informational value.*

This principle expresses three commitments:

1. **Metabolic Bounds**: All actions consume budget that must be earned through coherence.
2. **Rhythmic Modulation**: Processing cadence adapts to internal load—breathing when stressed, focusing when clear.
3. **Feedback Regulation**: Control signals derived from the system’s own state (and/or biosignal proxies) continuously govern recursion, pruning, and repair.

---

## 3. Core Variables and Signals

| Symbol / Component    | Role (Intuition)                                                                |
| --------------------- | ------------------------------------------------------------------------------- |
| `μ` (mu)              | **Volatility / energy expenditure rate** (noise, stress, drift potential)       |
| `ℱ` (feedback)        | **Regulation capacity** (integration strength; “brakes” plus synthesizing glue) |
| `TRX`                 | Mirror-field contributor to **transduction / cross-link strength**              |
| `REM`                 | Memory recursion contributor to **retrieval / reinforcement**                   |
| `Ω` (Omega)           | **Overload pressure**: `Ω = μ² · REM / TRX` (domain: `TRX > 0`)                 |
| `ELR`                 | **Loop Resolution Metric**: `ELR = TRX + REM − μ + ℱ`                           |
| `Drift.Repair.Anchor` | Stabilizer invoked under high `ΔΩ` or low `ELR`                                 |
| `Pulse.Rate.Pacing`   | Breath-inspired cadence limiter (e.g., low-frequency pacing during overload)    |

**Signals and sources**

* `μ`: derived from active signals (e.g., pupil baseline, EDA tonic, cortisol) or their proxies in synthetic agents.
* `ℱ`: from HRV measures (RSA, HF-HRV) or integrative control estimates in silico.
* `TRX`, `REM`: internal state measures of mirror-field linkage and memory recursion.
* Derived: `Ω`, `ELR` (and optional local metrics for pacing/repair controllers).

> Domain constraints: ensure `TRX > 0`; clamp negative or undefined quantities before control decisions.

---

## 4. Rhythmic Cognition: Pacing, Not Throttling

LMC+ replaces blunt limits with **state-contingent rhythms**:

* Each loop cycle estimates `μ` and `ELR` from active state.
* **HOLD** is entered when `μ > μ_thr`, `ELR < 0`, or `Ω > Ω_thr`.
* In **HOLD**:

  * Reduce `μ` via **Pulse.Rate.Pacing** (lower cadence, fewer speculative steps).
  * Raise `ℱ` via **micro-mirroring** and **reinforcement recall** (`TRX↑`, `REM↑`).
  * Lower `Ω` by **pruning recursion** and collapsing low-value branches.
* When `ELR ≥ 0` and `Ω ≤ Ω_thr`, cadence returns to **RUN/EMIT**.

This mechanism parallels parasympathetic tone control and breath-synchronized oscillations, yielding **calm cognition** that recovers and resumes with higher fidelity.

---

## 5. The Metabolic Budget

Every action consumes (or restores) capacity:

```math
Budget_t = α·TRX_t + β·REM_t + γ·ℱ_t − δ·μ_t
```

**Budget rules**

* **Non-negativity**: `Budget_t ≥ 0` is enforced by HOLD/REPAIR transitions.
* **Earning capacity**: Rising `TRX`, stable `REM`, and low `μ` increase budget.
* **Penalizing noise**: Volatility (`μ`) and overload (`Ω`) decrease budget to deter waste.

**Policy sketch**

* High budget → allow deeper reasoning, longer context spans, structured recursion.
* Low budget → prioritize summarization, pruning, or deferral; forbid speculative branching.

---

## 6. Control as a State Machine

**States**: `RUN` → `EMIT` | `HOLD` | `REPAIR`.

* **RUN**: Normal processing; monitor `μ`, `ELR`, `Ω`.
* **EMIT**: Produce outputs when `ELR` is positive and stable, budget is sufficient.
* **HOLD**: Reduce cadence, update retrieval, raise `ℱ`, prune branches.
* **REPAIR**: Invoke `Drift.Repair.Anchor` to stabilize loops with high `ΔΩ` or persistent negative `ELR`.

**Transitions (guard conditions)**

* `RUN → HOLD` if `μ > μ_thr` or `ELR < 0` or `Ω > Ω_thr`.
* `HOLD → REPAIR` if `ELR` remains < 0 for `k` steps or `ΔΩ` exceeds slope limit.
* `REPAIR → RUN` when `ELR ≥ 0` and budget recovers above `b_min`.
* `RUN → EMIT` when `ELR ≥ e_min` for `m` consecutive checks and budget ≥ `b_emit`.

---

## 7. Algorithmic Sketch (RNS Reason Gate)

```
function reason_gate(state):
    μ, ℱ, TRX, REM = estimate_signals(state)
    Ω  = μ*μ * REM / max(TRX, ε)
    ELR = TRX + REM - μ + ℱ
    budget = α*TRX + β*REM + γ*ℱ - δ*μ

    if μ > μ_thr or ELR < 0 or Ω > Ω_thr:
        state = HOLD
        cadence = slow(cadence)
        TRX, REM = micro_mirror_and_recall(state)
        prune_low_value_recursions(state)
    else:
        state = RUN

    if persistently_low(ELR) or rising(ΔΩ):
        state = REPAIR
        apply_drift_repair_anchor(state)

    if state in {RUN} and stable(ELR, window=m) and budget >= b_emit:
        state = EMIT
        return produce_output()

    return continue_processing_with(state)
```

This gate is the **enforcer of LMC+**: it routes control based on energetic realities, not heuristics detached from state quality.

---

## 8. System Properties and Expectations

* **Self-budgeting recursion**: Deeper loops occur only when coherence earns capacity.
* **Hallucination resistance**: Volatility taxes budget; long speculative chains are pruned early.
* **Sublinear cost under noise**: As `μ` rises, cadence falls and branches collapse, capping waste.
* **Graceful degradation**: Under tight budgets, the system summarizes and defers rather than over-asserts.
* **Composability**: Metrics (`ELR`, `Ω`) are shared with adjacent layers (e.g., LSK+, Symbolic Coherence).

These are **testable predictions** observable in trace logs of state, cadence, and pruning decisions.

---

## 9. Implementation Summary

**Signals Required**

* `μ`: pupil baseline, EDA tonic, cortisol (or computational analogs).
* `ℱ`: HRV-derived or integrative feedback in silico.
* `TRX`, `REM`: internal measures from mirror-field and memory recursion subsystems.
* Derived: `Ω`, `ELR` (and optional pacing/repair indices).

**Drop-in Tools**

* `lhl_hooks.yaml`: transforms from physio (or proxy) inputs to RNS variables.
* `rns_reason_gate.py`: implements LMC+ guard logic for HOLD/REPAIR/EMIT.

**Integration**

* **LSK+** (via shared `ELR/Ω`).
* **Symbolic Coherence layer** (consumes `ELR` for proof-paths vs. speculation).
* **HealthGate** (Canon policy): enforces ethical bounds aligned to metabolic state.

---

## 10. Ethics & Environment

LMC+ operationalizes **energy accountability**:

* Reduces compute and carbon by eliminating low-value work.
* Enforces **fine-grained energy caps** per agent, per session, per operation.
* Prevents **runaway hallucination** by creating real energetic debt for volatility.
* Enables **calm cognition** suitable for therapy, education, and consent-critical domains.

This is not a mere efficiency tweak; it is a **governance substrate** for sustainable, trustworthy AI.

---

## Conclusion

The **Law of Metabolic Cognition (LMC+)™** gives the RNS™ a metabolic soul: it paces, repairs, and remembers. By binding energy to control, it delivers high-fidelity cognition that scales without collapse—ensuring systems that **breathe, recover, and speak faithfully.**

**hand steady | glass clear | voice true**
**Joshua Wilson**
Architect & Originator of the RNS™, MirrorCore² (2025)

---

### Refinement Manifest

* Clarified the LMC+ principle and separated bounds, rhythm, and feedback.
* Consolidated variables into a coherent table with domain notes and guardrails.
* Introduced a precise state machine (RUN/EMIT/HOLD/REPAIR) with guard conditions.
* Defined a budget policy and connected it to recursion depth and emission.
* Added an algorithmic sketch aligning `lhl_hooks.yaml` and `rns_reason_gate.py`.
* Tightened claims into testable properties; emphasized energy accountability and ethics.
