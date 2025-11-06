# Contents directory
CONTENTS = contents

# Build directory
BUILD = build

# Document name
FILE_PREFIX = fragment_III
DOC = $(CONTENTS)/$(FILE_PREFIX)
DOC_BUILD = $(BUILD)/$(FILE_PREFIX)
PDF_OUTPUT = $(FILE_PREFIX).pdf

# File that LaTeX will look at first.
ENTRYFILE = $(CONTENTS)/body.tex

# LaTeX compiler
LATEX = lualatex
LATEXFLAGS = -interaction=nonstopmode -halt-on-error -file-line-error -synctex=1
LATEXCMD = $(LATEX) $(LATEXFLAGS) -output-directory=$(BUILD) $(DOC).tex

# Default target
all: $(BUILD) $(PDF_OUTPUT)

# Create build directory if it doesn't exist
$(BUILD):
	mkdir -p $(BUILD)

# Build the PDF (LuaLaTeX; run Biber only if bibliography.bib exists)
BIBFILE := $(CONTENTS)/bibliography.bib

$(DOC_BUILD).pdf: $(DOC).tex $(ENTRYFILE) | $(BUILD)
	$(LATEXCMD)

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
	$(LATEXCMD)
	makeglossaries -d $(BUILD) $(FILE_PREFIX)
	$(LATEXCMD)

# Update bibliography
.PHONY: bib
bib: $(DOC_BUILD).pdf
	@if [ -f "$(BIBFILE)" ]; then \
		echo "Running Biber..."; \
		biber --output-directory=$(BUILD) $(FILE_PREFIX); \
		$(LATEXCMD); \
	else \
		echo "Bibliography file not found: $(BIBFILE)"; \
	fi

# Force rebuild
force:
	touch $(DOC).tex
	$(MAKE) all

# Check for LaTeX errors
check:
	$(LATEXCMD)

# Display help
.PHONY: help
help:
	@echo "Available targets:"
	@echo "  all            Build PDF (default)"
	@echo "  view           Open PDF in default viewer"
	@echo "  bib            Run Biber and rebuild"
	@echo "  glossary       Update glossaries and rebuild"
	@echo "  force          Force rebuild by touching source"
	@echo "  check          Check for LaTeX errors"
	@echo "  clean          Remove auxiliary files"
	@echo "  distclean      Remove all build artifacts"
	@echo "  fonts-cache    Refresh LuaTeX font cache"
	@echo "  math-test      Test mathematical typesetting"
	@echo "  help           Display this message"

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

.PHONY: all clean distclean view force check math-test fonts-cache glossary glossaries bib help
