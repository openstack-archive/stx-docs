===============================
Documentation Contributor Guide
===============================

This guide provides information that allows you to contribute to the
`StarlingX documentation <https://docs.starlingx.io/>`_.

The information focuses on what it takes to materially contribute to the
StarlingX documentation.
Information common to OpenStack workflow, writing styles, and conventions
is not included in this guide.
You can find that type of information in the
`OpenStack Documentation Contributor Guide <https://docs.openstack.org/doc-contrib-guide/index.html>`_.

---------
Locations
---------

StarlingX documentation consists of several types of manuals and is found
in the **stx-docs** and **stx-specs** projects (i.e. repositories).
These projects contain hierarchy that organizes the documentation by topic:

-  **Installation Guide**

   - Describes how to install StarlingX onto Bare Metal or into a virtual
     environment.
     This guide is located in **stx-docs/doc/source/installation_guide**.
-  **Developer Guide**

   - Describes how to build a StarlingX ISO image from the "master" branch.
     This guide is located in **stx-docs/doc/source/developer_guide**.
-  **Project Specifications**

   - Describes specifications, specification templates, and processes
     for submitting a specification.
     This guide is located in **stx-specs/doc/source**.
-  **REST API Reference**

   - Describes StarlingX APIs.
     The landing page for the API Reference manuals is located in
     **stx-docs/doc/source/api-ref**.
     The RST files that provide content to the API Reference manuals
     are located in **stx-docs/api-ref/source**.
-  **Release Notes**

   - Provides release-specific information.
     This guide is located in **stx-docs/doc/source/releasenotes**.
-  **Contribute**

   - Provides guides on how to contribute to StarlingX API documentation,
     Release Notes, and general documentation.
     These guides are located in **stx-docs/doc/source/contributor**.

--------------------
Directory Structures
--------------------

Directory structures vary depending on the type of documentation involved.
Basically, you can think of the structure as one or more RST files per
book.
A simple book consists of a single **index.rst** file.
A more complicated book could consist of an **index.rst** file as the
book's landing page and a set of additional RST files for major sections
of the book.

Following is the structure of the **stx-docs** repository:

::

    .
    ├── api-ref
    │   └── source
    │       ├── api-ref-blockstorage-v2-cgcs-ext.rst
    │       ├── api-ref-compute-v2-cgcs-ext.rst
    │       ├── api-ref-image-v2-cgcs-ext.rst
    │       ├── api-ref-networking-v2-cgcs-ext.rst
    │       ├── conf.py
    │       └── index.rst
    ├── doc
    │   └── source
    │       ├── api-ref
    │       │   └── index.rst
    │       ├── conf.py
    │       ├── contributor
    │       │   ├── api_contribute_guide.rst
    │       │   ├── doc_contribute_guide.rst
    │       │   ├── index.rst
    │       │   └── release_note_contribute_guide.rst
    │       ├── developer_guide
    │       │   └── index.rst
    │       ├── index.rst
    │       ├── installation_guide
    │       │   ├── controller_storage.rst
    │       │   ├── dedicated_storage.rst
    │       │   ├── duplex.rst
    │       │   ├── index.rst
    │       │   ├── installation_libvirt_qemu.rst
    │       │   └── simplex.rst
    │       └── releasenotes
    │           └── index.rst
    ├── README.rst
    ├── test-requirements.txt
    └── tox.ini

The structure for the API Reference documentation deserves
some extra explanation.
Most RST files for the API Reference content reside in top-level
StarlingX repositories (e.g. **stx-metal**, **stx-config**, and so
forth).
However, four API Reference RST files do not: "Block Storage",
"Compute", "Image", and "Network".
These RST files reside in **stx-docs/api-ref/source** as seen
in the previous tree diagram.
While the **stx-docs/api-ref/source/index.rst** file does exist along
side these other four RST files, it does so only because the Sphinx
process needs that index file to build out the final web documentation
tree.
The actual landing page (content) for the API Reference documents
is in the **stx-docs/doc/source/api-ref/index.rst** file.

