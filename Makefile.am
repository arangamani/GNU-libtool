## Makefile.am -- Process this file with automake to produce Makefile.in
##
##   Copyright (C) 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011
##   Free Software Foundation, Inc.
##   Written by Gary V. Vaughan, 2003
##
##   This file is part of GNU Libtool.
##
## GNU Libtool is free software; you can redistribute it and/or
## modify it under the terms of the GNU General Public License as
## published by the Free Software Foundation; either version 2 of
## the License, or (at your option) any later version.
##
## GNU Libtool is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with GNU Libtool; see the file COPYING.  If not, a copy
## can be downloaded from  http://www.gnu.org/licenses/gpl.html,
## or obtained by writing to the Free Software Foundation, Inc.,
## 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA.
#####

ACLOCAL_AMFLAGS		= -I m4

AM_CPPFLAGS		=
AM_LDFLAGS		=

SUBDIRS			= .
DIST_SUBDIRS		= $(SUBDIRS)
EXTRA_DIST		=

BUILT_SOURCES		= libtool libtoolize

CLEANFILES		=
MOSTLYCLEANFILES	=
DISTCLEANFILES		=
MAINTAINERCLEANFILES	=

include_HEADERS		=
noinst_LTLIBRARIES	=
lib_LTLIBRARIES		=
EXTRA_LTLIBRARIES	=

m4sh_dir		= $(aux_dir)

LT_M4SH			= $(M4SH) -B '$(srcdir)/$(m4sh_dir)'

# Using `cd' in backquotes may print the directory name, use this instead:
lt__cd		= CDPATH="$${ZSH_VERSION+.}$(PATH_SEPARATOR)" && cd

git_version_gen = '$(SHELL)' '$(aux_dir)/git-version-gen' '.tarball-version'
rebuild = rebuild=:; revision=`$(lt__cd) $(srcdir) && $(git_version_gen) | sed 's|-.*$$||g'`


# ---------- #
# Bootstrap. #
# ---------- #

defs_m4sh	= $(tests_dir)/defs.m4sh
general_m4sh	= $(srcdir)/$(m4sh_dir)/general.m4sh
getopt_m4sh	= $(srcdir)/$(m4sh_dir)/getopt.m4sh
libtoolize_m4sh	= $(srcdir)/libtoolize.m4sh
ltmain_m4sh	= $(srcdir)/$(m4sh_dir)/ltmain.m4sh

sh_files	= $(general_m4sh) $(getopt_m4sh)
build_scripts	= $(srcdir)/$(aux_dir)/announce-gen \
		  $(srcdir)/$(aux_dir)/cvsu \
		  $(srcdir)/$(aux_dir)/do-release-commit-and-tag \
		  $(srcdir)/$(aux_dir)/gendocs.sh \
		  $(srcdir)/$(aux_dir)/git-version-gen \
		  $(srcdir)/$(aux_dir)/gnu-web-doc-update \
		  $(srcdir)/$(aux_dir)/gnupload \
		  $(srcdir)/$(aux_dir)/useless-if-before-free \
		  $(srcdir)/$(aux_dir)/vc-list-files

EXTRA_DIST     += bootstrap bootstrap.conf $(build_scripts) cfg.mk maint.mk \
		  $(sh_files) ChangeLog.1996 ChangeLog.1997 ChangeLog.1998 \
		  ChangeLog.1999 ChangeLog.2000 ChangeLog.2001 ChangeLog.2002 \
		  ChangeLog.2003 ChangeLog.2004 ChangeLog.2005 ChangeLog.2006 \
		  ChangeLog.2007 ChangeLog.2008 ChangeLog.2009 ChangeLog.2010 \
		  GNUmakefile
CLEANFILES     += libtool libtoolize

## If a file is named several times below, and especially if it
## is a distributed file created during Libtool bootstrap, we
## put it in a variable in accordance with the DRY principle.
## Use `$(srcdir)' for the benefit of non-GNU makes: referring
## to them by their variable makes sure they appear as targets
## and dependencies with `$(srcdir)' consistently.
configure_ac	= $(srcdir)/configure.ac
config_status	= $(top_builddir)/config.status
extract_trace	= $(srcdir)/$(aux_dir)/extract-trace
libtoolize_in	= $(srcdir)/libtoolize.in
ltmain_sh	= $(srcdir)/$(aux_dir)/ltmain.sh
libtool_m4	= $(srcdir)/$(macro_dir)/libtool.m4
ltversion_in	= $(srcdir)/$(macro_dir)/ltversion.in
ltversion_m4	= $(srcdir)/$(macro_dir)/ltversion.m4
options_parser	= $(srcdir)/$(aux_dir)/options-parser

EXTRA_DIST     += $(extract_trace) $(libtoolize_in) $(libtoolize_m4sh) \
		  $(ltmain_m4sh) $(ltmain_sh) $(ltversion_in) \
		  $(ltversion_m4) $(options_parser)

## These are the replacements that need to be made at bootstrap time,
## because they must be static in distributed files, and not accidentally
## changed by configure running on the build machine.
bootstrap_edit  = $(SED) \
		  -e 's|@MACRO_VERSION\@|$(VERSION)|g' \
		  -e "s|@MACRO_REVISION\@|$$revision|g" \
		  -e "s|@MACRO_SERIAL\@|$$serial|g" \
		  -e 's|@PACKAGE\@|$(PACKAGE)|g' \
		  -e 's|@PACKAGE_BUGREPORT\@|$(PACKAGE_BUGREPORT)|g' \
		  -e 's|@PACKAGE_URL\@|$(PACKAGE_URL)|g' \
		  -e 's|@PACKAGE_NAME\@|$(PACKAGE_NAME)|g' \
		  -e "s|@package_revision\@|$$revision|g" \
		  -e 's|@PACKAGE_STRING\@|$(PACKAGE_NAME) $(VERSION)|g' \
		  -e 's|@PACKAGE_TARNAME\@|$(PACKAGE)|g' \
		  -e 's|@PACKAGE_VERSION\@|$(VERSION)|g' \
		  -e 's|@VERSION\@|$(VERSION)|g'

## ltmain.sh needs some additional editing to remove unsubstituted
## variable defaulting lines, because ltmain.sh never gets passed
## through $(configure_edit).
ltmain_sh_edit	= $(bootstrap_edit) \
		  -e '/^: \$${.*="@.*@"}$$/d'

## We build ltversion.m4 here, instead of from config.status,
## because config.status is rerun each time one of configure's
## dependencies change and ltversion.m4 happens to be a configure
## dependency.  configure and ltversion.m4 would be rebuilt in
## a loop otherwise.
## We used to do this with a 'stamp-vcl' file, but non-gmake builds
## would rerun configure on every invocation, so now we manually
## check the version numbers from the build rule when necessary.
$(ltversion_m4): $(ltversion_in) $(dotversion)
	@$(rebuild); \
	if test -f '$@'; then \
	  eval `'$(SED)' -n '/^macro_revision=/p' '$@'`; \
	  test "x$$macro_revision" = "x$$revision" && rebuild=false; \
	fi; \
	for prereq in $?; do \
	  case $$prereq in *.version);; *) rebuild=:;; esac; \
	done; \
	if $$rebuild; then \
	  rm -f '$@'; \
	  if test -f '$(srcdir)/.serial'; then \
	    serial=`cat '$(srcdir)/.serial'`; \
	  else \
	    serial=`$(git_commit_count)`; \
	  fi; \
	  if test 0 = '$(V)'; then echo "  GEN   " $@; \
	  else echo $(bootstrap_edit) "'$(ltversion_in)' > '$@'"; fi; \
	  $(bootstrap_edit) '$(ltversion_in)' > '$@'; \
	  chmod a-w '$@'; \
	fi

