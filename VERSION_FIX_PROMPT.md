KDSZ Investments Ltd — Proprietary work | 2026-03-26

# Quick Fix: Version Management

## Step 1: Fix experiment count in current (corrected) file

In specs/SYSTEM_SPECIFICATION_v2.tex, find all instances of "260+" and replace with "346+":

```bash
cd specs
grep -n "260" SYSTEM_SPECIFICATION_v2.tex
sed -i 's/260+/346+/g' SYSTEM_SPECIFICATION_v2.tex
```

Verify the change:
```bash
grep -n "346" SYSTEM_SPECIFICATION_v2.tex
```

## Step 2: Compile the corrected version

```bash
pdflatex SYSTEM_SPECIFICATION_v2.tex
pdflatex SYSTEM_SPECIFICATION_v2.tex
```

## Step 3: Save corrected files as v3

```bash
cp SYSTEM_SPECIFICATION_v2.tex SYSTEM_SPECIFICATION_v3.tex
cp SYSTEM_SPECIFICATION_v2.pdf SYSTEM_SPECIFICATION_v3.pdf
```

## Step 4: Restore original v2 from git history (before audit)

The audit commit was 3b64890. We need the version BEFORE that:

```bash
git show 3b64890^:specs/SYSTEM_SPECIFICATION_v2.tex > SYSTEM_SPECIFICATION_v2.tex
pdflatex SYSTEM_SPECIFICATION_v2.tex
pdflatex SYSTEM_SPECIFICATION_v2.tex
```

## Step 5: Verify

```bash
# v2 should have the OLD equation (k=1 to K) and say "260+"
grep "sum_{k=1}" SYSTEM_SPECIFICATION_v2.tex && echo "v2: OLD equation ✓"
grep "260" SYSTEM_SPECIFICATION_v2.tex && echo "v2: OLD experiment count ✓"

# v3 should have the NEW equation (n ∈ S_t) and say "346+"
grep "S_t" SYSTEM_SPECIFICATION_v3.tex && echo "v3: NEW equation ✓"
grep "346" SYSTEM_SPECIFICATION_v3.tex && echo "v3: NEW experiment count ✓"
```

## Step 6: Commit

```bash
cd ..
git add specs/SYSTEM_SPECIFICATION_v2.tex specs/SYSTEM_SPECIFICATION_v2.pdf \
        specs/SYSTEM_SPECIFICATION_v3.tex specs/SYSTEM_SPECIFICATION_v3.pdf
git commit -m "Version management: restore v2 (original published), create v3 (all audit corrections + 346 experiments)"
git push
```

## Result

- v2 = original published Zenodo version (with known MoE equation error)
- v3 = fully corrected version ready for Zenodo v2 upload (fixed equations, 346 experiments, stated assumptions, MoE differentiation, revision history)
