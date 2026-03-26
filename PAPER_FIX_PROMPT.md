KDSZ Investments Ltd — Proprietary work | 2026-03-26

# Recreate AIxSE Position Paper .tex from PDF with All Audit Fixes

## CONTEXT
The original AIxSE position paper exists ONLY as a PDF (no .tex source survived).
The PDF is the layout/content reference.
The corrected Zenodo spec at specs/SYSTEM_SPECIFICATION_v3.tex is the SOURCE OF TRUTH
for all equations, property definitions, and citations.

## TASK
Recreate papers/AIxSE_Position_Paper_v2.tex from scratch:
1. Match the structure and content of the original PDF
2. Apply ALL corrections from specs/SYSTEM_SPECIFICATION_v3.tex
3. IEEE 2-column conference format, 2 pages maximum

## STEP 1: Read the corrected spec (source of truth for equations)

```bash
cat /Users/kim/code/storm-safety-proofs/specs/SYSTEM_SPECIFICATION_v3.tex
```

## STEP 2: Read the original PDF for structure reference

The PDF is at the path provided. Extract the structure:
- Title: "Open Problems in Formal Verification of Modular Compositional AI Systems"
- Author: Kim Daniel Schulte-Zurhausen, Modulith Research CIC, London, UK
- Email: research@modulith.ai
- Sections: I. Introduction, II. Architecture, III. Safety Conjectures, IV. Empirical Motivation, V. Discussion
- 6 properties, 11 references

## STEP 3: Create papers/AIxSE_Position_Paper_v2.tex

Use IEEEtran document class. Recreate the full paper content from the PDF BUT
apply these corrections:

### Correction 1: Composition equation
PDF has:
  yt = Σ(k=1 to K) wk · m_ik(x)

REPLACE with (from spec v3):
  yt = Σ(n ∈ S_t(x)) w_n(x) · m_n(x),  |S_t| = K ≪ N

The Definition must include:
- Router selects subset S(x) ⊂ {1,...,N} with |S| = K ≪ N
- Non-negative weights w_n(x) summing to 1 over S
- Explicit K ≪ N in the equation

### Correction 2: All w_k / m_{i_k} → w_n / m_n
Every reference to routing weights and modules must use consistent
w_n and m_n notation throughout the paper. No w_k, no m_{i_k}.

### Correction 3: Experiment count
PDF says "over 300" — change ALL instances to "346"

### Correction 4: PAC-Bayes citation
PDF reference [11] is Valiant 1984 and is cited for "PAC-Bayes bounds" in Property 3.
This is WRONG. Valiant 1984 = PAC learning, NOT PAC-Bayes.

Add new reference:
  D. A. McAllester, "PAC-Bayesian model averaging," in Proc. COLT, pp. 164-170, 1999.

Cite McAllester for PAC-Bayes. Keep Valiant ONLY for PAC learning if used elsewhere.

### Correction 5: Bounded Compute (Property 5)
PDF says "O(K) regardless of N" — this is imprecise.
From spec v3, use the precise formulation:
- Module execution: O(K)
- Router selection: O(N) or O(N log K)
- Total: O(N log K + K·C_mod)
- Dominated by K when C_mod >> N

The paper already has a decent Computational Cost paragraph — align it with spec v3.

### Correction 6: Property statements
Align all 6 properties with spec v3:
- Property 1 (Attribution): use w_n notation, mention MoE gating comparison
- Property 2 (Forgetting): scope to modular knowledge, backbone caveat
- Property 3 (Non-Degradation): "retraining only the router" explicit
- Property 4 (Memory Isolation): add K ≪ N requirement
- Property 5 (Bounded Compute): split router O(N) from module O(K)
- Property 6 (Monotonicity): "expected cross-entropy loss", transitivity remark

### Correction 7: MoE differentiation
After the 4 structural constraints in Section II, add a brief remark
(1-2 sentences, space permitting) distinguishing from MoE.
Pull from spec v3 lines 105-109.

### Correction 8: Revision footnote
Add footnote on page 1:
"v2: Corrected composition equation notation to explicit selection-set
formulation, bounded compute specification, experiment count (346),
and PAC-Bayes citation."

## STEP 4: Compile

```bash
cd /Users/kim/code/storm-safety-proofs/papers
pdflatex AIxSE_Position_Paper_v2.tex
pdflatex AIxSE_Position_Paper_v2.tex
```

## STEP 5: Verify (CRITICAL — check ALL of these)

```bash
# Must use selection-set notation, NOT k=1 to K
grep -c "S_t" AIxSE_Position_Paper_v2.tex && echo "Selection set: ✓"
grep -c "k=1" AIxSE_Position_Paper_v2.tex && echo "FAIL: old MoE notation still present!"

# Must cite McAllester for PAC-Bayes
grep "McAllester\|mcallester" AIxSE_Position_Paper_v2.tex && echo "McAllester: ✓"

# Must say 346 not 300
grep "346" AIxSE_Position_Paper_v2.tex && echo "Experiment count: ✓"
grep "300" AIxSE_Position_Paper_v2.tex && echo "WARNING: old count still present"

# Must use w_n not w_k throughout
grep "w_k" AIxSE_Position_Paper_v2.tex && echo "FAIL: old w_k notation!"
grep "w_n" AIxSE_Position_Paper_v2.tex && echo "w_n notation: ✓"

# Check page count
echo "Check PDF is exactly 2 pages"
```

If paper exceeds 2 pages: trim Discussion section or Empirical Motivation prose.
Equations and property statements are more important than prose.

## STEP 6: Commit

```bash
cd /Users/kim/code/storm-safety-proofs
git add papers/AIxSE_Position_Paper_v2.tex papers/AIxSE_Position_Paper_v2.pdf
git commit -m "Recreate AIxSE paper .tex with all audit fixes: selection-set equation, McAllester citation, 346 experiments, bounded compute split, MoE differentiation"
```

## HARD CONSTRAINTS
1. Equation MUST use S_t(x) selection-set notation — if k=1 to K appears, REJECT
2. PAC-Bayes cites McAllester 1999 — NOT Valiant 1984
3. PUBLIC LANGUAGE ONLY — never "STORM", "IFB", "byte bridge", "neural database"
4. Must fit 2 pages IEEE format — trim prose if needed, never trim equations
5. All notation matches spec v3 (w_n, m_n, S_t, N, K)
6. 346 experiments, not 300
7. Do NOT modify any other files — only create new files in papers/