## And for similar reasons, ltmain.sh can't be built from config.status.
## We used to do this with a 'stamp-vcl' file, but non-gmake builds
## would rerun configure on every invocation, so now we manually
## check the version numbers from the build rule when necessary.
## !WARNING! If you edit this rule to change the contents of ltmain.sh,
##           you must `touch $(m4sh_dir)/ltmain.m4sh' from the
##           shell if you need ltmain.sh to be regenerated.  Ideally, we
##           should make this rule depend on Makefile but that will break
##           distcheck (at least) by rebuilding ltmain.sh in the source
##           tree whenever config.status regenerates the Makefile.
$(ltmain_sh): $(ltmain_m4sh) $(sh_files) $(dotversion)
	@$(rebuild); \
	if test -f '$@'; then \
	  eval `'$(SED)' -n '/^package_revision=/p' '$@'`; \
	  test "x$$package_revision" = "x$$revision" && rebuild=false; \
	fi; \
	for prereq in $?; do \
	  case $$prereq in *.version);; *) rebuild=:;; esac; \
	done; \
	if $$rebuild; then \
	  rm -f '$@'; \
	  if test 0 = '$(V)'; then echo "  GEN   " $@; \
	  else echo "$(LT_M4SH) '$(ltmain.m4sh)' |" $(ltmain_sh_edit) "> '$@'"; fi; \
	  $(LT_M4SH) '$(ltmain_m4sh)' | $(ltmain_sh_edit) > '$@'; \
	  chmod a-w '$@'; \
	fi

$(libtoolize_in): $(libtoolize_m4sh) $(sh_files) Makefile.am
	$(AM_V_GEN)$(LT_M4SH) '$(libtoolize_m4sh)' | $(bootstrap_edit) > '$@'

lt_Makefile_am	= $(srcdir)/$(ltdl_dir)/Makefile.am
lt_Makefile_in	= $(srcdir)/$(ltdl_dir)/Makefile.in
ltdl_mk		= $(srcdir)/$(ltdl_dir)/ltdl.mk

$(lt_Makefile_am): $(ltdl_mk)
	$(AM_V_at)rm -f '$@'
	$(AM_V_GEN)( '$(SED)' -n '1,/^.. DO NOT REMOVE THIS LINE -- /p' \
	      '$(ltdl_mk)'; \
	  { echo 'ACLOCAL_AMFLAGS = -I ../m4'; \
	    echo 'AUTOMAKE_OPTIONS = foreign'; \
	    echo 'AM_CPPFLAGS ='; \
	    echo 'AM_LDFLAGS ='; \
	    echo 'BUILT_SOURCES ='; \
	    echo 'include_HEADERS ='; \
	    echo 'noinst_LTLIBRARIES ='; \
	    echo 'lib_LTLIBRARIES ='; \
	    echo 'EXTRA_LTLIBRARIES ='; \
	    echo 'EXTRA_DIST ='; \
	    echo 'CLEANFILES ='; \
	    echo 'MOSTLYCLEANFILES ='; \
	  }; \
	  '$(SED)' -n '/^.. DO NOT REMOVE THIS LINE -- /,$$p' \
	      '$(ltdl_mk)' \
	    |'$(SED)' -e 's|libltdl_||; s|libltdl/||; s|: libltdl/|: |' \
	      -e 's|\$$(libltdl_|$$(|' \
	) |'$(SED)' -e '/^.. DO NOT REMOVE THIS LINE -- /d' \
	      -e '1s,^\(.. Makefile.\)inc.*,\1am -- Process this file with automake to produce Makefile.in,' > '$@'
	$(AM_V_at)chmod a-w '$@'

## Document the make macros that are needed to build bootstrap-deps
## dependencies when called from `bootstrap' (developer's machine),
## where the Makefile.am is fed to make in its raw format before
## `configure' has found the correct values (on the build machine).
bootstrap_files = \
    $(lt_Makefile_am) \
    $(ltmain_sh) \
    $(ltversion_m4)

.PHONY: bootstrap-deps bootstrap-deps-prep
bootstrap-deps: bootstrap-deps-prep $(bootstrap_files)
bootstrap-deps-prep:
## The following variables are substituted by `bootstrap-dep-preps'
	@exit_cmd=:; \
	test -z '$(srcdir)' \
	    && echo "ERROR: don't call $(MAKE) with srcdir unset." \
	    && exit_cmd=exit; \
	test -z '$(M4SH)' \
	    && echo "ERROR: don't call $(MAKE) with M4SH unset." \
	    && exit_cmd=exit; \
	test -z '$(PACKAGE)' \
	    && echo "ERROR: don't call $(MAKE) with PACKAGE unset." \
	    && exit_cmd=exit; \
	test -z '$(PACKAGE_BUGREPORT)' \
	    && echo "ERROR: don't call $(MAKE) with PACKAGE_BUGREPORT unset." \
	    && exit_cmd=exit; \
	test -z '$(PACKAGE_NAME)' \
	    && echo "ERROR: don't call $(MAKE) with PACKAGE_NAME unset." \
	    && exit_cmd=exit; \
	test -z '$(PACKAGE_URL)' \
	    && echo "ERROR: don't call $(MAKE) with PACKAGE_URL unset." \
	    && exit_cmd=exit; \
	test -z '$(SED)' \
	    && echo "ERROR: don't call $(MAKE) with SED unset." \
	    && exit_cmd=exit; \
	test -z '$(VERSION)' \
	    && echo "ERROR: don't call $(MAKE) with VERSION unset." \
	    && exit_cmd=exit; \
	$$exit_cmd 1
	$(AM_V_at)rm -f $(bootstrap_files)

## Unfortunately, all this bogeyness means that we have to manually
## keep the generated files in libltdl up to date.
LTDL_BOOTSTRAP_DEPS = \
	$(stamp_mk) \
	$(lt_aclocal_m4) \
	$(lt_config_h_in) \
	$(lt_configure)

all-local: $(LTDL_BOOTSTRAP_DEPS)


## ---------------- ##
## Libtool scripts. ##
## ---------------- ##

ltdl_ac_aux_dir = `$(extract_trace) AC_CONFIG_AUX_DIR $(srcdir)/libltdl/configure.ac`

configure_edit = $(SED) \
	-e 's|@aclocaldir\@|$(aclocaldir)|g' \
	-e 's|@aux_dir\@|$(aux_dir)|g' \
	-e 's|@datadir\@|$(datadir)|g' \
	-e 's|@EGREP\@|$(EGREP)|g' \
	-e 's|@FGREP\@|$(FGREP)|g' \
	-e 's|@GREP\@|$(GREP)|g' \
	-e 's|@host_triplet\@|$(host_triplet)|g' \
	-e 's|@LN_S\@|$(LN_S)|g' \
	-e "s|@ltdl_ac_aux_dir\@|$(ltdl_ac_aux_dir)|g" \
	-e 's|@macro_dir\@|$(macro_dir)|g' \
	-e 's|@pkgauxdir\@|$(pkgauxdir)|g' \
	-e 's|@pkgaux_files\@|$(pkgaux_scripts) $(pkgaux_data_files)|g' \
	-e 's|@pkgaux_install_files\@|$(pkgaux_install_files)|g' \
	-e 's|@pkgaux_parent_files\@|$(pkgaux_parent_files)|g' \
	-e 's|@pkgdatadir\@|$(pkgdatadir)|g' \
	-e 's|@pkgltdl_files\@|$(pkgltdl_files)|g' \
	-e 's|@pkgmacro_files\@|$(pkgmacro_files)|g' \
	-e 's|@prefix\@|$(prefix)|g' \
	-e 's|@SED\@|$(SED)|g' \
	-e 's|@srcdir\@|$(srcdir)|g'

# The libtool distributor and the standalone libtool script.
bin_SCRIPTS = libtoolize libtool

libtoolize: $(libtoolize_in) $(config_status)
	$(AM_V_at)rm -f '$@'
	$(AM_V_GEN)$(configure_edit) '$(libtoolize_in)' > '$@'
	$(AM_V_at)chmod a+x '$@'
	$(AM_V_at)chmod a-w '$@'

# We used to do this with a 'stamp-vcl' file, but non-gmake builds
# would rerun configure on every invocation, so now we manually
# check the version numbers from the build rule when necessary.
libtool: $(ltmain_sh) $(config_status) $(dotversion)
	@$(rebuild); \
	if test -f '$@'; then \
	  eval `'$(SED)' -n '/^package_revision=/p' '$@'`; \
	  test "$$package_revision" = "$$revision" && rebuild=false; \
	fi; \
	for prereq in $?; do \
	  case $$prereq in *.version);; *) rebuild=:;; esac; \
	done; \
	if $$rebuild; then \
	  if test 0 = '$(V)'; then echo "  GEN   " $@; \
	  else echo '$(SHELL) $(top_builddir)/config.status "$@"'; fi; \
	  cd '$(top_builddir)' && '$(SHELL)' ./config.status '$@'; \
	fi

.PHONY: configure-subdirs
configure-subdirs distdir: $(DIST_MAKEFILE_LIST)
$(DIST_MAKEFILE_LIST):
	$(AM_V_at)dir=`echo '$@' |'$(SED)' 's|^[^/]*$$|.|;s|/[^/]*$$||'`; \
	test -d "$$dir" || mkdir "$$dir" || exit 1; \
	abs_srcdir=`$(lt__cd) '$(srcdir)' && pwd`; \
	(cd "$$dir" && "$$abs_srcdir/$$dir/configure" --with-dist) || exit 1


## -------- ##
## Libltdl. ##
## -------- ##

include libltdl/ltdl.mk

lt_aclocal_m4	= $(srcdir)/$(ltdl_dir)/aclocal.m4
lt_config_h_in	= $(srcdir)/$(ltdl_dir)/config-h.in
lt_configure	= $(srcdir)/$(ltdl_dir)/configure
lt_configure_ac	= $(srcdir)/$(ltdl_dir)/configure.ac
stamp_mk	= $(srcdir)/$(ltdl_dir)/stamp-mk

lt_obsolete_m4	= $(srcdir)/$(macro_dir)/lt~obsolete.m4

EXTRA_DIST     += $(lt_aclocal_m4) \
		  $(lt_configure) \
		  $(lt_configure_ac) \
		  $(lt_config_h_in) \
		  $(lt_Makefile_am) \
		  $(lt_Makefile_in) \
		  $(lt_obsolete_m4) \
		  $(stamp_mk)

$(lt_Makefile_in): $(lt_Makefile_am) $(lt_aclocal_m4)
	$(AM_V_GEN)cd '$(srcdir)/$(ltdl_dir)' && $(AUTOMAKE) Makefile

