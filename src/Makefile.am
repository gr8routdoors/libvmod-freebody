AM_CFLAGS = $(VARNISHAPI_CFLAGS) -Wall -Werror
AM_LDFLAGS = $(VARNISHAPI_LIBS) $(VMOD_LDFLAGS)

vmod_LTLIBRARIES = libvmod_freebody.la

libvmod_freebody_la_SOURCES = \
	vmod_freebody.c

nodist_libvmod_freebody_la_SOURCES = \
	vcc_freebody_if.c \
	vcc_freebody_if.h

dist_man_MANS = vmod_freebody.3

@BUILD_VMOD_FREEBODY@

AM_TESTS_ENVIRONMENT = \
	PATH="$(VMOD_TEST_PATH)" \
	LD_LIBRARY_PATH="$(VARNISH_LIBRARY_PATH)"
TEST_EXTENSIONS = .vtc
VTC_LOG_COMPILER = varnishtest -v
AM_VTC_LOG_FLAGS = -Dvmod_freebody="$(VMOD_FREEBODY)"

TESTS = @VMOD_TESTS@

EXTRA_DIST = \
	vmod_freebody.vcc \
	$(VMOD_TESTS)
