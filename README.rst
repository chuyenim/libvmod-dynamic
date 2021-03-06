============
vmod-dynamic
============

This branch is for varnish master (post 5.2 / 6.0)

Description
===========

The purpose of this module is to provide a dynamic director similar to
the DNS director from Varnish 3. It also was previously known as
`vmod-named`. This is not a drop-in replacement for the DNS director,
because in Varnish 3 the director had two modes of execution that
aren't compatible with changes in the backend and director subsystems
introduced by Varnish 4.0.

Instead a dynamic director relies on dynamic backends, supports white-listing
and even probes. However, just like the DNS director from Varnish 3 it has
limited capabilities because it relies on the system's resolver. It builds
against Varnish 4.1.3 and later versions.

Further documentation is available in the manual page ``vmod_dynamic(3)``.

Installation
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

If you build a dist tarball, you don't need any of the autotools, only
pkg-config and Varnish. You can build the module simply by running::

 ./configure
 make

For the test suite to work, please add this line to your ``/etc/hosts``::

	127.0.0.1 www.localhost img.localhost

then run::

	make check

Alternatively, the ``make check`` can also be skipped.

You can then proceed with the installation::

    sudo make install

Installation directories
------------------------

By default, the vmod ``configure`` script installs the built vmod in the
directory relevant to the prefix. The vmod installation directory can be
overridden by passing the ``vmoddir`` variable to ``make install``.

RPMs
----

Binary, debuginfo and source RPMs for VMOD dynamic are available at
packagecloud::

	https://packagecloud.io/uplex/varnish

The packages are built for Enterprise Linux 7 (el7), and hence will
run on compatible distros (such as RHEL7, Fedora, CentOS 7 and Amazon
Linux).

To set up your YUM repository for the RPMs, follow these instructions::

	https://packagecloud.io/uplex/varnish/install#manual-rpm

You will also need these additional repositories:

* EPEL7

  * ``yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm``

* Official Varnish packages from packagecloud (since version 5.2.0)

  * Follow the instructions at: https://packagecloud.io/varnishcache/varnish52/install#manual-rpm

  * Or (for version 6.0.0): https://packagecloud.io/varnishcache/varnish60/install#manual-rpm

If you have problems or questions concerning the RPMs, post an issue
to one of the source repository web sites, or contact
<varnish-support@uplex.de>.

See also
========

If you want to learn more about DNS, you can start with `RFC 1034`_ and other
RFCs that updated it over time. You may also have DNS already in place, or may
be interested in setting up a name server in your infrastructure. Below is a
non-exhaustive list of tools and services, but for free software name servers
you can have a look at debianadmin_.

.. _RFC 1034: https://tools.ietf.org/html/rfc1034
.. _debianadmin: http://www.debianadmin.com/open-source-domain-name-systemdns-servers.html

DNS in the cloud (in alphabetic order):

- AWS_
- Azure_
- `Digital Ocean`_
- `Google Cloud`_
- Heroku_

.. _AWS: https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-instance-addressing.html
.. _Azure: https://azure.microsoft.com/en-us/documentation/articles/dns-overview/
.. _Digital Ocean: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean
.. _Google Cloud: https://cloud.google.com/dns/
.. _Heroku: https://devcenter.heroku.com/articles/zerigo_dns

DNS and containers (in alphabetic order):

* `DC/OS`_ (Mesos)
* `Docker Machine`_ (sort of)
* Kubernetes_

.. _DC/OS: https://docs.mesosphere.com/1.7/usage/service-discovery/mesos-dns/
.. _Docker Machine: https://www.npmjs.com/package/docker-machine-dns
.. _Kubernetes: http://kubernetes.io/docs/admin/dns/
