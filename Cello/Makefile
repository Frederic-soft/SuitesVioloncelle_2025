PDFLATEX = pdflatex -shell-escape
# ABCPS    = abcm2ps
# ABCMIDI  = abc2svgmidi
# ABCPP    = abcpp
# PSPDF    = ps2pdf -dPDFSETTINGS=/prepress
# CPPPS    = -ABCM2PS -SCORE
RM       = rm -f

DOCUMENT = Suites
SOURCES = I-1-prelude.abc \
          I-2-allemande.abc \
          I-3-courante.abc \
          I-4-sarabande.abc \
          I-5-menuets.abc \
          I-6-gigue.abc \
          II-1-prelude.abc \
          II-2-allemande.abc \
          II-3-courante.abc \
          II-4-sarabande.abc \
          II-5-menuets.abc \
          II-6-gigue.abc \
          III-1-prelude.abc \
          III-2-allemande.abc \
          III-3-courante.abc \
          III-4-sarabande.abc \
          III-5-bourrees.abc \
          III-6-gigue.abc \
          IV-1-prelude.abc \
          IV-2-allemande.abc \
          IV-3-courante.abc \
          IV-4-sarabande.abc \
          IV-5-bourrees.abc \
          IV-6-gigue.abc \
          V-1-prelude.abc \
          V-2-allemande.abc \
          V-3-courante.abc \
          V-4-sarabande.abc \
          V-5-gavottes.abc \
          V-6-gigue.abc \
          VI-1-prelude.abc \
          VI-2-allemande.abc \
          VI-3-courante.abc \
          VI-4-sarabande.abc \
          VI-5-gavottes.abc \
          VI-6-gigue.abc \

INCLUDES = inc/a4paper.abc \
           inc/ps-header.abc \
           inc/by-sa_small.pdf \
           inc/by-sa.pdf \
           inc/by.pdf \
           inc/sa.pdf \
           inc/share.pdf \
           inc/remix.pdf \
           inc/accord.abc \
           inc/accord.pdf \
           inc/CelloCloseup1_gray.png

MIDI = $(SOURCES:.abc=.mid)
XML  = $(SOURCES:.abc=.xml)
PDFS = $(SOURCES:.abc=.pdf)

BOOKSRC = $(DOCUMENT)_inside.tex $(DOCUMENT)_cover.tex
BOOKPDF = $(BOOKSRC:.tex=.pdf)

all :
	make pdf
	make xml
	make midi

press :
	make cover
	make inside

# Use MuseScore to convert to MIDI from MusicXML format
%.mid : %.abc
	abc2svgxml "$<" -o "$*_midi" -s %M -real_sound 1
	mscore --export-to "$@" "$*_midi_1.musicxml" && rm "$*_midi_1.musicxml"

# # Use abcmidi to convert to MIDI
# %.mid : %.abc
# 	abc2svgmidi $< $* --ss-pref %M && mv $*_1.mid $@

# Use --ss-pref %M for producing XML and MIDI output
%.xml : %.abc
	abc2svgxml $< -s "%M"; mv "$*_1.musicxml" $@
#	$(ABCPP) $(CPPPS) $< > `dirname $<`/`basename $< .abc`_pp.abc
#	abc2xml `dirname $<`/`basename $< .abc`_pp.abc > $@ && rm `dirname $<`/`basename $< .abc`_pp.abc

# Use --ss-pref %P for producing PDF output
%.pdf : %.abc
	abc2svgpdf $< --ss-pref "%P"

%.pdf : %.tex
	$(PDFLATEX) $<

$(DOCUMENT).pdf : $(DOCUMENT).tex $(DOCUMENT)-core.pdf
	$(PDFLATEX) $(DOCUMENT).tex
	$(PDFLATEX) $(DOCUMENT).tex

$(DOCUMENT)-core.pdf : $(DOCUMENT)-core.abc $(SOURCES)

pdf : $(DOCUMENT).pdf

midi : $(MIDI)

xml : $(XML)

zip : $(DOCUMENT).zip

cover : $(DOCUMENT)_cover.pdf

inside : $(DOCUMENT)_inside.pdf

$(DOCUMENT)_cover.pdf : $(DOCUMENT)_cover.tex $(DOCUMENT).pdf
	$(PDFLATEX) $(DOCUMENT)_cover.tex

$(DOCUMENT)_inside.pdf : $(DOCUMENT)_inside.tex $(DOCUMENT).pdf
	$(PDFLATEX) $(DOCUMENT)_inside.tex

$(DOCUMENT).zip : $(DOCUMENT).pdf $(MIDI) $(XML) $(DOCUMENT).tex $(BOOKSRC) Makefile $(SOURCES) $(DOCUMENT)-core.abc $(INCLUDES)
	$(RM) $(DOCUMENT).zip
	zip -r $(DOCUMENT).zip $(DOCUMENT).pdf $(MIDI) $(XML) $(DOCUMENT).tex $(BOOKSRC) Makefile $(SOURCES) $(DOCUMENT)-core.abc $(INCLUDES)

clean :
	$(RM) *.log *.aux *.ps *.synctex.gz $(DOCUMENT)-core.pdf abcsvgtmp/*.* *.abcpp $(PDFS)

clobber : clean
	$(RM) $(DOCUMENT).pdf $(BOOKPDF)
	$(RM) *.mid *.xml
