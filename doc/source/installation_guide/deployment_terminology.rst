.. _incl-simplex-deployment-terminology:

**All-In-One Controller Node**
    A single physical node that provides a Controller Function, Compute
    Function, and Storage Function.

.. _incl-simplex-deployment-terminology-end:


.. _incl-standard-controller-deployment-terminology:

**Controller Node / Function**
    A node that runs Cloud Control Function for managing Cloud Resources.

    - Runs Cloud Control Functions for managing Cloud Resources.
    - Runs all OpenStack Control Functions (e.g. managing Images, Virtual
      Volumes, Virtual Network, and Virtual Machines).
    - Can be part of a two-node HA Control Node Cluster for running Control
      Functions either Active/Active or Active/Standby.

**Compute ( & Network ) Node / Function**
    A node that hosts applications in Virtual Machines using Compute Resources
    such as CPU, Memory, and Disk.

    - Runs Virtual Switch for realizing virtual networks.
    - Provides L3 Routing and NET Services.

.. _incl-standard-controller-deployment-terminology-end:


.. _incl-dedicated-storage-deployment-terminology:

**Storage Node / Function**
    A node that contains a set of Disks (e.g. SATA, SAS, SSD, and/or NVMe).

    - Runs CEPH Distributed Storage Software.
    - Part of an HA multi-node CEPH Storage Cluster supporting a replication
      factor of two or three, Journal Caching, and Class Tiering.
    - Provides HA Persistent Storage for Images, Virtual Volumes
      (i.e. Block Storage), and Object Storage.

.. _incl-dedicated-storage-deployment-terminology-end:

.. _incl-common-deployment-terminology:

**OAM Network**
    The network on which all external StarlingX Platform APIs are exposed,
    (i.e. REST APIs, Horizon Web Server, SSH, and SNMP), typically 1GE.

    Only Controller type nodes are required to be connected to the OAM
    Network.

**Management Network**
    A private network (i.e. not connected externally), tipically 10GE,
    used for the following:

    - Internal OpenStack / StarlingX monitoring and control.
    - VM I/O access to a storage cluster.

    All nodes are required to be connected to the Management Network.

**Data Network(s)**
    Networks on which the OpenStack / Neutron Provider Networks are realized
    and become the VM Tenant Networks.

    Only Compute type and All-in-One type nodes are required to be connected
    to the Data Network(s); these node types require one or more interface(s)
    on the Data Network(s).

**IPMI Network**
    An optional network on which IPMI interfaces of all nodes are connected.
    The network must be reachable using L3/IP from the Controller's OAM
    Interfaces.

    You can optionally connect all node types to the IPMI Network.

**PXEBoot Network**
    An optional network for Controllers to boot/install other nodes over the
    network.

    By default, Controllers use the Management Network for boot/install of other
    nodes in the openstack cloud. If this optional network is used, all node
    types are required to be connected to the PXEBoot Network.

    A PXEBoot network is required for a variety of special case situations:

    - Cases where the Management Network must be IPv6:

      - IPv6 does not support PXEBoot. Therefore, IPv4 PXEBoot network must be
        configured.

    - Cases where the Management Network must be VLAN tagged:

      - Most Server's BIOS do not support PXEBooting over tagged networks.
        Therefore, you must configure an untagged PXEBoot network.

    - Cases where a Management Network must be shared across regions but
      individual regions' Controllers want to only network boot/install nodes
      of their own region:

      - You must configure separate, per-region PXEBoot Networks.

**Infra Network**
    A deprecated optional network that was historically used for access to the
    Storage cluster.

    If this optional network is used, all node types are required to be
    connected to the INFRA Network,

**Node Interfaces**
    All Nodes' Network Interfaces can, in general, optionally be either:

    - Untagged single port.
    - Untagged two-port LAG and optionally split between redudant L2 Switches
      running vPC (Virtual Port-Channel), also known as multichassis
      EtherChannel (MEC).
    - VLAN on either single-port ETH interface or two-port LAG interface.

.. _incl-common-deployment-terminology-end:
