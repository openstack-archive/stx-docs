.. SPDX-License-Identifier: Apache-2.0
   Copyright (C) 2019 Intel Corporation
===============================================
All-In-One Simplex deployment guide stx.2019.05
===============================================

.. contents::
   :local:
   :depth: 1

**NOTE:**  The instructions to setup a StarlingX One Node Configuration
(AIO-SX) system with containerized openstack services in this guide
are under development.
For approved instructions, see the
`One Node Configuration wiki page <https://wiki.openstack.org/wiki/StarlingX/Containers/Installation>`__.

----------------------
Deployment description
----------------------

The All-In-One Simplex (AIO-SX) deployment option provides all three cloud
gunctions (controller, compute, and storage) on a single physical server. With
these cloud functions, multiple application types can be deployed and
consolidated onto a single physical server. For example, with a AIO-SX
deployment you can:

- Consolidate legacy applications that must run standalone on a server by using
  multiple virtual machines on a single physical server.
- Consolidate legacy applications that run on different operating systems or
  different distributions of operating systems by using multiple virtual
  machines on a single physical server.

Only a small amount of cloud processing / storage power is required with an
All-In-One Simplex deployment.

.. figure:: figures/starlingx-deployment-options-simplex.png
   :scale: 50%
   :alt: All-In-One Simplex deployment configuration

   *All-In-One Simplex deployment configuration*

An All-In-One Simplex deployment provides no protection against an overall
server hardware fault. Protection against overall server hardware fault is
either not required, or done at a higher level. Hardware component protection
could be enabled if, for example, an HW RAID or 2x Port LAG is used in the
deployment.

--------------------------------------
Preparing an All-In-One Simplex server
--------------------------------------

**********
Bare metal
**********

Required Server:

-  Combined server (controller + compute): 1

^^^^^^^^^^^^^^^^^^^^^
Hardware requirements
^^^^^^^^^^^^^^^^^^^^^

The recommended minimum requirements for the physical servers where
All-In-One Simplex will be deployed are:

-  Minimum processor:

   -  Typical hardware form factor:

      - Dual-CPU Intel® Xeon® E5 26xx family (SandyBridge) 8 cores/socket
   -  Low cost / low power hardware form factor

      - Single-CPU Intel Xeon D-15xx family, 8 cores

-  Memory: 64 GB
-  BIOS:

   -  Hyper-Threading technology: Enabled
   -  Virtualization technology: Enabled
   -  VT for directed I/O: Enabled
   -  CPU power and performance policy: Performance
   -  CPU C state control: Disabled
   -  Plug & play BMC detection: Disabled

-  Primary disk:

   -  500 GB SDD or NVMe

-  Additional disks:

   -  Zero or more 500 GB disks (min. 10K RPM)

-  Network ports

   **NOTE:** All-In-One Simplex configuration requires one or more data ports.
   This configuration does not require a management port.

   -  OAM: 10GE
   -  Data: n x 10GE

*******************
Virtual environment
*******************

Run the libvirt qemu setup scripts. Setting up virtualized OAM and
management networks:

::

   $ bash setup_network.sh

Building XML for definition of virtual servers:

::

   $ bash setup_configuration.sh -c simplex -i <starlingx iso image>

The default XML server definition created by the previous script is:

- simplex-controller-0

^^^^^^^^^^^^^^^^^^^^^^^^^
Power up a virtual server
^^^^^^^^^^^^^^^^^^^^^^^^^

To power up the virtual server, run the following command:

::

    $ sudo virsh start <server-xml-name>

e.g.

::

    $ sudo virsh start simplex-controller-0

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Access a virtual server console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The XML for virtual servers in stx-tools repo, deployment/libvirt,
provides both graphical and text consoles.

Access the graphical console in virt-manager by right-click on the
domain (the server) and selecting "Open".

Access the textual console with the command "virsh console $DOMAIN",
where DOMAIN is the name of the server shown in virsh.

When booting the controller-0 for the first time, both the serial and
graphical consoles will present the initial configuration menu for the
cluster. One can select serial or graphical console for controller-0.
For the other nodes however only serial is used, regardless of which
option is selected.

Open the graphic console on all servers before powering them on to
observe the boot device selection and PXI boot progress. Run "virsh
console $DOMAIN" command promptly after power on to see the initial boot
sequence which follows the boot device selection. One has a few seconds
to do this.

