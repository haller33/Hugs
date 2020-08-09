# demos/Makefile.  Generated from Makefile.in by configure.
# Makefile (just install) for Hugs demo programs
# (this should be a POSIX 1003.2-1992 Makefile)

# These variables determine where various parts of the Hugs system are
# installed.  (They are ignored in Windows or DOS.)
# Binaries are installed in $(bindir); libraries go in $(hugsdir)/libraries
prefix		= /usr/local
exec_prefix	= ${prefix}
bindir		= ${exec_prefix}/bin
libdir		= ${exec_prefix}/lib
datarootdir	= ${prefix}/share
datadir		= ${datarootdir}
mandir          = ${datarootdir}/man
hugsdir		= ${libdir}/hugs

RM		= /usr/bin/rm -f

INSTALL		= ../install-sh
INSTALL_DATA	= /bin/cp

all:

install: all
	$(INSTALL) -d $(DESTDIR)$(hugsdir)/demos
	$(INSTALL) -d $(DESTDIR)$(hugsdir)/demos/prolog
	$(INSTALL_DATA) ../demos/*.*           $(DESTDIR)$(hugsdir)/demos
	$(INSTALL_DATA) ../demos/prolog/*.*    $(DESTDIR)$(hugsdir)/demos/prolog
	$(INSTALL_DATA) ../demos/prolog/readme $(DESTDIR)$(hugsdir)/demos/prolog
	$(INSTALL_DATA) ../demos/prolog/stdlib $(DESTDIR)$(hugsdir)/demos/prolog

clean:
	$(RM) *~

distclean: clean
	$(RM) Makefile

veryclean: distclean
