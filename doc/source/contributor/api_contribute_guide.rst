=====================
API Contributor Guide
=====================

---------
Locations
---------

OpenStack API working group has defined a guideline to follow for API
documentation when a project provides a REST API service. API
documentation information comes from RST source files stored in the
project repository, that when built, generate HTML files. More details
about the OpenStack API documentation can be found at:
https://docs.openstack.org/doc-contrib-guide/api-guides.html.

StarlingX API Reference documentation exists in the following projects:

-  **stx-config:** StarlingX System Configuration Management
-  **stx-docs:** StarlingX Documentation

   -  **stx-python-cinderclient** // i.e. only StarlingX-specific
      extensions to Cinder API are documented here
   -  **stx-nova** // i.e. only StarlingX-specific extensions to Nova
      API are documented here
   -  **stx-glance** // i.e. only StarlingX-specific extensions to
      Glance API are documented here
   -  **stx-neutron** // i.e. only StarlingX-specific extensions to
      Neutron API are documented here

-  **stx-distcloud:** StarlingX Distributed Cloud
-  **stx-fault:** StarlingX Fault Management
-  **stx-ha:** StarlingX High Availability/Process Monitoring/Service
   Management
-  **stx-metal:** StarlingX Bare Metal and Node Management, Hardware
   Maintenance
-  **stx-nfv:** StarlingX NFVI Orchestration

--------------------
Directory Structures
--------------------

The directory structure of the API Reference documentation under each
StarlingX project repository is fixed. Here is an example showing
**stx-config** StarlingX System Configuration Management

::

	 stx-config/api-ref/
	 └── source
	     ├── api-ref-sysinv-v1-config.rst
	     ├── conf.py
	     └── index.rst

The initial modifications and additions to enable the API Documentation
service in each StarlingX project are as follows:

-  **.gitignore** modifications to ignore the building directories and
   HTML files for the API reference
-  **.zuul.yaml** modifications to add the jobs to build and publish the
   api-ref document
-  **api-ref/source** directory creation to store your API Reference
   project directory
-  **api-ref/source/conf.py** configuration file to determine the HTML
   theme, Sphinx extensions and project information
-  **api-ref/source/index.rst** source file to create your index RST
   source file
-  **doc/requiremets.txt** modifications to add the os-api-ref Sphinx
   extension
-  **tox.ini** modifications to add the configuration to build the API
   reference locally

See stx-config [Doc] OpenStack API Reference Guide as an example of this
first commit: https://review.openstack.org/#/c/603258/

----------------------------
Creating the RST Source File
----------------------------

Once the API Documentation service has been enabled, you create the RST
source files that document the API operations under the same API
Reference documentation project directory. The following shows the RST
source file for the **stx-config** StarlingX System Configuration
Management: Configuration API v1

::

	stx-config/api-ref/
	└── source
		└── api-ref-sysinv-v1-config.rst

-----------------------
Creating the Index File
-----------------------

After providing the RST source file as shown in the previous example,
you add the **index.rst** file. This file provides captioning, a brief
description of the document, and the table-of-contents structure with
depth restrictions. The **index.rst** file resides in the same folder as
the RST source file.

Here is an example using the **stx-config** StarlingX System
Configuration Management: Configuration API v1:

::

	stx-config/api-ref/
	|___source
	    |___api-ref-sysinv-v1-config.rst
	    |___index.rst

The syntax of the **index.rst** file is fixed. Following shows the
**index.rst** file used in the **stx-config**:

::

	========================
	stx-config API Reference
	========================
	StarlingX System Configuration Management

	.. toctree::
	   :maxdepth: 2

	   api-ref-sys-v1-config


Following are explanations for each of the four areas of the
**index.rst** file:

-  **Reference title:** Literal title that is used in the rendered
   document. In this case it is "stx-config API Reference".
-  **Reference summary:** Literal summary of the rendered document. In
   this case it is "StarlingX System Configuration Management".
-  **Table-of-Contents tree structure and depth parameters:** The
   directive to create a TOC and to limit the depth of topics to "2".
-  **RST source file root name:** The source file to use as content. In
   this case, the file reference is "api-ref-sys-v1-config". This
   references the **api-ref-sys-v1-config.rst** file in the same folder
   as the **index.rst** file.

