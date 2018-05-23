## This is an automake file, part of the GPTL-fortran package.

# This is the automake file for GPTL Fortran tests.
# Ed Hartnett, 5/21/18

# Find include directory and src directory (for .mod file).
AM_CPPFLAGS = -I$(top_srcdir)/include -I$(top_srcdir)/src

# Link to GPTL library.
AM_LDFLAGS = ${top_builddir}/src/libgptlf.la

# # Test programs that will be built for all configurations.
check_PROGRAMS = testinit
TESTS = testinit
testinit_SOURCES = testinit.F90

# # Build these tests if PAPI is present.
# if HAVE_PAPI
# check_PROGRAMS += avail testpapi
# TESTS += avail testpapi
# noinst_PROGRAMS += papiomptest knownflopcount
# endif

# # Build these tests if MPI is present.
# if HAVE_MPI
# check_PROGRAMS += summary pmpi
# TESTS += run_par_test.sh
# endif

# # Build these if the user selected --enable-autoprofile-tests during configure.
# if TEST_AUTOPROFILE
# check_PROGRAMS += cygprofile cygprofilesubs
# TESTS += cygprofile cygprofilesubs
# endif

# # Build these if the user selected --enable-nestedomp during configure.
# if ENABLE_NESTEDOMP
# check_PROGRAMS += nestedomp
# TESTS += nestedomp
# endif

# Test output to be deleted.
CLEANFILES = timing.*
