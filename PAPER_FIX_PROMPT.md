KDSZ Investments Ltd — Proprietary work | 2026-03-26

# Recreate AIxSE Position Paper with Audit Fixes

## CONTEXT
The original AIxSE position paper (IEEE 2-page format) has the same bugs found in the
Zenodo spec audit. The corrected spec is at specs/SYSTEM_SPECIFICATION_v3.tex — use
it as the SOURCE OF TRUTH for all equations and definitions.

The original paper PDF is provided for structure/layout reference only. Do NOT copy
equations from it — they have known errors.

## TASK
Create papers/AIxSE_Position_Paper_v2.tex — a corrected IEEE 2-page position paper
that incorporates ALL audit fixes from the spec v3.

## STEP 1: Read the corrected spec

```bash
cat specs/SYSTEM_SPECIFICATION_v3.tex
```

This has the corrected equations, MoE differentiation, stated assumptions, fixed citations.

## STEP 2: Create the corrected position paper

Create papers/AIxSE_Position_Paper_v2.tex with these corrections vs the original:

### Fix 1: Composition equation (Equation 1)
WRONG (original):
```latex
y_t = \sum_{k=1}^{K} w_k \cdot m_{i_k}(\mathbf{x})
```

CORRECT (from spec v3):
```latex
\mathbf{y}_t = \sum_{n \in S_t(\mathbf{x})} w_n(\mathbf{x}) \cdot m_n(\mathbf{x}), \quad |S_t| = K \ll N
```

The Definition must include the selection set S(x) ⊂ {1,...,N} with |S| = K ≪ N
and weights w_n(x) ≥ 0 summing to 1 over S.

### Fix 2: Computational cost section
The original is actually decent here but align with spec v3:
- Router: O(N) or O(N log K) for top-K selection
- Module execution: O(K) — only K modules run
- Total: O(N log K + K·C_mod), dominated by K when C_mod >> N
- Keep the mention of approximate nearest-neighbour for large N

### Fix 3: Experiment count
Replace all instances of "300" or "over 300" with "346"

### Fix 4: PAC-Bayes citation
Reference [11] in the original is Valiant 1984 — that's PAC learning, NOT PAC-Bayes.
In Property 3, the text says "PAC-Bayes bounds [11]" — this is WRONG.

Fix: Add McAllester 1999 as a new reference and cite it for PAC-Bayes:
```bibtex
D. A. McAllester, "PAC-Bayesian model averaging," in Proc. COLT, pp. 164–170, 1999.
```

Keep Valiant 1984 ONLY if used elsewhere for PAC learning (not PAC-Bayes).

### Fix 5: Notation consistency
Use w_n and m_n throughout (not w_k and m_{i_k}).
Use S_t(x) for the selection set.
Match spec v3 notation exactly.

### Fix 6: Property statements
Align ALL 6 property statements with spec v3:
- Attribution: w_n notation, add MoE gating comparison in proof strategy
- Guaranteed Forgetting: scope to modular knowledge, add backbone caveat
- Non-Degradation: "retraining only the router" explicit
- Memory Isolation: add K ≪ N requirement to property statement
- Bounded Compute: split router O(N) from module O(K) clearly
- Compositional Monotonicity: "expected cross-entropy loss", add transitivity remark

### Fix 7: Add MoE differentiation
After the 4 structural constraints, add a sentence or brief remark (space permitting
in 2 pages) distinguishing from MoE. Pull from spec v3 lines 105-109.

### Fix 8: Revision note
Add a footnote on page 1:
"v2: Corrected composition equation notation (selection-set formulation),
bounded compute specification, experiment count, and PAC-Bayes citation."

## STEP 3: IEEE format requirements

- 2-column IEEE conference format
- Maximum 2 pages (position paper)
- Use IEEEtran document class
- Single-blind (author name visible)
- All references must fit in 2 pages

Check that the corrected paper still fits in 2 pages. If it overflows, trim
the Discussion section or the empirical motivation — the equations and properties
are more important than prose.

## STEP 4: Compile and verify

```bash
cd papers
pdflatex AIxSE_Position_Paper_v2.tex
pdflatex AIxSE_Position_Paper_v2.tex
```

Verify:
1. PDF compiles without errors
2. Equations render correctly
3. Paper fits in 2 pages
4. All references resolve
5. Composition equation uses S_t(x) notation (NOT k=1 to K)
6. PAC-Bayes cites McAllester 1999 (NOT Valiant 1984)
7. Experiment count says 346

## STEP 5: Keep the original

Do NOT modify or delete the original paper if it exists anywhere in the repo.
The v2 is a NEW file: papers/AIxSE_Position_Paper_v2.tex

## STEP 6: Commit

```bash
cd ..
git add papers/AIxSE_Position_Paper_v2.tex papers/AIxSE_Position_Paper_v2.pdf
git commit -m "AIxSE v2: Apply all audit fixes from spec v3. Fixed composition equation (was MoE notation), PAC-Bayes citation, experiment count 346, bounded compute O(N)+O(K), notation consistency, MoE differentiation."
git push
```

## HARD CONSTRAINTS
1. Equation (1) MUST use selection-set notation from spec v3. If it says k=1 to K, REJECT.
2. PAC-Bayes MUST cite McAllester 1999, NOT Valiant 1984.
3. PUBLIC LANGUAGE ONLY — never "STORM", "IFB", "byte bridge", "neural database"
4. Must fit in 2 pages IEEE format
5. All notation must match spec v3 exactly (w_n, m_n, S_t, N, K)
6. Experiment count: 346