------------------------------
Installing the controller host
------------------------------

Installing controller-0 involves initializing a host with software and
then applying a bootstrap configuration from the command line. The
configured bootstrapped host becomes controller-0.

Procedure:

#. Power on the server that will be controller-0 with the StarlingX ISO
   on a USB in a bootable USB slot.
#. Configure the controller using the config_controller script.

*************************
Initializing controller-0
*************************

This section describes how to initialize StarlingX in host controller-0.
Except where noted, all the commands must be executed from a console of
the host.

Power on the host to be configured as controller-0, with the StarlingX
ISO on a USB in a bootable USB slot. Wait for the console to show the
StarlingX ISO booting options:

-  **All-in-one Controller Configuration**

   -  When the installer is loaded and the installer welcome screen
      appears in the controller-0 host, select the type of installation
      "All-in-one Controller Configuration".

-  **Graphical Console**

   -  Select the "Graphical Console" as the console to use during
      installation.

-  **Standard Security Boot Profile**

   -  Select "Standard Security Boot Profile" as the Security Profile.

Monitor the initialization. When it is complete, a reboot is initiated
on the controller-0 host, briefly displays a GNU GRUB screen, and then
boots automatically into the StarlingX image.

Log into controller-0 as user wrsroot, with password wrsroot. The
first time you log in as wrsroot, you are required to change your
password. Enter the current password (wrsroot):

::

   Changing password for wrsroot.
   (current) UNIX Password:


Enter a new password for the wrsroot account:

::

   New password:

Enter the new password again to confirm it:

::

   Retype new password:

controller-0 is initialized with StarlingX, and is ready for configuration.

************************
Configuring controller-0
************************

This section describes how to perform the controller-0 configuration
interactively just to bootstrap system with minimum critical data.
Except where noted, all the commands must be executed from the console
of the active controller (here assumed to be controller-0).

When run interactively, the config_controller script presents a series
of prompts for initial configuration of StarlingX:

-  For the virtual environment, you can accept all the default values
   immediately after ‘system date and time’.
-  For a physical deployment, answer the bootstrap configuration
   questions with answers applicable to your particular physical setup.

The script is used to configure the first controller in the StarlingX
cluster as controller-0. The prompts are grouped by configuration
area. To start the script interactively, use the following command
with no parameters:

::

   controller-0:~$ sudo config_controller
   System Configuration
   ================
   Enter ! at any prompt to abort...
   ...

Select [y] for System date and time:

::

   System date and time:
   -----------------------------

   Is the current date and time correct?  [y/N]: y

For System mode choose "simplex":

::

   ...
   1) duplex-direct: two node-redundant configuration. Management and
   infrastructure networks are directly connected to peer ports
   2) duplex - two node redundant configuration
   3) simplex - single node non-redundant configuration
   System mode [duplex-direct]: 3

After System date and time and System mode:

::

   Applying configuration (this will take several minutes):

   01/08: Creating bootstrap configuration ... DONE
   02/08: Applying bootstrap manifest ... DONE
   03/08: Persisting local configuration ... DONE
   04/08: Populating initial system inventory ... DONE
   05:08: Creating system configuration ... DONE
   06:08: Applying controller manifest ... DONE
   07:08: Finalize controller configuration ... DONE
   08:08: Waiting for service activation ... DONE

   Configuration was applied

   Please complete any out of service commissioning steps with system
   commands and unlock controller to proceed.

After config_controller bootstrap configuration, REST API, CLI and
Horizon interfaces are enabled on the controller-0 OAM IP address. The
remaining installation instructions will use the CLI.

--------------------------------
Provisioning the controller host
--------------------------------

On controller-0, acquire Keystone administrative privileges:

::

   controller-0:~$ source /etc/nova/openrc

*********************************************
Configuring provider networks at installation
*********************************************

Set up one provider network of the vlan type, named providernet-a:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ neutron providernet-create providernet-a --type=vlan
   [wrsroot@controller-0 ~(keystone_admin)]$ neutron providernet-range-create --name providernet-a-range1 --range 100-400 providernet-a

*****************************************
Providing data interfaces on controller-0
*****************************************