# Don't let unused scripts leak into the libltdl Makefile
$(stamp_mk): $(lt_Makefile_in)
	$(AM_V_at)T='$(srcdir)/$(aux_dir)/Makefile.tmp'; \
	for p in $(srcdir)/$(aux_dir)/*; do \
	  test -f "$$p" || continue; \
	  f=`echo "$$p" | $(SED) 's|^$(srcdir)/$(aux_dir)/||'`; \
	  case " $(pkgaux_scripts) ltmain.sh " in \
	    *" $$f "*) ;; \
	    *) '$(SED)' \
		  -e 's|\(\$$([^)]*)/\)*'"$(ltdl_ac_aux_dir)/$$f"'\$$||' \
		  -e 's|\(\$$([^)]*)/\)*'"$(ltdl_ac_aux_dir)/$$f"' ||' \
		  -e '/^[	 ]*\\$$/d' \
		'$(lt_Makefile_in)' > "$$T" \
		  && mv -f "$$T" '$(lt_Makefile_in)';; \
	  esac; \
	done
	$(AM_V_GEN)echo stamp > '$@'

lt_aclocal_m4_deps = \
	$(lt_obsolete_m4) \
	$(ltversion_m4) \
	$(libtool_m4) \
	$(srcdir)/$(macro_dir)/ltoptions.m4 \
	$(srcdir)/$(macro_dir)/ltdl.m4 \
	$(srcdir)/$(macro_dir)/ltsugar.m4 \
	$(srcdir)/$(macro_dir)/argz.m4 \
	$(srcdir)/$(ltdl_dir)/configure.ac

lt_configure_deps = $(lt_aclocal_m4) $(lt_aclocal_m4_deps)

$(lt_aclocal_m4): $(lt_aclocal_m4_deps)
	$(AM_V_GEN)cd '$(srcdir)/$(ltdl_dir)' && $(ACLOCAL) -I ../m4

$(lt_configure): $(lt_configure_deps)
	$(AM_V_GEN)cd '$(srcdir)/$(ltdl_dir)' && $(AUTOCONF)

$(lt_config_h_in): $(lt_configure_deps)
	$(AM_V_GEN)cd '$(srcdir)/$(ltdl_dir)' && $(AUTOHEADER)
	$(AM_V_at)touch '$@'


## -------------- ##
## Documentation. ##
## -------------- ##

doc_dir		= $(srcdir)/doc

libtool_1	= $(doc_dir)/libtool.1
libtoolize_1	= $(doc_dir)/libtoolize.1
notes_texi	= $(doc_dir)/notes.texi
notes_txt	= $(doc_dir)/notes.txt

EXTRA_DIST	+= $(doc_dir)/gendocs_template

# A bug in automake 1.11.1 (at least) prevents us from using:
#
#   info_TEXINFOS	= $(doc_dir)/libtool.texi
#
# Producing the following error, even though srcdir is implicitly set:
# "cannot open < ./$(srcdir)/doc/libtool.texi: No such file or directory"
info_TEXINFOS		= doc/libtool.texi
doc_libtool_TEXINFOS	= $(doc_dir)/PLATFORMS $(doc_dir)/fdl.texi \
			  $(notes_texi)
EXTRA_DIST	       += $(notes_txt)
## Workaround for a texinfo bug described here:
## http://thread.gmane.org/gmane.comp.tex.texinfo.bugs/3930
DISTCLEANFILES	       += libtool.dvi

all-local: $(notes_txt)

$(notes_txt): $(notes_texi)
	$(AM_V_GEN)$(MAKEINFO) -P '$(srcdir)/doc' --no-headers \
	    $(MAKEINFOFLAGS) -o '$@' '$(notes_texi)'

dist_man1_MANS		= $(libtool_1) $(libtoolize_1)
MAINTAINERCLEANFILES	+= $(dist_man1_MANS)
update_mans = \
  PATH=".$(PATH_SEPARATOR)$$PATH"; export PATH; \
  $(HELP2MAN) --output='$@'

# It's wrong to make distributed files (e.g. $(libtool_1)) rely on
# files created in the build tree, so instead we regenerate the
# manual pages if the sources for the build-tree files we want to
# run have changed.
$(libtool_1): $(ltmain_sh)
	$(AM_V_GEN)$(update_mans) --help-option=--help-all libtool
$(libtoolize_1): $(libtoolize_in)
	$(AM_V_GEN)$(update_mans) libtoolize


## ------------- ##
## Installation. ##
## ------------- ##

ltdldir		= $(srcdir)/libltdl
pkgauxdir	= $(pkgdatadir)/build-aux

# The timestamps on these files must be preserved carefully so we install,
# uninstall and set executable with custom rules here.
pkgaux_parent_files	= ltmain.sh
pkgaux_install_files	= config.guess config.sub install-sh
pkgaux_data_files	= $(pkgaux_parent_files)

# Everything that gets picked up by aclocal is automatically distributed,
# this is the list of macro files we install on the user's system.
pkgmacro_files	= argz.m4 libtool.m4 ltdl.m4 ltoptions.m4 ltsugar.m4 \
		  ltversion.m4 lt~obsolete.m4

## These are installed as a subdirectory of pkgdatadir so that
## libtoolize --ltdl can find them later:
pkgltdl_files	= COPYING.LIB \
		  Makefile.am \
		  Makefile.in \
		  README \
		  configure.ac \
		  aclocal.m4 \
		  argz_.h \
		  argz.c \
		  config-h.in \
		  configure \
		  libltdl/lt__alloc.h \
		  libltdl/lt__dirent.h \
		  libltdl/lt__glibc.h \
		  libltdl/lt__private.h \
		  libltdl/lt__strl.h \
		  libltdl/lt_dlloader.h \
		  libltdl/lt_error.h \
		  libltdl/lt_system.h \
		  libltdl/slist.h \
		  loaders/dld_link.c \
		  loaders/dlopen.c \
		  loaders/dyld.c \
		  loaders/load_add_on.c \
		  loaders/loadlibrary.c \
		  loaders/preopen.c \
		  loaders/shl_load.c \
		  lt__alloc.c \
		  lt__dirent.c \
		  lt__strl.c \
		  lt_dlloader.c \
		  lt_error.c \
		  ltdl.c \
		  ltdl.h \
		  ltdl.mk \
		  slist.c

install-data-local: $(lt_Makefile_in)
	@$(NORMAL_INSTALL)
## Don't install over the top of an old pkgdatadir
	-rm -rf '$(DESTDIR)$(pkgdatadir)'/*
## First, put a copy of the libtool m4 macros in the aclocal dir
	@list='$(pkgmacro_files)'; for p in $$list; do \
	  d=`echo "$(DESTDIR)$(aclocaldir)/$$p" |$(SED) 's|[^/]*$$||'`; \
	  test -d "$$d" || $(mkinstalldirs) "$$d"; \
	  echo " $(INSTALL_DATA) '$(srcdir)/$(macro_dir)/$$p' '$(DESTDIR)$(aclocaldir)/$$p'"; \
	  $(INSTALL_DATA) "$(srcdir)/$(macro_dir)/$$p" "$(DESTDIR)$(aclocaldir)/$$p"; \
	done
## install the helper scripts
	@list='extract-trace options-parser $(pkgaux_scripts)' && \
	for p in $$list; do \
	  d=`echo "$(DESTDIR)$(pkgauxdir)/$$p" |$(SED) 's|[^/]*$$||'`; \
	  test -d "$$d" || $(mkinstalldirs) "$$d"; \
	  echo " $(INSTALL_SCRIPT) '$(srcdir)/$(aux_dir)/$$p' '$(DESTDIR)$(pkgauxdir)/$$p'"; \
	  $(INSTALL_SCRIPT) "$(srcdir)/$(aux_dir)/$$p" "$(DESTDIR)$(pkgauxdir)/$$p"; \
	done
	@list='$(pkgaux_data_files)' && for p in $$list; do \
	  d=`echo "$(DESTDIR)$(pkgauxdir)/$$p" |$(SED) 's|[^/]*$$||'`; \
	  test -d "$$d" || $(mkinstalldirs) "$$d"; \
	  echo " $(INSTALL_DATA) '$(srcdir)/$(aux_dir)/$$p' '$(DESTDIR)$(pkgauxdir)/$$p'"; \
	  $(INSTALL_DATA) "$(srcdir)/$(aux_dir)/$$p" "$(DESTDIR)$(pkgauxdir)/$$p"; \
	done
## install the libltdl files
	@list='$(pkgltdl_files)' && for p in $$list; do \
	  d=`echo "$(DESTDIR)$(pkgdatadir)/$$p" |$(SED) 's|[^/]*$$||'`; \
	  test -d "$$d" || $(mkinstalldirs) "$$d"; \
	  echo " $(INSTALL_DATA) '$(ltdldir)/$$p' '$(DESTDIR)$(pkgdatadir)/$$p'"; \
	  $(INSTALL_DATA) "$(ltdldir)/$$p" "$(DESTDIR)$(pkgdatadir)/$$p"; \
	done
	-chmod a+x '$(DESTDIR)$(pkgdatadir)/configure'


## ------------- ##
## Distribution. ##
## ------------- ##

edit_readme_alpha	= $(srcdir)/$(aux_dir)/edit-readme-alpha
gitlog_to_changelog	= $(srcdir)/$(aux_dir)/gitlog-to-changelog
git_log_fix		= $(srcdir)/$(aux_dir)/git-log-fix

dotserial		= $(distdir)/.serial
dotversion		= $(srcdir)/.version
tarball_version		= $(distdir)/.tarball-version
readme			= $(distdir)/README
changelog		= $(distdir)/ChangeLog

# Generate ChangeLog for this year until it rotates out, when this start
# date is updated to the following year.
changelog_start_date = 2011-01-01
$(changelog): FORCE
	$(AM_V_GEN)if test -d '$(srcdir)/.git'; then \
	  $(gitlog_to_changelog) --amend=$(git_log_fix) \
	      --since=$(changelog_start_date) > '$@T'; \
	  rm -f '$@'; mv '$@T' '$@'; \
	fi

## Arrange so that .tarball-version appears only in the distribution
## tarball, and never in a checked-out repository.
EXTRA_DIST += $(dotversion)
BUILT_SOURCES += $(dotversion)
$(dotversion):
	$(AM_V_GEN)echo '$(VERSION)' > '$@T' && mv '$@T' '$@'

## Edit the README file for alpha releases.
EXTRA_DIST += $(edit_readme_alpha)
re_odd_version	= '\([0-9][0-9]*.[0-9][0-9]*.[0-9]*[13579]\)'
$(readme): FORCE
	@if test -n `expr $(VERSION) : $(re_odd_version)`; then \
	  if test 0 = '$(V)'; then echo "  GEN   " $@; \
	  else echo "$(SHELL) $(edit_readme_alpha) $@"; fi; \
	  $(SHELL) $(edit_readme_alpha) '$@'; \
	fi

git_commit_count = git log --pretty=oneline |wc -l |$(SED) 's|[	 ]||g'

dist-hook: $(changelog) $(dotversion) $(readme)
## Arrange so that .tarball-version appears only in the distribution
## tarball, and never in a checked-out repository.
	echo '$(VERSION)' > $(tarball_version)
## ...and similarly for .serial.
	$(git_commit_count) > $(dotserial)
## Ensure aclocal has not wrongly picked up old macro definitions.
	@for macro in LT_INIT AC_PROG_LIBTOOL AM_PROG_LIBTOOL; do \
	  if grep "$$macro" '$(srcdir)/aclocal.m4' '$(lt_aclocal_m4)'; then \
	    echo "Bogus $$macro macro contents in an aclocal.m4 file." >&2; \
	    exit 1; \
	  else :; fi; \
	done

distcheck-hook: syntax-check

uninstall-hook:
	@$(NORMAL_UNINSTALL)
	@list='$(pkgltdl_files)'; \
	for f in $$list; do \
	  echo " rm -f '$(DESTDIR)$(pkgdatadir)/$$f'"; \
	  rm -f "$(DESTDIR)$(pkgdatadir)/$$f"; \
	done
	@list='extract-trace options-parser $(pkgaux_scripts) $(pkgaux_data_files)'; \
	for f in $$list; do \
	  echo " rm -f '$(DESTDIR)$(pkgauxdir)/$$f'"; \
	  rm -f "$(DESTDIR)$(pkgauxdir)/$$f"; \
	done
	@for p in $(pkgmacro_files); do \
	  f=`echo "$$p" |$(SED) 's|^.*/||'`; \
	  echo " rm -f '$(DESTDIR)$(aclocaldir)/$$f'"; \
	  rm -f "$(DESTDIR)$(aclocaldir)/$$f"; \
	done


