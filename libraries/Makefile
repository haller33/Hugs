# Makefile.  Generated from Makefile.in by configure.
################################################################
# Conversion of libraries from raw sources in the packages tree
# (this should be a POSIX 1003.2-1992 Makefile)
################################################################

##########################################################################
# MkDefs.  Generated from MkDefs.in by configure.
# Makefile definitions also used by the libraries

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
CP		= /usr/bin/cp

HOST		= armv7l-unknown-linux
HOST_OS		= linux
HOST_ARCH	= armv7l

DLL		= .so
BAT		= 
# end of MkDefs.  Generated from MkDefs.in by configure.
##########################################################################

# We use the autoconf-supplied install-sh to create directories
# but use /bin/cp to copy data because install-sh can't copy
# multiple files at once.

INSTALL		= ../install-sh
INSTALL_DATA	= /bin/cp
CPPFLAGS	= -D__HUGS__ -D$(HOST_OS)_HOST_OS -D$(HOST_OS)_TARGET_OS

BUILD_DIR	= ../hugsdir

HEADERS		= ../src/HsFFI.h
FP_HEADERS	= ../src/platform.h include/MachDeps.h include/ghcconfig.h
CPPHS_SRC	= ../cpphs
HSC2HS_SRC	= ../hsc2hs

# set the path both for runhugs and any ffihugs invoked by Cabal
# HUGSDIR gives the location of HsFFI.h
HUGS_SETUP	= HUGSFLAGS=-P../libraries/bootlib HUGSDIR=../hugsdir ../src/runhugs -98 ../packages/Cabal/examples/hapax.hs

INCLUDES	= $(BUILD_DIR)/include/HsFFI.h
# represents header files used by the base, network and unix packages
FP_INCLUDES	= ../ghc/includes/ghcconfig.h

# We use a bootstrap procedure to construct the libraries:
#
# 1. Preprocess hugsbase, base, haskell98 and Cabal into bootlibs
#    using the shell script make_bootlib.  The result is incomplete,
#    in particular because hsc2hs wasn't available yet, but it's enough
#    to get ffihugs, runhugs, cpphs, hsc2hs and Cabal working,
#
# 2. Build cpphs and then hsc2hs into ../hugsdir.
#
# 3. Build all the packages into ../hugsdir, using cpphs, hsc2hs and Cabal.
#
# Unfortunately that means building (and configuring) base twice.

CPPHS		= tools/cpphs
HSC2HS		= tools/hsc2hs
CPPHS_FILE	= $(CPPHS)$(BAT)
HSC2HS_FILE	= $(HSC2HS)$(BAT)
MAKE_BOOT	= tools/make_bootlib
CONVERT		= tools/convert_libraries
MAKE_OLDLIB	= tools/make_oldlib
BOOTLIB		= bootlib/.stamp
LIBS		= $(BUILD_DIR)/.stamp

all: $(LIBS)

$(LIBS): $(BOOTLIB) $(INCLUDES) $(CPPHS_FILE) $(HSC2HS_FILE) $(CONVERT) $(MAKE_OLDLIB)
	cd ..; HOST='$(HOST)' PREFIX='$(prefix)' libraries/$(CONVERT) fptools hugsdir
	$(RM) -r $(BUILD_DIR)/oldlib
	cd ..; libraries/$(MAKE_OLDLIB) fptools hugsdir
	echo timestamp for libraries >$@

$(HSC2HS): $(BUILD_DIR)/programs/hsc2hs/Main.hs
	(echo '#! /bin/sh'; \
	 echo "rootdir='`cd ..; pwd`'"; \
	 echo '$$rootdir/src/runhugs -98 -P$$rootdir/libraries/bootlib $$rootdir/hugsdir/programs/hsc2hs/Main.hs -I$$rootdir/hugsdir/include -t$$rootdir/hsc2hs/template-hsc.h "$$@"') >$@
	chmod 755 $@

$(HSC2HS).bat: $(BUILD_DIR)/programs/hsc2hs/Main.hs
	(echo '@echo off'; \
	 echo "set rootdir=`cd ..; src/runhugs -Plibraries/bootlib libraries/pwd.hs`"; \
	 echo '%rootdir%/src/runhugs -98 -P%rootdir%/libraries/bootlib %rootdir%/hugsdir/programs/hsc2hs/Main.hs -I%rootdir%/hugsdir/include -t%rootdir%/hsc2hs/template-hsc.h %*') >$@

$(BUILD_DIR)/programs/hsc2hs/Main.hs: $(HSC2HS_SRC)/Main.hs $(BOOTLIB) $(INCLUDES) $(CPPHS_FILE)
	cd $(HSC2HS_SRC); $(HUGS_SETUP) configure --verbose --hugs --prefix='$(prefix)' --scratchdir='$(BUILD_DIR)' --with-compiler=../src/ffihugs --with-cpphs='../libraries/tools/cpphs$(BAT)'
	cd $(HSC2HS_SRC); $(HUGS_SETUP) build --verbose
	$(RM) -r $(BUILD_DIR)/autogen

$(CPPHS): $(BUILD_DIR)/programs/cpphs/Main.hs
	(echo '#! /bin/sh'; \
	 echo "rootdir='`cd ..; pwd`'"; \
	 echo '$$rootdir/src/runhugs -P$$rootdir/libraries/bootlib $$rootdir/hugsdir/programs/cpphs/Main.hs "$$@"') >$@
	chmod 755 $@