List all interfaces:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-if-list -a controller-0
   +--------------------------------------+---------+----------+...+------+--------------+------+---------+------------+..
   | uuid                                 | name    | class    |...| vlan | ports        | uses | used by | attributes |..
   |                                      |         |          |...| id   |              | i/f  | i/f     |            |..
   +--------------------------------------+----------+---------+...+------+--------------+------+---------+------------+..
   | 49fd8938-e76f-49f1-879e-83c431a9f1af | enp0s3  | platform |...| None | [u'enp0s3']  | []   | []      | MTU=1500   |..
   | 8957bb2c-fec3-4e5d-b4ed-78071f9f781c | eth1000 | None     |...| None | [u'eth1000'] | []   | []      | MTU=1500   |..
   | bf6f4cad-1022-4dd7-962b-4d7c47d16d54 | eth1001 | None     |...| None | [u'eth1001'] | []   | []      | MTU=1500   |..
   | f59b9469-7702-4b46-bad5-683b95f0a1cb | enp0s8  | platform |...| None | [u'enp0s8']  | []   | []      | MTU=1500   |..
   +--------------------------------------+---------+----------+...+------+--------------+------+---------+------------+..

Configure the data interfaces:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c data controller-0 eth1000 -p providernet-a
   +------------------+--------------------------------------+
   | Property         | Value                                |
   +------------------+--------------------------------------+
   | ifname           | eth1000                              |
   | iftype           | ethernet                             |
   | ports            | [u'eth1000']                         |
   | providernetworks | providernet-a                        |
   | imac             | 08:00:27:c4:ad:3e                    |
   | imtu             | 1500                                 |
   | ifclass          | data                                 |
   | aemode           | None                                 |
   | schedpolicy      | None                                 |
   | txhashpolicy     | None                                 |
   | uuid             | 8957bb2c-fec3-4e5d-b4ed-78071f9f781c |
   | ihost_uuid       | 9c332b27-6f22-433b-bf51-396371ac4608 |
   | vlan_id          | None                                 |
   | uses             | []                                   |
   | used_by          | []                                   |
   | created_at       | 2018-08-28T12:50:51.820151+00:00     |
   | updated_at       | 2018-08-28T14:46:18.333109+00:00     |
   | sriov_numvfs     | 0                                    |
   | ipv4_mode        | disabled                             |
   | ipv6_mode        | disabled                             |
   | accelerated      | [True]                               |
   +------------------+--------------------------------------+

*************************************
Configuring Cinder on controller disk
*************************************

Review the available disk space and capacity and obtain the uuid of the
physical disk:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
   +--------------------------------------+-----------+---------+---------+---------+------------+...
   | uuid                                 | device_no | device_ | device_ | size_mi | available_ |...
   |                                      | de        | num     | type    | b       | mib        |...
   +--------------------------------------+-----------+---------+---------+---------+------------+...
   | 6b42c9dc-f7c0-42f1-a410-6576f5f069f1 | /dev/sda  | 2048    | HDD     | 600000  | 434072     |...
   |                                      |           |         |         |         |            |...
   |                                      |           |         |         |         |            |...
   | 534352d8-fec2-4ca5-bda7-0e0abe5a8e17 | /dev/sdb  | 2064    | HDD     | 16240   | 16237      |...
   |                                      |           |         |         |         |            |...
   |                                      |           |         |         |         |            |...
   | 146195b2-f3d7-42f9-935d-057a53736929 | /dev/sdc  | 2080    | HDD     | 16240   | 16237      |...
   |                                      |           |         |         |         |            |...
   |                                      |           |         |         |         |            |...
   +--------------------------------------+-----------+---------+---------+---------+------------+...

Create the 'cinder-volumes' local volume group:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-0 cinder-volumes
   +-----------------+--------------------------------------+
   | lvm_vg_name     | cinder-volumes                       |
   | vg_state        | adding                               |
   | uuid            | 61cb5cd2-171e-4ef7-8228-915d3560cdc3 |
   | ihost_uuid      | 9c332b27-6f22-433b-bf51-396371ac4608 |
   | lvm_vg_access   | None                                 |
   | lvm_max_lv      | 0                                    |
   | lvm_cur_lv      | 0                                    |
   | lvm_max_pv      | 0                                    |
   | lvm_cur_pv      | 0                                    |
   | lvm_vg_size     | 0.00                                 |
   | lvm_vg_total_pe | 0                                    |
   | lvm_vg_free_pe  | 0                                    |
   | created_at      | 2018-08-28T13:45:20.218905+00:00     |
   | updated_at      | None                                 |
   | parameters      | {u'lvm_type': u'thin'}               |
   +-----------------+--------------------------------------+

