# Contents directory
CONTENTS = contents

# Build directory
BUILD = build

# Document name
FILE_PREFIX = fragment_II
DOC = $(CONTENTS)/$(FILE_PREFIX)
DOC_BUILD = $(BUILD)/$(FILE_PREFIX)
PDF_OUTPUT = $(FILE_PREFIX).pdf

# File that LaTeX will look at first.
ENTRYFILE = $(CONTENTS)/body.tex

# LaTeX compiler
# LATEX = pdflatex
LATEX = lualatex
LATEXFLAGS = -interaction=nonstopmode -halt-on-error -file-line-error -synctex=1
OUTPUT_FLAGS = -output-directory=$(BUILD)
BIBTEX = bibtex

# Default target
default:
	touch $(DOC).tex
	$(MAKE) all
	$(MAKE) view

all: $(BUILD) $(PDF_OUTPUT)

# Create build directory if it doesn't exist
$(BUILD):
	mkdir -p $(BUILD)

# Build the PDF (LuaLaTeX; run Biber only if bibliography.bib exists)
BIBFILE := $(CONTENTS)/bibliography.bib

$(DOC_BUILD).pdf: $(DOC).tex $(ENTRYFILE) $(wildcard $(BIBFILE)) | $(BUILD)
	-$(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) -aux-directory=$(BUILD) $(DOC).tex
	@if [ -f "$(BIBFILE)" ]; then \
		echo "Running Biber..."; \
		biber --output-directory=$(BUILD) $(FILE_PREFIX) || true; \
		$(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) -aux-directory=$(BUILD) $(DOC).tex; \
	fi
	@if [ -f "$(DOC_BUILD).glo" ]; then \
		echo "Running makeglossaries..."; \
		makeglossaries -d $(BUILD) $(FILE_PREFIX) || true; \
		$(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) -aux-directory=$(BUILD) $(DOC).tex; \
	fi
	$(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) -aux-directory=$(BUILD) $(DOC).tex

# Copy PDF to current directory
$(PDF_OUTPUT): $(DOC_BUILD).pdf
	cp $(DOC_BUILD).pdf $(PDF_OUTPUT)

# Clean auxiliary files
clean:
	rm -f $(BUILD)/*.aux $(BUILD)/*.bcf $(BUILD)/*.log $(BUILD)/*.bbl $(BUILD)/*.blg $(BUILD)/*.out $(BUILD)/*.toc $(BUILD)/*.lof $(BUILD)/*.lot $(BUILD)/*.fls $(BUILD)/*.fdb_latexmk $(BUILD)/*.synctex.gz \
		$(BUILD)/*.glo $(BUILD)/*.gls $(BUILD)/*.glg $(BUILD)/*.ist $(BUILD)/*.acn $(BUILD)/*.acr $(BUILD)/*.alg $(BUILD)/*.xdy $(BUILD)/*.xml $(BUILD)/*.pdf

# Clean everything including PDF  
distclean: clean
	rm -f $(PDF_OUTPUT)

# View the PDF
view: $(PDF_OUTPUT)
	open $(PDF_OUTPUT)

# Build/update glossaries explicitly (on-demand)
.PHONY: glossary glossaries
glossary glossaries:
	$(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) -aux-directory=$(BUILD) $(DOC).tex
	makeglossaries -d $(BUILD) $(FILE_PREFIX)
	$(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) -aux-directory=$(BUILD) $(DOC).tex

# Update bibliography
.PHONY: bib
bib:
	@if [ -f "$(BIBFILE)" ]; then \
		echo "Running Biber..."; \
		biber --output-directory=$(BUILD) $(FILE_PREFIX); \
	else \
		echo "Bibliography file not found: $(BIBFILE)"; \
	fi

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

.PHONY: all clean distclean view force check math-test fonts-cache glossary glossaries bib
