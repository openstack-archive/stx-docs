.. SPDX-License-Identifier: Apache-2.0
   Copyright (C) 2019 Intel Corporation
===================
Installation guides
===================

Installation steps for StarlingX are release specific. To install the
latest release of StarlingX, use the :doc:`/installation_guide/2018_10/index`.

To install a previous release of StarlingX, use the installation guide
for your specific release:

.. toctree::
   :maxdepth: 1

   /installation_guide/latest/index
   /installation_guide/2018_10/index

.. How to add a new release (installer and developer guides):
   1. Archive previous release
         1. Rename old 'latest' folder to the release name e.g. Year_Month
         2. Update links in old 'latest' to use new path e.g.
         :doc:`Libvirt/QEMU </installation_guide/latest/installation_libvirt_qemu>`
         becomes
         :doc:`Libvirt/QEMU </installation_guide/2018_10/installation_libvirt_qemu>`
   2. Add new release
         1. Add a new 'latest' dir and add the new version - likely this will be a copy of the previous version, with updates applied
         2. Make sure the new files have the correct version in the page title and intro sentence e.g. '2018.10.rc1 Installation Guide'
         3. Make sure all files in new 'latest' link to the correct versions of supporting docs (do this via doc link, so that it goes to top of page e.g. :doc:`/installation_guide/latest/index`)
         4. Make sure the new release index is labeled with the correct version name e.g
         .. _index-2019-05:
   3. Add the archived version to the toctree on this page
   4. If adding a new version *before* it is available (e.g. to begin work on new docs),
      make sure page text still directs user to the *actual* current release, not the
      future-not-yet-released version.
   5. When the release is *actually* available, make sure to update these pages:
      - index
      - installation guide
      - developer guide
      - release notes