## ----------- ##
## Test suite. ##
## ----------- ##

tests_dir	= $(top_srcdir)/tests
package_m4	= $(tests_dir)/package.m4
testsuite	= $(tests_dir)/testsuite

# The testsuite files are evaluated in the order given here.
TESTSUITE	= tests/testsuite
TESTSUITE_AT	= tests/testsuite.at \
		  tests/getopt-m4sh.at \
		  tests/libtoolize.at \
		  tests/help.at \
		  tests/duplicate_members.at \
		  tests/duplicate_conv.at \
		  tests/duplicate_deps.at \
		  tests/flags.at \
		  tests/inherited_flags.at \
		  tests/convenience.at \
		  tests/link-order.at \
		  tests/link-order2.at \
		  tests/fail.at \
		  tests/shlibpath.at \
		  tests/runpath-in-lalib.at \
		  tests/static.at \
		  tests/export.at \
		  tests/search-path.at \
		  tests/indirect_deps.at \
		  tests/archive-in-archive.at \
		  tests/exeext.at \
		  tests/execute-mode.at \
		  tests/bindir.at \
		  tests/cwrapper.at \
		  tests/deplib-in-subdir.at \
		  tests/infer-tag.at \
		  tests/localization.at \
		  tests/nocase.at \
		  tests/install.at \
		  tests/versioning.at \
		  tests/destdir.at \
		  tests/old-m4-iface.at \
		  tests/old-ltdl-iface.at \
		  tests/am-subdir.at \
		  tests/lt_dlexit.at \
		  tests/lt_dladvise.at \
		  tests/lt_dlopen.at \
		  tests/lt_dlopen_a.at \
		  tests/lt_dlopenext.at \
		  tests/ltdl-libdir.at \
		  tests/ltdl-api.at \
		  tests/dlloader-api.at \
		  tests/loadlibrary.at \
		  tests/lalib-syntax.at \
		  tests/resident.at \
		  tests/slist.at \
		  tests/need_lib_prefix.at \
		  tests/standalone.at \
		  tests/subproject.at \
		  tests/nonrecursive.at \
		  tests/recursive.at \
		  tests/template.at \
		  tests/ctor.at \
		  tests/exceptions.at \
		  tests/early-libtool.at \
		  tests/with-pic.at \
		  tests/no-executables.at \
		  tests/deplibs-ident.at \
		  tests/configure-iface.at \
		  tests/stresstest.at \
		  tests/cmdline_wrap.at \
		  tests/pic_flag.at \
		  tests/darwin.at \
		  tests/dumpbin-symbols.at \
		  tests/deplibs-mingw.at \
		  tests/sysroot.at

EXTRA_DIST     += $(testsuite) $(TESTSUITE_AT) $(package_m4)

# Be sure to reexport important environment variables:
TESTS_ENVIRONMENT = MAKE="$(MAKE)" CC="$(CC)" CFLAGS="$(CFLAGS)" \
	CPP="$(CPP)" CPPFLAGS="$(CPPFLAGS)" LD="$(LD)" LDFLAGS="$(LDFLAGS)" \
	LIBS="$(LIBS)" LN_S="$(LN_S)" NM="$(NM)" RANLIB="$(RANLIB)" \
	AR="$(AR)" \
	M4SH="$(M4SH)" SED="$(SED)" STRIP="$(STRIP)" lt_INSTALL="$(INSTALL)" \
	MANIFEST_TOOL="$(MANIFEST_TOOL)" \
	OBJEXT="$(OBJEXT)" EXEEXT="$(EXEEXT)" \
	SHELL="$(SHELL)" CONFIG_SHELL="$(SHELL)" \
	CXX="$(CXX)" CXXFLAGS="$(CXXFLAGS)" CXXCPP="$(CXXCPP)" \
	F77="$(F77)" FFLAGS="$(FFLAGS)" \
	FC="$(FC)" FCFLAGS="$(FCFLAGS)" \
	GCJ="$(GCJ)" GCJFLAGS="$(GCJFLAGS)" \
	lt_cv_to_host_file_cmd="$(to_host_file_cmd)" \
	lt_cv_to_tool_file_cmd="$(to_tool_file_cmd)"

