===================
Installation guides
===================

Installation steps for StarlingX are release-specific.
The following list provides help on choosing the correct installation steps:

- The "current" release is the most recent offically released version of StarlingX.
  To install the current release of StarlingX, use the :doc:`/installation_guide/current/index`.

- The "latest" release is the forthcoming version under development.
  To install the latest release of StarlingX, use the :doc:`/installation_guide/latest/index`.

- Following are all available installation steps for StarlingX:

  .. toctree::
     :maxdepth: 1

     /installation_guide/current/index
     /installation_guide/latest/index


.. Steps you must take when a new release of the installer and developer guides occurs:

.. 1. Archive the "current" release:
         1. Rename the "current" folder to the release name using the <Year_Month> convention (e.g. 2018_10).
         2. Get inside your new folder (i.e. the old "current" folder) and update all links in the *.rst
         files to use the new path (e.g. :doc:`Libvirt/QEMU </installation_guide/current/installation_libvirt_qemu>`
         becomes
         :doc:`Libvirt/QEMU </installation_guide/<Year_Month>/installation_libvirt_qemu>`
         3. You might want to change your working directory to /<Year_Month> and use Git to grep for
         the "current" string (i.e. 'git grep "current" *').  For each applicable occurence, make
         the call whether or not to convert the string to the actual archived string "<Year_Month>".
         Be sure to scrub all files for the "current" string in both the "installation_guide"
         and "developer_guide" folders downward.
   2. Add the new "current" release:
         1. Rename the existing "latest" folders to "current".  This assumes that "latest" represented
         the under-development release that just officially released.
         2. Get inside your new folder (i.e. the old "latest" folder) and update all links in the *.rst
         files to use the new path (e.g. :doc:`Libvirt/QEMU </installation_guide/latest/installation_libvirt_qemu>`
         becomes
         :doc:`Libvirt/QEMU </installation_guide/current/installation_libvirt_qemu>`
         3. You might want to change your working directory to the "current" directory and use Git to grep for
         the "latest" string (i.e. 'git grep "latest" *').  For each applicable occurence, make
         the call whether or not to convert the string to "current".
         Be sure to scrub all files for the "latest" string in both the "installation_guide"
         and "developer_guide" folders downward.
         4. Because the "current" release is now available, make sure to update these pages:
            - index
            - installation guide
            - developer guide
            - release notes
   3. Create a new "latest" release, which are the installation and developer guides under development:
         1. Copy your "current" folders and rename them "latest".
         2. Make sure the new files have the correct version in the page title and intro
         sentence (e.g. '2019.10.rc1 Installation Guide').
         3. Make sure all files in new "latest" link to the correct versions of supporting
         docs.  You do this through the doc link, so that it resolves to the top of the page
         (e.g. :doc:`/installation_guide/latest/index`)
         4. Make sure the new release index is labeled with the correct version name
         (e.g .. _index-2019-05:)
         5. Add the archived version to the toctree on this page.  You want all possible versions
         to build.
         6. Since you are adding a new version ("latest") *before* it is available
         (e.g. to begin work on new docs), make sure page text still directs user to the
         "current" release and not to the under development version of the manuals.