Here is the structure of the **stx-specs** project:

::

    .
    ├── doc
    │   └── source
    │       ├── conf.py
    │       ├── index.rst
    │       └── specs
    │           ├── 2019.03
    │           │   ├── approved -> ../../../../specs/2019.03/approved
    │           │   ├── implemented -> ../../../../specs/2019.03/implemented
    │           │   ├── index.rst
    │           │   └── template.rst -> ../../../../specs/2019.03/approved/STX_Example_Spec.rst
    │           └── instructions.rst -> ../../../specs/instructions.rst
    ├── README.rst
    ├── specs
    │   ├── 2019.03
    │   │   ├── approved
    │   │   │   ├── containerization-2002840-local-docker-registry.rst
    │   │   │   ├── containerization-2002843-kubernetes-platform-support.rst
    │   │   │   ├── containerization_2003907_docker-image-generation.rst
    │   │   │   ├── containerization-2003908-armada-integration.rst
    │   │   │   ├── containerization-2003909-helm-chart-overrides.rst
    │   │   │   ├── containerization-2003910-system-deployment-of-containerized-openstack-infrastructure.rst
    │   │   │   ├── mirror_2003906_enable_external_mirror.rst
    │   │   │   ├── multi-os-2003768-refactor-init-config-patches.rst
    │   │   │   ├── multi-os-2004039-variable-substitution.rst
    │   │   │   ├── standardize-makefiles-for-multi-os.rst
    │   │   │   ├── STX_Example_Spec.rst
    │   │   │   └── sysinv_2002950-decouple-system-configuration-from-inventory.rst
    │   │   └── implemented
    │   │       └── _placeholder.rst
    │   ├── approved
    │   │   └── containerization-2002844-CEPH-persistent-storage-backend-for-Kubernetes.rst
    │   ├── instructions.rst
    │   └── STX_Example_Spec.rst -> 2019.03/approved/STX_Example_Spec.rst
    ├── test-requirements.txt
    └── tox.ini