BUILDCHECK_ENVIRONMENT = _lt_pkgdatadir="$(abs_top_srcdir)" \
	LIBTOOLIZE="$(abs_top_builddir)/libtoolize" \
	LIBTOOL="$(abs_top_builddir)/libtool" \
	tst_aclocaldir="$(abs_top_srcdir)/m4"

INSTALLCHECK_ENVIRONMENT = \
	LIBTOOLIZE="$(bindir)/`echo libtoolize |$(SED) '$(program_transform_name)'`" \
	LIBTOOL="$(bindir)/`echo libtool |$(SED) '$(program_transform_name)'`" \
	LTDLINCL="-I$(includedir)" \
	LIBLTDL="$(libdir)/libltdl.la" \
	tst_aclocaldir="$(aclocaldir)"

$(testsuite): $(package_m4) $(TESTSUITE_AT) Makefile.am
	$(AM_V_GEN)$(AUTOTEST) -I '$(srcdir)' -I '$(srcdir)/tests' $(TESTSUITE_AT) -o '$@'

$(package_m4): $(dotversion) Makefile.am
	$(AM_V_GEN){ \
	  echo '# Signature of the current package.'; \
	  echo 'm4_define([AT_PACKAGE_NAME],      [$(PACKAGE_NAME)])'; \
	  echo 'm4_define([AT_PACKAGE_TARNAME],   [$(PACKAGE_TARNAME)])'; \
	  echo 'm4_define([AT_PACKAGE_VERSION],   [$(PACKAGE_VERSION)])'; \
	  echo 'm4_define([AT_PACKAGE_STRING],    [$(PACKAGE_STRING)])'; \
	  echo 'm4_define([AT_PACKAGE_BUGREPORT], [$(PACKAGE_BUGREPORT)])'; \
	  echo 'm4_define([AT_PACKAGE_URL],       [$(PACKAGE_URL)])'; \
	} > '$@'

tests/atconfig: $(config_status)
	$(AM_V_GEN)$(SHELL) '$(config_status)' '$@'

DISTCLEANFILES += tests/atconfig

CD_TESTDIR	= abs_srcdir=`$(lt__cd) $(srcdir) && pwd`; cd tests

testsuite_deps = tests/atconfig $(srcdir)/$(TESTSUITE)
testsuite_deps_uninstalled = $(testsuite_deps) libltdl/libltdlc.la \
			     $(bin_SCRIPTS) $(LTDL_BOOTSTRAP_DEPS)

# Hook the test suite into the check rule
check-local: $(testsuite_deps_uninstalled)
	$(AM_V_at)$(CD_TESTDIR); \
	CONFIG_SHELL='$(SHELL)' '$(SHELL)' "$$abs_srcdir/$(TESTSUITE)" \
	  $(TESTS_ENVIRONMENT) $(BUILDCHECK_ENVIRONMENT) $(TESTSUITEFLAGS)

# Run the test suite on the *installed* tree.
installcheck-local: $(testsuite_deps)
	$(AM_V_at)$(CD_TESTDIR); \
	CONFIG_SHELL='$(SHELL)' '$(SHELL)' "$$abs_srcdir/$(TESTSUITE)" \
	  $(TESTS_ENVIRONMENT) $(INSTALLCHECK_ENVIRONMENT) $(TESTSUITEFLAGS) \
	  AUTOTEST_PATH='$(exec_prefix)/bin'

check-noninteractive-old:
	$(AM_V_at)'$(MAKE)' $(AM_MAKEFLAGS) check-TESTS TESTS='$(NONINTERACTIVE_TESTS)'
check-interactive-old:
	$(AM_V_at)'$(MAKE)' $(AM_MAKEFLAGS) check-TESTS TESTS='$(INTERACTIVE_TESTS)'

# Run only noninteractive parts of the new testsuite.
check-noninteractive-new: $(testsuite_deps_uninstalled)
	$(AM_V_at)$(CD_TESTDIR); \
	CONFIG_SHELL='$(SHELL)' '$(SHELL)' "$$abs_srcdir/$(TESTSUITE)" \
	  $(TESTS_ENVIRONMENT) $(BUILDCHECK_ENVIRONMENT) \
	  -k '!interactive' INNER_TESTSUITEFLAGS=',!interactive' \
	  $(TESTSUITEFLAGS)

# Run only interactive parts of the new testsuite.
check-interactive-new: $(testsuite_deps_uninstalled)
	$(AM_V_at)$(CD_TESTDIR); \
	CONFIG_SHELL='$(SHELL)' '$(SHELL)' "$$abs_srcdir/$(TESTSUITE)" \
	  $(TESTS_ENVIRONMENT) $(BUILDCHECK_ENVIRONMENT) \
	  -k interactive -k recursive INNER_TESTSUITEFLAGS=',interactive' \
	  $(TESTSUITEFLAGS)

check-interactive: check-interactive-old check-interactive-new
check-noninteractive: check-noninteractive-old check-noninteractive-new

# We need to remove any file droppings left behind by testsuite
clean-local: clean-local-legacy
	-$(CD_TESTDIR); \
	test -f "$$abs_srcdir/$(TESTSUITE)" && \
	    '$(SHELL)' "$$abs_srcdir/$(TESTSUITE)" --clean


## ------------- ##
## Legacy tests. ##
## ------------- ##

# !WARNING! Don't add any new tests here, we are migrating to an
#           Autotest driven framework, please add new test cases
#           using the new framework above.  When the migration is
#           complete this section should be removed.

CXX_TESTS = \
	tests/tagdemo-static.test \
	tests/tagdemo-static-make.test \
	tests/tagdemo-static-exec.test \
	tests/tagdemo-conf.test \
	tests/tagdemo-make.test \
	tests/tagdemo-exec.test \
	tests/tagdemo-shared.test \
	tests/tagdemo-shared-make.test \
	tests/tagdemo-shared-exec.test \
	tests/tagdemo-undef.test \
	tests/tagdemo-undef-make.test \
	tests/tagdemo-undef-exec.test

tests/tagdemo-undef-exec.log:	tests/tagdemo-undef-make.log
tests/tagdemo-undef-make.log:	tests/tagdemo-undef.log
tests/tagdemo-undef.log:	tests/tagdemo-shared-exec.log
tests/tagdemo-shared-exec.log:	tests/tagdemo-shared-make.log
tests/tagdemo-shared-make.log:	tests/tagdemo-shared.log
tests/tagdemo-shared.log:	tests/tagdemo-exec.log
tests/tagdemo-exec.log:		tests/tagdemo-make.log
tests/tagdemo-make.log:		tests/tagdemo-conf.log
tests/tagdemo-conf.log:		tests/tagdemo-static-exec.log
tests/tagdemo-static-exec.log:	tests/tagdemo-static-make.log
tests/tagdemo-static-make.log:	tests/tagdemo-static.log