Create a disk partition to add to the volume group:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 534352d8-fec2-4ca5-bda7-0e0abe5a8e17 16237 -t lvm_phys_vol
   +-------------+--------------------------------------------------+
   | Property    | Value                                            |
   +-------------+--------------------------------------------------+
   | device_path | /dev/disk/by-path/pci-0000:00:0d.0-ata-2.0-part1 |
   | device_node | /dev/sdb1                                        |
   | type_guid   | ba5eba11-0000-1111-2222-000000000001             |
   | type_name   | None                                             |
   | start_mib   | None                                             |
   | end_mib     | None                                             |
   | size_mib    | 16237                                            |
   | uuid        | 0494615f-bd79-4490-84b9-dcebbe5f377a             |
   | ihost_uuid  | 9c332b27-6f22-433b-bf51-396371ac4608             |
   | idisk_uuid  | 534352d8-fec2-4ca5-bda7-0e0abe5a8e17             |
   | ipv_uuid    | None                                             |
   | status      | Creating                                         |
   | created_at  | 2018-08-28T13:45:48.512226+00:00                 |
   | updated_at  | None                                             |
   +-------------+--------------------------------------------------+

Wait for the new partition to be created (i.e. status=Ready):

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk  534352d8-fec2-4ca5-bda7-0e0abe5a8e17
   +--------------------------------------+...+------------+...+---------------------+----------+--------+
   | uuid                                 |...| device_nod |...| type_name           | size_mib | status |
   |                                      |...| e          |...|                     |          |        |
   +--------------------------------------+...+------------+...+---------------------+----------+--------+
   | 0494615f-bd79-4490-84b9-dcebbe5f377a |...| /dev/sdb1  |...| LVM Physical Volume | 16237    | Ready  |
   |                                      |...|            |...|                     |          |        |
   |                                      |...|            |...|                     |          |        |
   +--------------------------------------+...+------------+...+---------------------+----------+--------+

Add the partition to the volume group:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 cinder-volumes 0494615f-bd79-4490-84b9-dcebbe5f377a
   +--------------------------+--------------------------------------------------+
   | Property                 | Value                                            |
   +--------------------------+--------------------------------------------------+
   | uuid                     | 9a0ad568-0ace-4d57-9e03-e7a63f609cf2             |
   | pv_state                 | adding                                           |
   | pv_type                  | partition                                        |
   | disk_or_part_uuid        | 0494615f-bd79-4490-84b9-dcebbe5f377a             |
   | disk_or_part_device_node | /dev/sdb1                                        |
   | disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:0d.0-ata-2.0-part1 |
   | lvm_pv_name              | /dev/sdb1                                        |
   | lvm_vg_name              | cinder-volumes                                   |
   | lvm_pv_uuid              | None                                             |
   | lvm_pv_size              | 0                                                |
   | lvm_pe_total             | 0                                                |
   | lvm_pe_alloced           | 0                                                |
   | ihost_uuid               | 9c332b27-6f22-433b-bf51-396371ac4608             |
   | created_at               | 2018-08-28T13:47:39.450763+00:00                 |
   | updated_at               | None                                             |
   +--------------------------+--------------------------------------------------+

*********************************************
Adding an LVM storage backend at installation
*********************************************

Ensure requirements are met to add LVM storage:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add lvm -s cinder

   WARNING : THIS OPERATION IS NOT REVERSIBLE AND CANNOT BE CANCELLED.

   By confirming this operation, the LVM backend will be created.

   Please refer to the system admin guide for minimum spec for LVM
   storage. Set the 'confirmed' field to execute this operation
   for the lvm backend.

Add the LVM storage backend:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-add lvm -s cinder --confirmed

   System configuration has changed.
   Please follow the administrator guide to complete configuring the system.

   +--------------------------------------+------------+---------+-------------+...+----------+--------------+
   | uuid                                 | name       | backend | state       |...| services | capabilities |
   +--------------------------------------+------------+---------+-------------+...+----------+--------------+
   | 6d750a68-115a-4c26-adf4-58d6e358a00d | file-store | file    | configured  |...| glance   | {}           |
   | e2697426-2d79-4a83-beb7-2eafa9ceaee5 | lvm-store  | lvm     | configuring |...| cinder   | {}           |
   +--------------------------------------+------------+---------+-------------+...+----------+--------------+

