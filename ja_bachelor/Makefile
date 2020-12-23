# makefile for master thesis compilings

# target file
MAIN = main.tex

# other formats
AUX = $(MAIN:.tex=)
DVI = $(MAIN:.tex=.dvi)
PS  = $(MAIN:.tex=.ps)
PDF = $(MAIN:.tex=.pdf)

# for partitions
IN_FILE = introduction.tex
IN_CHAP = 0
TOP = partition.tex
TOPDVI = $(TOP:.tex=.dvi)

#programs
TEX    = platex
BIBTEX = pbibtex
DVIPS  = dvips -f
DVIPDF = dvipdfmx
XDVI   = xdvi
LPR    = lpr
NUP    = psnup -2
MV     = mv -f
RM     = rm -f
CP     = cp
SED    = sed

# TeX sources
TEXS = abstract.tex \
	introduction.tex \
	background.tex \
	proposal.tex \
	design.tex \
	implement.tex \
	evaluation.tex \
	conclusion.tex \
	thanks.tex\
	reference.bib

all: verbose

verbose: $(DVI)

silent:
	rm -f tex.err
	$(TEX) $(MAIN) > tex.err
	$(BIBTEX) $(AUX) > tex.err
	$(TEX) $(MAIN) > tex.err
	$(TEX) $(MAIN) > tex.err

drafts: $(TOPDVI)

$(TOPDVI): $(IN_FILE) $(TOP)
	$(TEX) $(TOP)
	$(TEX) $(TOP)

$(TOP): Makefile
	$(SED) -e 's/\(\\include\){[0-9a-zA-Z]*}/\1{$(IN_FILE:.tex=)}/' \
	-e 's/\(\\setcounter{chapter}\){[0-9]*}/\1{$(IN_CHAP)}/' \
	$(TOP) > $(TOP:=.tmp)
	$(MV) $(TOP:=.tmp) $(TOP)

ps: $(PS)

pdf: $(PDF)

show: $(DVI)
	$(XDVI) $(DVI)

print: $(DVI)
	$(DVIPS) $(DVI) | $(LPR)

dprint: $(DVI)
	$(DVIPS) $(DVI) | $(NUP) | $(LPR)

clean:
	$(RM) *.aux *.toc *.bbl *.blg *.dvi *.lof *.log *.lot


# meta rules
.PHONEY: all clean show print ps pdf dprint
.SUFFIXES: .tex .dvi .ps .pdf

# dependency
main.dvi: $(TEXS)

# suffix rules
$(DVI): $(MAIN)
	$(TEX) $(MAIN)
	$(BIBTEX) $(AUX)
	$(TEX) $(MAIN)
	$(TEX) $(MAIN)

.dvi.pdf:
	$(DVIPDF) $<

.dvi.ps:
	$(DVIPS) $< > $(<:.dvi=.ps)

# end of Makefile