F77_TESTS = \
	tests/f77demo-static.test \
	tests/f77demo-static-make.test \
	tests/f77demo-static-exec.test \
	tests/f77demo-conf.test \
	tests/f77demo-make.test \
	tests/f77demo-exec.test \
	tests/f77demo-shared.test \
	tests/f77demo-shared-make.test \
	tests/f77demo-shared-exec.test

tests/f77demo-shared-exec.log:	tests/f77demo-shared-make.log
tests/f77demo-shared-make.log:	tests/f77demo-shared.log
tests/f77demo-shared.log:	tests/f77demo-exec.log
tests/f77demo-exec.log:		tests/f77demo-make.log
tests/f77demo-make.log:		tests/f77demo-conf.log
tests/f77demo-conf.log:		tests/f77demo-static-exec.log
tests/f77demo-static-exec.log:	tests/f77demo-static-make.log
tests/f77demo-static-make.log:	tests/f77demo-static.log

FC_TESTS = \
	tests/fcdemo-static.test \
	tests/fcdemo-static-make.test \
	tests/fcdemo-static-exec.test \
	tests/fcdemo-conf.test \
	tests/fcdemo-make.test \
	tests/fcdemo-exec.test \
	tests/fcdemo-shared.test \
	tests/fcdemo-shared-make.test \
	tests/fcdemo-shared-exec.test

tests/fcdemo-shared-exec.log:	tests/fcdemo-shared-make.log
tests/fcdemo-shared-make.log:	tests/fcdemo-shared.log
tests/fcdemo-shared.log:	tests/fcdemo-exec.log
tests/fcdemo-exec.log:		tests/fcdemo-make.log
tests/fcdemo-make.log:		tests/fcdemo-conf.log
tests/fcdemo-conf.log:		tests/fcdemo-static-exec.log
tests/fcdemo-static-exec.log:	tests/fcdemo-static-make.log
tests/fcdemo-static-make.log:	tests/fcdemo-static.log

COMMON_TESTS = \
	tests/link.test \
	tests/link-2.test \
	tests/nomode.test \
	tests/objectlist.test \
	tests/quote.test \
	tests/suffix.test \
	tests/tagtrace.test \
	tests/cdemo-static.test \
	tests/cdemo-static-make.test \
	tests/cdemo-static-exec.test \
	tests/demo-static.test \
	tests/demo-static-make.test \
	tests/demo-static-exec.test \
	tests/demo-static-inst.test \
	tests/demo-static-unst.test \
	tests/depdemo-static.test \
	tests/depdemo-static-make.test \
	tests/depdemo-static-exec.test \
	tests/depdemo-static-inst.test \
	tests/depdemo-static-unst.test \
	tests/mdemo-static.test \
	tests/mdemo-static-make.test \
	tests/mdemo-static-exec.test \
	tests/mdemo-static-inst.test \
	tests/mdemo-static-unst.test \
	tests/cdemo-conf.test \
	tests/cdemo-make.test \
	tests/cdemo-exec.test \
	tests/demo-conf.test \
	tests/demo-make.test \
	tests/demo-exec.test \
	tests/demo-inst.test \
	tests/demo-unst.test \
	tests/demo-deplibs.test \
	tests/depdemo-conf.test \
	tests/depdemo-make.test \
	tests/depdemo-exec.test \
	tests/depdemo-inst.test \
	tests/depdemo-unst.test \
	tests/mdemo-conf.test \
	tests/mdemo-make.test \
	tests/mdemo-exec.test \
	tests/mdemo-inst.test \
	tests/mdemo-unst.test \
	tests/mdemo-dryrun.test \
	tests/mdemo2-conf.test \
	tests/mdemo2-make.test \
	tests/mdemo2-exec.test \
	tests/pdemo-conf.test \
	tests/pdemo-make.test \
	tests/pdemo-exec.test \
	tests/pdemo-inst.test \
	tests/demo-nofast.test \
	tests/demo-nofast-make.test \
	tests/demo-nofast-exec.test \
	tests/demo-nofast-inst.test \
	tests/demo-nofast-unst.test \
	tests/depdemo-nofast.test \
	tests/depdemo-nofast-make.test \
	tests/depdemo-nofast-exec.test \
	tests/depdemo-nofast-inst.test \
	tests/depdemo-nofast-unst.test \
	tests/demo-pic.test \
	tests/demo-pic-make.test \
	tests/demo-pic-exec.test \
	tests/demo-nopic.test \
	tests/demo-nopic-make.test \
	tests/demo-nopic-exec.test \
	tests/cdemo-shared.test \
	tests/cdemo-shared-make.test \
	tests/cdemo-shared-exec.test \
	tests/mdemo-shared.test \
	tests/mdemo-shared-make.test \
	tests/mdemo-shared-exec.test \
	tests/mdemo-shared-inst.test \
	tests/mdemo-shared-unst.test \
	tests/cdemo-undef.test \
	tests/cdemo-undef-make.test \
	tests/cdemo-undef-exec.test

# Actually, only demo-relink and depdemo-relink require interaction,
# but they depend on the other tests being run beforehand.
INTERACTIVE_TESTS = \
	tests/demo-shared.test \
	tests/demo-shared-make.test \
	tests/demo-shared-exec.test \
	tests/demo-shared-inst.test \
	tests/demo-hardcode.test \
	tests/demo-relink.test \
	tests/demo-noinst-link.test \
	tests/demo-shared-unst.test \
	tests/depdemo-shared.test \
	tests/depdemo-shared-make.test \
	tests/depdemo-shared-exec.test \
	tests/depdemo-shared-inst.test \
	tests/depdemo-relink.test \
	tests/depdemo-shared-unst.test

tests/cdemo-undef-exec.log:	tests/cdemo-undef-make.log
tests/cdemo-undef-make.log:	tests/cdemo-undef.log
tests/cdemo-undef.log: $(ORDER)	tests/cdemo-shared-exec.log
tests/cdemo-shared-exec.log:	tests/cdemo-shared-make.log
tests/cdemo-shared-make.log:	tests/cdemo-shared.log
tests/cdemo-shared.log: $(ORDER) tests/cdemo-exec.log
tests/cdemo-exec.log:		tests/cdemo-make.log
tests/cdemo-make.log:		tests/cdemo-conf.log
tests/cdemo-conf.log: $(ORDER)	tests/cdemo-static-exec.log
tests/cdemo-static-exec.log:	tests/cdemo-static-make.log
tests/cdemo-static-make.log:	tests/cdemo-static.log

