
dnl  Copyright 2005 Red Hat, Inc.
dnl 
dnl  Permission to use, copy, modify, distribute, and sell this software and its
dnl  documentation for any purpose is hereby granted without fee, provided that
dnl  the above copyright notice appear in all copies and that both that
dnl  copyright notice and this permission notice appear in supporting
dnl  documentation, and that the name of Red Hat not be used in
dnl  advertising or publicity pertaining to distribution of the software without
dnl  specific, written prior permission.  Red Hat makes no
dnl  representations about the suitability of this software for any purpose.  It
dnl  is provided "as is" without express or implied warranty.
dnl 
dnl  RED HAT DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE,
dnl  INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO
dnl  EVENT SHALL RED HAT BE LIABLE FOR ANY SPECIAL, INDIRECT OR
dnl  CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE,
dnl  DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
dnl  TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
dnl  PERFORMANCE OF THIS SOFTWARE.
dnl
dnl Process this file with autoconf to create configure.

# Initialize Autoconf
AC_PREREQ([2.60])
AC_INIT([xrx], [1.0.4],
        [https://bugs.freedesktop.org/enter_bug.cgi?product=xorg], [xrx])
AC_CONFIG_SRCDIR([Makefile.am])
AC_CONFIG_HEADERS([config.h])

# Initialize Automake
AM_INIT_AUTOMAKE([foreign dist-bzip2])
AM_MAINTAINER_MODE

# Require xorg-macros minimum of 1.10 for DocBook XML documentation
m4_ifndef([XORG_MACROS_VERSION],
          [m4_fatal([must install xorg-macros 1.10 or later before running autoconf/autogen])])
XORG_MACROS_VERSION(1.10)
XORG_DEFAULT_OPTIONS
XORG_ENABLE_SPECS
XORG_WITH_XMLTO(0.0.20)
XORG_WITH_FOP
XORG_CHECK_SGML_DOCTOOLS(1.5)

AM_PROG_LIBTOOL

dnl Issue an error if xtrans.m4 was not found and XTRANS_CONNECTION_FLAGS macro
dnl was not expanded, since libX11 with no transport types is rather useless.
dnl
dnl If you're seeing an error here, be sure you installed the lib/xtrans module
dnl first and if it's not in the default location, that you set the ACLOCAL
dnl environment variable to find it, such as:
dnl     ACLOCAL="aclocal -I ${PREFIX}/share/aclocal"
m4_pattern_forbid(XTRANS_CONNECTION_FLAGS)

# Transport selection macro from xtrans.m4
XTRANS_CONNECTION_FLAGS

# Checks for pkg-config packages
PKG_CHECK_MODULES(XRX, x11 xt xext xtrans xproxymngproto xau ice)

# Check for mozilla/firefox plugin headers via pkg-config.
if test "x$PLUGIN_CFLAGS" = x ; then
    PKG_CHECK_MODULES([MOZILLA_PLUGIN], [mozilla-plugin], 
	[PLUGIN_CFLAGS="${MOZILLA_PLUGIN_CFLAGS}"], [:])

    if test "x$PLUGIN_CFLAGS" = x ; then
	PKG_CHECK_MODULES([FIREFOX_PLUGIN], [firefox-plugin],
	    [PLUGIN_CFLAGS="${FIREFOX_PLUGIN_CFLAGS}"], [:])
    fi
fi

# Check which header to include, the older npupp.h or the newer npfunctions.h
# See https://bugzilla.mozilla.org/show_bug.cgi?id=455458
save_CPPFLAGS="${CPPFLAGS}"
CPPFLAGS="${PLUGIN_CFLAGS} ${save_CPPFLAGS}"
AC_CHECK_HEADERS([npupp.h npfunctions.h], [PLUGIN_HEADERS_FOUND=yes])
CPPFLAGS="${save_CPPFLAGS}"

# If neither header found, fall back to the ancient copy we carry around.
if test "x$PLUGIN_HEADERS_FOUND" != "xyes" ; then
    PLUGIN_CFLAGS='-I$(top_srcdir)/plugin/include'
fi
AC_SUBST(PLUGIN_CFLAGS)


PKG_CHECK_MODULES(XRX_PLUGIN, xaw7)

AC_OUTPUT([
	Makefile
	helper/Makefile
	plugin/Makefile
	libxplugin/Makefile
	htdocs/Makefile
	cgi-bin/Makefile
	xnest-plugin/Makefile
	testplugin/Makefile
	rx/Makefile
	plugin/common/Makefile
	plugin/include/Makefile
	specs/Makefile
])
