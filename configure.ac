dnl Process this file with autoconf to produce a configure script.
AC_PREREQ(2.61)
AC_INIT([gsocket], 1.4.22)
dnl AC_CONFIG_AUX_DIR(config-x86_64-apple-darwin19.6.0)
AC_CONFIG_AUX_DIR(config)
AC_CANONICAL_SYSTEM

dnl we use automake
AM_INIT_AUTOMAKE
AM_CONFIG_HEADER(config.h)
AM_PROG_AR
dnl for --enable-maintainer-mode fun use:
dnl AM_MAINTAINER_MODE 
dnl LT_INIT()

dnl AC_CONFIG_MACRO_DIRS([m4])

dnl Checks for programs.
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_RANLIB

dnl
dnl Use these compiler flags if we have gcc.
dnl
if test $ac_cv_prog_gcc = yes; then
  CCOPTS='-O2 -Wall'
  CFLAGS="$CCOPTS"
fi
test "x$prefix" != "xNONE" || prefix="/usr/local"
test "x$exec_prefix" != "xNONE" || exec_prefix="${prefix}"
trydir_i="${prefix}/include /usr/include"
trydir_l="${prefix}/lib /usr/lib"

if test "${prefix}" != "/usr/local" ; then
	trydir_i="${trydir_i} /usr/local/include"
	trydir_l="${trydir_l} /usr/local/lib"
fi

dnl Try include paths (especially on OSX)
trydir_i="${trydir_i} /usr/local/opt/openssl/include"
for xincdir in $trydir_i ; do
	if test ! -d "$xincdir" ; then
		continue;
	fi
	if test x"${INCLUDES}" = x; then
		INCLUDES="-I${xincdir}";
	else
		INCLUDES="$INCLUDES -I${xincdir}";
	fi
done
CPPFLAGS="${INCLUDES} $CPPFLAGS"

dnl Try library paths...
trydir_l="${trydir_l} /usr/local/opt/openssl/lib"
for xlibdir in $trydir_l ; do
	if test ! -d "$xlibdir" ; then
		continue;
	fi
	if test x"${LIBDIR}" = x; then
		LIBDIR="-L${xlibdir}";
	else
		LIBDIR="$LIBDIR -L${xlibdir}";
	fi
done
LDFLAGS="${LIBDIR} $LDFLAGS"

case "$host" in
mips-sony-bsd|mips-sony-newsos4)
        AC_DEFINE([NEED_SETPGRP], [1], [Need setpgrp to acquire controlling tty])
	;;
*-*-ultrix*)
        AC_DEFINE([NEED_SETPGRP], [1], [Need setpgrp to acquire controlling tty])
	;;
*-*-darwin*|*-*-*bsd*)
	AC_DEFINE([BSD_SCRIPT], [1], [/usr/bin/script is the bsd variant])
	;;
esac

AC_CHECK_LIB(util, forkpty)
dnl AC_CHECK_LIB(bsd, main)
AC_CHECK_LIB(socket, socket)
AC_CHECK_LIB(nsl, gethostbyname)
dnl AC_CHECK_LIB([m], [sqrt])
dnl AC_CHECK_LIB([net], [libnet_name_resolve], [AC_MSG_ERROR([libnet 1.0.x found. Requires libnet 1.1 or newer])])
dnl AC_CHECK_LIB([net], [libnet_init], ,[AC_MSG_ERROR([libnet 1.1.x not found])])
AC_CHECK_LIB(dl, dlopen)
AC_CHECK_LIB([crypto], [ENGINE_init], [], [AC_MSG_ERROR([libcrypto not found])])
AC_CHECK_LIB([ssl], [SSL_new], [], [AC_MSG_ERROR([libssl not found])])


dnl Checks for header files.
AC_HEADER_STDC
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS(sys/time.h unistd.h string.h pty.h openssl/srp.h util.h libutil.h netinet/in_systm.h)

AC_CHECK_HEADER(openssl/srp.h, [], [AC_MSG_ERROR([openssl/srp.h not found. Update OpenSSL?])])

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_PID_T
AC_HEADER_TIME

dnl Checks for library functions.
AC_FUNC_MEMCMP

AC_TYPE_SIGNAL
dnl If size_t is not defined, define size_t to be unsigned. 
AC_TYPE_SIZE_T 
dnl If uid_t is not defined, define uid_t to be int and gid_t to be int.
AC_TYPE_UID_T

AC_CHECK_FUNCS(gettimeofday memcpy strchr strlcat forkpty openpty getline stat64 open64 statvfs64)

AC_ARG_ENABLE([31337],
	AS_HELP_STRING([--enable-31337],
		[Enable experimental features.]),
	AC_DEFINE(D31337, 1, [Expermental feature])
)

AC_ARG_ENABLE(debug,
[  --enable-debug          Enable debug information],
	AC_DEFINE(DEBUG, 1, [Debug infos])
)

AS_IF([test x$enable_debug = xyes], AC_DEFINE(D31337, 1, [Expermental feature]))

AC_ARG_ENABLE(dist,
[  --enable-dist           Enable distribution mode, Use own libraries.],
	[DIST="yes"], [DIST="no"]
)

AC_ARG_ENABLE(static,
[  --enable-static          Compile static binary],
	[STATIC="yes"], [STATIC="no"]
)

if test x"$STATIC" = x"yes"; then
	CFLAGS="-static ${CFLAGS}"
fi

AC_OUTPUT([Makefile lib/Makefile tools/Makefile man/Makefile])

echo "
${PACKAGE_NAME}-${PACKAGE_VERSION} has been configured:

 Host..............: ${host}
 Compiler..........: ${CC}
 Compiler flags....: ${CFLAGS}
 Preprocessor flags: ${CPPFLAGS}
 Linker flags......: ${LDFLAGS}
 Libraries.........: ${LIBS}

Configuration complete. Now type: make all install"


