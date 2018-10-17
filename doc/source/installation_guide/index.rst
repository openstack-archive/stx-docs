==================
Installation Guide
==================

-----
Intro
-----

StarlingX may be installed in:

-  **Bare Metal**: Real deployments of StarlingX are only supported on
   physical servers.
-  **Virtual Environment**: It should only be used for evaluation or
   development purposes.


StarlingX installed in virtual environments has two options:

- :ref:`Libvirt/QEMU <Installation-libvirt-qemu>`
- VirtualBox

------------
Requirements
------------

Different use cases require different configurations.

Bare Metal
**********

The minimum requirements for the physical servers where StarlingX might
be deployed, include:

-  **Controller Hosts**

   -  Minimum Processor is:

      -  Dual-CPU Intel® Xeon® E5 26xx Family (SandyBridge) 8
         cores/socket

   -  Minimum Memory: 64 GB
   -  Hard Drives:

      -  Primary Hard Drive, minimum 500 GB for OS and system databases.
      -  Secondary Hard Drive, minimum 500 GB for persistent VM storage.

   -  2 physical Ethernet interfaces: OAM and MGMT Network.
   -  USB boot support.
   -  PXE boot support.

-  **Storage Hosts**

   -  Minimum Processor is:

      -  Dual-CPU Intel® Xeon® E5 26xx Family (SandyBridge) 8
         cores/socket.

   -  Minimum Memory: 64 GB.
   -  Hard Drives:

      -  Primary Hard Drive, minimum 500 GB for OS.
      -  1 or more additional Hard Drives for CEPH OSD storage, and
      -  Optionally 1 or more SSD or NVMe Drives for CEPH Journals.

   -  1 physical Ethernet interface: MGMT Network
   -  PXE boot support.

-  **Compute Hosts**

   -  Minimum Processor is:

      -  Dual-CPU Intel® Xeon® E5 26xx Family (SandyBridge) 8
         cores/socket.

   -  Minimum Memory: 32 GB.
   -  Hard Drives:

      -  Primary Hard Drive, minimum 500 GB for OS.
      -  1 or more additional Hard Drives for ephemeral VM Storage.

   -  2 or more physical Ethernet interfaces: MGMT Network and 1 or more
      Provider Networks.
   -  PXE boot support.

The recommended minimum requirements for the physical servers are
described later in each StarlingX Deployment Options guide.

Virtual Environment
*******************

The recommended minimum requirements for the workstation, hosting the
Virtual Machine(s) where StarlingX will be deployed, include:

Hardware Requirements
^^^^^^^^^^^^^^^^^^^^^

A workstation computer with:

-  Processor: x86_64 only supported architecture with BIOS enabled
   hardware virtualization extensions
-  Cores: 8 (4 with careful monitoring of cpu load)
-  Memory: At least 32GB RAM
-  Hard Disk: 500GB HDD
-  Network: Two network adapters with active Internet connection

Software Requirements
^^^^^^^^^^^^^^^^^^^^^

A workstation computer with:

-  Operating System: Freshly installed Ubuntu 16.04 LTS 64-bit
-  Proxy settings configured (if applies)
-  Git
-  KVM/VirtManager
-  Libvirt Library
-  QEMU Full System Emulation Binaries
-   project
-  StarlingX ISO Image

Deployment Environment Setup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section describes how to set up the workstation computer which will
host the Virtual Machine(s) where StarlingX will be deployed.

Updating Your Operating System
''''''''''''''''''''''''''''''

Before proceeding with the build, ensure your OS is up to date. You’ll
first need to update the local database list of available packages:

::

   $ sudo apt-get update


Install stx-tools project
'''''''''''''''''''''''''

Clone the stx-tools project. Usually you’ll want to clone it under your
user’s home directory.

::

   $ cd $HOME
   $ git clone git://git.openstack.org/openstack/stx-tools


Installing Requirements and Dependencies
''''''''''''''''''''''''''''''''''''''''

Navigate to the stx-tools installation libvirt directory:

::

   $ cd $HOME/stx-tools/deployment/libvirt/


Install the required packages:

::

   $ bash install_packages.sh


Disabling Firewall
''''''''''''''''''

Unload firewall and disable firewall on boot:

::

   $ sudo ufw disable
   Firewall stopped and disabled on system startup
   $ sudo ufw status
   Status: inactive


-------------------------------
Getting the StarlingX ISO Image
-------------------------------

Follow the instructions from the :ref:`developer-guide` to build a
StarlingX ISO image.



Bare Metal
**********

A bootable USB flash drive containing StarlingX ISO image.



Virtual Environment
*******************

Copy the StarlingX ISO Image to the stx-tools deployment libvirt project
directory:

::

   $ cp <starlingx iso image> $HOME/stx-tools/deployment/libvirt/


------------------
Deployment Options
------------------

-  Standard Controller

   - :ref:`StarlingX Cloud with Dedicated Storage <dedicated-storage>`
   - :ref:`StarlingX Cloud with Controller Storage <controller-storage>`

-  All-in-one

   - :ref:`StarlingX Cloud Duplex <duplex>`
   - :ref:`StarlingX Cloud Simplex <simplex>`


.. toctree::
   :hidden:

   installation_libvirt_qemu
   controller_storage
   dedicated_storage
   duplex
   simplex