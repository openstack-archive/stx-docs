.. _incl-simplex-deployment-terminology:

**All-in-one controller node**
    A single physical node that provides a controller function, compute
    function, and storage function.

.. _incl-simplex-deployment-terminology-end:


.. _incl-standard-controller-deployment-terminology:

**Controller node / function**
    A node that runs cloud control function for managing cloud resources.

    - Runs cloud control functions for managing cloud resources.
    - Runs all OpenStack control functions (e.g. managing images, virtual
      volumes, virtual network, and virtual machines).
    - Can be part of a two-node HA control node cluster for running control
      functions either active/active or active/standby.

**Compute ( & network ) node / function**
    A node that hosts applications in virtual machines using compute resources
    such as CPU, memory, and disk.

    - Runs virtual switch for realizing virtual networks.
    - Provides L3 routing and NET services.

.. _incl-standard-controller-deployment-terminology-end:


.. _incl-dedicated-storage-deployment-terminology:

**Storage node / function**
    A node that contains a set of disks (e.g. SATA, SAS, SSD, and/or NVMe).

    - Runs CEPH distributed storage software.
    - Part of an HA multi-node CEPH storage cluster supporting a replication
      factor of two or three, journal caching, and class tiering.
    - Provides HA persistent storage for images, virtual volumes
      (i.e. block storage), and object storage.

.. _incl-dedicated-storage-deployment-terminology-end:

.. _incl-common-deployment-terminology:

**OAM network**
    The network on which all external StarlingX platform APIs are exposed,
    (i.e. REST APIs, Horizon web server, SSH, and SNMP), typically 1GE.

    Only controller type nodes are required to be connected to the OAM
    network.

**Management network**
    A private network (i.e. not connected externally), tipically 10GE,
    used for the following:

    - Internal OpenStack / StarlingX monitoring and control.
    - VM I/O access to a storage cluster.

    All nodes are required to be connected to the management network.

**Data network(s)**
    Networks on which the OpenStack / Neutron provider networks are realized
    and become the VM tenant networks.

    Only compute type and all-in-one type nodes are required to be connected
    to the data network(s); these node types require one or more interface(s)
    on the data network(s).

**IPMI network**
    An optional network on which IPMI interfaces of all nodes are connected.
    The network must be reachable using L3/IP from the controller's OAM
    interfaces.

    You can optionally connect all node types to the IPMI network.

**PXEBoot network**
    An optional network for controllers to boot/install other nodes over the
    network.

    By default, controllers use the management network for boot/install of other
    nodes in the openstack cloud. If this optional network is used, all node
    types are required to be connected to the PXEBoot network.

    A PXEBoot network is required for a variety of special case situations:

    - Cases where the management network must be IPv6:

      - IPv6 does not support PXEBoot. Therefore, IPv4 PXEBoot network must be
        configured.

    - Cases where the management network must be VLAN tagged:

      - Most server's BIOS do not support PXEBooting over tagged networks.
        Therefore, you must configure an untagged PXEBoot network.

    - Cases where a management network must be shared across regions but
      individual regions' controllers want to only network boot/install nodes
      of their own region:

      - You must configure separate, per-region PXEBoot networks.

**Infra network**
    A deprecated optional network that was historically used for access to the
    storage cluster.

    If this optional network is used, all node types are required to be
    connected to the INFRA network,

**Node interfaces**
    All nodes' network interfaces can, in general, optionally be either:

    - Untagged single port.
    - Untagged two-port LAG and optionally split between redudant L2 switches
      running vPC (Virtual Port-Channel), also known as multichassis
      EtherChannel (MEC).
    - VLAN on either single-port ETH interface or two-port LAG interface.

.. _incl-common-deployment-terminology-end:
