## This is an automake file, part of the GPTL-fortran package.

# This is the automake file for GPTL Fortran tests.
# Ed Hartnett, 5/21/18

# Find include directory and src directory (for .mod file).
AM_CPPFLAGS = -I$(top_srcdir)/include -I$(top_srcdir)/src

# Link to GPTL library.
AM_LDFLAGS = ${top_builddir}/src/libgptlf.la

# Test programs that will be built for all configurations.
check_PROGRAMS = errtest handle nlreader outoforder overhead testbacktrace
TESTS = errtest handle nlreader outoforder testbacktrace
# overhead

errtest_SOURCES = errtest.F90
handle_SOURCES = handle.F90
nlreader_SOURCES = nlreader.F90
outoforder_SOURCES = outoforder.F90
overhead_SOURCES = overhead.F90

# Extra test if we have backtrace.
if HAVE_BACKTRACE
check_PROGRAMS += testbacktrace
TESTS += testbacktrace
testbacktrace_SOURCES = testbacktrace.F90
endif

# Test PAPI functionality if libpapi was found.
if HAVE_PAPI
AM_CPPFLAGS += -DHAVE_PAPI
check_PROGRAMS += testinit testpapi
TESTS += testinit testpapi 
testpapi_SOURCES = testpapi.F90
testinit_SOURCES = testinit.F90
endif

if HAVE_MPI
AM_CPPFLAGS += -DHAVE_MPI
endif

# Test output to be deleted.
CLEANFILES = timing.*
