WWW=$(HOME)/www/noweb
SHELL=/bin/sh
.SUFFIXES: .1 .7 .txt .ps
.1.txt: ;	nroff -man $*.1 > $*.txt
.1.ps: ;	psroff -t -man $*.1 > $*.ps
.7.txt: ;	nroff -man $*.7 > $*.txt
.7.ps: ;	psroff -t -man $*.7 > $*.ps

MANPAGES=notangle.1   cpif.1   noweb.1   nodefs.1   noroots.1   noindex.1   \
         nowebstyle.7   nowebfilters.7   nuweb2noweb.1   sl2h.1 htmltoc.1
TXTPAGES=notangle.txt cpif.txt noweb.txt nodefs.txt noroots.txt noindex.txt \
         nowebstyle.txt nowebfilters.txt nuweb2noweb.txt sl2h.txt htmltoc.txt
SRCS=$(MANPAGES) $(TXTPAGES)

NAME="name of version checked in"
CIMSG="message for version checked in"
RCSFILES=*.nw *.tex Makefile
DOVERSION=sed '/^version $$Name:/s/_/./g;/^version $$Name:/s/ $$\././;/^version $$Name:/s/$$Name: v//'

all: $(SRCS)
source: $(SRCS)
touch: $(SRCS)
	touch $(SRCS)

www: $(WWW)/guide.ps $(WWW)/guide.html

checkin:
	ci -l $(CINAME) $(CIMSG) $(RCSFILES)

$(WWW)/guide.ps: guide.ps
	cp guide.ps $(WWW)

$(WWW)/guide.html: guide.html
	cp guide.html $(WWW)

notangle.1: manpage.nw ../shell/noweave.nw docdate.nw
	notangle -t8 -Rnotangle.1  manpage.nw ../shell/noweave.nw docdate.nw | $(DOVERSION) > notangle.1

nowebstyle.7: manpage.nw ../tex/support.nw docdate.nw
	notangle -t8 -Rnowebstyle.7 manpage.nw ../tex/support.nw docdate.nw | $(DOVERSION) > nowebstyle.7

nowebfilters.7: nowebfilters.nw docdate.nw
	notangle -t8 nowebfilters.nw docdate.nw | $(DOVERSION) > nowebfilters.7

noweb.1: manpage.nw docdate.nw
	notangle -t8 -Rnoweb.1 manpage.nw docdate.nw | $(DOVERSION) > noweb.1

nodefs.1: nodefs.nw docdate.nw
	notangle -t8 nodefs.nw docdate.nw | $(DOVERSION) > nodefs.1

noroots.1: noroots.nw docdate.nw
	notangle -t8 noroots.nw docdate.nw | $(DOVERSION) > noroots.1

noindex.1: noindex.nw docdate.nw
	notangle -t8 noindex.nw docdate.nw | $(DOVERSION) > noindex.1

sl2h.1: sl2h.nw docdate.nw
	notangle -t8 sl2h.nw docdate.nw | $(DOVERSION) > sl2h.1

htmltoc.1: htmltoc.nw docdate.nw
	notangle -t8 htmltoc.nw docdate.nw | $(DOVERSION) > htmltoc.1

cpif.1: manpage.nw docdate.nw
	notangle -t8 -Rcpif.1  manpage.nw docdate.nw | $(DOVERSION) > cpif.1

nuweb2noweb.1: manpage.nw docdate.nw
	notangle -t8 -Rnuweb2noweb.1  manpage.nw docdate.nw | $(DOVERSION) > nuweb2noweb.1

wc.tex: ../../examples/wc.nw
	(cd ../../examples; noweave -n -index wc.nw) > wc.tex

techrep.dvi: techrep.tex wc.tex
	latex '\scrollmode \input '"techrep"
	while grep -s 'Rerun to get cross-references right' techrep.log; do latex '\scrollmode \input '"techrep"; done

guide.dvi: guide.tex
	latex '\scrollmode \input '"guide"
	while grep -s 'Rerun to get cross-references right' guide.log; do latex '\scrollmode \input '"guide"; done

guide.ps: guide.dvi
	dvips -D 300 -o guide.ps guide

guide.uu: guide.ps
	gzip < guide.ps > guide.ps.gz
	uuencode guide.ps.gz < guide.ps.gz > guide.uu

guide.html: guide.dvi
	sl2h guide.tex | htmltoc > guide.html

clean: ;	/bin/rm -f *.dvi *.log *.blg *~ wc.tex *.ps *.gz *.uu *.html
clobber: clean
	rm -f *.1 *.7 *.txt
