KDSZ Investments Ltd — Proprietary work | 2026-03-26

# Post-Audit Fixes — 4 Remaining Decisions

Read specs/SYSTEM_SPECIFICATION_v2.tex and apply these 4 corrections:

## Fix 1: Backbone & Forgetting Scope

In the Guaranteed Forgetting section, add this clarification after the property statement:

"The forgetting guarantee applies to knowledge contained in frozen modules. The shared backbone is a fixed embedding space containing no domain-specific knowledge — empirically confirmed by the observation that the backbone alone produces degenerate output. Formally, the property bounds I(D_j; Y_t) contributed through modular composition; residual information in the shared backbone, if any, is bounded by the backbone's representational capacity, which is orders of magnitude smaller than the module pool."

Do NOT weaken the property. Strengthen it by being precise about scope.

## Fix 2: Compositional Monotonicity Metric

In the Compositional Monotonicity conjecture, replace "expected loss" with:

"expected cross-entropy loss over domain-specific held-out evaluation sets"

This is the training objective, it's continuous (unlike accuracy), and PPL is derived from it. Be explicit.

## Fix 3: Keep Abstract Thresholds, Add Experimental Reference

Do NOT change the abstract ε, γ in the conjectures. They are mathematical generalisations.

Add a footnote or remark after the conjectures section:

"In our proof-of-concept system (N=44, K=6, 15 training domains, 346 experiments), we observe: attribution correlation r > 0.9, forgetting residual < 1\% of pre-removal accuracy, non-degradation bound < 2\% across three expansion orderings, and inference time constant in N at fixed K=6. These values instantiate the abstract thresholds above and motivate the conjecture that they hold generally for systems satisfying the structural constraints."

## Fix 4: PAC-Bayes Citation

Find the citation to Valiant 1984 that is used in the context of "PAC-Bayes bounds" (around line 161). This is WRONG:
- Valiant 1984 = PAC learning (Probably Approximately Correct)
- PAC-Bayes = McAllester 1999 ("PAC-Bayesian model averaging")

Replace with:
```bibtex
@inproceedings{mcallester1999pac,
  title={PAC-Bayesian model averaging},
  author={McAllester, David A},
  booktitle={Proceedings of the twelfth annual conference on Computational learning theory},
  pages={164--170},
  year={1999}
}
```

Keep the Valiant citation if it's used elsewhere correctly for PAC learning. Only fix the PAC-Bayes reference.

## After all fixes:

```bash
cd specs && pdflatex SYSTEM_SPECIFICATION_v2.tex && pdflatex SYSTEM_SPECIFICATION_v2.tex && cd ..
git add specs/SYSTEM_SPECIFICATION_v2.tex specs/SYSTEM_SPECIFICATION_v2.pdf
git commit -m "Post-audit fixes: scope forgetting to modular knowledge, specify CE loss metric, add experimental thresholds footnote, fix PAC-Bayes citation (was Valiant 1984, should be McAllester 1999)"
git push
```

## Revision Notes

Add a revision note at the end of the document (before references) as a new subsection:

"\subsection*{Revision History}
\textbf{v2.1} (March 2026): Corrected composition equation notation to use explicit selection-set formulation distinguishable from standard mixture-of-experts. Clarified bounded compute property to distinguish router complexity O(N) from module execution complexity O(K). Added explicit assumptions, MoE differentiation, and experimental threshold instantiation. Fixed PAC-Bayes citation. Scoped forgetting guarantee to modular knowledge with backbone capacity bound."

This revision note shows mathematical rigour — we caught our own errors and corrected them precisely.

## HARD CONSTRAINTS
- PUBLIC LANGUAGE ONLY — never use "STORM", "IFB", "byte bridge"
- Do NOT weaken any property — strengthen by being more precise
- Compile twice (for references) and verify PDF renders correctly
- Commit and push when done
