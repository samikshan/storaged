
m4_define([storaged_major_version], [2])
m4_define([storaged_minor_version], [4])
m4_define([storaged_micro_version], [1])

m4_define([storaged_version], [storaged_major_version.storaged_minor_version.storaged_micro_version])

AC_INIT([storaged],[storaged_version],[https://storaged-project.github.io],[storaged])

STORAGED_MAJOR_VERSION=storaged_major_version
STORAGED_MINOR_VERSION=storaged_minor_version
STORAGED_MICRO_VERSION=storaged_micro_version
STORAGED_VERSION=storaged_version
AC_SUBST(STORAGED_MAJOR_VERSION)
AC_SUBST(STORAGED_MINOR_VERSION)
AC_SUBST(STORAGED_MICRO_VERSION)
AC_SUBST(STORAGED_VERSION)

AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_AUX_DIR([build-aux])

AM_INIT_AUTOMAKE([1.11 foreign silent-rules dist-bzip2 no-dist-gzip])
AM_MAINTAINER_MODE
AM_SILENT_RULES([yes])

AX_CHECK_ENABLE_DEBUG

AC_PROG_CC
AC_ISC_POSIX
AC_HEADER_STDC
AC_PROG_LIBTOOL


# Compilation
#

GNOME_COMPILE_WARNINGS([maximum])

CC_CHECK_CFLAGS_APPEND([                          \
        -Waggregate-return                        \
        -Wall                                     \
        -W                                        \
        -Wextra                                   \
        -Wcast-align                              \
        -Wdeclaration-after-statement             \
        -Wendif-labels                            \
        -Werror=format-security                   \
        -Werror=implicit-function-declaration     \
        -Werror=overflow                          \
        -Wformat-nonliteral                       \
        -Wformat=2                                \
        -Wimplicit                                \
        -Winit-self                               \
        -Wlogical-op                              \
        -Wmissing-declarations                    \
        -Wmissing-format-attribute                \
        -Wmissing-include-dirs                    \
        -Wmissing-noreturn                        \
        -Wmissing-prototypes                      \
        -Wnested-externs                          \
        -Wno-inline                               \
        -Wno-long-long                            \
        -Wno-missing-field-initializers           \
        -Wno-overlength-strings                   \
        -Wno-switch-enum                          \
        -Wno-unused-parameter                     \
        -Wno-unused-result                        \
        -Wold-style-definition                    \
        -Wpacked                                  \
        -Wpointer-arith                           \
        -Wshadow                                  \
        -Wsign-compare                            \
        -Wstrict-aliasing=2                       \
        -Wstrict-prototypes                       \
        -Wundef                                   \
        -Wuninitialized                           \
        -Wmaybe-uninitialized                     \
        -Wunsafe-loop-optimizations               \
        -Wvla                                     \
        -Wwrite-strings                           \
        -fdata-sections                           \
        -fdiagnostics-show-option                 \
        -ffast-math                               \
        -ffunction-sections                       \
        -fno-common                               \
        -fno-strict-aliasing                      \
        -D_LARGEFILE64_SOURCE                     \
        -D_FILE_OFFSET_BITS=64                    \
])

AC_PATH_PROG([XSLTPROC], [xsltproc])
if test -z "$XSLTPROC"; then
  AC_MSG_ERROR([xsltproc is needed])
fi

GTK_DOC_CHECK([1.3],[--flavour no-tmpl])

AC_ARG_ENABLE(man,
              [AS_HELP_STRING([--enable-man],
                              [generate man pages [default=auto]])],,
              enable_man=maybe)
AS_IF([test "$enable_man" != no], [
  AC_PATH_PROG([XSLTPROC], [xsltproc])
  AS_IF([test -z "$XSLTPROC"], [
    AS_IF([test "$enable_man" = yes], [
      AC_MSG_ERROR([xsltproc is required for --enable-man])
    ])
    enable_man=no
  ])
  enable_man=yes
])
AM_CONDITIONAL(ENABLE_MAN, test "$enable_man" != no)

GOBJECT_INTROSPECTION_CHECK([0.6.2])

# Behavior
#

AC_ARG_ENABLE(fhs-media,
              [AS_HELP_STRING([--enable-fhs-media],
                              [Mount devices in /media instead of /run/media [default=no]])],
              fhs_media=yes,
              fhs_media=no)
if test "x$fhs_media" = "xyes"; then
  AC_DEFINE([HAVE_FHS_MEDIA], 1, [Define to 1 to use /media for mounting])
fi

# Libraries
#

PKG_CHECK_MODULES(GUDEV, [gudev-1.0 >= 165])
AC_SUBST(GUDEV_CFLAGS)
AC_SUBST(GUDEV_LIBS)

PKG_CHECK_MODULES(GLIB, [glib-2.0 >= 2.36])
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

PKG_CHECK_MODULES(GIO, [gio-unix-2.0 >= 2.36])
AC_SUBST(GIO_CFLAGS)
AC_SUBST(GIO_LIBS)

PKG_CHECK_MODULES(GMODULE, [gmodule-2.0])
AC_SUBST(GMODULE_CFLAGS)
AC_SUBST(GMODULE_LIBS)

PKG_CHECK_MODULES(POLKIT_GOBJECT_1, [polkit-gobject-1 >= 0.102])
AC_SUBST(POLKIT_GOBJECT_1_CFLAGS)
AC_SUBST(POLKIT_GOBJECT_1_LIBS)

PKG_CHECK_MODULES(POLKIT_AGENT_1, [polkit-agent-1 >= 0.102])
AC_SUBST(POLKIT_AGENT_1_CFLAGS)
AC_SUBST(POLKIT_AGENT_1_LIBS)

PKG_CHECK_MODULES(LIBXML, [libxml-2.0])
AC_SUBST(LIBXML_CFLAGS)
AC_SUBST(LIBXML_LIBS)

PKG_CHECK_MODULES(LIBATASMART, [libatasmart >= 0.17])
AC_SUBST(LIBATASMART_CFLAGS)
AC_SUBST(LIBATASMART_LIBS)

PKG_CHECK_MODULES(LIBSYSTEMD_LOGIN, [libsystemd >= 209], [have_libsystemd_login=yes],
                  [PKG_CHECK_MODULES(LIBSYSTEMD_LOGIN, [libsystemd-login >= 44 libsystemd-daemon],
                  [have_libsystemd_login=yes],
                  [have_libsystemd_login=no])])
AM_CONDITIONAL(HAVE_LIBSYSTEMD_LOGIN, test x$have_libsystemd_login = xyes)
if test "x$have_libsystemd_login" = "xyes"; then
  AC_DEFINE([HAVE_LIBSYSTEMD_LOGIN], 1, [Define to 1 if libsystemd-login is available])
fi
AC_SUBST(HAVE_LIBSYSTEMD_LOGIN)
AC_SUBST(LIBSYSTEMD_LOGIN_CFLAGS)
AC_SUBST(LIBSYSTEMD_LOGIN_LIBS)

# udevdir
AC_ARG_WITH([udevdir],
            AS_HELP_STRING([--with-udevdir=DIR], [Directory for udev]),
            [],
            [with_udevdir=$($PKG_CONFIG --variable=udevdir udev)])
AC_SUBST([udevdir], [$with_udevdir])

# systemd
AC_ARG_WITH([systemdsystemunitdir],
            AS_HELP_STRING([--with-systemdsystemunitdir=DIR], [Directory for systemd service files]),
            [],
            [with_systemdsystemunitdir=$($PKG_CONFIG --variable=systemdsystemunitdir systemd)])
if test "x$with_systemdsystemunitdir" != "xno"; then
  AC_SUBST([systemdsystemunitdir], [$with_systemdsystemunitdir])
fi
AM_CONDITIONAL(HAVE_SYSTEMD, [test -n "$systemdsystemunitdir"])

# libacl
AC_CHECK_HEADERS(
        [sys/acl.h acl/libacl.h],
        [ACL_CFLAGS=""],
        AC_MSG_ERROR([*** ACL headers not found.]))
AC_CHECK_LIB(
        [acl],
        [acl_get_file],
        [ACL_LIBS="-lacl"],
        AC_MSG_ERROR([*** libacl not found.]))
AC_SUBST(ACL_CFLAGS)
AC_SUBST(ACL_LIBS)

# Enable the modules; except the dummy module.
AC_ARG_ENABLE(modules, AS_HELP_STRING([--enable-modules], [enable modules]))
# Enable all the modules; including the dummy one.
AC_ARG_ENABLE(all_modules, AS_HELP_STRING([--enable-all-modules], [enable all modules]))

# Dummy module
have_dummy=no
AC_ARG_ENABLE(dummy, AS_HELP_STRING([--enable-dummy], [enable dummy test module]))
if test "x$enable_dummy" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then
  have_dummy=yes
fi
AM_CONDITIONAL(HAVE_DUMMY, [test "$have_dummy" = "yes"])

# LVM2 module
have_lvm2=no
have_devmapper=no
AC_ARG_ENABLE(lvm2, AS_HELP_STRING([--enable-lvm2], [enable LVM2 support]))
if test "x$enable_lvm2" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then
  PKG_CHECK_MODULES(DEVMAPPER, devmapper >= 1.02,
                    [AC_DEFINE(HAVE_DEVMAPPER, 1, [Define if DEVMAPPER is available]) have_devmapper=yes],
                    have_devmapper=no)
  if test "x$have_devmapper" = "xyes"; then
    PKG_CHECK_MODULES(LVM2, lvm2app >= 2.2,
                      [AC_DEFINE(HAVE_LVM2, 1, [Define if LVM2 is available]) have_lvm2=yes],
                      have_lvm2=no)
  fi
  AC_SUBST(DEVMAPPER_CFLAGS)
  AC_SUBST(DEVMAPPER_LIBS)
  AC_SUBST(LVM2_CFLAGS)
  AC_SUBST(LVM2_LIBS)
  if test \( "x$have_devmapper" = xno  -o "x$have_lvm2"      = xno  \) \
       -a \( "x$enable_lvm2"    = xyes -o "x$enable_modules" = xyes \); then
    AC_MSG_ERROR([lvm2 support requested but libraries not found])
  fi
fi
AM_CONDITIONAL(HAVE_LVM2, [test "$have_lvm2" = "yes"])
AM_CONDITIONAL(HAVE_DEVMAPPER, [test "$have_devmapper" = "yes"])

# LVMCache
have_lvmcache=no
AC_ARG_ENABLE(lvmcache, AS_HELP_STRING([--enable-lvmcache], [enable LVMCache support]))
if test "x$enable_lvmcache" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then

  if test "x$have_lvm2" = "xno"; then
    AC_MSG_ERROR([lvmcache support requested, but lvm2 module not enabled])
  fi
  have_lvmcache=yes
  AC_DEFINE(HAVE_LVMCACHE, 1, [Define if lvmcache is available])
fi
AM_CONDITIONAL(HAVE_LVMCACHE, [test "$have_lvmcache" = "yes"])

# iSCSI module
have_iscsi=no
have_libiscsi_session_info="no"
have_libiscsi_session_info_msg=""
AC_ARG_ENABLE(iscsi, AS_HELP_STRING([--enable-iscsi], [enable iSCSI support]))
if test "x$enable_iscsi" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then
  # libiscsi.h
  AC_CHECK_HEADER([libiscsi.h],
                  [AC_DEFINE(HAVE_ISCSI, 1, [Define if libiscsi from iscsi-initiator-utils is available])
                   have_iscsi=yes],
                  [have_iscsi=no])
  if test "x$have_iscsi" = "xyes"; then
    AC_MSG_CHECKING([whether libiscsi.h defines iscsi_err])
    AC_TRY_COMPILE([#include <libiscsi.h>],
                   [int err = ISCSI_SUCCESS;],
                   [AC_MSG_RESULT([yes])
                    AC_DEFINE([HAVE_LIBISCSI_ERR], [1], [libiscsi error states])],
                   [AC_MSG_RESULT([no])])
  fi

  # libiscsi
  AC_CHECK_LIB([iscsi],
               [libiscsi_init],
               [ISCSI_LIBS="-liscsi"],
               [have_iscsi="no"])
  if test "x$have_iscsi" = "xyes"; then
    AC_CHECK_LIB([iscsi],
                 [libiscsi_get_session_info_by_id],
                 [have_libiscsi_session_info="yes"
                  have_libiscsi_session_info_msg=", with iscsi sessions"
                  AC_DEFINE([HAVE_LIBISCSI_GET_SESSION_INFOS], [1],
                            [libiscsi can retrieve session information])],
                 [have_libiscsi_session_info_msg=", without iscsi sessions"])
  fi

  if test "x$have_iscsi" = "xno"; then
    AC_MSG_ERROR([iSCSI support requested but libraries not found])
  fi
  AC_SUBST([ISCSI_LIBS])
fi
AM_CONDITIONAL(HAVE_ISCSI, [test "x$have_iscsi" = "xyes"])
AM_CONDITIONAL(BUILD_ISCSI_SESSION_OBJECT, [test "x$have_libiscsi_session_info" = "xyes"])

# GlusterFS module
have_xml=no
AC_ARG_ENABLE(glusterfs, AS_HELP_STRING([--enable-glusterfs], [enable GlusterFS support]))
if test "x$enable_glusterfs" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then

  AC_CHECK_LIB([xml2],
               [xmlParseDoc],
               [have_xml="yes"],
               [have_xml="no"])

  if test "x$have_xml" = "xno"; then
    AC_MSG_ERROR([GlusterFS support requested but xml libraries not found])
  fi
fi
AM_CONDITIONAL(HAVE_XML, [test "$have_xml" = "yes"])

# BTRFS module
have_btrfs=no
AC_ARG_ENABLE(btrfs, AS_HELP_STRING([--enable-btrfs], [enable BTRFS support]))
if test "x$enable_btrfs" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then
    # libblockdev
    SAVE_CFLAGS=$CFLAGS
    SAVE_LDFLAGS=$LDFLAGS
    CFLAGS="$GLIB_CFLAGS -I/usr/include/blockdev"
    LDFLAGS="$GLIB_LIBS"
    # We don't use AC_CHECK_HEADER to avoid these warnings:
    #   - accepted by the compiler, rejected by the preprocessor!
    #   - proceeding with the compiler's result
    AC_MSG_CHECKING(["libblockdev-btrfs presence"])
    AC_TRY_COMPILE([#include <btrfs.h>], [],
                   [AC_MSG_RESULT([yes])
                    AC_DEFINE(HAVE_BTRFS, 1, [Define, if libblockdev-btrfs is available])
                    have_btrfs=yes],
                   [AC_MSG_RESULT([no])
                    have_btrfs=no])
    CFLAGS=$SAVE_CFLAGS
    LDFLAGS=$SAVE_LDFLAGS

    BTRFS_CFLAGS="-I/usr/include/blockdev"
    BTRFS_LIBS="-lbd_btrfs"

    if test "x$have_btrfs" = "xno"; then
      AC_MSG_ERROR([BTRFS support requested but header or library not found])
    fi

    AC_SUBST([BTRFS_CFLAGS])
    AC_SUBST([BTRFS_LIBS])
fi
AM_CONDITIONAL(HAVE_BTRFS, [test "x$have_btrfs" = "xyes"])

# ZRAM module
have_kbd="no"
have_swap="no"
have_zram="no"
AC_ARG_ENABLE(zram, AS_HELP_STRING([--enable-zram], [enable ZRAM support]))
if test "x$enable_zram" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then
    # libblockdev
    SAVE_CFLAGS=$CFLAGS
    SAVE_LDFLAGS=$LDFLAGS
    CFLAGS="$GLIB_CFLAGS -I/usr/include/blockdev"
    LDFLAGS="$GLIB_LIBS"

    # libblockdev-kbd
    AC_MSG_CHECKING([libblockdev-kbd presence])
    AC_TRY_COMPILE([#include <kbd.h>], [],
                   [AC_MSG_RESULT([yes])
                    have_kbd=yes],
                   [AC_MSG_RESULT([no])
                    have_kbd=no])

    KBD_CFLAGS="-I/usr/include/blockdev"
    KBD_LIBS="-lbd_kbd"

    if test "x$have_kbd" = "xno"; then
      AC_MSG_ERROR([KBD support requested but header or library not found])
    fi

    AC_SUBST([KBD_CFLAGS])
    AC_SUBST([KBD_LIBS])

    CFLAGS="$GLIB_CFLAGS -I/usr/include/blockdev"
    LDFLAGS="$GLIB_LIBS"

    # libblockdev-swap
    AC_MSG_CHECKING([libblockdev-swap presence])
    AC_TRY_COMPILE([#include <swap.h>], [],
                   [AC_MSG_RESULT([yes])
                   have_swap=yes],
                   [AC_MSG_RESULT([no])
                   have_swap=no])

    CFLAGS=$SAVE_CFLAGS
    LDFLAGS=$SAVE_LDFLAGS

    SWAP_CFLAGS="-I/usr/include/blockdev"
    SWAP_LIBS="-lbd_swap"

    if test "x$have_swap" = "xno"; then
      AC_MSG_ERROR([SWAP support requested but header or library not found])
    fi

    AC_SUBST([SWAP_CFLAGS])
    AC_SUBST([SWAP_LIBS])

    have_zram="yes"

    if test "$have_kbd" = "yes" -a "$have_swap" = "yes"; then
       AC_DEFINE(HAVE_ZRAM, 1, [Define, if libblockdev-kbd and libblockdev-swap are available])
    fi
fi
AM_CONDITIONAL(HAVE_ZRAM, [test "$have_kbd" = "yes" -a "$have_swap" = "yes"])


# LSM module
have_lsm=no
AC_ARG_ENABLE(
    lsm, AS_HELP_STRING([--enable-lsm], [enable LibStorageMgmt support]))
if test "x$enable_lsm" = "xyes" \
        -o "x$enable_modules" = "xyes" \
        -o "x$enable_all_modules" = "xyes"; then
    PKG_CHECK_MODULES(
        [LIBLSM], [libstoragemgmt >= 1.2.0],
        [AC_DEFINE(HAVE_LSM, 1, [Define if liblibstoragemgmt is available])
            have_lsm=yes],
        [AC_MSG_ERROR([libstoragemgmt 1.2.0 or newer not found.])
            have_lsm=no])

    PKG_CHECK_MODULES(
        [LIBCONFIG], [libconfig >= 1.3.2],
        [AC_DEFINE(HAVE_LSM, 1, [Define if libconfig is available])
            have_lsm=yes],
        [AC_MSG_ERROR([libconfig 1.3.2 or newer not found.])
            have_lsm=no])

  if test "x$have_lsm" = "xno"; then
    AC_MSG_ERROR([LibStorageMgmt support requested but libraries not found])
  fi
fi
AM_CONDITIONAL(HAVE_LSM, [test "x$have_lsm" = "xyes"])

# Bcache module
have_bcache=no
AC_ARG_ENABLE(bcache, AS_HELP_STRING([--enable-bcache], [enable Bcache support]))
if test "x$enable_bcache" = "xyes" \
     -o "x$enable_modules" = "xyes" \
     -o "x$enable_all_modules" = "xyes"; then
    # libblockdev
    SAVE_CFLAGS=$CFLAGS
    SAVE_LDFLAGS=$LDFLAGS
    CFLAGS="$GLIB_CFLAGS -I/usr/include/blockdev"
    LDFLAGS="$GLIB_LIBS"

    # libblockdev-kbd
    AC_MSG_CHECKING([libblockdev-kbd presence])
    AC_TRY_COMPILE([#include <kbd.h>], [],
                   [AC_MSG_RESULT([yes])
                    have_bcache=yes],
                   [AC_MSG_RESULT([no])
                    have_bcache=no])
    CFLAGS=$SAVE_CFLAGS
    LDFLAGS=$SAVE_LDFLAGS

    BCACHE_CFLAGS="-I/usr/include/blockdev"
    BCACHE_LIBS="-lbd_kbd"

    if test "x$have_bcache" = "xno"; then
      AC_MSG_ERROR([Bcache support requested but header or library not found])
    fi

    AC_SUBST([BCACHE_CFLAGS])
    AC_SUBST([BCACHE_LIBS])
fi
AM_CONDITIONAL(HAVE_BCACHE, [test "x$have_bcache" = "xyes"])

# Internationalization
#

IT_PROG_INTLTOOL([$INTLTOOL_REQUIRED])
GETTEXT_PACKAGE=storaged
AC_SUBST([GETTEXT_PACKAGE])
AM_GLIB_GNU_GETTEXT
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE],["$GETTEXT_PACKAGE"],[gettext domain])

# Generate
#

AC_OUTPUT([
Makefile
data/Makefile
storaged/Makefile
storaged/storaged.pc
storaged/modules.conf.d/Makefile
storaged/modules.conf.d/storaged_lsm.conf
src/Makefile
src/tests/Makefile
tools/Makefile
modules/Makefile
modules/btrfs/Makefile
modules/btrfs/data/Makefile
modules/dummy/Makefile
modules/dummy/data/Makefile
modules/iscsi/Makefile
modules/iscsi/data/Makefile
modules/lvm2/Makefile
modules/lvm2/data/Makefile
modules/glusterfs/Makefile
modules/glusterfs/data/Makefile
modules/zram/Makefile
modules/zram/data/Makefile
modules/lsm/Makefile
modules/lsm/data/Makefile
modules/bcache/Makefile
modules/bcache/data/Makefile
doc/Makefile
doc/storaged-docs.xml.in
doc/storaged-sections.txt.in
doc/storaged.types.in
doc/version.xml
doc/man/Makefile
doc/man/storaged-lvm.xml
doc/man/storaged-project.xml
doc/man/storaged.xml
doc/man/storagectl.xml
doc/man/umount.storaged.xml
doc/man/storaged_lsm.conf.5
doc/storaged-docs.xml.iscsi.dbus
doc/storaged-docs.xml.iscsi.generated
doc/storaged-sections.txt.iscsi.sections
doc/storaged.types.iscsi
doc/storaged-docs.xml.lsm.dbus
doc/storaged-docs.xml.lsm.generated
doc/storaged-sections.txt.lsm.sections
doc/storaged.types.lsm
doc/storaged-docs.xml.lvm2.man
doc/storaged-docs.xml.lvm2.dbus
doc/storaged-docs.xml.lvm2.generated
doc/storaged-sections.txt.lvm2.sections
doc/storaged.types.lvm2
doc/storaged-docs.xml.btrfs.dbus
doc/storaged-docs.xml.btrfs.generated
doc/storaged-sections.txt.btrfs.sections
doc/storaged.types.btrfs
doc/storaged-docs.xml.zram.dbus
doc/storaged-docs.xml.zram.generated
doc/storaged-docs.xml.zram.block_devices
doc/storaged-sections.txt.zram.sections
doc/storaged.types.zram
doc/storaged-docs.xml.bcache.dbus
doc/storaged-docs.xml.bcache.generated
doc/storaged-sections.txt.bcache.sections
doc/storaged.types.bcache
po/Makefile.in
])

dnl ==========================================================================
echo "
               storaged $VERSION
             ==================

        prefix:                     ${prefix}
        libdir:                     ${libdir}
        libexecdir:                 ${libexecdir}
        bindir:                     ${bindir}
        sbindir:                    ${sbindir}
        datadir:                    ${datadir}
        sysconfdir:                 ${sysconfdir}
        localstatedir:              ${localstatedir}
        docdir:                     ${docdir}
        introspection:              ${found_introspection}

        udevdir:                    ${udevdir}
        systemdsystemunitdir:       ${systemdsystemunitdir}
        using libsystemd-login:     ${have_libsystemd_login}
        use /media for mounting:    ${fhs_media}

        compiler:                   ${CC}
        cflags:                     ${CFLAGS}
        cppflags:                   ${CPPFLAGS}

        Maintainer mode:            ${USE_MAINTAINER_MODE}
        Building api docs:          ${enable_gtk_doc}
        Building man pages:         ${enable_man}

        BTRFS module:               ${have_btrfs}
        Dummy module:               ${have_dummy}
        iSCSI module:               ${have_iscsi}${have_libiscsi_session_info_msg}
        LVM2  module:               ${have_lvm2}
        GlusterFS module:           ${have_xml}
        LVMCache:                   ${have_lvmcache}
        Zram module:                ${have_zram}
        LibStorageMgmt module:      ${have_lsm}
        Bcache module:              ${have_bcache}
"
