# This is the autoconf file for GPTL-fortran package, a Fortran API
# for the GPTL C library. See the COPYING file for more information.
# Ed Hartnett, 5/21/18

# Specify minimum autoconf version.
AC_PREREQ([2.59])

# Initialize autoconf.
AC_INIT([GPTL-fortran], [5.6.0], [james.rosinski@noaa.gov])

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

# Check for papi library.
AC_CHECK_LIB([papi], [PAPI_library_init])
AM_CONDITIONAL([HAVE_PAPI], [test "x$ac_cv_lib_papi_PAPI_library_init" = xyes])
if test "x$ac_cv_lib_papi_PAPI_library_init" = xyes; then
   AC_DEFINE([HAVE_PAPI], [1], [PAPI library is present])
fi

# Find the GPTL C library.
AC_CHECK_LIB([gptl], [GPTLinitialize], [],
                  [AC_MSG_ERROR([Can't find or link to the GPTL C library.])])

# Do we have MPI?
AC_CHECK_FUNC([MPI_Init],
        [AC_DEFINE([HAVE_MPI], [1], [MPI is present])])
AM_CONDITIONAL([HAVE_MPI], [test "x$ac_cv_func_MPI_Init" = xyes])

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
