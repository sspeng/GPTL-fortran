# This is the main automake file for the GPTL fortran API.
# Ed Hartnett, 5/21/18

# This directory stores libtool macros, put there by aclocal.
ACLOCAL_AMFLAGS = -I m4

# These files get added to the distribution.
EXTRA_DIST = COPYING

# This is the list of subdirs for which Makefiles will be constructed
# and run.
SUBDIRS = src test include

install-data-hook:

	@echo ''
	@echo '+-------------------------------------------------------------+'
	@echo '| Congratulations! You have successfully installed            |'
	@echo '| GPTL-fortran!                                               |'
	@echo '|                                                             |'
	@echo '| CAUTION:                                                    |'
	@echo '|                                                             |'
	@echo '| If you have not already run "make check", then we strongly  |'
	@echo '| recommend you do so. It does not take very long.            |'
	@echo '|                                                             |'
	@echo '| GPTL was developed with support from multiple institutions, |'
	@echo '| including NOAA, NCAR, and ORNL. It is maintained by the GSD |'
	@echo '| group at NOAA.                                              |'
	@echo '+-------------------------------------------------------------+'
	@echo ''
