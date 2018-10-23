===============================
Release Notes Contributor Guide
===============================

Release notes for StarlingX projects are managed using Reno allowing release
notes go through the same review process used for managing code changes.
Release documentation information comes from YAML source files stored in the
project repository, that when built in conjuction with RST source files,
generate HTML files. More details about the Reno Release Notes Manager can
be found at: https://docs.openstack.org/reno

---------
Locations
---------

StarlingX Release Notes documentation exists in the following projects:

-  **stx-clients:** StarlingX Client Libraries
-  **stx-config:** StarlingX System Configuration Management
-  **stx-distcloud:** StarlingX Distributed Cloud
-  **stx-distcloud-client:** StarlingX Distributed Cloud Client
-  **stx-fault:** StarlingX Fault Management
-  **stx-gui:**  StarlingX Horizon plugins for new StarlingX services
-  **stx-ha:** StarlingX High Availability/Process Monitoring/Service Management
-  **stx-integ:** StarlingX Integration and Packaging
-  **stx-metal:** StarlingX Bare Metal and Node Management, Hardware Maintenance
-  **stx-nfv:** StarlingX NFVI Orchestration
-  **stx-tools:** StarlingX Build Tools
-  **stx-update:** StarlingX Installation/Update/Patching/Backup/Restore
-  **stx-upstream:** StarlingX Upstream Packaging

--------------------
Directory Structures
--------------------

The directory structure of Release documentation under each StarlingX project
repository is fixed.  Here is an example showing **stx-confi** StarlingX System
Configuration Management:

::

	releasenotes/
	├── notes
	│   └── release-summary-6738ff2f310f9b57.yaml
	└── source
	    ├── conf.py
	    ├── index.rst
	    └── unreleased.rst


The initial modifications and additions to enable the API Documentation service
in each StarlingX project are as follows:

-  **.gitignore** modifications to ignore the building directories and HTML files
   for the Release Notes
-  **.zuul.yaml** modifications to add the jobs to build and publish the api-ref
   document
-  **releasenotes/notes/** directory creation to store your release notes files
   in YAML format
-  **releasenotes/source** directory creation to store your API Reference project
   directory
-  **releasenotes/source/conf.py** configuration file to determine the HTML theme,
   Sphinx extensions and project information
-  **releasenotes/source/index.rst** source file to create your index RST source
   file
-  **releasenotes/source/unrelased.rst** source file to avoid breaking  the real
   release notes build job on the master branch
-  **doc/requiremets.txt** modifications to add the os-api-ref Sphinx extension
-  **tox.ini** modifications to add the configuration to build the API reference
   locally

See stx-config [Doc] Release Notes Management as an example of this first commit:
https://review.openstack.org/#/c/603257/

Once the Release Notes Documentation service has been enabled, you can create a new
release notes.

-------------------
Release Notes Files
-------------------

The following shows the YAML source file for the stx-config StarlingX System
Configuration Management:
`Release Summary r/2018.10 <http://git.openstack.org/cgit/openstack/stx-config/tree/releasenotes/notes/release-summary-6738ff2f310f9b57.yaml>`_

::

	stx-config/releasenotes/
	├── notes
	│   └── release-summary-6738ff2f310f9b57.yaml


To create a new release note that document your code changes via tox newnote environment:

$ tox -e newnote hello-my-change

A YAML source file is created with a unique name under releasenote/notes/ directory:

::

	stx-config/releasenotes/
	├── notes
	│   ├── hello-my-change-dcef4b934a670160.yaml

The content are gound into logical sections based in the default template used by reno:

::

	features
	issues
	upgrade
	deprecations
	critical
	security
	fixes
	other

Modify the content in the YAML source file based on
`reStructuredText <http://www.sphinx-doc.org/en/stable/rest.html>`_ format.

------------------
Developer Workflow
------------------

#. Start common development workflow to create your change: "Hello My Change"
#. Create its release notes, no major effort since title and content might be reused from git commit information:
#. Add your change including its release notes and submit for review.

---------------------
Release Team Workflow
---------------------

#. Start development work to prepare the release, this might include git tag.
#. Generate the Reno Report
#. Add your change and submit for review
