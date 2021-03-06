# docs/users_guide/Makefile.  Generated from Makefile.in by configure.
# Makefile for the Hugs 98 User's Guide
# (this should be a POSIX 1003.2-1992 Makefile)

INSTALL		= ../../install-sh
CP		= /usr/bin/cp
RM		= /usr/bin/rm -f
DOCBOOK2HTML	= 
DOCBOOK2DVI	= 
DOCBOOK2PDF	= 
DOCBOOK2PS	= 

prefix		= /usr/local
exec_prefix	= ${prefix}
libdir		= ${exec_prefix}/lib
datarootdir	= ${prefix}/share
datadir		= ${datarootdir}
hugsdir		= ${libdir}/hugs

SGMLDocWays	= html

ROOT		= users_guide
XML_FILES	= $(ROOT).xml faq.xml haskell98.xml hugs_ghc.xml \
		  hugs_only.xml introduction.xml license.xml \
		  miscellaneous.xml options.xml others.xml using_hugs.xml
SGML_FILES		= $(ROOT).sgml faq.sgml haskell98.sgml hugs_ghc.sgml \
		  hugs_only.sgml introduction.sgml license.sgml \
		  miscellaneous.sgml options.sgml others.sgml using_hugs.sgml
CSS_FILE	= hugs-ug.css
DSL_FILE	= hugs-ug.dsl
IMAGE		= winhugs.png

# Don't use $(ROOT)/index.html, because depending on $(DOCBOOK2HTML) it
# could be $(ROOT)/$(ROOT).html instead.
HTML		= $(ROOT)/license.html

%.sgml: %.xml xml2sgml-no-cr.sed
	LC_ALL=C sed -f xml2sgml-no-cr.sed $*.xml >$@

all: $(SGMLDocWays)

install: $(SGMLDocWays)
	if test -f $(HTML); \
	then	$(INSTALL) -d $(DESTDIR)$(hugsdir)/docs/users_guide; \
		$(CP) users_guide/*.html users_guide/$(CSS_FILE) users_guide/$(IMAGE) $(DESTDIR)$(hugsdir)/docs/users_guide; \
	fi
	for ext in dvi pdf ps;\
	do	if test -f $(ROOT).$$ext;\
		then	$(CP) $(ROOT)$$ext $(DESTDIR)$(hugsdir)/docs;\
		fi;\
	done

clean:
	$(RM) $(SGML_FILES)
	$(RM) *~
	$(RM) xml2sgml-no-cr.sed

distclean: clean
	$(RM) $(ROOT).dvi $(ROOT).ps $(ROOT).pdf
	$(RM) -r $(ROOT)
	$(RM) Makefile

veryclean: distclean

# MSYS sed cannot handle carriage returns in sed scripts,
# so generate one that is guaranteed not to have any.
# (The wierd sed command below is a portable way of stripping CRs.)
xml2sgml-no-cr.sed: xml2sgml.sed
	cat xml2sgml.sed | LC_ALL=C sed 's/[^	 -~]//g' >$@

html: $(HTML)

dvi: $(ROOT).dvi

ps: $(ROOT).ps

pdf: $(ROOT).pdf

htmlhelp: $(ROOT).chm

$(HTML): $(SGML_FILES) $(DSL_FILE) $(CSS_FILE)
	if test -n '$(DOCBOOK2HTML)'; then \
		$(RM) -r $(ROOT); \
		mkdir $(ROOT); \
		$(CP) $(CSS_FILE) $(ROOT); \
		$(CP) $(IMAGE) $(ROOT); \
		$(DOCBOOK2HTML) --dsl $(DSL_FILE) $(ROOT).sgml; \
	fi

$(ROOT).dvi: $(SGML_FILES)
	test -z '$(DOCBOOK2DVI)' || $(DOCBOOK2DVI) $(ROOT).sgml

$(ROOT).ps: $(SGML_FILES)
	test -z '$(DOCBOOK2PS)' || $(DOCBOOK2PS) $(ROOT).sgml

$(ROOT).pdf: $(SGML_FILES)
	test -z '$(DOCBOOK2PDF)' || $(DOCBOOK2PDF) $(ROOT).sgml

# TODO: assumes xsltproc & hhc, without checking

$(ROOT).chm: $(ROOT)-htmlhelp/htmlhelp.hhp
	-cd $(ROOT)-htmlhelp; hhc htmlhelp.hhp
	test -f $@

$(ROOT)-htmlhelp/htmlhelp.hhp: $(XML_FILES) htmlhelp.xsl
	xsltproc htmlhelp.xsl $(ROOT).xml
	$(CP) $(IMAGE) $(ROOT)-htmlhelp
