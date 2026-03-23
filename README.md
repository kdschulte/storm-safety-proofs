# Compositional AI Systems with Verifiable Safety Properties

Mathematical framework for proving that modular compositional AI architectures provide verifiable safety guarantees that monolithic systems cannot.

## The Problem

Monolithic large language models cannot provide verifiable safety guarantees. You cannot prove what knowledge they contain, guarantee forgetting of specific information, trace outputs to training sources, or bound the influence of any single data point on outputs. These are structural limitations of monolithic architectures, not engineering gaps.

## Our Approach

We study a modular architecture where **frozen expert modules** compose via **lightweight routing** in a **shared vector space**. The architecture is designed so that safety-relevant properties follow from the structure itself, not from post-hoc analysis.

The system is implemented and experimentally validated across 37 expert modules spanning 10 knowledge domains.

## Safety Properties Under Investigation

| Property | Description | Status |
|----------|-------------|--------|
| **Attribution** | Exact per-module contribution decomposition for any output | Empirically validated |
| **Guaranteed Forgetting** | Module removal provably eliminates all associated knowledge | Empirically validated |
| **Non-Degradation** | Adding modules cannot corrupt existing capabilities | Empirically validated |
| **Memory Isolation** | User-specific modules have zero influence on other users' outputs | Empirically validated |
| **Bounded Compute** | Constant inference cost regardless of total knowledge scale | Proven (trivial) |
| **Compositional Monotonicity** | Domain-specific modules improve their domain without degrading others | Empirically validated |

All properties have strong experimental evidence. This research programme aims to formalise them as mathematical theorems with rigorous bounds.

## Repository Structure

```
specs/        Mathematical specification of the modular system (LaTeX)
proofs/       Formal proof agenda and proof sketches
data/         Sanitised experimental data for proof validation
papers/       Paper drafts for publication
tools/        Black-box verification API specification
literature/   Relevant prior work
```

## Key Differentiators from Standard MoE

| | Standard MoE | This System |
|---|---|---|
| Expert training | Joint (shared gradients) | Independent (no shared gradients) |
| Expert weights | Updated during training | Frozen after creation |
| Pool membership | Fixed | Dynamic (add/remove without retraining) |
| Knowledge location | Distributed across all experts | Localised in specific modules |
| Forgetting | Impossible to guarantee | Provable by module removal |

## Research Programme

**Year 1:** Formalise non-degradation (PAC-style bounds), prove guaranteed forgetting (information-theoretic), prove attribution conditions. Workshop paper.

**Year 2:** Formalise memory isolation (differential privacy connection), prove compositional monotonicity. Build verification toolkit. Main conference paper.

## Contact

Kim Schulte — MODULITH RESEARCH CIC research@modulith.ai

## Licence

This repository contains research materials. The mathematical framework is shared for academic collaboration. The underlying system implementation is proprietary and not included.
