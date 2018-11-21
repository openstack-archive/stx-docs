.. _Installation-libvirt-qemu:

=========================
Installation libvirt qemu
=========================

Installation for StarlingX using Libvirt/QEMU virtualization.

---------------------
Hardware Requirements
---------------------

A workstation computer with:

-  Processor: x86_64 only supported architecture with BIOS enabled
   hardware virtualization extensions
-  Memory: At least 32GB RAM
-  Hard Disk: 500GB HDD
-  Network: One network adapter with active Internet connection

---------------------
Software Requirements
---------------------

A workstation computer with:

-  Operating System: This process is known to work on Ubuntu 16.04 and
   is likely to work on other Linux OS's with some appropriate
   adjustments.
-  Proxy settings configured (if applies)
-  Git
-  KVM/VirtManager
-  Libvirt Library
-  QEMU Full System Emulation Binaries
-  stx-tools project
-  StarlingX ISO Image

----------------------------
Deployment Environment Setup
----------------------------

*************
Configuration
*************

These scripts are configured using environment variables that all have
built-in defaults. On shared systems you probably do not want to use the
defaults. The simplest way to handle this is to keep an rc file that can
be sourced into an interactive shell that configures everything. Here's
an example called madcloud.rc:

::

   export CONTROLLER=madcloud
   export COMPUTE=madnode
   export STORAGE=madstorage
   export BRIDGE_INTERFACE=madbr
   export INTERNAL_NETWORK=172.30.20.0/24
   export INTERNAL_IP=172.30.20.1/24
   export EXTERNAL_NETWORK=192.168.20.0/24
   export EXTERNAL_IP=192.168.20.1/24


This rc file shows the defaults baked into the scripts:

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
Install stx-tools Project
*************************

Clone the stx-tools project into a working directory.

::

   git clone git://git.openstack.org/openstack/stx-tools.git


It will be convenient to set up a shortcut to the deployment script
directory:

::

   SCRIPTS=$(pwd)/stx-tools/deployment/libvirt


Load the configuration (if you created one) from madcloud.rc:

::

   source madcloud.rc


****************************************
Installing Requirements and Dependencies
****************************************

Install the required packages and configure QEMU. This only needs to be
done once per host. (NOTE: this script only knows about Ubuntu at this
time):

::

   $SCRIPTS/install_packages.sh


******************
Disabling Firewall
******************

Unload firewall and disable firewall on boot:

::

   sudo ufw disable
   sudo ufw status


******************
Configure Networks
******************

Configure the network bridges using setup_network.sh before doing
anything else. It will create 4 bridges named stxbr1, stxbr2, stxbr3 and
stxbr4. Set the BRIDGE_INTERFACE environment variable if you need to
change stxbr to something unique.

::

   $SCRIPTS/setup_network.sh


The destroy_network.sh script does the reverse, and should not be used
lightly. It should also only be used after all of the VMs created below
have been destroyed.

There is also a script cleanup_network.sh that will remove networking
configuration from libvirt.

*********************
Configure Controllers
*********************

One script exists for building different StarlingX cloud
configurations: setup_configuration.sh.

The script uses the cloud configuration with the -c option:

- simplex
- duplex
- controllerstorage
- dedicatedstorage

You need an ISO file for the installation, the script takes a file name
with the -i option:

::

   $SCRIPTS/setup_configuration.sh -c <cloud configuration> -i <starlingx iso image>


And the setup will begin. The scripts create one or more VMs and start
the boot of the first controller, named oddly enough \``controller-0``.
If you have Xwindows available you will get virt-manager running. If
not, Ctrl-C out of that attempt if it doesn't return to a shell prompt.
Then connect to the serial console:

::

   virsh console controller-0


Continue the usual StarlingX installation from this point forward.

Tear down the VMs using destroy_configuration.sh.

::

   $SCRIPTS/destroy_configuration.sh -c <cloud configuration>


--------
Continue
--------

Pick up the installation in one of the existing guides at the
'Initializing Controller-0 step.

-  Standard Controller

   - :ref:`StarlingX Cloud with Dedicated Storage Virtual Environment <dedicated-storage>`
   - :ref:`StarlingX Cloud with Controller Storage Virtual Environment <controller-storage>`

-  All-in-one

   - :ref:`StarlingX Cloud Duplex Virtual Environment <duplex>`
   - :ref:`StarlingX Cloud Simplex Virtual Environment <simplex>`