$(CPPHS).bat: $(BUILD_DIR)/programs/cpphs/Main.hs
	(echo '@echo off'; \
	 echo "set rootdir=`cd ..; src/runhugs -Plibraries/bootlib libraries/pwd.hs`"; \
	 echo '%rootdir%/src/runhugs -P%rootdir%/libraries/bootlib %rootdir%/hugsdir/programs/cpphs/Main.hs %*') >$@

$(BUILD_DIR)/programs/cpphs/Main.hs: $(BOOTLIB) $(INCLUDES)
	cd $(CPPHS_SRC); $(HUGS_SETUP) configure --verbose --hugs --prefix='$(prefix)' --scratchdir='$(BUILD_DIR)/packages/cpphs' --with-compiler=../src/ffihugs
	cd $(CPPHS_SRC); $(HUGS_SETUP) build --verbose
	$(RM) -r $(BUILD_DIR)/packages/cpphs/autogen
	mkdir -p $(BUILD_DIR)/programs
	mv $(BUILD_DIR)/packages/cpphs/programs/cpphs $(BUILD_DIR)/programs
	rmdir $(BUILD_DIR)/packages/cpphs/programs

$(BOOTLIB): hugsbase/Hugs/*.* $(MAKE_BOOT) $(INCLUDES) $(FP_INCLUDES)
	CPPFLAGS='$(CPPFLAGS)' $(MAKE_BOOT)
	echo timestamp for bootlib >$@

$(INCLUDES): $(HEADERS)
	mkdir -p $(BUILD_DIR)/include
	$(CP) $(HEADERS) $(BUILD_DIR)/include

$(FP_INCLUDES): $(FP_HEADERS)
	mkdir -p ../ghc/includes
	$(CP) $(FP_HEADERS) ../ghc/includes

clean:
	$(RM) LibStatus
	$(RM) a.out
	$(RM) *~
	$(RM) -r bootlib
	$(RM) hugsbase/.installed-pkg-config
	$(RM) ../cpphs/.installed-pkg-config
	$(RM) ../packages/*/.installed-pkg-config
	find ../packages -name \*.hsc | sed 's/c$$//' | xargs $(RM)
	$(RM) -r ../ghc/includes
# package configuration is part of the build process, so clean it here
	$(RM) hugsbase/.setup-config
	$(RM) ../cpphs/.setup-config
	$(RM) ../hsc2hs/.setup-config
	$(RM) ../packages/*/.setup-config
	$(RM) ../packages/*/config.log
	$(RM) ../packages/*/config.status
	$(RM) -r ../packages/*/autom4te.cache
	find ../packages -name \*.in | sed 's/\.in$$//' | xargs $(RM)

distclean: clean
	$(RM) $(HSC2HS_FILE)
	$(RM) $(CPPHS_FILE)
	$(RM) -r $(BUILD_DIR)
	$(RM) Makefile tools/config include/ghcconfig.h
	$(RM) config.log config.status
	$(RM) -r autom4te.cache

veryclean: distclean

install: all
	$(INSTALL) -d $(DESTDIR)$(hugsdir)/include
	$(INSTALL_DATA) $(BUILD_DIR)/include/* $(DESTDIR)$(hugsdir)/include
	$(INSTALL) -d $(DESTDIR)$(hugsdir)/oldlib
	$(INSTALL_DATA) $(BUILD_DIR)/oldlib/* $(DESTDIR)$(hugsdir)/oldlib
	(cd $(BUILD_DIR); find packages -type f -print) | while read file;\
		do	dir=`dirname $$file`;\
			case $$file in \
			*.c)	;;\
			*) \
				$(INSTALL) -d $(DESTDIR)$(hugsdir)/$$dir;\
				$(INSTALL) -c -m 0644 $(BUILD_DIR)/$$file $(DESTDIR)$(hugsdir)/$$file;;\
			esac;\
		done
	(cd $(BUILD_DIR); find demos -type f -print) | while read file;\
		do	dir=`dirname $$file`;\
			$(INSTALL) -d $(DESTDIR)$(hugsdir)/$$dir;\
			$(INSTALL) -c -m 0644 $(BUILD_DIR)/$$file $(DESTDIR)$(hugsdir)/$$file;\
		done
	(cd $(BUILD_DIR); find programs -type f -print) | while read file;\
		do	dir=`dirname $$file`;\
			$(INSTALL) -d $(DESTDIR)$(hugsdir)/$$dir;\
			$(INSTALL) -c -m 0644 $(BUILD_DIR)/$$file $(DESTDIR)$(hugsdir)/$$file;\
		done
	(echo '#! /bin/sh'; \
	 echo 'runhugs $(hugsdir)/programs/cpphs/Main.hs "$$@"') >$(DESTDIR)$(bindir)/cpphs-hugs
	chmod 755 $(DESTDIR)$(bindir)/cpphs-hugs
# install template file where hsc2hs-hugs expects to find it
	hsc2hs_data=`sed -n 's/^datadir *= "\(.*\)"/\1/p' $(BUILD_DIR)/programs/hsc2hs/Paths_hsc2hs.hs`; \
	  $(INSTALL) -d $(DESTDIR)$$hsc2hs_data; \
	  $(INSTALL) -c -m 0644 ../hsc2hs/template-hsc.h $(DESTDIR)$$hsc2hs_data/template-hsc.h
	(echo '#! /bin/sh'; \
	 echo 'runhugs -98 $(hugsdir)/programs/hsc2hs/Main.hs -I$(hugsdir)/include "$$@"') >$(DESTDIR)$(bindir)/hsc2hs-hugs
	chmod 755 $(DESTDIR)$(bindir)/hsc2hs-hugs

LibStatus: $(LIBS)
	tools/test_libraries ../packages >$@
