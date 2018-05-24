## This is an automake file, part of the GPTL-fortran package.

# This is the automake file for GPTL Fortran tests.
# Ed Hartnett, 5/21/18

# Find include directory and src directory (for .mod file).
AM_CPPFLAGS = -I$(top_srcdir)/include -I$(top_srcdir)/src

# Link to GPTL library.
AM_LDFLAGS = ${top_builddir}/src/libgptlf.la

libgptlf_la_FCFLAGS = -I$(top_srcdir)/include

if HAVE_PAPI
libgptlf_la_FCFLAGS += -DHAVE_PAPI
endif

if HAVE_MPI
libgptlf_la_FCFLAGS += -DHAVE_MPI
endif

# Test programs that will be built for all configurations.
check_PROGRAMS = testinit
TESTS = testinit
testinit_SOURCES = testinit.F90

# Test output to be deleted.
CLEANFILES = timing.*
