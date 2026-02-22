# vim:set noet ts=4:
#
# ibus-hangul - The Hangul Engine For IBus
#
# Copyright (c) 2009 Choe Hwanjin <choe.hwanjin@gmail.com>
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

setup_hangul_PYTHON = \
	main.py \
	keycapturedialog.py \
	setup.ui \
	$(NULL)

nodist_setup_hangul_PYTHON = config.py

setup_hanguldir = $(pkgdatadir)/setup

libexec_SCRIPTS = ibus-setup-hangul

desktop_in_in_files = ibus-setup-hangul.desktop.in.in
desktop_in_files = $(desktop_in_in_files:.desktop.in.in=.desktop.in)
desktop_files = $(desktop_in_files:.desktop.in=.desktop)
desktop_DATA = $(desktop_files)
desktopdir = $(datadir)/applications

# replace libexecdir
$(desktop_in_files): %.desktop.in: %.desktop.in.in Makefile
	$(AM_V_GEN) sed \
	        -e "s|\@libexecdir\@|$(libexecdir)|" \
	        -e "s|\@pkgdatadir\@|$(pkgdatadir)|" \
	        $< > $@.tmp && mv $@.tmp $@

$(desktop_files): $(desktop_in_files) Makefile
	$(AM_V_GEN)$(MSGFMT) --desktop --template $< -d $(top_srcdir)/po -o $@

hicolor_icon_64_DATA = ibus-setup-hangul.png
hicolor_icon_64dir = $(datadir)/icons/hicolor/64x64/apps

hicolor_icon_scalable_DATA = ibus-setup-hangul.svg
hicolor_icon_scalabledir = $(datadir)/icons/hicolor/scalable/apps

CLEANFILES = \
	ibus-setup-hangul \
	config.py \
	*.pyc \
	ibus-setup-hangul.desktop \
	$(NULL)

EXTRA_DIST = \
	config.py.in \
	ibus-setup-hangul.in \
	ibus-setup-hangul.desktop.in \
	$(hicolor_icon_64_DATA) \
	$(hicolor_icon_scalable_DATA) \
	$(NULL)

test:
	$(ENV_PROG) DBUS_DEBUG=true \
	       PYTHONPATH=$(abs_builddir):$(pyexecdir) \
	       $(PYTHON) $(srcdir)/main.py

config.py: config.py.in Makefile
	sed -e 's&@SETUP_GETTEXT_PACKAGE@&$(GETTEXT_PACKAGE)&g' \
	    -e 's&@SETUP_LOCALEDIR@&$(localedir)&g' \
	    -e 's&@SETUP_PKGDATADIR@&$(pkgdatadir)&g' $< > $@

ibus-setup-hangul: ibus-setup-hangul.in config.py Makefile
	sed -e 's&@SETUP_PKGDATADIR@&$(pkgdatadir)&g' \
	    -e 's&\@PYTHON\@&$(PYTHON)&g' $< > $@
