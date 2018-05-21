# This is part of NOAAs GPTL-fortran package.

# This automake file generates the Makefile to build the GPTL-fotran
# library.
# Ed Hartnett 5/21/18

#AM_CPPFLAGS = -I$(top_srcdir)/include 

libgptlf_la_CPPFLAGS = ${AM_CPPFLAGS}

# This is our output. The GPTL-fortran library.
lib_LTLIBRARIES = libgptlf.la
libgptlf_la_SOURCES = gptlf.F90 printmpistatussize.F90	\
process_namelist.F90 timingModule.F90

