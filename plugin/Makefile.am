#  Copyright 2005  Red Hat, Inc.
# 
#  Permission to use, copy, modify, distribute, and sell this software and its
#  documentation for any purpose is hereby granted without fee, provided that
#  the above copyright notice appear in all copies and that both that
#  copyright notice and this permission notice appear in supporting
#  documentation, and that the name of Red Hat not be used in
#  advertising or publicity pertaining to distribution of the software without
#  specific, written prior permission.  Red Hat makes no
#  representations about the suitability of this software for any purpose.  It
#  is provided "as is" without express or implied warranty.
# 
#  RED HAT DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE,
#  INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO
#  EVENT SHALL RED HAT BE LIABLE FOR ANY SPECIAL, INDIRECT OR
#  CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE,
#  DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
#  TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
#  PERFORMANCE OF THIS SOFTWARE.

SUBDIRS = common include

lib_LTLIBRARIES = libxrx.la

RXSRC = $(top_srcdir)/rx

INCLUDES = -I$(RXSRC) -I$(srcdir)/include

# -DPLUGIN_TRACE can be added to the following definition to debug
PLUGIN_DEFINES = -DXP_UNIX -DMOZ_X11 -DOJI

DEFINES = -DNETSCAPE_PLUGIN $(PLUGIN_DEFINES)

AM_CPPFLAGS = $(DEFINES)
AM_CFLAGS = -DXP_UNIX $(CWARNFLAGS) \
    $(XRX_CFLAGS) $(XRX_PLUGIN_CFLAGS) $(PLUGIN_CFLAGS)

libxrx_la_LIBADD = $(XRX_LIBS) $(XRX_PLUGIN_LIBS)
libxrx_la_LDFLAGS = -avoid-version -module -no-undefined

PARAMS_SRCS = $(RXSRC)/PRead.c $(RXSRC)/PParse.c $(RXSRC)/BuildReq.c
XMISC_SRCS = $(RXSRC)/XUrls.c $(RXSRC)/XAuth.c $(RXSRC)/XDpyName.c \
	$(RXSRC)/Prefs.c

libxrx_la_SOURCES = \
	$(PARAMS_SRCS)	\
	$(XMISC_SRCS)	\
	Global.c	\
	Main.c		\
	NewNDest.c	\
	PProcess.c	\
	RxPlugin.h	\
	SetWin.c	\
	stubs.c

appman_PRE = libxrx.man

appmandir = $(APP_MAN_DIR)

appman_DATA = $(appman_PRE:man=@APP_MAN_SUFFIX@)

EXTRA_DIST = $(appman_PRE)
CLEANFILES = $(appman_DATA)

SUFFIXES = .$(APP_MAN_SUFFIX) .man

# String replacements in MAN_SUBSTS now come from xorg-macros.m4 via configure
.man.$(APP_MAN_SUFFIX):
	$(AM_V_GEN)$(SED) $(MAN_SUBSTS) < $< > $@
