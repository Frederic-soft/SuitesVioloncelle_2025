PDFLATEX = pdflatex -shell-escape
# ABCPS    = abcm2ps
# ABCMIDI  = abc2midi
# ABCPP    = abcpp
# PSPDF    = ps2pdf -dPDFSETTINGS=/prepress
# CPPPS    = -ABCM2PS -SCORE
RM       = rm -f

DOCUMENT = Suites_clar
SOURCES = I-1-prelude_clar.abc \
          I-2-allemande_clar.abc \
          I-3-courante_clar.abc \
          I-4-sarabande_clar.abc \
          I-5-menuets_clar.abc \
          I-6-gigue_clar.abc \
          II-1-prelude_clar.abc \
          II-2-allemande_clar.abc \
          II-3-courante_clar.abc \
          II-4-sarabande_clar.abc \
          II-5-menuets_clar.abc \
          II-6-gigue_clar.abc \
          III-1-prelude_clar.abc \
          III-2-allemande_clar.abc \
          III-3-courante_clar.abc \
          III-4-sarabande_clar.abc \
          III-5-bourrees_clar.abc \
          III-6-gigue_clar.abc \
          IV-1-prelude_clar.abc \
          IV-2-allemande_clar.abc \
          IV-3-courante_clar.abc \
          IV-4-sarabande_clar.abc \
          IV-5-bourrees_clar.abc \
          IV-6-gigue_clar.abc \
          V-1-prelude_clar.abc \
          V-2-allemande_clar.abc \
          V-3-courante_clar.abc \
          V-4-sarabande_clar.abc \
          V-5-gavottes_clar.abc \
          V-6-gigue_clar.abc \
          VI-1-prelude_clar.abc \
          VI-2-allemande_clar.abc \
          VI-3-courante_clar.abc \
          VI-4-sarabande_clar.abc \
          VI-5-gavottes_clar.abc \
          VI-6-gigue_clar.abc \

INCLUDES = inc/a4paper.abc \
           inc/ps-header.abc \
           inc/by-sa_small.pdf \
           inc/by-sa.pdf \
           inc/by.pdf \
           inc/sa.pdf \
           inc/share.pdf \
           inc/remix.pdf \
           inc/MaBasseBuffetUt_transp.png

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

# %.mid : %.abc
# 	$(ABCPP) -ABC2MIDI -MIDI $< | $(ABCMIDI) - -o $@

# Use --ss-pref %M for producing XML and MIDI output
%.xml : %.abc
	abc2svgxml $< -s "%M"; mv "$*_1.musicxml" $@

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

xml : $(XML)

midi : $(MIDI)

zip : $(DOCUMENT).zip

cover : $(DOCUMENT)_cover.pdf

inside : $(DOCUMENT)_inside.pdf

$(DOCUMENT)_cover.pdf : $(DOCUMENT)_cover.tex $(DOCUMENT).pdf
	$(PDFLATEX) $(DOCUMENT)_cover.tex

$(DOCUMENT)_inside.pdf : $(DOCUMENT)_inside.tex $(DOCUMENT).pdf
	$(PDFLATEX) $(DOCUMENT)_inside.tex

$(DOCUMENT).zip : $(DOCUMENT).pdf $(MIDI) $(DOCUMENT).tex $(BOOKSRC) Makefile $(SOURCES) $(DOCUMENT)-core.abc $(INCLUDES)
	$(RM) $(DOCUMENT).zip
	zip -r $(DOCUMENT).zip $(DOCUMENT).pdf $(MIDI) $(DOCUMENT).tex $(BOOKSRC) Makefile $(SOURCES) $(DOCUMENT)-core.abc $(INCLUDES)

clean :
	$(RM) *.log *.aux *.ps *.synctex.gz $(DOCUMENT)-core.pdf abcsvgtmp/*.* *.abcpp $(PDFS)

clobber : clean
	$(RM) $(DOCUMENT).pdf  $(BOOKPDF)
	$(RM) *.mid *.xml

