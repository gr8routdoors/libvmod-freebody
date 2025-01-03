============
vmod-freebody
============

SYNOPSIS
========

import freebody;

DESCRIPTION
===========

The VMOD provides access to request body content so that it can be altered and/or hashed as part of object keys.

Essentially, it attempts to provide a subset of [xbody](https://docs.varnish-software.com/varnish-enterprise/vmods/xbody) functionality for use in vanilla Varnish installs.

FUNCTIONS
=========

get_req_body
-----

Prototype
        ::

                get_req_body()
Return value
	STRING
Description
	Returns the request body.
Freebody
        ::

                set resp.http.x-body = freebody.get_req_body();

INSTALLATION
============

The source tree is based on autotools to configure the building, and
does also have the necessary bits in place to do functional unit tests
using the ``varnishtest`` tool.

Building requires the Varnish header files and uses pkg-config to find
the necessary paths.

Usage::

 ./autogen.sh
 ./configure

If you have installed Varnish to a non-standard directory, call
``autogen.sh`` and ``configure`` with ``PKG_CONFIG_PATH`` pointing to
the appropriate path. For instance, when varnishd configure was called
with ``--prefix=$PREFIX``, use

::

 export PKG_CONFIG_PATH=${PREFIX}/lib/pkgconfig
 export ACLOCAL_PATH=${PREFIX}/share/aclocal

The module will inherit its prefix from Varnish, unless you specify a
different ``--prefix`` when running the ``configure`` script for this
module.

Make targets:

* make - builds the vmod.
* make install - installs your vmod.
* make check - runs the unit tests in ``src/tests/*.vtc``.
* make distcheck - run check and prepare a tarball of the vmod.

If you build a dist tarball, you don't need any of the autotools or
pkg-config. You can build the module simply by running::

 ./configure
 make

Installation directories
------------------------

By default, the vmod ``configure`` script installs the built vmod in the
directory relevant to the prefix. The vmod installation directory can be
overridden by passing the ``vmoddir`` variable to ``make install``.

USAGE
=====

In your VCL you could then use this vmod along the following lines::

        import std;
        import freebody;

        sub vcl_recv {
                # We must first cache the request body before we can access it
                if (!std.cache_req_body(8KB)) {
                        return (hash);
                }

                # Grab the request so we can manipulate it
                set resp.http.X-Body = freebody.get_req_body();
        }

COMMON PROBLEMS
===============

* configure: error: Need varnish.m4 -- see README.rst

  Check whether ``PKG_CONFIG_PATH`` and ``ACLOCAL_PATH`` were set correctly
  before calling ``autogen.sh`` and ``configure``

* Incompatibilities with different Varnish Cache versions

  Make sure you build this vmod against its correspondent Varnish Cache version.
  For instance, to build against Varnish Cache 4.1, this vmod must be built from
  branch 4.1.
