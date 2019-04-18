## This is an automake file, part of the GPTL-fortran package.

# This is the automake file for GPTL-fortran documentation.
# Ed Hartnett, 4/18/19

directory = $(top_srcdir)/docs

man_MANS = $(directory)

$(directory): doxyfile.stamp

doxyfile.stamp: Doxyfile
	$(DOXYGEN) $^
	echo Timestamp > $@

CLEANFILES = doxyfile.stamp

all-local: doxyfile.stamp
clean-local:
	-rm -rf $(top_srcdir)/docs/man

EXTRA_DIST = Doxyfile.in
