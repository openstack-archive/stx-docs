=====================================
Installation libvirt qemu stx.2019.05
=====================================

Installation for StarlingX stx.2019.05 using Libvirt/QEMU virtualization.

---------------------
Hardware requirements
---------------------

A workstation computer with the following:

-  Processor: x86_64 only supported architecture with BIOS-enabled
   hardware virtualization extensions
-  Memory: Minimum 32 GB RAM
-  Hard disk: 500 GB HDD
-  Network: One network adapter with active Internet connection

---------------------
Software requirements
---------------------

A workstation computer with the following:

-  Operating system: This process is known to work on Ubuntu 16.04 and
   is likely to work on other Linux distributions with some appropriate adjustments.
-  If applicable, proxy settings
-  Git
-  KVM/VirtManager
-  Libvirt library
-  QEMU full-system emulation binaries
-  stx-tools project
-  StarlingX ISO image

----------------------------
Deployment environment setup
----------------------------

*************
Configuration
*************

These scripts are configured using environment variables that all have
built-in defaults. On shared systems, you probably do not want to use the
defaults. The easiest way to handle this is to keep an rc file that can
be sourced into an interactive shell that configures everything. Following
is an example called stxcloud.rc:

::

   export CONTROLLER=stxcloud
   export COMPUTE=stxnode
   export STORAGE=stxstorage
   export BRIDGE_INTERFACE=stxbr
   export INTERNAL_NETWORK=172.30.20.0/24
   export INTERNAL_IP=172.30.20.1/24
   export EXTERNAL_NETWORK=192.168.20.0/24
   export EXTERNAL_IP=192.168.20.1/24


This rc file shows the defaults as part of the scripts:

::

   export CONTROLLER=controller
   export COMPUTE=compute
   export STORAGE=storage
   export BRIDGE_INTERFACE=stxbr
   export INTERNAL_NETWORK=10.10.10.0/24
   export INTERNAL_IP=10.10.10.1/24
   export EXTERNAL_NETWORK=192.168.204.0/24
   export EXTERNAL_IP=192.168.204.1/24


*************************
Install stx-tools project
*************************

Clone the stx-tools project into a working directory.

::

   git clone git://git.openstack.org/openstack/stx-tools.git


It is convenient to set up a shortcut to the deployment script
directory:

::

   SCRIPTS=$(pwd)/stx-tools/deployment/libvirt


If you created a configuration, load it from stxcloud.rc:

::

   source stxcloud.rc


****************************************
Installing requirements and dependencies
****************************************

Install the required packages and configure QEMU. This only needs to be
done once per host.

**NOTE:** At this time, the script only supports the Ubuntu distribution:

::

   $SCRIPTS/install_packages.sh


**********************
Disabling the firewall
**********************

Unload the firewall and disable it during boot:

::

   sudo ufw disable
   sudo ufw status


******************
Configure networks
******************

Before doing anything else, configure the network bridges using
setup_network.sh. Configuration creates four bridges named stxbr1,
stxbr2, stxbr3 and stxbr4.
Set the BRIDGE_INTERFACE environment variable if you need to
change stxbr to something unique.

::

   $SCRIPTS/setup_network.sh


The destroy_network.sh script does the reverse.
You should only use this script after all the VMs created below
have been destroyed.

**WARNING:** Use the destroy_network.sh script cautiously.

There is also a script cleanup_network.sh that removes networking
configuration from libvirt.

*********************
Configure controllers
*********************

One script exists for building different StarlingX cloud configurations:
setup_configuration.sh.

The script uses the cloud configuration with the -c option:

- simplex
- duplex
- controllerstorage
- dedicatedstorage

You need an ISO file for the installation.
The script requires a file name with the -i option:

::

   $SCRIPTS/setup_configuration.sh -c <cloud configuration> -i <starlingx iso image>


Running the script causes setup to begin.
The script creates one or more VMs and boots the first controller,
which is named \``controller-0``.

If you have Xwindows available, virt-manager starts to run.
If you do not have Xwindows available and you do not have a shell prompt,
press CTRL-C to abandon the attempt.
From the shell prompt, connect to the serial console:

::

   virsh console controller-0


Continue the usual StarlingX installation from this point forward.

Tear down the VMs using destroy_configuration.sh.

::

   $SCRIPTS/destroy_configuration.sh -c <cloud configuration>


--------
Continue
--------

Use the appropriate installation guide and continue the installation
process from the "initializing controller-0" step.

-  Standard controller

   - :doc:`StarlingX Cloud with Dedicated Storage Virtual Environment </installation_guide/latest/dedicated_storage>`
   - :doc:`StarlingX Cloud with Controller Storage Virtual Environment </installation_guide/latest/controller_storage>`

-  All-in-one

   - :doc:`StarlingX Cloud Duplex Virtual Environment </installation_guide/latest/duplex>`
   - :doc:`StarlingX Cloud Simplex Virtual Environment </installation_guide/latest/simplex>`
