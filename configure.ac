# This is the autoconf file for GPTL-fortran package, a Fortran API
# for the GPTL C library. See the COPYING file for more information.
# Ed Hartnett, 5/21/18

# Specify minimum autoconf version.
AC_PREREQ([2.59])

# Initialize autoconf.
AC_INIT([GPTL-fortran], [5.6.0], [rosinski@ucar.edu])

# Keep libtool macros in an m4 directory.
AC_CONFIG_MACRO_DIR([m4])

# Find out about the host we're building on.
AC_CANONICAL_HOST

# Find out about the target we're building for.
AC_CANONICAL_TARGET

# Initialize automake.
AM_INIT_AUTOMAKE([foreign subdir-objects])

# Set up libtool.
LT_PREREQ([2.4])
LT_INIT()

# Find the Fortran compiler.
AC_PROG_FC()

# When built as part of the combined C/Fortran library distribution,
# the fortran library needs to be built with
# --enable-package-build. This tells the fortran library where to find
# the C library.
AC_ARG_ENABLE([package-build],
  [AS_HELP_STRING([--enable-package-build],
    [Set internally for package builds, should not be used by user.])])
test "x$enable_package_build" = xyes || enable_package_build=no
AM_CONDITIONAL([BUILD_PACKAGE], [test "x$enable_package_build" = xyes])

# Find the GPTL C library, unless this is a combined C/Fortran library
# build.
if test $enable_package_build = no; then
   AC_CHECK_LIB([gptl], [GPTLinitialize], [],
                        [AC_MSG_ERROR([Can't find or link to the GPTL C library.])])
fi

# Do we have MPI? If so, for now just ASSUME it provides a Fortran interface
AC_CHECK_FUNC([MPI_Init],
        [AC_DEFINE([HAVE_LIBMPI], [1], [MPI is present])])
AM_CONDITIONAL([HAVE_LIBMPI], [test "x$ac_cv_func_MPI_Init" = xyes])

# Determine the have_papi settings for this build.
AC_MSG_CHECKING([whether PAPI library support was built into the C library])
AC_CHECK_FUNC([gptl_papilibraryinit], [c_has_papi=yes], [c_has_papi=no])
if test "$c_has_papi" = yes; then
   AC_DEFINE([HAVE_PAPI], [1], [PAPI library support is present])
fi
AC_MSG_RESULT($c_has_papi)
AM_CONDITIONAL([HAVE_PAPI], [test "$c_has_papi" = yes])

# See if the C GPTL was built with ENABLE_PMPI, and set it here in accordance
AC_MSG_CHECKING([whether ENABLE_PMPI was set in the C library build])
AC_CHECK_FUNC([MPI_Send], [c_enabled_pmpi=yes], [c_enabled_pmpi=no])
if test $c_enabled_pmpi = yes; then
   AC_DEFINE([ENABLE_PMPI], [1], [--enable-pmpi was specified in the C library build])
fi
AC_MSG_RESULT($have_pmpi)
AM_CONDITIONAL([ENABLE_PMPI], [test "c$enabled_pmpi" = xyes])

# Check for function backtrace_symbols. If HAVE_BACKTRACE=yes will
# enable auto-generation of function name when auto-profiling and
# GPTLdopr_memusage has been enabled at run-time. If
# HAVE_BACKTRACE=no, function address will be printed instead.  GNU
# compilers: compile application code with -finstrument-functions
# -rdynamic Intel compilers: compile application code with
# -finstrument-functions -rdynamic -g
AC_CHECK_FUNC([backtrace_symbols], [have_backtrace=yes], [have_backtrace=no])
if test $have_backtrace = yes; then
   AC_DEFINE([HAVE_BACKTRACE], [1], [backtrace_symbols function is present])
fi
AM_CONDITIONAL([HAVE_BACKTRACE], [test "x$have_backtrace" = xyes])

# Need current language to be Fortran to get AC_OPENMP to do the right thing
AC_LANG_PUSH(Fortran)

# Enable OpenMP support if compiler supports it
useomp=no;
AC_OPENMP
if test "x$ac_cv_prog_c_openmp" != xunsupported; then
  useomp=yes;
  AC_DEFINE([THREADED_OMP], [1], [openmp support is present])
  FCFLAGS="$FCFLAGS $OPENMP_FCFLAGS"
fi
AM_CONDITIONAL([HAVE_OPENMP], [test x$useomp = xyes])

# --enable-debug means build with low optimization and add some code inside some #ifdef DEBUG
AC_ARG_ENABLE([debug], [AS_HELP_STRING([--enable-debug],
              [enable DEBUG ifdef])])
test "x$enable_debug" = xyes || enable_debug=no
if test $enable_debug = yes; then
   AC_DEFINE([DEBUG], [1], [set debug ifdef])
   FCFLAGS="$FCFLAGS -O0"
fi

AC_LANG_POP(Fortran)

dnl # We need the math library for some tests.
dnl AC_CHECK_LIB([m], [floor], [],
dnl                   [AC_MSG_ERROR([Can't find or link to the math library.])])

dnl dnl This barfs when CC is pgcc and F77 pgf90
dnl AC_FC_FUNC(z_zz,z_zz)
dnl case $z_zz in
dnl     z_zz__)     echo "Fortran name mangling: UNDERSCORING=-DFORTRANDOUBLEUNDERSCORE" ;;
dnl     z_zz_)      echo "Fortran name mangling: UNDERSCORING=-DFORTRANUNDERSCORE" ;;
dnl     Z_ZZ)       echo "Fortran name mangling: UNDERSCORING=-DFORTRANCAPS" ;;
dnl     *)          echo "Fortran name mangling: UNDERSCORING=" ;;
dnl esac

# Make sure this file is copied to build directories for tests to
# work.
AC_CONFIG_LINKS([test/gptlnl:test/gptlnl])

# Is doxygen installed?
AC_CHECK_PROGS([DOXYGEN], [doxygen])
if test -z "$DOXYGEN"; then
   AC_MSG_WARN([Doxygen not found - documentation will not be built])
fi

# This is a list of files to be built.
AC_CONFIG_FILES([Makefile
                 include/Makefile
                 src/Makefile
                 test/Makefile
		 ])

# Build the files listed above.
AC_OUTPUT()
