Quick fix: Update affiliation from London to Hastings in both documents.

```bash
cd /Users/kim/code/storm-safety-proofs

# Fix spec v3
sed -i '' 's/London, UK/Hastings, UK/g' specs/SYSTEM_SPECIFICATION_v3.tex
pdflatex -output-directory=specs specs/SYSTEM_SPECIFICATION_v3.tex
pdflatex -output-directory=specs specs/SYSTEM_SPECIFICATION_v3.tex

# Fix position paper v2
sed -i '' 's/London, UK/Hastings, UK/g' papers/AIxSE_Position_Paper_v2.tex
pdflatex -output-directory=papers papers/AIxSE_Position_Paper_v2.tex
pdflatex -output-directory=papers papers/AIxSE_Position_Paper_v2.tex

# Verify
grep -n "Hastings" specs/SYSTEM_SPECIFICATION_v3.tex papers/AIxSE_Position_Paper_v2.tex
grep -n "London" specs/SYSTEM_SPECIFICATION_v3.tex papers/AIxSE_Position_Paper_v2.tex

# Commit
git add specs/SYSTEM_SPECIFICATION_v3.tex specs/SYSTEM_SPECIFICATION_v3.pdf \
        papers/AIxSE_Position_Paper_v2.tex papers/AIxSE_Position_Paper_v2.pdf
git commit -m "Update affiliation: London → Hastings, UK (registered address change)"
```

Do NOT touch the revision history — this is an administrative fix, not a mathematical correction.