The **stx-specs/docs/source/index.rst** file is the main landing page for the
StarlingX specifications page (<https://docs.starlingx.io/specs/index.html>`_).

The **stx-specs/specs/2019.03** area contains the RST files for approved and
implemented specs.

-----------------
Updating a Manual
-----------------

If you need to update an existing manual, you need to find the
appropriate RST source file, make your modifications, test them
(i.e. build the manual), and then submit the changes to Gerrit
for approval.

As an example, suppose you wanted to update the
`Developer Guide <https://docs.starlingx.io/developer_guide/index.html>`_.

The structure for the Developer Guide is as follows:

::

    ├── doc
    │   └── source
    │       ├── developer_guide
    │       │   └── index.rst

The content for the manual exists in the **index.rst** file.
This file is the landing page and all the content.

Suppose you needed to update a more complicated manual such as the
`Installation Guide <https://docs.starlingx.io/installation_guide/index.html>`_.
That manual's source structure is as follows:

::

    ├── doc
    │   └── source
    │       ├── installation_guide
    │       │   ├── controller_storage.rst
    │       │   ├── dedicated_storage.rst
    │       │   ├── duplex.rst
    │       │   ├── index.rst
    │       │   ├── installation_libvirt_qemu.rst
    │       │   └── simplex.rst

The **index.rst** file is the landing page plus the main content for the
Installation Guide.
The remaining five RST files hold additional content for the guide accessed
through links from the landing page.

-----------------
Creating a Manual
-----------------

Creating a new manual for **stx-docs** involves minimally providing the
**index.rst** file.
If the manual is more complex with additional content outside of the
**index.rst** file, you need to provide additional RST files as well.

As an example, consider a new manual that resides in
**stx-docs/doc/source/my-guide**.
Furthermore, suppose this manual's **index.rst** file contained two
links to additional complicated topics: "Topic 1" and
"Topic 2".

The content for the new manual exists in three files:

* stx-docs/doc/source/my-guide/index.rst**
* stx-docs/doc/source/my-guide/topic_1.rst**
* stx-docs/doc/source/my-guide/topic_2.rst**

Following shows the hierarchy:

::

    ├── doc
    │   └── source
    │       ├── my_guide
    │       │   ├── index.rst
    │       │   ├── topic_1.rst
    │       │   ├── topic_2.rst


-----------------------
Creating the Index File
-----------------------

The **index.rst** file provides captioning, a brief
description of the document, and the table-of-contents (TOC) structure
with instructions on how to display or hide sub-topics.

The syntax of the **index.rst** file is fixed. Following shows the
sample **index.rst** file for the new guide:

::

     ========
     My Guide
     ========

     The new guide.

     - :ref:`Topic 1 <topic_1>`
     - :ref:`Topic 2 <topic_2>`

     .. toctree::
        :hidden:

        topic_1
        topic_2

Following are explanations for each of the four areas of the
**index.rst** file:

-  **Reference title:** Literal title that is used in the rendered
   document.
   In this case it is "My Guide".
-  **Reference summary:** Literal summary of the rendered document.
   In this case it is "The new guide."
-  **Table-of-Contents tree structure and sub-topic parameters:** The
   directive to create a TOC and to specify the embedded topic links
   should remain hidden.
   If you want sub-topics to be part of the TOC, use the
   ":maxdepth: x" directive where "x" is the depth you desire for
   sub-topics in the TOC.
-  **RST source file root name:** The source files to use as content.
   In this case, the file references are "topic_1" and "topic_2".
   These reference the **topic_1.rst** and **topic_2.rst** files
   in the same folder as the **index.rst** file.

----------------------------------------------------
Integrating the New Guide Into the Documentation Set
----------------------------------------------------

The previous section described how you can provide the files
you need to create a new guide.
This section describes how you can get the new guide to be part
of the TOC StarlingX uses when displaying all the documentation
(i.e. `StarlingX Documentation <https://docs.starlingx.io/>`_).

The **stx-docs/doc/source/index.rst** file contains the structure
that defines the StarlingX Documentation landing page.
Inside the file, is a "Sections" area that lists the documents
that appear in the TOC.
Following is the updated file that shows the example guide
included.
The "my_guide/index" line ensures the new guide is included
in the TOC along with the existing guides:

::

     --------
     Sections
     --------

     .. toctree::
        :maxdepth: 1

        installation_guide/index
        developer_guide/index
        Project Specifications <https://docs.starlingx.io/specs/>
        api-ref/index
        releasenotes/index
        contributor/index
        my_guide/index

--------------------------
Closing Out a Bug or Story
--------------------------

If you are modifying a document as a result of a defect or
feature that is associated with a StoryBoard Story or Launchpad
Bug, you must take steps to link your submission (Gerrit Review)
to the story or bug.

To link a story, add the following lines in your
commit message.
Be sure to use the actual story ID and task ID with the commit:

* Story: $story_id
* Task: $task_id

Following is an example that links a Gerrit Review with Story
2003375 and Task 2444:

::

   Change the tox.ini directory regarding tox.ini dependencies

   Story: 2003375
   Task: 24444

To link a bug, add the approprite lines in your commit message.
Be sure to provide the actual bug numbers:

* Closes-Bug: $bug_id
* Partial-Bug: $bug_id
* Related-Bug: $bug_id

If your fix requires multiple commits, use "Partial-Bug"
for all the commits except the final one.
For the final commit, use "Closes-Bug".

Following is an example commit message that closes out bug
1804024:

::

   AIO Hardware Requirements: Updated AIO HW requirements.

   Added Small HW form factor information simplex/duplex
   AIO hardware requirements.

   Closes-Bug: #1804024

When you associate a story or bug with a Gerrit review, Gerrit
automatically updates the status of the story or bug once the
commit is merged.

You can find more information on the StarlingX code submission
guidelines on the
`wiki <https://wiki.openstack.org/wiki/StarlingX/CodeSubmissionGuidelines>`_.

To see the list of defects against StarlingX, see the
`Launchpad Application <https://bugs.launchpad.net/starlingx>`_.

--------------------------
Building the Documentation
--------------------------

To build the documentation locally in HTML format, use the
following command:

.. code:: sh

   $ tox -e docs

The resulting directories and HTML files looks like:

::

     stx-docs/doc/
     ├── build
     │   ├── doctrees
     │   │   ├── api-ref
     │   │   │   └── index.doctree
     │   │   ├── contributor
     │   │   │   ├── api_contribute_guide.doctree
     │   │   │   ├── doc_contribute_guide.doctree
     │   │   │   ├── index.doctree
     │   │   │   └── release_note_contribute_guide.doctree
     │   │   ├── developer_guide
     │   │   │   └── index.doctree
     │   │   ├── environment.pickle
     │   │   ├── index.doctree
     │   │   ├── installation_guide
     │   │   │   ├── controller_storage.doctree
     │   │   │   ├── dedicated_storage.doctree
     │   │   │   ├── duplex.doctree
     │   │   │   ├── index.doctree
     │   │   │   ├── installation_libvirt_qemu.doctree
     │   │   │   └── simplex.doctree
     │   │   ├── my_guide
     │   │   │   ├── index.doctree
     │   │   │   ├── topic_1.doctree
     │   │   │   ├── topic_2.doctree
     │   │   └── releasenotes
     │   │       └── index.doctree
     │   └── html
     │       ├── api-ref
     │       │   └── index.html
     │       ├── contributor
     │       │   ├── api_contribute_guide.html
     │       │   ├── doc_contribute_guide.html
     │       │   ├── index.html
     │       │   └── release_note_contribute_guide.html
     │       ├── developer_guide
     │       │   └── index.html
     │       ├── genindex.html
     │       ├── index.html
     │       ├── installation_guide
     │       │   ├── controller_storage.html
     │       │   ├── dedicated_storage.html
     │       │   ├── duplex.html
     │       │   ├── index.html
     │       │   ├── installation_libvirt_qemu.html
     │       │   └── simplex.html
     │       ├── my_guide
     │       │   ├── index.html
     │       │   ├── topic_1.html
     │       │   ├── topic_2.html
     │       ├── objects.inv
     │       ├── releasenotes
     │       │   └── index.html
     │       ├── search.html
     │       ├── searchindex.js
     │       ├── _sources
     │       │   ├── api-ref
     │       │   │   └── index.rst.txt
     │       │   ├── contributor
     │       │   │   ├── api_contribute_guide.rst.txt
     │       │   │   ├── doc_contribute_guide.rst.txt
     │       │   │   ├── index.rst.txt
     │       │   │   └── release_note_contribute_guide.rst.txt
     │       │   ├── developer_guide
     │       │   │   └── index.rst.txt
     │       │   ├── index.rst.txt
     │       │   ├── installation_guide
     │       │   │   ├── controller_storage.rst.txt
     │       │   │   ├── dedicated_storage.rst.txt
     │       │   │   ├── duplex.rst.txt
     │       │   │   ├── index.rst.txt
     │       │   │   ├── installation_libvirt_qemu.rst.txt
     │       │   │   └── simplex.rst.txt
     │       │   ├── my_guide
     │       │   │   ├── index.rst.txt
     │       │   │   ├── topic_1.rst.txt
     │       │   │   ├── topic_2.rst.txt
     │       │   │   ├── topic_3.rst.txt
     │       │   │   ├── topic_4.rst.txt
     │       │   │   └── topic_5.rst.txt
     │       │   └── releasenotes
     │       │       └── index.rst.txt
     │       └── _static

       ...

     ├── Makefile
     ├── requirements.txt
     ├── setup.cfg
     ├── setup.py

----------------------------------
Viewing the Rendered Documentation
----------------------------------

To view the rendered document in a browser, open up
the **index.html** file in your browser.
For the new example guide, the file is
**stx-docs/doc/build/my_guide/index.html**.

**NOTE:** The PDF build uses a different tox environment and is
currently not supported for StarlingX.
