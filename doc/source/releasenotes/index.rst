.. _release-notes:

=============
Release Notes
=============

These are the release notes for StarlingX release stx.2018.10.

**Note:** StarlingX uses a "year.month" pattern for release version
numbers, indicating the year and month of release.

---------
ISO Image
---------

You can find a pre-built image at
`CENGN’s StarlingX mirror
<http://mirror.starlingx.cengn.ca/mirror/starlingx/centos/2018.10/20181110/outputs/iso/>`__.

------------
New Features
------------

+-----------------------------------+-----------------------------------+
| StoryBoard ID                     | Feature                           |
+===================================+===================================+
| N/A                               | ovs-dpdk integration              |
+-----------------------------------+-----------------------------------+
| 2002820                           | Support for external Ceph backend |
+-----------------------------------+-----------------------------------+
| 2202821                           | Support for adding compute nodes  |
|                                   | to all-in-one duplex deployments  |
+-----------------------------------+-----------------------------------+
| 2002822                           | Support remote client for Windows |
|                                   | and Mac OS                        |
+-----------------------------------+-----------------------------------+
| 2003115                           | Deprecate proprietary Cinder      |
|                                   | volume backup and restore         |
+-----------------------------------+-----------------------------------+
| 2002825                           | Support Gnocchi storage backend   |
|                                   | for OpenStack telemetry           |
+-----------------------------------+-----------------------------------+
| 2002847                           | Add ntfs-3g packages              |
+-----------------------------------+-----------------------------------+
| 2002826                           | Memcached integration             |
+-----------------------------------+-----------------------------------+
| 2002935                           | Support for Precision Time        |
|                                   | Protocol (PTP)                    |
+-----------------------------------+-----------------------------------+
| 2003087                           | Generalized interface and network |
|                                   | configuration                     |
+-----------------------------------+-----------------------------------+
| 2003518                           | Enable Swift on controllers       |
+-----------------------------------+-----------------------------------+
| 2002712                           | StarlingX API documentation       |
+-----------------------------------+-----------------------------------+

-------------
Other changes
-------------

+-----------------------------------+-----------------------------------+
| StoryBoard ID                     | Change                            |
+===================================+===================================+
| 2002827                           | Decouple Service Management REST  |
|                                   | API from sysinv                   |
+-----------------------------------+-----------------------------------+
| 2002828                           | Decouple Fault Management from    |
|                                   | stx-config                        |
+-----------------------------------+-----------------------------------+
| 2002829                           | Decouple Guest-server/agent from  |
|                                   | stx-metal                         |
+-----------------------------------+-----------------------------------+
| 2002832                           | Replace compute-huge init script  |
+-----------------------------------+-----------------------------------+
| 2002834                           | Add distributed cloud repos to    |
|                                   | StarlingX                         |
+-----------------------------------+-----------------------------------+
| 2002846                           | Python Optimization               |
+-----------------------------------+-----------------------------------+
| 2003389, 2003596                  | Upgrade kernel and srpm/rpms to   |
|                                   | CentOS 7.5                        |
+-----------------------------------+-----------------------------------+
| 3003396, 2003339                  | Upgrade libvirt to 4.7.0          |
+-----------------------------------+-----------------------------------+
| 3002891                           | Stx-gui plug-in for Horizon       |
+-----------------------------------+-----------------------------------+
| Many                              | Build enhancements, cleanups and  |
|                                   | optimizations                     |
+-----------------------------------+-----------------------------------+
| Many                              | Enable basic zuul checks and      |
|                                   | linters                           |
+-----------------------------------+-----------------------------------+
| Many                              | Python 2 to 3 upgrade for         |
|                                   | stx-update, stx-metal, stx-fault, |
|                                   | stx-integ                         |
+-----------------------------------+-----------------------------------+

-------
Testing
-------

Please take a look at our
`test plan <https://wiki.openstack.org/wiki/StarlingX/stx.2018.10_Testplan>`__
for the list of tests executed on this release.

View the
`testing summary <https://wiki.openstack.org/wiki/StarlingX/stx.2018.10_TestingSummary>`__
to see the status of testing for this release.

------------------------------
Project-specific release notes
------------------------------

* `Bare Metal <stx-metal/index.html>`__
* `Clients <stx-clients/index.html>`__
* `Configuration <stx-config/index.html>`__
* `Distributed Cloud <stx-distcloud/index.html>`__
* `Distributed Cloud Client <stx-distcloud-client/index.html>`__
* `Fault Management <stx-fault/index.html>`__
* `High Availability <stx-ha/index.html>`__
* `Horizon Plugin (GUI) <stx-gui/index.html>`__
* `Integration <stx-integ/index.html>`__
* `NFV <stx-nfv/index.html>`__
* `Software Updates <stx-update/index.html>`__
* `Tools <stx-tools/index.html>`__
* `Upstream <stx-upstream/index.html>`__