Wait for the LVM storage backend to be configured (i.e. state=configured):

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system storage-backend-list
   +--------------------------------------+------------+---------+------------+------+----------+--------------+
   | uuid                                 | name       | backend | state      | task | services | capabilities |
   +--------------------------------------+------------+---------+------------+------+----------+--------------+
   | 6d750a68-115a-4c26-adf4-58d6e358a00d | file-store | file    | configured | None | glance   | {}           |
   | e2697426-2d79-4a83-beb7-2eafa9ceaee5 | lvm-store  | lvm     | configured | None | cinder   | {}           |
   +--------------------------------------+------------+---------+------------+------+----------+--------------+

***********************************************
Configuring VM local storage on controller disk
***********************************************

Review the available disk space and capacity and obtain the uuid of the
physical disk:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-list controller-0
   +--------------------------------------+-----------+---------+---------+---------+------------+...
   | uuid                                 | device_no | device_ | device_ | size_mi | available_ |...
   |                                      | de        | num     | type    | b       | mib        |...
   +--------------------------------------+-----------+---------+---------+---------+------------+...
   | 6b42c9dc-f7c0-42f1-a410-6576f5f069f1 | /dev/sda  | 2048    | HDD     | 600000  | 434072     |...
   |                                      |           |         |         |         |            |...
   |                                      |           |         |         |         |            |...
   | 534352d8-fec2-4ca5-bda7-0e0abe5a8e17 | /dev/sdb  | 2064    | HDD     | 16240   | 0          |...
   |                                      |           |         |         |         |            |...
   |                                      |           |         |         |         |            |...
   | 146195b2-f3d7-42f9-935d-057a53736929 | /dev/sdc  | 2080    | HDD     | 16240   | 16237      |...
   |                                      |           |         |         |         |            |...
   |                                      |           |         |         |         |            |...
   +--------------------------------------+-----------+---------+---------+---------+------------+...

Create the 'nova-local' volume group:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-lvg-add controller-0 nova-local
   +-----------------+-------------------------------------------------------------------+
   | Property        | Value                                                             |
   +-----------------+-------------------------------------------------------------------+
   | lvm_vg_name     | nova-local                                                        |
   | vg_state        | adding                                                            |
   | uuid            | 517d313e-8aa0-4b4d-92e6-774b9085f336                              |
   | ihost_uuid      | 9c332b27-6f22-433b-bf51-396371ac4608                              |
   | lvm_vg_access   | None                                                              |
   | lvm_max_lv      | 0                                                                 |
   | lvm_cur_lv      | 0                                                                 |
   | lvm_max_pv      | 0                                                                 |
   | lvm_cur_pv      | 0                                                                 |
   | lvm_vg_size     | 0.00                                                              |
   | lvm_vg_total_pe | 0                                                                 |
   | lvm_vg_free_pe  | 0                                                                 |
   | created_at      | 2018-08-28T14:02:58.486716+00:00                                  |
   | updated_at      | None                                                              |
   | parameters      | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
   +-----------------+-------------------------------------------------------------------+

Create a disk partition to add to the volume group:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-add controller-0 146195b2-f3d7-42f9-935d-057a53736929 16237 -t lvm_phys_vol
   +-------------+--------------------------------------------------+
   | Property    | Value                                            |
   +-------------+--------------------------------------------------+
   | device_path | /dev/disk/by-path/pci-0000:00:0d.0-ata-3.0-part1 |
   | device_node | /dev/sdc1                                        |
   | type_guid   | ba5eba11-0000-1111-2222-000000000001             |
   | type_name   | None                                             |
   | start_mib   | None                                             |
   | end_mib     | None                                             |
   | size_mib    | 16237                                            |
   | uuid        | 009ce3b1-ed07-46e9-9560-9d2371676748             |
   | ihost_uuid  | 9c332b27-6f22-433b-bf51-396371ac4608             |
   | idisk_uuid  | 146195b2-f3d7-42f9-935d-057a53736929             |
   | ipv_uuid    | None                                             |
   | status      | Creating                                         |
   | created_at  | 2018-08-28T14:04:29.714030+00:00                 |
   | updated_at  | None                                             |
   +-------------+--------------------------------------------------+

