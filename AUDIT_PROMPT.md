KDSZ Investments Ltd — Proprietary work | 2026-03-26

# STORM Safety Proofs — Full Mathematical Audit
# Project: storm-safety-proofs | Task: 001 | Chunks: 1-3

## MODE: AUTONOMOUS AUDIT
- You are in the storm-safety-proofs repo
- The .tex source is at specs/SYSTEM_SPECIFICATION_v2.tex
- Read the .tex file FIRST, then audit every equation
- Edit equations IN PLACE in the .tex file
- Recompile with pdflatex after corrections
- Commit corrections with clear changelog

## KNOWN ERROR

Lines 82-85 of specs/SYSTEM_SPECIFICATION_v2.tex:
```latex
$(i_1, w_1), \ldots, (i_K, w_K) = R(\mathbf{x})$ with $\sum_{k=1}^{K} w_k = 1$
\mathbf{y}_t = \sum_{k=1}^{K} w_k \cdot m_{i_k}(\mathbf{x})
```

The router R is defined on line 79 as R: R^d → Δ^K_N showing selection from N.
BUT the composition equation (eq:composition) sums k=1 to K — which in isolation
reads as standard MoE. A reviewer seeing ONLY the equation will think "this is MoE."

The equation must make the selection from N explicit WITHOUT requiring the reader
to cross-reference the router definition. The equation alone must be self-contained
and distinguishable from MoE.

## STEP 1: Read the full .tex file

```bash
cat specs/SYSTEM_SPECIFICATION_v2.tex
```

Understand the full document before making any changes.

## STEP 2: Fix the composition equation (Chunk 1)

Evaluate these candidates for replacing lines 82-85:

**Candidate A — Selection set notation:**
```latex
\text{Let } S_t(\mathbf{x}) \subset \{1,\ldots,N\}, \; |S_t| = K \text{ be the set selected by } R.
\mathbf{y}_t = \sum_{n \in S_t(\mathbf{x})} w_n(\mathbf{x}) \cdot m_n(\mathbf{x})
```

**Candidate B — Full sum with sparse weights:**
```latex
\mathbf{y}_t = \sum_{n=1}^{N} w_n(\mathbf{x}) \cdot m_n(\mathbf{x}), \quad w_n = 0 \;\forall\; n \notin \text{TopK}(R(\mathbf{x}))
```

**Candidate C — Indicator function:**
```latex
\mathbf{y}_t = \sum_{n=1}^{N} \mathbb{1}[n \in S_t] \cdot w_n(\mathbf{x}) \cdot m_n(\mathbf{x})
```

For each, check:
1. Does it look like MoE when read in ISOLATION? (Must answer NO)
2. Is N (pool size) visible in the equation?
3. Is the selection mechanism explicit?
4. Is O(K) computation obvious?
5. Is it standard notation a reviewer will understand?
6. Does it fit in a 2-page position paper?

Pick the best. Edit the .tex file. Make sure the surrounding text
(lines 79-85) is consistent with the new equation.

ALSO check: the router definition on line 79 says Δ^K_N — is this
standard notation? Does a reviewer know what this means?

## STEP 3: Audit ALL 6 property definitions (Chunk 2)

For each property in the .tex file, check:

### Attribution (look for "Attribution" section)
- Is the routing weight w properly defined?
- Is ablation delta defined (what changes when you remove a module)?
- Pearson r vs Spearman — which is correct here?
- CRITICAL: Could MoE claim attribution via gating weights? If yes, strengthen the definition.

### Guaranteed Forgetting
- Does "remove module" mean physically remove or set weight to 0?
- Is router retraining after removal stated/assumed?
- CRITICAL: MoE cannot guarantee forgetting because knowledge distributes across shared backbone. Our modules are independently trained — knowledge is ONLY in the module. Is this structural guarantee STATED?

### Non-Degradation
- Is baseline (pool at N) and expansion (N → N+k) clear?
- Is 5% relative or absolute?
- Does router retrain when pool changes? Stated?

### Memory Isolation
- Is the weight metric (softmax vs raw logit) specified?
- Does this hold for all K? At K=N every module runs — isolation fails.

### Bounded Compute
- CRITICAL: The router is Linear(d, N) — that IS O(N). 
- Line 183 says "top-K selection is O(N)" — GOOD, this is honest.
- But the PROPERTY TITLE says "Bounded Compute" implying O(K).
- Check: does the text clearly distinguish router O(N) from module execution O(K)?
- Check: "inference is constant in N" on line 183 — is this true? Router grows with N!

### Compositional Monotonicity
- What metric? PPL? Accuracy? Loss? Must specify.
- 2% vs Non-Degradation 5% — why different?
- Transitivity: A+B OK, B+C OK, but A+B+C might violate.

## STEP 4: Check notation consistency (Chunk 3)

Grep for every use of: N, K, m_i, w_k, w_n, x, y_t, S, d
```bash
grep -n "m_i\|w_k\|w_n\|\\\\mathcal{M}\|\\\\mathcal{S}" specs/SYSTEM_SPECIFICATION_v2.tex
```

Flag ANY inconsistency. The same symbol must mean the same thing everywhere.

## STEP 5: Write MoE differentiation paragraph

Write 2-3 sentences for the paper explaining why this is NOT MoE:
1. Modules independently trained (MoE trains jointly)
2. Modules permanently frozen (MoE experts share gradients via backbone)
3. Modules added/removed without retraining (MoE cannot)
4. Safety properties are ARCHITECTURAL GUARANTEES from independence

Insert this paragraph in the appropriate location in the .tex file
(probably after the composition equation or in the related work section).

## STEP 6: Check implicit assumptions

List every assumption made but NOT stated in the paper:
- Bridge is frozen?
- Modules trained on non-overlapping data? (NOT required — state this)
- Router retrained when pool changes?
- Composition is additive in d_shared?
- Properties claimed for specific K or all K?
- Minimum N for properties to hold?

Add a "Stated Assumptions" subsection to the paper listing these.

## STEP 7: Compile and verify

```bash
cd specs && pdflatex SYSTEM_SPECIFICATION_v2.tex && cd ..
```

Verify the PDF compiles without errors. Check the corrected equations render correctly.

## STEP 8: Summary and commit

Print:
```
MATHEMATICAL AUDIT RESULTS
═══════════════════════════
Equations reviewed:      X
Errors found:            X
Implicit assumptions:    X
Notation inconsistencies:X

CORRECTIONS APPLIED:
1. [line X] — [what was wrong] → [what it should be]
2. ...

REMAINING CONCERNS:
1. [issues needing Kim's decision]
```

Then commit:
```bash
git add specs/SYSTEM_SPECIFICATION_v2.tex specs/SYSTEM_SPECIFICATION_v2.pdf
git commit -m "AUDIT: Fix composition equation (was MoE notation), check all 6 properties, add MoE differentiation, add stated assumptions"
```

## HARD CONSTRAINTS
1. NEVER leave k=1 to K summation if it looks like MoE in isolation
2. PUBLIC LANGUAGE ONLY — never use "STORM", "IFB", "neural database", "byte bridge"
3. Use: "modular sparse composition", "frozen specialist modules", "sparse routing"
4. Edit the .tex IN PLACE — do not create a new file
5. Recompile after changes
6. Be PRECISE about O(K) vs O(N) — the router IS O(N) and that's stated on line 183

## SANITY CHECK BEFORE COMMITTING
Read the corrected composition equation cold. Ask yourself:
"If I showed ONLY this equation to an ML reviewer, would they say MoE?"
If YES → fix it again.
If NO → commit.
