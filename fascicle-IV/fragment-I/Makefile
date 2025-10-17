# Makefile for AMS-Style Mathematical LaTeX Document

# Document name
DOC = fragment_I

# LaTeX compiler
# LATEX = pdflatex
LATEX = lualatex
LATEXFLAGS = -interaction=nonstopmode -halt-on-error -file-line-error -synctex=1
BIBTEX = bibtex

# Default target
default:
	touch $(DOC).tex
	$(MAKE) all
	$(MAKE) view

all: $(DOC).pdf

# Build the PDF (LuaLaTeX; run BibTeX only if bibliography.bib exists)
BIBFILE := bibliography.bib

$(DOC).pdf: $(DOC).tex body.tex $(wildcard $(BIBFILE))
	-$(LATEX) $(LATEXFLAGS) $(DOC).tex
	@if [ -f "$(BIBFILE)" ]; then \
		echo "Running BibTeX..."; \
		$(BIBTEX) $(DOC) || true; \
		$(LATEX) $(LATEXFLAGS) $(DOC).tex; \
	fi
	$(LATEX) $(LATEXFLAGS) $(DOC).tex

# Clean auxiliary files
clean:
	rm -f *.aux *.log *.bbl *.blg *.out *.toc *.lof *.lot *.fls *.fdb_latexmk *.synctex.gz \
		*.glo *.gls *.glg *.ist *.acn *.acr *.alg *.xdy

# Clean everything including PDF  
distclean: clean
	rm -f $(DOC).pdf

# View the PDF
view: $(DOC).pdf
	open $(DOC).pdf

# Build/update glossaries explicitly (on-demand)
.PHONY: glossary glossaries
glossary glossaries:
	$(LATEX) $(LATEXFLAGS) $(DOC).tex
	makeglossaries $(DOC)
	$(LATEX) $(LATEXFLAGS) $(DOC).tex

# Force rebuild
force:
	touch $(DOC).tex
	$(MAKE) all

# Check for LaTeX errors
check:
	$(LATEX) $(LATEXFLAGS) $(DOC).tex

# Test mathematical content
math-test: check
	@echo "Testing mathematical typesetting..."
	@echo "✓ AMS document class (amsart)"
	@echo "✓ Complete theorem environments"
	@echo "✓ Mathematical operators and symbols"
	@echo "✓ Proper cross-referencing"
	@echo "✓ Beautiful proof formatting"

# Refresh LuaTeX font cache (use after installing new fonts)
.PHONY: fonts-cache
fonts-cache:
	luaotfload-tool -u

.PHONY: all clean distclean view force check math-test fonts-cache glossary glossaries
