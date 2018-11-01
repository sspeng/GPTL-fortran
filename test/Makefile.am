## This is an automake file, part of the GPTL-fortran package.

# This is the automake file for GPTL Fortran tests.
# Ed Hartnett, 5/21/18

# Find include directory and src directory (for .mod file).
AM_CPPFLAGS = -I$(top_srcdir)/include -I$(top_srcdir)/src

# Link to GPTL fortran library.
AM_LDFLAGS = ${top_builddir}/src/libgptlf.la

if BUILD_PACKAGE
LDADD = ${top_builddir}/../GPTL/src/libgptl.la
endif

MODFILE = ${top_builddir}/src/gptl.mod

# Test programs that will be built for all configurations.
check_PROGRAMS = testbasics errtest handle  outoforder	\
overhead testbacktrace nlreader
TESTS = testbasics errtest handle outoforder testbacktrace nlreader

# Tests overhead and nlreader do not succeed yet.
# overhead  

# Test dependancies needed.
testbasics_SOURCES = testbasics.F90 ${MODFILE}
errtest_SOURCES = errtest.F90 ${MODFILE}
handle_SOURCES = handle.F90 ${MODFILE}
nlreader_SOURCES = nlreader.F90 ${MODFILE}
outoforder_SOURCES = outoforder.F90 ${MODFILE}
overhead_SOURCES = overhead.F90 ${MODFILE}

# Extra test if we have backtrace.
if HAVE_BACKTRACE
check_PROGRAMS += testbacktrace
TESTS += testbacktrace
testbacktrace_SOURCES = testbacktrace.F90 ${MODFILE}
endif

# Test PAPI functionality if libpapi was found.
if HAVE_PAPI
AM_CPPFLAGS += -DHAVE_PAPI
check_PROGRAMS += testinit testpapi
TESTS += testinit testpapi 
testpapi_SOURCES = testpapi.F90 ${MODFILE}
testinit_SOURCES = testinit.F90 ${MODFILE}
endif

# Extra test for parallel builds. (Does not build yet).
#if HAVE_MPI
# check_PROGRAMS += threadohd
# TESTS += threadohd
# threadohd_SOURCES = threadohd.F90 ${MODFILE}
#endif

# Test output to be deleted.
CLEANFILES = timing.*

# This file is required for the nlreader test.
EXTRA_DIST = gptlnl
