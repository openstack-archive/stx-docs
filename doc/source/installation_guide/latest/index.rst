==============================
Installation guide stx.2019.05
==============================

This is the installation guide for the "latest" StarlingX software
(i.e. the "under development" branch).
If this is not the the installation guide you want to use,
see the 
:doc:`available installation guides </installation_guide/index>`.

------------
Introduction
------------

You can install StarlingX in the following:

-  **Bare metal**: Real deployments of StarlingX are only supported on
   physical servers.
-  **Virtual environment**: Only used for evaluation or
   development purposes.

StarlingX installed in virtual environments has two options:

- :doc:`Libvirt/QEMU </installation_guide/latest/installation_libvirt_qemu>`
- VirtualBox

------------
Requirements
------------

Different use cases require different configurations.

**********
Bare metal
**********

The minimum requirements for the physical servers where StarlingX might
be deployed include:

-  **Controller hosts**

   -  Minimum processor:

      -  Dual-CPU Intel® Xeon® E5 26xx family (SandyBridge) 8
         cores/socket

   -  Minimum memory: 64 GB
   -  Hard drives:

      -  Primary hard drive with a minimum 500 GB for OS and system databases.
      -  Secondary hard drive with a minimum 500 GB for persistent VM storage.

   -  Two physical Ethernet interfaces: OAM and MGMT network.
   -  USB boot support.
   -  PXE boot support.

-  **Storage hosts**

   -  Minimum processor:

      -  Dual-CPU Intel® Xeon® E5 26xx family (SandyBridge) 8
         cores/socket.

   -  Minimum memory: 64 GB.
   -  Hard drives:

      -  Primary hard drive with a minimum 500 GB for OS.
      -  One or more additional hard drives for CEPH OSD storage.
      -  Optionally, one or more SSD or NVMe drives for CEPH journals.

   -  One physical Ethernet interface: MGMT network
   -  PXE boot support.

-  **Compute hosts**

   -  Minimum processor:

      -  Dual-CPU Intel® Xeon® E5 26xx family (SandyBridge) 8
         cores/socket.

   -  Minimum memory: 32 GB.
   -  Hard drives:

      -  Primary hard drive with a minimum 500 GB for OS.
      -  One or more additional hard drives for ephemeral VM storage.

   -  Two or more physical Ethernet interfaces: MGMT network and one or more
      provider networks.
   -  PXE boot support.

-  **All-In-One Simplex or Duplex, controller + compute hosts**

   -  Minimum processor:

      -  Typical hardware form factor:

         - Dual-CPU Intel® Xeon® E5 26xx family (SandyBridge) 8 cores/socket
      -  Low cost / low power hardware form factor

         - Single-CPU Intel Xeon D-15xx family, 8 cores

   -  Minimum memory: 64 GB.
   -  Hard drives:

      -  Primary hard drive with a minimum 500 GB SDD or NVMe.
      -  Zero or more 500 GB disks (min. 10K RPM).

   -  Network ports:

      **NOTE:** Duplex and Simplex configurations require one or more data
      ports.
      The Duplex configuration requires a management port.

      - Management: 10GE (Duplex only)
      - OAM: 10GE
      - Data: n x 10GE

The recommended minimum requirements for the physical servers are
described later in each StarlingX deployment guide.

^^^^^^^^^^^^^^^^^^^^^^^^
NVMe drive as boot drive
^^^^^^^^^^^^^^^^^^^^^^^^

To use a Non-Volatile Memory Express (NVMe) drive as the boot drive for any of
your nodes, you must configure your host and adjust kernel parameters during
installation:

- Configure the host to be in UEFI mode.
- Edit the kernel boot parameter. After you are presented with the StarlingX
  ISO boot options and after you have selected the preferred installation option
  (e.g. Standard Configuration / All-in-One Controller Configuration), press the
  TAB key to edit the kernel boot parameters. Modify the **boot_device** and
  **rootfs_device** from the default **sda** so that it is the correct device
  name for the NVMe drive (e.g. "nvme0n1").

  ::

     vmlinuz rootwait console=tty0 inst.text inst.stage2=hd:LABEL=oe_iso_boot
     inst.ks=hd:LABEL=oe_iso_boot:/smallsystem_ks.cfg boot_device=nvme0n1
     rootfs_device=nvme0n1 biosdevname=0 usbcore.autosuspend=-1 inst.gpt
     security_profile=standard user_namespace.enable=1 initrd=initrd.img


*******************
Virtual environment
*******************

The recommended minimum requirements for the workstation that hosts the
virtual machine(s) where StarlingX is deployed includes the following:

^^^^^^^^^^^^^^^^^^^^^
Hardware requirements
^^^^^^^^^^^^^^^^^^^^^

A workstation computer with the following:

-  Processor: x86_64 only supported architecture with BIOS enabled
   hardware virtualization extensions
-  Cores: 8 (4 with careful monitoring of CPU load)
-  Memory: Minimum 32 GB RAM
-  Hard Disk: 500 GB HDD
-  Network: Two network adapters with active Internet connection

^^^^^^^^^^^^^^^^^^^^^
Software requirements
^^^^^^^^^^^^^^^^^^^^^

A workstation computer with the following:

-  Newly installed Ubuntu 16.04 LTS 64-bit operating system
-  If applicable, proxy settings
-  Git
-  KVM/VirtManager
-  Libvirt library
-  QEMU full-system emulation binaries
-  stx-tools project
-  StarlingX ISO image

^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Deployment environment setup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section describes how to set up the workstation computer that
hosts the virtual machine or machine where StarlingX is deployed.

''''''''''''''''''''''''''''''
Updating your operating system
''''''''''''''''''''''''''''''

Before proceeding with the build, ensure your OS is current. You will
first need to update the local database list of available packages:

::

   $ sudo apt-get update

'''''''''''''''''''''''''
Install stx-tools project
'''''''''''''''''''''''''

Clone the stx-tools project. Usually, you want to clone the
stx-tools repository under your user’s home directory.

::

   $ cd $HOME
   $ git clone https://git.starlingx.io/stx-tools


''''''''''''''''''''''''''''''''''''''''
Installing requirements and dependencies
''''''''''''''''''''''''''''''''''''''''

Navigate to the stx-tools installation libvirt directory:

::

   $ cd $HOME/stx-tools/deployment/libvirt/


Install the required packages:

::

   $ bash install_packages.sh


''''''''''''''''''''''
Disabling the firewall
''''''''''''''''''''''

Unload the firewall and disable it during boot:

::

   $ sudo ufw disable
   Firewall stopped and disabled on system startup
   $ sudo ufw status
   Status: inactive


-------------------------------
Getting the StarlingX ISO image
-------------------------------

Follow the instructions from the :doc:`/developer_guide/latest/index` to build a
StarlingX ISO image.

**********
Bare metal
**********

A bootable USB flash drive containing StarlingX ISO image.


*******************
Virtual environment
*******************

Copy the StarlingX ISO Image to the stx-tools deployment libvirt project
directory:

::

   $ cp <starlingx iso image> $HOME/stx-tools/deployment/libvirt/


------------------
Deployment options
------------------

-  Standard controller

   - :doc:`StarlingX Cloud with Dedicated Storage </installation_guide/latest/dedicated_storage>`
   - :doc:`StarlingX Cloud with Controller Storage </installation_guide/latest/controller_storage>`

-  All-in-one

   - :doc:`StarlingX Cloud Duplex </installation_guide/latest/duplex>`
   - :doc:`StarlingX Cloud Simplex </installation_guide/latest/simplex>`

.. toctree::
   :hidden:

   installation_libvirt_qemu
   controller_storage
   dedicated_storage
   duplex
   simplex
