# Proof Agenda — Compositional AI Safety Properties

## Status Key
- 🔴 Not started
- 🟡 In progress
- 🟢 Complete

## Proofs

### P1: Non-Degradation Under Pool Expansion 🔴
**Statement:** Adding module f_{N+1} to frozen pool cannot increase optimal loss.
**Approach:** Show policy over expanded pool has strictly more options. Bound finite-sample gap.
**Target:** Formal theorem with PAC-style bounds.
**Difficulty:** Medium (core argument is straightforward; finite-sample bounds are the work).

### P2: Exact Output Attribution 🔴
**Statement:** Per-module contribution is exactly decomposable from weighted sum.
**Approach:** Linear decomposition is immediate. Non-trivial part: when does the decomposition faithfully represent influence through the output mapping g?
**Target:** Conditions under which attribution is meaningful (not just mathematically correct).
**Difficulty:** Hard (involves analysing nonlinear output mapping).

### P3: Guaranteed Forgetting 🔴
**Statement:** Removing a memory module provably eliminates all information from its training data.
**Approach:** Information-theoretic argument. Module independence + frozen parameters + no shared weights = information is localised.
**Target:** Information-theoretic bound showing I(output; D_m | F\{f_m}) = 0.
**Difficulty:** Medium (clean if independence holds; subtle if bridge/output mapping is shared).

### P4: Bounded Inference Cost 🟢
**Statement:** Per-position compute is O(K) regardless of pool size N.
**Proof:** Trivial from architecture definition. K is constant. Done.

### P5: Memory Isolation 🔴
**Statement:** User A's memory module has zero influence on User B's outputs when not routed.
**Approach:** Zero gradient from definition. Connect to differential privacy framework.
**Target:** ε-differential privacy bounds for the modular system.
**Difficulty:** Hard (DP connection is novel contribution).

### P6: Compositional Monotonicity 🔴
**Statement:** Domain-specific module improves its domain without degrading others.
**Approach:** Leave-one-out data provides empirical evidence. Need formal argument from routing optimality.
**Target:** Formal theorem with empirical validation.
**Difficulty:** Medium.
