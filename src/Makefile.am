# Nasty hack; there seems to be no other way of disabling libtool for the
# link of the main executable...
if PLUGINS
MYLINK = $(LIBTOOL) --mode=link --tag=CC $(CCLD)
else
MYLINK = $(CCLD)
endif

if GUI_CLIENT
GUISUBDIR = gui_client
endif
if CURSES_CLIENT
CURSESSUBDIR = curses_client
endif
if GTKPORT
GTKPORTSUBDIR = gtkport
endif
if CURSESPORT
CURSESPORTSUBDIR = cursesport
endif

SUBDIRS = $(GUISUBDIR) $(CURSESSUBDIR) $(GTKPORTSUBDIR) $(CURSESPORTSUBDIR) plugins
dopewars_LDADD = @GUILIB@ @CURSESLIB@ @GTKPORTLIB@ @CURSESPORTLIB@ @GTK_LIBS@ @LTLIBINTL@ @WNDRES@ @PLUGOBJS@ @PLUGLIBS@ @GLIB_LIBS@ @LIBCURL@
dopewars_DEPENDENCIES = @GUILIB@ @CURSESLIB@ @GTKPORTLIB@ @CURSESPORTLIB@ @WNDRES@ @PLUGOBJS@

bin_PROGRAMS = dopewars
dopewars_SOURCES = admin.c admin.h AIPlayer.c AIPlayer.h util.c util.h \
                   configfile.c configfile.h convert.c convert.h \
                   dopewars.c dopewars.h error.c error.h log.c log.h \
                   message.c message.h network.c network.h nls.h \
                   serverside.c serverside.h sound.c sound.h \
                   tstring.c tstring.h winmain.c winmain.h mac_helpers.h
AM_CPPFLAGS= -I${srcdir} @GLIB_CFLAGS@ @GTK_CFLAGS@ @LIBCURL_CPPFLAGS@
if APPLE
dopewars_SOURCES += mac_helpers.m
MACLDFLAGS = -framework AppKit
else
MACLDFLAGS =
endif
# Since we included (even optionally) an Objective C file, automake will
# use OBJC rather than CC to link
LINK = $(MYLINK) $(AM_CFLAGS) $(CFLAGS) $(LDFLAGS) $(MACLDFLAGS) -o $@
OBJCLINK = $(MYLINK) $(AM_CFLAGS) $(CFLAGS) $(LDFLAGS) $(MACLDFLAGS) -o $@
DEFS       = @DEFS@
PIXDIR     = ${DESTDIR}${datadir}/pixmaps
DOPEDIR    = ${DESTDIR}${bindir}
DOPEBIN    = ${DOPEDIR}/dopewars
PIXMAPS    = dopewars-pill.png dopewars-shot.png dopewars-weed.png
EXTRA_DIST = ${PIXMAPS} pill.ico magic dopewars.rc dopewars.manifest
CLEANFILES = dopewars.res dopewars.exe
WINDRES    = @WINDRES@

install-exec-hook:
	@chgrp games ${DOPEBIN} || chgrp wheel ${DOPEBIN} || \
	  ( echo "WARNING: Cannot change group of dopewars binary - the high"; \
	    echo "score file may be unreadable or unwriteable by some users" )
	chmod 2755 ${DOPEBIN}

install-data-local:
	${mkinstalldirs} ${PIXDIR}
	for pix in ${PIXMAPS}; do \
	  ${INSTALL} -m 0644 ${srcdir}/$${pix} ${PIXDIR}; \
	done

uninstall-local:
	for pix in ${PIXMAPS}; do \
	  /bin/rm -f ${PIXDIR}/$${pix}; \
	done

%.res: %.rc
	${WINDRES} -O coff -o $@ $<