Wait for the new partition to be created (i.e. status=Ready):

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-disk-partition-list controller-0 --disk 146195b2-f3d7-42f9-935d-057a53736929
   +--------------------------------------+...+------------+...+---------------------+----------+--------+
   | uuid                                 |...| device_nod |...| type_name           | size_mib | status |
   |                                      |...| e          |...|                     |          |        |
   +--------------------------------------+...+------------+...+---------------------+----------+--------+
   | 009ce3b1-ed07-46e9-9560-9d2371676748 |...| /dev/sdc1  |...| LVM Physical Volume | 16237    | Ready  |
   |                                      |...|            |...|                     |          |        |
   |                                      |...|            |...|                     |          |        |
   +--------------------------------------+...+------------+...+---------------------+----------+--------+

Add the partition to the volume group:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-pv-add controller-0 nova-local 009ce3b1-ed07-46e9-9560-9d2371676748
   +--------------------------+--------------------------------------------------+
   | Property                 | Value                                            |
   +--------------------------+--------------------------------------------------+
   | uuid                     | 830c9dc8-c71a-4cb2-83be-c4d955ef4f6b             |
   | pv_state                 | adding                                           |
   | pv_type                  | partition                                        |
   | disk_or_part_uuid        | 009ce3b1-ed07-46e9-9560-9d2371676748             |
   | disk_or_part_device_node | /dev/sdc1                                        |
   | disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:0d.0-ata-3.0-part1 |
   | lvm_pv_name              | /dev/sdc1                                        |
   | lvm_vg_name              | nova-local                                       |
   | lvm_pv_uuid              | None                                             |
   | lvm_pv_size              | 0                                                |
   | lvm_pe_total             | 0                                                |
   | lvm_pe_alloced           | 0                                                |
   | ihost_uuid               | 9c332b27-6f22-433b-bf51-396371ac4608             |
   | created_at               | 2018-08-28T14:06:05.705546+00:00                 |
   | updated_at               | None                                             |
   +--------------------------+--------------------------------------------------+

**********************
Unlocking controller-0
**********************

You must unlock controller-0 so that you can use it to install
controller-1. Use the system host-unlock command:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-0

The host is rebooted. During the reboot, the command line is
unavailable, and any ssh connections are dropped. To monitor the
progress of the reboot, use the controller-0 console.

****************************************
Verifying the controller-0 configuration
****************************************

On controller-0, acquire Keystone administrative privileges:

::

   controller-0:~$ source /etc/nova/openrc

Verify that the controller-0 services are running:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system service-list
   +-----+-------------------------------+--------------+----------------+
   | id  | service_name                  | hostname     | state          |
   +-----+-------------------------------+--------------+----------------+
   ...
   | 1   | oam-ip                        | controller-0 | enabled-active |
   | 2   | management-ip                 | controller-0 | enabled-active |
   ...
   +-----+-------------------------------+--------------+----------------+

Verify that controller-0 has controller and compute subfunctions:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-show 1 | grep subfunctions
   | subfunctions        | controller,compute                         |

Verify that controller-0 is unlocked, enabled, and available:

::

   [wrsroot@controller-0 ~(keystone_admin)]$ system host-list
   +----+--------------+-------------+----------------+-------------+--------------+
   | id | hostname     | personality | administrative | operational | availability |
   +----+--------------+-------------+----------------+-------------+--------------+
   | 1  | controller-0 | controller  | unlocked       | enabled     | available    |
   +----+--------------+-------------+----------------+-------------+--------------+

*****************
System alarm list
*****************

When all nodes are unlocked, enabled, and available: check 'fm alarm-list' for
issues.

Your StarlingX deployment is now up and running with one controller with Cinder
storage and all OpenStack services up and running. You can now proceed with
standard OpenStack APIs, CLIs and/or Horizon to load Glance images, configure
Nova Flavors, configure Neutron networks and launch Nova virtual machines.

----------------------
Deployment terminology
----------------------

.. include:: deployment_terminology.rst
   :start-after: incl-simplex-deployment-terminology:
   :end-before: incl-simplex-deployment-terminology-end:

.. include:: deployment_terminology.rst
   :start-after: incl-standard-controller-deployment-terminology:
   :end-before: incl-standard-controller-deployment-terminology-end:

.. include:: deployment_terminology.rst
   :start-after: incl-common-deployment-terminology:
   :end-before: incl-common-deployment-terminology-end:
