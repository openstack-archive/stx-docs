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

**********
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

-  **All-In-One Simplex or Duplex, Controller + Compute Hosts**

   -  Minimum Processor is:

      -  Typical Hardware Form Factor:

         - Dual-CPU Intel® Xeon® E5 26xx Family (SandyBridge) 8 cores/socket
      -  Low Cost / Low Power Hardware Form Factor

         - Single-CPU Intel Xeon D-15xx Family, 8 cores

   -  Minimum Memory: 64 GB.
   -  Hard Drives:

      -  Primary Hard Drive, minimum 500 GB SDD or NVMe.
      -  0 or more 500 GB disks (min. 10K RPM).

   -  Network Ports:

      **NOTE:** Duplex and Simplex configurations require one or more data
      ports.
      The Duplex configuration requires a management port.

      - Management: 10GE (Duplex only)
      - OAM: 10GE
      - Data: n x 10GE

The recommended minimum requirements for the physical servers are
described later in each StarlingX Deployment Options guide.

*******************
Virtual Environment
*******************

The recommended minimum requirements for the workstation, hosting the
Virtual Machine(s) where StarlingX will be deployed, include:

^^^^^^^^^^^^^^^^^^^^^
Hardware Requirements
^^^^^^^^^^^^^^^^^^^^^

A workstation computer with:

-  Processor: x86_64 only supported architecture with BIOS enabled
   hardware virtualization extensions
-  Cores: 8 (4 with careful monitoring of cpu load)
-  Memory: At least 32GB RAM
-  Hard Disk: 500GB HDD
-  Network: Two network adapters with active Internet connection

^^^^^^^^^^^^^^^^^^^^^
Software Requirements
^^^^^^^^^^^^^^^^^^^^^

A workstation computer with:

-  Operating System: Freshly installed Ubuntu 16.04 LTS 64-bit
-  Proxy settings configured (if applies)
-  Git
-  KVM/VirtManager
-  Libvirt Library
-  QEMU Full System Emulation Binaries
-  stx-tools project
-  StarlingX ISO Image

^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Deployment Environment Setup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section describes how to set up the workstation computer which will
host the Virtual Machine(s) where StarlingX will be deployed.

''''''''''''''''''''''''''''''
Updating Your Operating System
''''''''''''''''''''''''''''''

Before proceeding with the build, ensure your OS is up to date. You’ll
first need to update the local database list of available packages:

::

   $ sudo apt-get update

'''''''''''''''''''''''''
Install stx-tools project
'''''''''''''''''''''''''

Clone the stx-tools project. Usually you’ll want to clone it under your
user’s home directory.

::

   $ cd $HOME
   $ git clone https://git.starlingx.io/stx-tools


''''''''''''''''''''''''''''''''''''''''
Installing Requirements and Dependencies
''''''''''''''''''''''''''''''''''''''''

Navigate to the stx-tools installation libvirt directory:

::

   $ cd $HOME/stx-tools/deployment/libvirt/


Install the required packages:

::

   $ bash install_packages.sh


''''''''''''''''''
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


**********
Bare Metal
**********

A bootable USB flash drive containing StarlingX ISO image.


*******************
Virtual Environment
*******************

Copy the StarlingX ISO Image to the stx-tools deployment libvirt project
directory:

::

   $ cp <starlingx iso image> $HOME/stx-tools/deployment/libvirt/


-------------------
Deployment Glossary
-------------------

- **Controller Node / Function**

  - Runs Cloud Control Functions for managing Cloud Resources.
  - Runs all OpenStack Control Functions (e.g. managing Images, Virtual Volumes,
    Virtual Network, and Virtual Machines).
  - Can be part of a two-node HA Control Node Cluster for running Control
    Functions either Active/Active or Active/Standby.

- **Compute ( & Network ) Node / Function**

  - Host Applications in Virtual Machines using Compute Resources such as CPU,
    Memory, and Disk.
  - Runs Virtual Switch for realizing virtual networks.
  - Provides L3 Routing and NET Services.

- **Storage Node / Function**

  - Contains a set of Disks (e.g. SATA, SAS, SSD, and/or NVMe).
  - Runs CEPH Distributed Storage Software.
  - Part of an HA multi-node CEPH Storage Cluster supporting a replication
    factor of two or three, Journal Caching and Class Tiering.
  - Provides HA Persistent Storage for Images, Virtual Volumes
    (i.e. Block Storage), and Object Storage.

- **All-In-One Controller Node**

  - A single physical node that provides a Controller Function, Compute
    Function, and Storage Function.

- **OAM Network**

  - Only Controller type nodes are required to be connected to the OAM
    Network.
  - The network on which all external StarlingX Platform APIs are exposed,
    (i.e. REST APIs, Horizon Web Server, SSH, and SNMP).
  - Typically 1GE.

- **Management Network**

  - All nodes are required to be connected to the Management Network.
  - A private network (i.e. not connected externally) used for the following:

    - Internal OpenStack / StarlingX monitoring and control.
    - VM I/O access to a storage cluster.

  - Typically 10GE.

- **Data Network(s)**

  - Only Compute type and All-in-One type nodes are required to be connected
    to the Data Network(s); these node types require one or more interface(s)
    on the Data Network(s).
  - Networks on which the OpenStack / Neutron Provider Networks are realized
    and subsequently the VM Tenant Networks.

- **IPMI Network**

  - All node types can optionally be connected to the IPMI Network.
  - An optional network on which IPMI interfaces of all nodes are connected.
  - It must be reachable using L3/IP from the Controller's OAM Interfaces.

- **PXEBoot Network**

  - If this optional network is used, all node types are required to be
    connected to the PXEBoot Network.
  - An optional network for Controllers to boot/install other nodes over the
    network.

    - By default, Controllers uses the Management Network for boot/install
      of other nodes in the openstack cloud.

  - A PXEBoot network is required for a variety of special case situations:

    - Management Network must be IPv6:

      - IPv6 does not support PXEBoot. Therefore IPv4 PXEBoot network must be
        configured.

    - Management Network must be VLAN tagged:

      - Most Server's BIOS do not support PXEBooting over tagged networks.
        Therefore, you must configure an untagged PXEBoot network.

    - Management Network must be shared across regions:

      - But individual regions' Controllers want to only network boot/install
        nodes of their own region. Therefore, you must configure separate
        per-region PXEBoot Networks.

- **Infra Network**

  - If this optional network is used, all node types are required to be
    connected to the INFRA Network,
  - A deprecated optional network that was historically used for access to the
    Storage cluster.

- **Node Interfaces**

  - All Nodes' Network Interfaces can, in general, optionally be either:

    - Untagged single port.
    - Untagged two port LAG and optionally split between redudant L2 Switches
      running vPC (Virtual Port-Channel), also known as multichassis
      EtherChannel (MEC).
    - VLAN on either single-port ETH interface or two-port LAG interface.


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