------------------
REST METHOD Syntax
------------------

Following is the syntax for each REST METHOD in the RST source file
(e.g. **api-ref-sys-v1-config.rst**).

::

	******************************************
	Modifies attributes of the System object
	******************************************
	.. rest_method:: PATCH /v1/isystems

	<  TEXT - description of the overall REST API >

	**Normal response codes**

	< TEXT - list of normal response codes  >

	**Error response codes**

	< TEXT – list of  error response codes  >

	**Request parameters**

	.. csv-table::
	   :header: "Parameter", "Style", "Type", "Description"
	   :widths: 20, 20, 20, 60
	   "ihosts (Optional)", "plain", "xsd:list", "Links for retreiving the list of hosts for this system."
	   "name (Optional)", "plain", "xsd:string", "A user-specified name of the cloud system. The default value is the system UUID."
	   < etc. >


::

	< verbatim list of an example REQUEST body >
	[
	    {
	       "path": "/name",
	       "value": "OTTAWA_LAB_WEST",
	       "op": "replace"
	    }
	    {
	       "path": "/description",
	       "value": "The Ottawa Cloud Test Lab - West Wing.",
	       "op": "replace"
	    }
	]


::

	**Response parameters**

	.. csv-table::
	   :header: "Parameter", "Style", "Type", "Description"
	   :widths: 20, 20, 20, 60
	   "ihosts (Optional)", "plain", "xsd:list", "Links for retreiving the list of hosts for this system."
	   "name (Optional)", "plain", "xsd:string", "A user-specified name of the cloud system. The default value is the system UUID."
	   < etc. >


::

	< verbatim list of an example RESPONSE body >
	{
	   "isystems": [
		  {
		    "links": [
		      {
		        "href": "http://192.168.204.2:6385/v1/isystems/5ce48a37-f6f5-4f14-8fbd-ac6393464b19",
		        "rel": "self"
		      },
		      {
		        "href": "http://192.168.204.2:6385/isystems/5ce48a37-f6f5-4f14-8fbd-ac6393464b19",
		        "rel": "bookmark"
		      }
		    ],
		    "description": "The Ottawa Cloud Test Lab - West Wing.",
		    "software_version": "18.03",
		    "updated_at": "2017-07-31T17:44:06.051441+00:00",
		    "created_at": "2017-07-31T17:35:46.836024+00:00",
	      }
	    ]
	}



------------------------------------
Building the Reference Documentation
------------------------------------

To build the API reference documentation locally in HTML format, use the
following command:

.. code:: sh

   $ tox -e api-ref

The resulting directories and HTML files looks like:

::

	api-ref
	|__build/
	├── doctrees
	│   ├── api-ref-sysinv-v1-config.doctree
	      ...
	└── html
	    ├── api-ref-sysinv-v1-config.html
	    ├── index.html
	     ...
	    └── _static


--------------------------
Closing Out a Bug or Story
--------------------------

If you are modifying a document as a result of a defect or
feature that is associated with a StoryBoard Story or Launchpad
Bug, you must take steps to link your submission (Gerrit Review)
to the story or bug.

To link a story, add the following lines in your
commit message.
Be sure to use the actual story ID and task ID:

* Story: $story_id
* Task: $task_id

Following is an example that links a Gerrit Review with Story
2003375 and Task 2444:

::

   Change the tox.ini directory regarding tox.ini dependencies

   Story: 2003375
   Task: 24444

**NOTE:** You must provide a blank line before the lines
used to identify the story and the task.
If you do not provide this line, your submission will not
link to the Storyboard's story.

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
Again, be sure to provide a blank line just before the line
identifying the bug.

You can find more information on the StarlingX code submission
guidelines on the
`wiki <https://wiki.openstack.org/wiki/StarlingX/CodeSubmissionGuidelines>`_.

To see the list of defects against StarlingX, see the
`Launchpad Application <https://bugs.launchpad.net/starlingx>`_.

--------------------------------------------
Viewing the Rendered Reference Documentation
--------------------------------------------

To view the rendered HTML API Reference document in a browser, open up
the **index.html** file.

**NOTE:** The PDF build uses a different tox environment and is
currently not supported for StarlingX.
