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


if test x$enable_papi = xyes; then
# Check for papi library.
  AC_CHECK_LIB([papi], [PAPI_library_init])
  AC_MSG_CHECKING([whether system can support PAPI])
  if test "x$ac_cv_lib_papi_PAPI_library_init" = xyes; then
    # If we have PAPI library, check /proc/sys/kernel/perf_event_paranoid
    # to see if we have permissions.
    if test -f /proc/sys/kernel/perf_event_paranoid; then
      if test `cat /proc/sys/kernel/perf_event_paranoid` != 1; then
         AC_MSG_ERROR([PAPI library found, but /proc/sys/kernel/perf_event_paranoid != 1
                            try sudo sh -c 'echo 1 >/proc/sys/kernel/perf_event_paranoid'])
      fi
    fi
    AC_DEFINE([HAVE_PAPI], [1], [PAPI library is present and usable])
    LDFLAGS="$LDFLAGS -lpapi"
    have_papi=yes
  else
    have_papi=no
  fi
  AC_MSG_RESULT($have_papi)
fi
AM_CONDITIONAL([HAVE_PAPI], [test "x$have_papi" = xyes])

# See if the C GPTL was built with ENABLE_PMPI, and set it here in accordance
AC_MSG_CHECKING([whether ENABLE_PMPI was set in the C library build])
AC_CHECK_FUNC([MPI_Send], [c_enabled_pmpi=yes], [c_enabled_pmpi=no])
if test $c_enabled_pmpi = yes; then
   AC_DEFINE([ENABLE_PMPI], [1], [--enable-pmpi was specified in the C library build])
fi
AC_MSG_RESULT($c_enabled_pmpi)
AM_CONDITIONAL([ENABLE_PMPI], [test "$c_enabled_pmpi" = yes])

# AC_CHECK_LIB doesn't work for libunwind due to funkiness in the lib so use AC_CHECK_HEADER
AC_MSG_CHECKING([for libunwind])
AC_CHECK_HEADER([libunwind.h], found_libunwind=yes;)
AC_MSG_RESULT([$ac_cv_libunwind_h])

AC_MSG_CHECKING([for backtrace])
AC_CHECK_FUNC([backtrace_symbols], found_backtrace=yes;)
AC_MSG_RESULT([$ac_cv_func_backtrace_symbols])

# For auto-profiling, backtrace is the default method to recover current function name.
# If not available, try libunwind. If neither are available, auto-profiling will not be
# possible. To use auto auto-profiling with GNU or Intel compilers:
# Compile application code with -g and appropriate auto-instrumentation flag
# Probably the appropriate dynamic linking flag (maybe -rdynamic or -Bdynamic) also is needed
#
AC_ARG_ENABLE([libunwind],
    AS_HELP_STRING([--enable-libunwind], [Enable auto-profiling capability via libunwind]))

AC_ARG_ENABLE([backtrace],
    AS_HELP_STRING([--enable-backtrace], [Enable auto-profiling capability via backtrace]))

# Default is to enable auto-profiling capability via backtrace
# Default is to disnable auto-profiling capability via libunwind.
# Cannot have both methods enabled at once
if test "x$enable_backtrace" != xno; then
  if test "x$enable_libunwind" = xyes; then
    AC_MSG_ERROR([--enable-libunwind requires --disable-backtrace])
  fi
# Set define for BACKTRACE if user didn't say no and backtrace found
  if test "x$found_backtrace" = xyes; then
    usebacktrace=yes;
    AC_DEFINE([HAVE_BACKTRACE], [1], [backtrace will be used])
    AC_MSG_NOTICE([backtrace enabled for auto-profiling: dynamic linking flag (see below) probably also needed])
  fi
fi

# Set define for LIBUNWIND if user said yes and backtrace found
if test "x$enable_libunwind" = xyes; then
  if test "x$found_libunwind" = xyes; then
    useunwind=yes;
    AC_DEFINE([HAVE_LIBUNWIND], [1], [libunwind will be used])
    AC_MSG_NOTICE([libunwind enabled for auto-profiling: dynamic linking flag (see below) probably also needed])
  fi
fi

# Above logic ensures that both haveunwind and havebacktrace cannot both = yes
AM_CONDITIONAL([HAVE_LIBUNWIND], [test x$useunwind = xyes])
AM_CONDITIONAL([HAVE_BACKTRACE], [test x$usebacktrace = xyes])

# Did the user specify an MPI launcher other than mpiexec?
AC_MSG_CHECKING([whether a user specified program to run mpi programs])
AC_ARG_WITH([mpiexec],
              [AS_HELP_STRING([--with-mpiexec=<command>],
                              [Specify command to launch MPI parallel tests.])],
            [MPIEXEC=$with_mpiexec], [MPIEXEC=mpiexec])
AC_MSG_RESULT([$MPIEXEC])
AC_SUBST([MPIEXEC], [$MPIEXEC])

# Need current language to be Fortran to get AC_OPENMP to do the right thing
AC_LANG_PUSH(Fortran)

# See if auto-instrumentation flag is available. If so, set INSTRFLAG for testing
if test "x$useunwind" = xyes || test "x$usebacktrace" = xyes; then
  AX_CHECK_COMPILE_FLAG([-finstrument-functions], [finstrf=yes], [finstrf=no])
  AX_CHECK_COMPILE_FLAG([-Minstrument:functions], [minstrf=yes], [minstrf=no])
fi

# Auto-instrumentation often requires a special link flag
AX_CHECK_LINK_FLAG(-rdynamic, [AC_SUBST([INSTR_LINK], [-rdynamic])], [rdynamic=no])
if test "x$rdynamic" = xno; then
  AX_CHECK_LINK_FLAG(-Bdynamic, [AC_SUBST([INSTR_LINK], [-Bdynamic])], [bdynamic=no])
  if test "x$bdynamic = xno"; then
    AC_MSG_WARN([Dynamic linking flag not found: Auto-instrumentation may not work properly])
  fi
fi

AM_CONDITIONAL([HAVE_INSTRFLAG], [test "x$finstrf" = xyes || test "x$minstrf" = xyes])
if test "x$finstrf" = xyes; then
  AC_SUBST([INSTRFLAG],[-finstrument-functions])
elif test "x$minstrf" = xyes; then
  AC_SUBST([INSTRFLAG],[-Minstrument:functions])
fi

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
AC_CONFIG_FILES([test/run_par_pmpi_test.sh], [chmod ugo+x test/run_par_pmpi_test.sh])

# Build the files listed above.
AC_OUTPUT()
