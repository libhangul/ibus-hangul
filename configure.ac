# vim:set et ts=4:
#
# ibus-hangul - The Hangul engine for IBus
#
# Copyright (c) 2007-2008 Huang Peng <shawn.p.huang@gmail.com>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License along
# with this program; if not, write to the Free Software Foundation, Inc.,
# 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

# if not 1, append datestamp to the version number.
m4_define([ibus_released], [1])
m4_define([ibus_major_version], [1])
m4_define([ibus_minor_version], [5])
m4_define([ibus_micro_version], [5])
m4_define(ibus_maybe_datestamp,
    m4_esyscmd([if test x]ibus_released[ != x1; then date +.%Y%m%d | tr -d '\n\r'; fi]))

m4_define([ibus_version],
    ibus_major_version.ibus_minor_version.ibus_micro_version[]ibus_maybe_datestamp)

AC_INIT([ibus-hangul], [ibus_version], [https://github.com/libhangul/ibus-hangul/issues],[ibus-hangul])
AM_INIT_AUTOMAKE([1.10 no-dist-gzip dist-xz])
AC_GNU_SOURCE

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# define PACKAGE_VERSION_* variables
AS_VERSION
AS_NANO
AM_SANITY_CHECK
AM_MAINTAINER_MODE
AM_DISABLE_STATIC
AC_PROG_CC
AM_PROG_CC_C_O
AC_PROG_CXX
AC_PROG_LN_S
AC_PROG_MKDIR_P
AC_ISC_POSIX
AC_HEADER_STDC
AM_PROG_LIBTOOL

AM_GNU_GETTEXT_VERSION([0.19.8])
AM_GNU_GETTEXT([external])

# check ibus
PKG_CHECK_MODULES(IBUS, [
    ibus-1.0 >= 1.5.4
])

# check libhangul
PKG_CHECK_MODULES(HANGUL, [
    libhangul >= 0.1.0
])

# check gtk
PKG_CHECK_MODULES(GTK, [
    gtk+-3.0 >= 3.0.0
])

# check hangul_ic_set_option() in libhangul newer version
LIBS_BACKUP=$LIBS
LIBS=$HANGUL_LIBS
AC_CHECK_LIB(hangul, [hangul_ic_set_option],
    [AC_DEFINE([LIBHANGUL_HAVE_HANGUL_IC_SET_OPTION], [1], [hangul_ic_set_option()])],
    []
)
LIBS=$LIBS_BACKUP

# check env
AC_PATH_PROG(ENV_PROG, env)
AC_SUBST(ENV_PROG)

# Define python version
AC_ARG_WITH(python,
    AS_HELP_STRING([--with-python[=PATH]],
        [Select python2 or python3]),
    [PYTHON=$with_python], []
)
AM_PATH_PYTHON([2.5])

# define GETTEXT_* variables
GETTEXT_PACKAGE=AC_PACKAGE_NAME
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(
  GETTEXT_PACKAGE, "$GETTEXT_PACKAGE",
    [Define to the read-only architecture-independent data directory.]
)

# --enable-installed-tests
AC_ARG_ENABLE(installed-tests,
    AS_HELP_STRING([--enable-installed-tests],
                   [Enable to installed tests]),
    [enable_installed_tests=$enableval],
    [enable_installed_tests=no]
)
AM_CONDITIONAL([ENABLE_INSTALLED_TESTS], [test x"$enable_installed_tests" = x"yes"])
if test x"$enable_installed_tests" = x"no"; then
    enable_installed_tests="no (disabled, use --enable-installed-tests to enable)"
fi

# OUTPUT files
AC_CONFIG_FILES([
po/Makefile.in
Makefile
ibus-hangul.spec
src/Makefile
src/hangul.xml.in
tests/Makefile
setup/Makefile
icons/Makefile
data/Makefile
m4/Makefile
])

AC_OUTPUT
