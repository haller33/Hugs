# docs/Makefile.  Generated from Makefile.in by configure.
# Makefile for Documentation
# (this should be a POSIX 1003.2-1992 Makefile)
# Note that some documentation is in other directories.

INSTALL		= ../install-sh
RM		= /usr/bin/rm -f
CP		= /usr/bin/cp

prefix		= /usr/local
exec_prefix	= ${prefix}
bindir		= ${exec_prefix}/bin
libdir		= ${exec_prefix}/lib
datarootdir	= ${prefix}/share
datadir		= ${datarootdir}
mandir		= ${datarootdir}/man
hugsdir		= ${libdir}/hugs

all:
		cd users_guide; $(MAKE) all

# all::		server.ps hugs.man

install		:: install_man			      # manual page
install		:: install_notes		      # Readme, etc
install		:: install_guide		      # User's Guide

install_man:
		$(INSTALL) -d $(DESTDIR)$(mandir)/man1
		$(CP) hugs.1 $(DESTDIR)$(mandir)/man1

install_notes:
		$(INSTALL) -d $(DESTDIR)$(hugsdir)/docs
		$(CP) *.txt  $(DESTDIR)$(hugsdir)/docs
		$(CP) server.*  $(DESTDIR)$(hugsdir)/docs
		$(CP) ../Readme  $(DESTDIR)$(hugsdir)
		$(CP) ../License $(DESTDIR)$(hugsdir)
		$(CP) ../Credits $(DESTDIR)$(hugsdir)

install_guide:
		cd users_guide; $(MAKE) install

server.ps:	server.dvi
		dvips server.dvi -o server.ps

server.dvi:	server.tex 
		latex server.tex

hugs.man:	hugs.1
		groff -Tascii -man hugs.1 | cat -s >$@

hugs.ps:	hugs.1
		groff -Tps -man hugs.1 >$@

clean:
		$(RM) *~
		cd users_guide; $(MAKE) clean

distclean:
		$(RM) server.ps server.dvi
		$(RM) hugs.man hugs.ps
		$(RM) Makefile hugs.1
		cd users_guide; $(MAKE) distclean

veryclean:	distclean

# End of Makefile