tests/demo-shared-unst.log:	tests/demo-noinst-link.log
tests/demo-noinst-link.log:	tests/demo-relink.log
tests/demo-relink.log:		tests/demo-hardcode.log
tests/demo-hardcode.log:	tests/demo-shared-inst.log
tests/demo-shared-inst.log:	tests/demo-shared-exec.log
tests/demo-shared-exec.log:	tests/demo-shared-make.log
tests/demo-shared-make.log:	tests/demo-shared.log
tests/demo-shared.log: $(ORDER)	tests/demo-nopic-exec.log
tests/demo-nopic-exec.log:	tests/demo-nopic-make.log
tests/demo-nopic-make.log:	tests/demo-nopic.log
tests/demo-nopic.log: $(ORDER)	tests/demo-pic-exec.log
tests/demo-pic-exec.log:	tests/demo-pic-make.log
tests/demo-pic-make.log:	tests/demo-pic.log
tests/demo-pic.log: $(ORDER)	tests/demo-nofast-unst.log
tests/demo-nofast-unst.log:	tests/demo-nofast-inst.log
tests/demo-nofast-inst.log:	tests/demo-nofast-exec.log
tests/demo-nofast-exec.log:	tests/demo-nofast-make.log
tests/demo-nofast-make.log:	tests/demo-nofast.log
tests/demo-nofast.log: $(ORDER)	tests/demo-deplibs.log
tests/demo-deplibs.log:		tests/demo-unst.log
tests/demo-unst.log:		tests/demo-inst.log
tests/demo-inst.log:		tests/demo-exec.log
tests/demo-exec.log:		tests/demo-make.log
tests/demo-make.log:		tests/demo-conf.log
tests/demo-conf.log: $(ORDER)	tests/demo-static-unst.log
tests/demo-static-unst.log:	tests/demo-static-inst.log
tests/demo-static-inst.log:	tests/demo-static-exec.log
tests/demo-static-exec.log:	tests/demo-static-make.log
tests/demo-static-make.log:	tests/demo-static.log

tests/depdemo-shared-unst.log:	tests/depdemo-relink.log
tests/depdemo-relink.log:	tests/depdemo-shared-inst.log
tests/depdemo-shared-inst.log:	tests/depdemo-shared-exec.log
tests/depdemo-shared-exec.log:	tests/depdemo-shared-make.log
tests/depdemo-shared-make.log:	tests/depdemo-shared.log
tests/depdemo-shared.log: $(ORDER) tests/depdemo-nofast-unst.log
tests/depdemo-nofast-unst.log:	tests/depdemo-nofast-inst.log
tests/depdemo-nofast-inst.log:	tests/depdemo-nofast-exec.log
tests/depdemo-nofast-exec.log:	tests/depdemo-nofast-make.log
tests/depdemo-nofast-make.log:	tests/depdemo-nofast.log
tests/depdemo-nofast.log: $(ORDER) tests/depdemo-unst.log
tests/depdemo-unst.log:		tests/depdemo-inst.log
tests/depdemo-inst.log:		tests/depdemo-exec.log
tests/depdemo-exec.log:		tests/depdemo-make.log
tests/depdemo-make.log:		tests/depdemo-conf.log
tests/depdemo-conf.log: $(ORDER) tests/depdemo-static-unst.log
tests/depdemo-static-unst.log:	tests/depdemo-static-inst.log
tests/depdemo-static-inst.log:	tests/depdemo-static-exec.log
tests/depdemo-static-exec.log:	tests/depdemo-static-make.log
tests/depdemo-static-make.log:	tests/depdemo-static.log

tests/mdemo-shared-unst.log:	tests/mdemo-shared-inst.log
tests/mdemo-shared-inst.log:	tests/mdemo-shared-exec.log
tests/mdemo-shared-exec.log:	tests/mdemo-shared-make.log
tests/mdemo-shared-make.log:	tests/mdemo-shared.log
tests/mdemo-shared.log: $(ORDER) tests/mdemo-dryrun.log \
				tests/mdemo2-exec.log

tests/mdemo-dryrun.log:		tests/mdemo-unst.log
tests/mdemo-unst.log:		tests/mdemo-inst.log
tests/mdemo-inst.log:		tests/mdemo-exec.log
tests/mdemo-exec.log:		tests/mdemo-make.log
tests/mdemo-make.log:		tests/mdemo-conf.log
tests/mdemo-conf.log: $(ORDER)	tests/mdemo-static-unst.log
tests/mdemo-static-unst.log:	tests/mdemo-static-inst.log
tests/mdemo-static-inst.log:	tests/mdemo-static-exec.log
tests/mdemo-static-exec.log:	tests/mdemo-static-make.log
tests/mdemo-static-make.log:	tests/mdemo-static.log

tests/mdemo2-exec.log:		tests/mdemo2-make.log
tests/mdemo2-make.log:		tests/mdemo2-conf.log \
				tests/mdemo-dryrun.log

tests/pdemo-inst.log:		tests/pdemo-exec.log
tests/pdemo-exec.log:		tests/pdemo-make.log
tests/pdemo-make.log:		tests/pdemo-conf.log


NONINTERACTIVE_TESTS = $(COMMON_TESTS)
if HAVE_CXX
NONINTERACTIVE_TESTS += $(CXX_TESTS)
endif
if HAVE_F77
# f77demo-static-exec.test might be interactive on MSYS.
INTERACTIVE_TESTS += $(F77_TESTS)
endif
if HAVE_FC
NONINTERACTIVE_TESTS += $(FC_TESTS)
endif
TESTS = $(NONINTERACTIVE_TESTS) $(INTERACTIVE_TESTS)

defs		= tests/defs
defs_in		= $(tests_dir)/defs.in

EXTRA_DIST     += $(defs_in) $(defs_m4sh) \
		  $(COMMON_TESTS) $(CXX_TESTS) $(F77_TESTS) $(FC_TESTS) \
		  $(INTERACTIVE_TESTS)
DIST_SUBDIRS   += $(CONF_SUBDIRS)

# The defs script shouldn't be recreated whenever the Makefile is
# regenerated since the source tree can be read-only.
check-recursive: $(defs)
$(defs): $(defs_in)
	$(AM_V_at)rm -f '$@'
	$(AM_V_GEN)$(configure_edit) '$(defs_in)' > '$@'

$(defs_in): $(defs_m4sh) $(general_m4sh) Makefile.am
	$(AM_V_at)rm -f '$@'
	$(AM_V_GEN)$(LT_M4SH) '$(defs_m4sh)' > '$@'

# We need to remove any files that the above tests created.
clean-local-legacy:
	-for dir in $(CONF_SUBDIRS); do \
	  if test -f "$$dir/Makefile"; then \
	    (cd "$$dir" && '$(MAKE)' $(AM_MAKEFLAGS) distclean); \
	  else :; fi; \
	done
	rm -rf _inst _inst-*

# For distclean, we may have to fake Makefiles in the test directories
# so that descending in DIST_SUBDIRS works.
# Hide the additional dependency from automake so it still outputs the rule.
distclean_recursive = distclean-recursive
$(distclean_recursive): fake-distclean-legacy
.PHONY: fake-distclean-legacy
fake-distclean-legacy:
	-for dir in $(CONF_SUBDIRS); do \
	  if test ! -f "$$dir/Makefile"; then \
	    $(mkinstalldirs) "$$dir"; \
	    echo 'distclean: ; rm -f Makefile' > "$$dir/Makefile"; \
	  else :; fi; \
	done

$(TESTS): $(defs)
DISTCLEANFILES += $(defs)

## An empty target to depend on when a rule needs to always run
## whenever it is visited.
FORCE:
