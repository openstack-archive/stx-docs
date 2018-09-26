====================================================
Compute API v2 Titanium extensions
====================================================

Titanium extensions to the OpenStack Compute API include:

-  Adding capability to specify VIF-Model on a per-NIC basis when
   creating/launching/booting a VM Server.

-  Adding capability to specify VIF-PCI Address on a per-NIC basis when
   creating/launching/booting a VM Server.

-  Adding an attribute to allow sorting of a VM Server's IP Addresses in
   order of network attachment(NIC).

-  Adding the ability to scale the resources (currently only vCPUs) of a
   VM Server up and down without requiring a restart of the VM Server.

-  Adding a number of new attributes and behavior to VM Server Groups; a
   project_id attribute for ownership, an affinity-hyperthread policy
   and best_effort and max group_size metadata attribute values.

-  Support for various new Flavor Extra Specs to enable
   Titanium-specific optimizations and capabilities for compute servers.

-  Adding the capability of displaying details about Provider Network
   with regards to Nova usage.

The typical port used for the Compute REST API is 8774. However, proper
technique would be to look up the nova service endpoint in keystone.

-----------
Extensions
-----------

The Extensions entity lists all available extensions

**********************************
Lists all extensions - Compute API
**********************************

.. rest_method:: GET /v2/​{tenant_id}​/extensions

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "namespace (Optional)", "plain", "xsd:string", "Indicates namespace of the extension."
   "name (Optional)", "plain", "xsd:string", "Indicates name of the extension."
   "updated (Optional)", "plain", "xsd:string", "Indicates updated time of the extension."
   "description (Optional)", "plain", "xsd:string", "Indicates description of the extension."
   "alias (Optional)", "plain", "xsd:string", "Indicates alias of the extension."
   "links (Optional)", "plain", "xsd:list", "A list of links for the extension."

::

   {
      "extensions" : [
         ...
         {
            "namespace" : "http://docs.windriver.com/tis/ext/wrs-if/api/v1.0",
            "name" : "wrs-server-if",
            "updated" : "2014-08-16T00:00:00+00:00",
            "description" : "WRS Interface Related Extensions",
            "alias" : "wrs-if",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.com/tis/ext/wrs-res/api/v1.0",
            "name" : "wrs-server-resource",
            "updated" : "2014-08-16T00:00:00+00:00",
            "description" : "WRS Server Resource Extensions",
            "alias" : "wrs-res",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.com/tis/ext/wrs-sg/api/v1.0",
            "name" : "wrs-server-group",
            "updated" : "2014-08-16T00:00:00+00:00",
            "description" : "WRS Server Grouip Extensions",
            "alias" : "wrs-sg",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.com/tis/ext/wrs/api/v1.0",
            "name" : "wrs-flavor-extra-spec",
            "updated" : "2014-08-16T00:00:00+00:00",
            "description" : "WRS Extended Flavor Extra Spec Keys"
            "alias" : "wrs",
            "links" : []
         },
         ...
      ]
   }

This operation does not accept a request body.

**********************************************************
Gets information about a specified extension - Compute API
**********************************************************

.. rest_method:: GET /v2/​{tenant_id}​/extensions/​{extension_alias}​

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "extension_alias", "URI", "xsd:string", "The alias for the extension to list."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "namespace (Optional)", "plain", "xsd:string", "Indicates namespace of the extension."
   "name (Optional)", "plain", "xsd:string", "Indicates name of the extension."
   "updated (Optional)", "plain", "xsd:string", "Indicates updated time of the extension."
   "description (Optional)", "plain", "xsd:string", "Indicates description of the extension."
   "alias (Optional)", "plain", "xsd:string", "Indicates alias of the extension."
   "links (Optional)", "plain", "xsd:list", "A list of links for the extension."

::

   {
      "extension" : {
         "namespace" : "http://docs.windriver.com/tis/ext/wrs-if/api/v1.0",
         "name" : "wrs-server-if",
         "updated" : "2014-08-16T00:00:00+00:00",
         "description" : "WRS Interface Related Extensions",
         "alias" : "wrs-if",
         "links" : []
      }
   }

   OR

   {
      "extension" : {
         "namespace" : "http://docs.windriver.com/tis/ext/wrs-res/api/v1.0",
         "name" : "wrs-server-resource",
         "updated" : "2014-08-16T00:00:00+00:00",
         "description" : "WRS Server Resource Extensions",
         "alias" : "wrs-res",
         "links" : []
      }
   }

   OR

   {
      "extension" : {
         "namespace" : "http://docs.windriver.com/tis/ext/wrs-sg/api/v1.0",
         "name" : "wrs-server-group",
         "updated" : "2014-08-16T00:00:00+00:00",
         "description" : "WRS Server Group Extensions",
         "alias" : "wrs-sg",
         "links" : []
      }
   }

   OR

   {
      "extension" : {
         "namespace" : "http://docs.windriver.com/tis/ext/wrs/api/v1.0",
         "name" : "wrs-flavor-extra-spec",
         "updated" : "2014-08-16T00:00:00+00:00",
         "description" : "WRS Extended Flavor Extra Spec Keys"
         "alias" : "wrs",
         "links" : []
      }
   }

This operation does not accept a request body.

-------
Server
-------

The Titanium extensions to the server entity are:

-  Adding capability to specify VIF-Model on a per-NIC basis when
   creating/launching/booting a VM Server.

-  Adding capability to specify VIF-PCI Address on a per-NIC basis when
   creating/launching/booting a VM Server.

-  Adding the ability to scale the resources (currently only vCPUs) of a
   server up and down without requiring a restart of the VM Server.

-  Adding an attribute to allow sorting of a VM Server's IP Addresses in
   order of network attachment(NIC).

******************
Creates a server
******************

.. rest_method:: POST /v2/​{tenant_id}​/servers

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "networks (Optional)", "plain", "xsd:list", "A ``networks`` object. By default, the server instance is provisioned with all isolated networks for the tenant. Optionally, you can create one or more NICs on the server. To provision the server instance with a NIC for a ``nova-network`` network, specify the UUID in the ``uuid`` attribute in a ``network`` object. To provision the server instance with a NIC for a ``neutron`` network, specify the UUID in the ``port`` attribute in a ``network`` object. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">In Titanium Cloud, to optionally provision the vif model of the NIC, specify the appropriate value in the <code xmlns=""http://www.w3.org/1999/xhtml"">wrs-if:vif_model`` attribute in the <code xmlns=""http://www.w3.org/1999/xhtml"">network`` object. Valid vif model values are: <code xmlns=""http://www.w3.org/1999/xhtml"">e1000``, <code xmlns=""http://www.w3.org/1999/xhtml"">virtio``, <code xmlns=""http://www.w3.org/1999/xhtml"">ne2k_pci``, <code xmlns=""http://www.w3.org/1999/xhtml"">pcnet``, <code xmlns=""http://www.w3.org/1999/xhtml"">rtl8139``, <code xmlns=""http://www.w3.org/1999/xhtml"">pci-passthrough``, <code xmlns=""http://www.w3.org/1999/xhtml"">pci-sriov``. If not specified, a vif model of <code xmlns=""http://www.w3.org/1999/xhtml"">virtio`` will be used. </emphasis>To provision the PCI address of the NIC, specify the appropriate value in the ``wrs-if:vif_pci_address`` attribute in the ``network`` object. Valid PCI address values are in the ``domain:bus:slot.function`` format. If not specified, a PCI address will be chosen by the hypervisor. You can specify multiple NICs on the server."

::

   {
     "server": {
       "name": "testvm",
       "imageRef": "ec42f67b-1dcd-4f09-aa02-7a426737c20a",
       "flavorRef": "2",
       "networks": [
         {
           "wrs-if:vif_model": "e1000",
           "uuid": "06937e9e-0acd-4ad5-a6bb-f82d8896d5e8"
         },
         {
           "wrs-if:vif_pci_address": "0000:04:12.0",
           "uuid": "cdc149b5-9122-4a16-975c-6acb973f49c3"
         },
         {
           "uuid": "b7adf5a0-3c5a-47f3-b733-8d56d12d2f45"
         }
       ]
     }
   }

::

   {
       "server": {
           "adminPass": "yjzytFHb7XHc",
           "id": "f8f4f3ce-f6e0-4e05-8f79-bf984fdfce45",
           "links": [
               {
                   "href": "http://openstack.example.com/v2/openstack/servers/f8f4f3ce-f6e0-4e05-8f79-bf984fdfce45",
                   "rel": "self"
               },
               {
                   "href": "http://openstack.example.com/openstack/servers/f8f4f3ce-f6e0-4e05-8f79-bf984fdfce45",
                   "rel": "bookmark"
               }
           ]
       }
   }

**************************
Lists details of servers
**************************

.. rest_method:: GET /v2/​{tenant_id}​/servers/detail

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "servers", "plain", "xsd:list", "The list of ``server`` objects."
   "nics (Optional)", "plain", "xsd:list", "A ``nics`` object. Contains the list of NICs provisioned on the server instance. Optionally, in Titanium Cloud, each NIC can contain: <ul><li>A ``wrs-if:vif_model`` attribute specifying the NICs vif model; where valid vif model values are: ``e1000``, ``virtio``, ``ne2k_pci``, ``pcnet``, ``rtl8139``, ``pci-passthrough``, ``pci-sriov``. If not specified, a vif model of ``virtio`` is being used. </li><li>A ``wrs-if:vif_pci_address`` attibute specifying the NICs PCI address. If not specified, the PCI address in the guest is chosen by the hypervisor and this value is empty. </li></ul>"
   "addresses (Optional)", "plain", "xsd:list", "An ``addresses`` object. Contains the list of addresses associated with the server instance."
   "wrs-if:nics (Optional)", "plain", "xsd:list", "An ``wrs-if:nics`` object. Contains the list of NIC devices allocated for a VM instance. These are a VM representation of the neutron port objects associated to the VM. They are listed in the same order which the network attachments were specified when the VM was launched."
   "wrs-res:topology (Optional)", "plain", "xsd:string", "This attribute specifies a number of resource details of the VM Server; the number of numa nodes, the amount of memory and the memory page size, and the current number of VCPUs."
   "wrs-res:pci_devices (Optional)", "plain", "xsd:string", "List of pci devices associated with the server instance; indicates the numa node, pci address, type of device, vendor id, product id."
   "wrs-res:vcpus (min/cur/max) (Optional)", "plain", "xsd:list", "This attribute specifies the minimum number of vcpus, current number of vcpus and maximum number of vcpus of a VM Server."
   "wrs-sg:server_group (Optional)", "plain", "xsd:string", "This attribute specifies the server group which the VM Server is in; a null-string if the VM Server is not in a server group."

::

   {
      "servers" : [
         {
            "accessIPv4" : "",
            "wrs-if:nics" : [
               {
                  "nic1" : {
                     "network" : "tenant1-mgmt-net",
                     "port_id" : "dc627524-64a9-4fec-957a-b271f353fb22",
                     "vif_model" : "virtio",
                     "vif_pci_address": "0000:04:12.0",
                     "mtu" : 1500
                  }
               }
            ],
            "OS-EXT-SRV-ATTR:instance_name" : "instance-0000003d",
            "OS-SRV-USG:terminated_at" : null,
            "accessIPv6" : "",
            "config_drive" : "",
            "OS-DCF:diskConfig" : "MANUAL",
            "wrs-sg:server_group" : "",
            "updated" : "2015-04-01T20:32:57Z",
            "metadata" : {},
            "id" : "770a214c-5d22-42ce-9273-f6baab0ad7fd",
            "flavor" : {
               "id" : "00bbded9-318a-461a-aef8-3904356ca8d9",
               "links" : [
                  {
                     "rel" : "bookmark",
                     "href" : "http://128.224.151.243:8774/101d1cffc5ec4accbdb075c89a4c5cd7/flavors/00bbded9-318a-461a-aef8-3904356ca8d9"
                  }
               ]
            },
            "links" : [
               {
                  "rel" : "self",
                  "href" : "http://128.224.151.243:8774/v2/101d1cffc5ec4accbdb075c89a4c5cd7/servers/770a214c-5d22-42ce-9273-f6baab0ad7fd"
               },
               {
                  "rel" : "bookmark",
                  "href" : "http://128.224.151.243:8774/101d1cffc5ec4accbdb075c89a4c5cd7/servers/770a214c-5d22-42ce-9273-f6baab0ad7fd"
               }
            ],
            "OS-EXT-SRV-ATTR:host" : "compute-0",
            "OS-EXT-AZ:availability_zone" : "nova",
            "name" : "vm07-shared-vcpu-id",
            "hostId" : "938254ae1b04aabc901dd4ad2cf2a561a4eab858efa0b0a48eb048ff",
            "user_id" : "13dbcb9d22474c39a4a612cd44bf58ad",
            "status" : "ACTIVE",
            "wrs-res:topology" : "node:1,  1024MB, pgsize:4K, vcpus:3",
            "wrs-res:pci_devices": "node:1, addr:0000:83:04.6, type:VF, vendor:8086, product:0443",
            "OS-EXT-STS:power_state" : 1,
            "OS-EXT-SRV-ATTR:hypervisor_hostname" : "compute-0",
            "tenant_id" : "101d1cffc5ec4accbdb075c89a4c5cd7",
            "OS-SRV-USG:launched_at" : "2015-04-01T20:32:57.000000",
            "OS-EXT-STS:vm_state" : "active",
            "OS-EXT-STS:task_state" : null,
            "progress" : 0,
            "key_name" : null,
            "image" : {
               "id" : "a99dfaa7-c850-4a63-ad99-d4a5f8da3069",
               "links" : [
                  {
                     "rel" : "bookmark",
                     "href" : "http://128.224.151.243:8774/101d1cffc5ec4accbdb075c89a4c5cd7/images/a99dfaa7-c850-4a63-ad99-d4a5f8da3069"
                  }
               ]
            },
            "wrs-res:vcpus (min/cur/max)" : [
               3,
               3,
               3
            ],
            "created" : "2015-04-01T20:32:49Z",
            "addresses" : {
               "tenant1-mgmt-net" : [
                  {
                     "OS-EXT-IPS:type" : "fixed",
                     "version" : 4,
                     "OS-EXT-IPS-MAC:mac_addr" : "fa:16:3e:fc:65:81",
                     "addr" : "192.168.102.6"
                  }
               ]
            },
            "os-extended-volumes:volumes_attached" : []
         }
      ]
   }

This operation does not accept a request body.

**************************************
Shows details for a specified server
**************************************

.. rest_method:: GET /v2/​{tenant_id}​/servers/​{server_id}​

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "server_id", "URI", "csapi:UUID", "The ID for the server of interest to you."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "server", "plain", "xsd:dict", "The requested ``server`` object."
   "nics (Optional)", "plain", "xsd:list", "A ``nics`` object. Contains the list of NICs provisioned on the server instance. Optionally, in Titanium Cloud, each NIC can contain: <ul><li>A ``wrs-if:vif_model`` attribute specifying the NICs vif model; where valid vif model values are: ``e1000``, ``virtio``, ``ne2k_pci``, ``pcnet``, ``rtl8139``, ``pci-passthrough``, ``pci-sriov``. If not specified, a vif model of ``virtio`` is being used. </li><li>A ``wrs-if:vif_pci_address`` attibute specifying the NICs PCI address. If not specified, the PCI address in the guest is chosen by the hypervisor and this value is empty. </li></ul>"
   "addresses (Optional)", "plain", "xsd:list", "An ``addresses`` object. Contains the list of addresses associated with the server instance."
   "wrs-if:nics (Optional)", "plain", "xsd:list", "An ``wrs-if:nics`` object. Contains the list of NIC devices allocated for a VM instance. These are a VM representation of the neutron port objects associated to the VM. They are listed in the same order which the network attachments were specified when the VM was launched."
   "wrs-res:topology (Optional)", "plain", "xsd:string", "This attribute specifies a number of resource details of the VM Server; the number of numa nodes, the amount of memory and the memory page size, and the current number of VCPUs."
   "wrs-res:pci_devices (Optional)", "plain", "xsd:string", "List of pci devices associated with the server instance; indicates the numa node, pci address, type of device, vendor id, product id."
   "wrs-res:vcpus (min/cur/max) (Optional)", "plain", "xsd:list", "This attribute specifies the minimum number of vcpus, current number of vcpus and maximum number of vcpus of a VM Server."
   "wrs-sg:server_group (Optional)", "plain", "xsd:string", "This attribute specifies the server group which the VM Server is in; a null-string if the VM Server is not in a server group."

::

   {
      "server" : {
         "accessIPv4" : "",
         "wrs-if:nics" : [
            {
               "nic1" : {
                  "network" : "tenant1-mgmt-net",
                  "port_id" : "dc627524-64a9-4fec-957a-b271f353fb22",
                  "vif_model" : "virtio",
                  "vif_pci_address": "0000:04:12.0",
                  "mtu" : 1500
               }
            }
         ],
         "OS-EXT-SRV-ATTR:instance_name" : "instance-0000003d",
         "OS-SRV-USG:terminated_at" : null,
         "accessIPv6" : "",
         "config_drive" : "",
         "OS-DCF:diskConfig" : "MANUAL",
         "wrs-sg:server_group" : "",
         "updated" : "2015-04-01T20:32:57Z",
         "metadata" : {},
         "id" : "770a214c-5d22-42ce-9273-f6baab0ad7fd",
         "flavor" : {
            "id" : "00bbded9-318a-461a-aef8-3904356ca8d9",
            "links" : [
               {
                  "rel" : "bookmark",
                  "href" : "http://128.224.151.243:8774/101d1cffc5ec4accbdb075c89a4c5cd7/flavors/00bbded9-318a-461a-aef8-3904356ca8d9"
               }
            ]
         },
         "links" : [
            {
               "rel" : "self",
               "href" : "http://128.224.151.243:8774/v2/101d1cffc5ec4accbdb075c89a4c5cd7/servers/770a214c-5d22-42ce-9273-f6baab0ad7fd"
            },
            {
               "rel" : "bookmark",
               "href" : "http://128.224.151.243:8774/101d1cffc5ec4accbdb075c89a4c5cd7/servers/770a214c-5d22-42ce-9273-f6baab0ad7fd"
            }
         ],
         "OS-EXT-SRV-ATTR:host" : "compute-0",
         "OS-EXT-AZ:availability_zone" : "nova",
         "name" : "vm07-shared-vcpu-id",
         "hostId" : "938254ae1b04aabc901dd4ad2cf2a561a4eab858efa0b0a48eb048ff",
         "user_id" : "13dbcb9d22474c39a4a612cd44bf58ad",
         "status" : "ACTIVE",
         "wrs-res:topology" : "node:1,  1024MB, pgsize:4K, vcpus:3",
         "wrs-res:pci_devices": "node:1, addr:0000:83:04.6, type:VF, vendor:8086, product:0443",
         "OS-EXT-STS:power_state" : 1,
         "OS-EXT-SRV-ATTR:hypervisor_hostname" : "compute-0",
         "tenant_id" : "101d1cffc5ec4accbdb075c89a4c5cd7",
         "OS-SRV-USG:launched_at" : "2015-04-01T20:32:57.000000",
         "OS-EXT-STS:vm_state" : "active",
         "OS-EXT-STS:task_state" : null,
         "progress" : 0,
         "key_name" : null,
         "image" : {
            "id" : "a99dfaa7-c850-4a63-ad99-d4a5f8da3069",
            "links" : [
               {
                  "rel" : "bookmark",
                  "href" : "http://128.224.151.243:8774/101d1cffc5ec4accbdb075c89a4c5cd7/images/a99dfaa7-c850-4a63-ad99-d4a5f8da3069"
               }
            ]
         },
         "wrs-res:vcpus (min/cur/max)" : [
            3,
            3,
            3
         ],
         "created" : "2015-04-01T20:32:49Z",
         "addresses" : {
            "tenant1-mgmt-net" : [
               {
                  "OS-EXT-IPS:type" : "fixed",
                  "version" : 4,
                  "OS-EXT-IPS-MAC:mac_addr" : "fa:16:3e:fc:65:81",
                  "addr" : "192.168.102.6"
               }
            ]
         },
         "os-extended-volumes:volumes_attached" : []
      }
   }

This operation does not accept a request body.

****************************************************************************************************************************************************
Allows the resources associated with the server (currently only the number of CPUs) to be scaled up and down without requiring a restart of the VM
****************************************************************************************************************************************************

.. rest_method:: POST /v2/​{tenant_id}​/servers/​{server_id}​/action

**Normal response codes**

202

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "server_id", "URI", "csapi:UUID", "The ID for the server of interest to you."
   "wrs-res:scale", "plain", "xsd:string", "Specify the ``wrs-res:scale`` action in the request body."
   "direction", "plain", "xsd:string", "Direction to scale, ""up"" or ""down"". This will result in scaling the specified resource by one unit in the specified direction."
   "resource", "plain", "xsd:string", "Resource to scale. Currently only ""cpu"" is supported."

::

   {
       "wrs-res:scale": {
           "direction": "up",
           "resource": cpu
       }
   }

This operation does not return a response body.

*****************
Create Interface
*****************

.. rest_method:: POST /v2/​{tenant_id}​/servers/​{server_id}​/os-interface

**Normal response codes**

202

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "server_id", "URI", "csapi:UUID", "The ID for the server of interest to you."
   "wrs-if:vif_model (Optional)", "plain", "string", "Requested VIF model."

::

   {
       "interfaceAttachment": {
           "net_id": "e8b9af5e-1f47-429e-9ee0-fef202d4ea14",
           "wrs-if:vif_model": "virtio"
       }
   }

This operation does not return a response body.

---------------------------------
Server Groups (os-server-groups)
---------------------------------

The Titanium extensions to the Server Groups entity are:

-  Added a 'wrs-sg:project_id' attribute to assign tenant ownership to a
   Server Group.

-  Added a 'wrs-sg:affinity-hyperthread' policy to indicate that members
   of the Server Group are allowed to share hyperthread siblings.

-  Added a boolean 'wrs-sg:best_effort' metadata key/value in order to
   specify whether the policy should be strictly enforced or not.

-  Added an integer 'wrs-sg:group_size' metadata key/value in order to
   specify the maximum number of members in the group.

*********************
Lists server groups
*********************

.. rest_method:: GET /v2/​{tenant_id}​/os-server-groups

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "server_groups", "plain", "xsd:list", "The list of ``server_group`` objects."
   "wrs-sg:project_id", "plain", "csapi:UUID", "The tenant or project owning the server group."
   "policies", "plain", "xsd:list", "A list of policies associated with the server group. Titanium Cloud added ``wrs-sg:affinity-hyperthread`` policy to indicate that ``only`` the members of this server group can share sibling threads with each other."
   "metadata", "plain", "xsd:dict", "Associated metadata key-and-value pairs. Titanium Cloud added a boolean valued ``wrs-sg:best_effort`` metadata key-and-value pair to indicate whether the server groups policy should be strictly enforced or not. Titanium Cloud added an integer valued ``wrs-sg:group_size`` metadata key-and-value pair to indicate the maximum number of members of the server group."

::

   {
       "server_groups": [
           {
               "id": "616fb98f-46ca-475e-917e-2563e5a8cd19",
               "wrs-sg:project_id": "28d41dbebab24bdf8854a6632271a3f6"
               "name": "callservergroup",
               "policies": [
                   "wrs-sg:affinity-hyperthread"
               ],
               "members": [],
               "metadata": {
                   "wrs-sg:best_effort": "1",
                   "wrs-sg:group_size": "2"
               }
           },
           {
               "id": "2fb919a2-4666-11e4-9255-080027367628",
               "wrs-sg:project_id": "28d41dbebab24bdf8854a6632271a3f6"
               "name": "antiaffinitygroup",
               "policies": [
                   "anti-affinity"
               ],
               "members": [],
               "metadata": {}
           }
       ]
   }

This operation does not accept a request body.

************************
Creates a server group
************************

.. rest_method:: POST /v2/​{tenant_id}​/os-server-groups

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "wrs-sg:project_id", "plain", "csapi:UUID", "The project or tenant ID which owns this server group."
   "policies (Optional)", "plain", "xsd:list", "The scheduler policy to associate with the server group. Modified by Titanium Cloud to include the following additional policy: <ul><li>``wrs-sg:affinity-hyperthread`` which will try to put servers on the same compute node and have servers sharing sibling hyperthread cores with each other, and only each other. Server Groups using this policy are restriceted to a maximum of 2 members. </li></ul>"
   "metadata (Optional)", "plain", "xsd:dict", "This parameter specifies a dictionary of optional metadata to be associated with the group. Additional keys added by Titanium Cloud are: <ul><li>``wrs-sg:best_effort`` (where a value of 0 means that the scheduler policy will be strictly applied and a value of 1 means that the server will still be scheduled even if the policy can't be met). </li><li>``wrs-sg:group_size`` (where the value is an integer specifying the max number of servers in the group). </li></ul>"

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "server_group", "plain", "xsd:dict", "The requested ``server_group`` object."
   "wrs-sg:project_id", "plain", "csapi:UUID", "The tenant or project owning the server group."
   "policies", "plain", "xsd:list", "A list of policies associated with the server group. Titanium Cloud added ``wrs-sg:affinity-hyperthread`` policy to indicate that ``only`` the members of this server group can share sibling threads with each other."
   "metadata", "plain", "xsd:dict", "Associated metadata key-and-value pairs. Titanium Cloud added a boolean valued ``wrs-sg:best_effort`` metadata key-and-value pair to indicate whether the server groups policy should be strictly enforced or not. Titanium Cloud added an integer valued ``wrs-sg:group_size`` metadata key-and-value pair to indicate the maximum number of members of the server group."

::

   {
       "server_group": {
           "name": "antiaffinitygroup",
           "wrs-sg:project_id": "28d41dbebab24bdf8854a6632271a3f6"
           "policies": [
               "anti-affinity"
           ],
           "metadata": {
               "wrs-sg:best_effort": "1",
               "wrs-sg:group_size": "2"
           }
       }
   }

::

   {
       "server_group": {
           "id": "5bbcc3c4-1da2-4437-a48a-66f15b1b13f9",
           "wrs-sg:project_id": "28d41dbebab24bdf8854a6632271a3f6"
           "name": "antiaffinitygroup",
           "policies": [
               "anti-affinity"
           ],
           "members": [],
           "metadata": {
               "wrs-sg:best_effort": "1",
               "wrs-sg:group_size": "2"
           }
       }
   }

********************************************
Shows details for a specified server group
********************************************

.. rest_method:: GET /v2/​{tenant_id}​/os-server-groups/​{ServerGroup_id}​

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "ServerGroup_id", "URI", "csapi:UUID", "The server group ID."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "server_group", "plain", "xsd:dict", "The requested ``server_group`` object."
   "wrs-sg:project_id", "plain", "csapi:UUID", "The tenant or project owning the server group."
   "policies", "plain", "xsd:list", "A list of policies associated with the server group. Titanium Cloud added ``wrs-sg:affinity-hyperthread`` policy to indicate that ``only`` the members of this server group can share sibling threads with each other."
   "metadata", "plain", "xsd:dict", "Associated metadata key-and-value pairs. Titanium Cloud added a boolean valued ``wrs-sg:best_effort`` metadata key-and-value pair to indicate whether the server groups policy should be strictly enforced or not. Titanium Cloud added an integer valued ``wrs-sg:group_size`` metadata key-and-value pair to indicate the maximum number of members of the server group."

::

   {
       "server_group": {
           "id": "616fb98f-46ca-475e-917e-2563e5a8cd19",
           "wrs-sg:project_id": "28d41dbebab24bdf8854a6632271a3f6"
           "name": "callservergroup",
           "policies": [
               "wrs-sg:affinity-hyperthread"
           ],
           "members": [],
           "metadata": {
               "wrs-sg:best_effort": "1",
               "wrs-sg:group_size": "2"
           }
       }
   }

This operation does not accept a request body.

-------------------
Flavor Extra Specs
-------------------

Titanium Cloud has added several flavor extra specs, e.g.
``sw:wrs:guest:heartbeat``, ``hw:wrs:shared_vcpu``,
``hw:wrs:min_vcpus``, ``sw:wrs:vtpm`` and many more. For the complete
list of additional flavor extra specs added by Titanium Cloud, and an
explanation of how to use them, please refer to the Wind River Titanium
Cloud Administration guide set.

********************************************************
Lists the extra-specs or keys for the specified flavor
********************************************************

.. rest_method:: GET /v2/​{tenant_id}​/flavors/​{flavor_id}​/os-extra_specs

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "flavor_id", "URI", "String", "The ID of the flavor of interest to you."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "extra_specs (Optional)", "plain", "xsd:list", "The list of flavor extra specs."
   "sw:wrs:guest:heartbeat (Optional)", "plain", "xsd:boolean", "Indicates whether or not the guest applications running in the virtual machine make use of the Titanium Cloud Heartbeat client API."
   "sw:wrs:vtpm (Optional)", "plain", "xsd:boolean", "Indicates whether or not to expose a TPM device to the Guest."
   "hw:wrs:shared_vcpu (Optional)", "plain", "xsd:integer", "Indicates the vCPU of the guest virtual machine that will be scheduled to run on a shared CPU of the host. Note, this can be specified even if hw:cpu_policy is set to dedicated; allowing the guest application to use dedicated cores exclusively for its high-load tasks, but use a shared core for its low-load (e.g. management type) tasks."
   "hw:wrs:min_vcpus (Optional)", "plain", "xsd:integer", "Indicates the minimum number of vCPUs for the virtual machine. The value must be between one and the number of VCPUs in the flavor of the virtual machine. If this extra_spec is specified then the server is assumed to support vCPU scaling."
   "extra spec (Optional)", "plain", "xsd:integer", "See Wind River Titanium Cloud Administration Guide for complete list of additional flavor extra specs."

::

   {
     "extra_specs": {
       "sw:wrs:guest:heartbeat": "True",
       "sw:wrs:srv_grp_messaging": "True",
       "sw:wrs:vtpm": "False",
       "hw:numa_node.0": "1",
       "hw:wrs:vcpu:scheduler": "fifo:50:0"
       "hw:wrs:min_vcpus": "2"
       "hw:wrs:shared_vcpu": "1"
       "hw:cpu_model": "Nehalem"
       "aggregate_instance_extra_specs:localstorage": "False"
     }
   }

This operation does not accept a request body.

**************************************
Gets the value of the specified key
**************************************

.. rest_method:: GET /v2/​{tenant_id}​/flavors/​{flavor_id}​/os-extra_specs/​{key_id}​

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "flavor_id", "URI", "String", "The ID of the flavor of interest to you."
   "key_id", "URI", "xsd:string", "The key of the extra-spec of interest to you."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "sw:wrs:guest:heartbeat (Optional)", "plain", "xsd:boolean", "Indicates whether or not the guest applications running in the virtual machine make use of the Titanium Cloud Heartbeat client API."
   "sw:wrs:vtpm (Optional)", "plain", "xsd:boolean", "Indicates whether or not to expose a TPM device to the Guest."
   "hw:wrs:shared_vcpu (Optional)", "plain", "xsd:integer", "Indicates the vCPU of the guest virtual machine that will be scheduled to run on a shared CPU of the host. Note, this can be specified even if hw:cpu_policy is set to dedicated; allowing the guest application to use dedicated cores exclusively for its high-load tasks, but use a shared core for its low-load (e.g. management type) tasks."
   "hw:wrs:min_vcpus (Optional)", "plain", "xsd:integer", "Indicates the minimum number of vCPUs for the virtual machine. The value must be between one and the number of VCPUs in the flavor of the virtual machine. If this extra_spec is specified then the server is assumed to support vCPU scaling."
   "extra spec (Optional)", "plain", "xsd:integer", "See Wind River Titanium Cloud Administration Guide for complete list of additional flavor extra specs."

::

   {
     "sw:wrs:guest:heartbeat": "True",
   }

This operation does not accept a request body.

******************************************************
Creates extra-specs or keys for the specified flavor
******************************************************

.. rest_method:: POST /v2/​{tenant_id}​/flavors/​{flavor_id}​/os-extra_specs

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "flavor_id", "URI", "String", "The ID of the flavor of interest to you."
   "extra_specs (Optional)", "plain", "xsd:list", "The list of flavor extra specs."
   "sw:wrs:guest:heartbeat (Optional)", "plain", "xsd:boolean", "Indicates whether or not the guest applications running in the virtual machine make use of the Titanium Cloud Heartbeat client API."
   "sw:wrs:vtpm (Optional)", "plain", "xsd:boolean", "Indicates whether or not to expose a TPM device to the Guest."
   "hw:wrs:shared_vcpu (Optional)", "plain", "xsd:integer", "Indicates the vCPU of the guest virtual machine that will be scheduled to run on a shared CPU of the host. Note, this can be specified even if hw:cpu_policy is set to dedicated; allowing the guest application to use dedicated cores exclusively for its high-load tasks, but use a shared core for its low-load (e.g. management type) tasks."
   "hw:wrs:min_vcpus (Optional)", "plain", "xsd:integer", "Indicates the minimum number of vCPUs for the virtual machine. The value must be between one and the number of VCPUs in the flavor of the virtual machine. If this extra_spec is specified then the server is assumed to support vCPU scaling."
   "extra spec (Optional)", "plain", "xsd:integer", "See Wind River Titanium Cloud Administration Guide for complete list of additional flavor extra specs."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "sw:wrs:guest:heartbeat (Optional)", "plain", "xsd:boolean", "Indicates whether or not the guest applications running in the virtual machine make use of the Titanium Cloud Heartbeat client API."
   "sw:wrs:vtpm (Optional)", "plain", "xsd:boolean", "Indicates whether or not to expose a TPM device to the Guest."
   "hw:wrs:shared_vcpu (Optional)", "plain", "xsd:integer", "Indicates the vCPU of the guest virtual machine that will be scheduled to run on a shared CPU of the host. Note, this can be specified even if hw:cpu_policy is set to dedicated; allowing the guest application to use dedicated cores exclusively for its high-load tasks, but use a shared core for its low-load (e.g. management type) tasks."
   "hw:wrs:min_vcpus (Optional)", "plain", "xsd:integer", "Indicates the minimum number of vCPUs for the virtual machine. The value must be between one and the number of VCPUs in the flavor of the virtual machine. If this extra_spec is specified then the server is assumed to support vCPU scaling."
   "extra spec (Optional)", "plain", "xsd:integer", "See Wind River Titanium Cloud Administration Guide for complete list of additional flavor extra specs."

::

   {
     "extra_specs": {
       "sw:wrs:guest:heartbeat": "True",
     }
   }

::

   {
     "extra_specs": {
       "sw:wrs:guest:heartbeat": "True",
     }
   }

-----------------
Provider Network
-----------------

The Titanium extensions to the Provider Network entity are:

********************************************
List the provider networks (not supported)
********************************************

.. rest_method:: GET /v2/​{tenant_id}​/wrs-providernet

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."

This operation does not accept a request body and does not return a
response body.

***************************************************
Show the details of a particular provider network
***************************************************

.. rest_method:: GET /v2/​{tenant_id}​/wrs-providernet/​{providernet_id}​

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "providernet_id", "URI", "String", "The ID of the provider network of interest to you."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernet (Optional)", "plain", "xsd:dict", "The requested ``provider network`` object."
   "id (Optional)", "plain", "csapi:UUID", "The ID of the provider network."
   "name (Optional)", "plain", "xsd:string", "The name of the provider network."
   "pci_pfs_configured (Optional)", "plain", "xsd:integer", "The number of configured PCI devices (PFs)."
   "pci_pfs_used (Optional)", "plain", "xsd:integer", "The number of used PCI devices (PFs)."
   "pci_vfs_configured (Optional)", "plain", "xsd:integer", "The number of configured SR-IOV PCI devices (VFs)."
   "pci_vfs_used (Optional)", "plain", "xsd:integer", "The number of used SR-IOV PCI devices (VFs)."

::

   {
      "providernet": {
         "pci_pfs_used": 0,
         "pci_pfs_configured": 0,
         "pci_vfs_used": 1,
         "pci_vfs_configured": 16,
         "id": "21c41131-07fb-43ac-a6f3-8a8020152530",
         "name": "group0-data0"
      }
   }

This operation does not accept a request body.

----
PCI
----

The Titanium extensions to the PCI device entity are:

*******************************************************************************
List PCI device usage statistics. This excludes network interface cards (NICs)
*******************************************************************************

.. rest_method:: GET /v2/​{tenant_id}​/wrs-pci

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "device_name (Optional)", "plain", "xsd:string", "The name of device."
   "device_id (Optional)", "plain", "xsd:string", "The device id of device."
   "vendor_id (Optional)", "plain", "xsd:string", "The vendor id of device."
   "class_id (Optional)", "plain", "xsd:string", "The class id of device."
   "pci_pfs_configured (Optional)", "plain", "xsd:integer", "The number of configured PCI devices (PFs)."
   "pci_pfs_used (Optional)", "plain", "xsd:integer", "The number of used PCI devices (PFs)."
   "pci_vfs_configured (Optional)", "plain", "xsd:integer", "The number of configured SR-IOV PCI devices (VFs)."
   "pci_vfs_used (Optional)", "plain", "xsd:integer", "The number of used SR-IOV PCI devices (VFs)."

::

   {
     "pci_device_usage": [
       {
         "pci_pfs_used": 0,
         "pci_pfs_configured": 0,
         "pci_vfs_used": 1,
         "vendor_id": "8086",
         "pci_vfs_configured": 64,
         "device_name": "Coleto Creek PCIe Co-processor",
         "device_id": "0443",
         "class_id": "0b4000"
       }
     ]
   }

This operation does not accept a request body.

****************************************************
Show the usage details of a particular PCI device
****************************************************

.. rest_method:: GET /v2/​{tenant_id}​/wrs-pci/​{device_id}​

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "xsd:string", "The ID for the tenant or account in a multi-tenancy cloud."
   "device_id", "URI", "String", "The device id of the pci device of interest to you."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "device_name (Optional)", "plain", "xsd:string", "The name of device."
   "device_id (Optional)", "plain", "xsd:string", "The device id of device."
   "vendor_id (Optional)", "plain", "xsd:string", "The vendor id of device."
   "class_id (Optional)", "plain", "xsd:string", "The class id of device."
   "host (Optional)", "plain", "xsd:string", "The name of the compute host."
   "pci_pfs_configured (Optional)", "plain", "xsd:integer", "The number of configured PCI devices (PFs)."
   "pci_pfs_used (Optional)", "plain", "xsd:integer", "The number of used PCI devices (PFs)."
   "pci_vfs_configured (Optional)", "plain", "xsd:integer", "The number of configured SR-IOV PCI devices (VFs)."
   "pci_vfs_used (Optional)", "plain", "xsd:integer", "The number of used SR-IOV PCI devices (VFs)."

::

   {
     "pci_device_usage": [
       {
         "pci_vfs_used": 0,
         "host": "compute-3",
         "pci_pfs_used": 0,
         "pci_pfs_configured": 0,
         "pci_vfs_configured": 0,
         "device_name": "Coleto Creek PCIe Co-processor",
         "vendor_id": "8086",
         "device_id": "0443",
         "class_id": "0b4000"
       },
       {
         "pci_vfs_used": 1,
         "host": "compute-1",
         "pci_pfs_used": 0,
         "pci_pfs_configured": 0,
         "pci_vfs_configured": 32,
         "device_name": "Coleto Creek PCIe Co-processor",
         "vendor_id": "8086",
         "device_id": "0443",
         "class_id": "0b4000"
       },
       {
         "pci_vfs_used": 0,
         "host": "compute-2",
         "pci_pfs_used": 0,
         "pci_pfs_configured": 0,
         "pci_vfs_configured": 0,
         "device_name": "Coleto Creek PCIe Co-processor",
         "vendor_id": "8086",
         "device_id": "0443",
         "class_id": "0b4000"
       },
       {
         "pci_vfs_used": 0,
         "host": "compute-0",
         "pci_pfs_used": 0,
         "pci_pfs_configured": 0,
         "pci_vfs_configured": 32,
         "device_name": "Coleto Creek PCIe Co-processor",
         "vendor_id": "8086",
         "device_id": "0443",
          "class_id": "0b4000"
       }
     ]
   }

This operation does not accept a request body.

------------
Hypervisors
------------

The Titanium extensions to the Hypervisor entity are:

******************************************
Shows details for a specified hypervisor
******************************************

.. rest_method:: GET /v2/os-hypervisors/​{hypervisor_id}​

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "hypervisor_id", "URI", "csapi:UUID", "The ID for the hypervisor of interest to you."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "memory_mb_by_node", "plain", "xsd:string", "lists available memory, in Megabytes, based on page size (4K, 2M, 1G) on each NUMA node. The sum total must match the number reported in ``memory_mb``."
   "memory_mb_used_by_node", "plain", "xsd:string", "lists memory currently in use, in Megabytes, based on page size (4K, 2M, 1G) on each NUMA node. The sum total must match the number reported in ``memory_mb_used``."
   "vcpus_by_node", "plain", "xsd:string", "lists available vcpus, on each NUMA node. The sum total must match the quantity reported by ``vcpus``."
   "vcpus_used_by_node", "plain", "xsd:string", "lists vcpus currently in use, on each NUMA node. The sum total must match the quantity reported by ``vcpus_used``."

::

   {
      "hypervisor":{
         "memory_mb_used_by_node":"{\"0\": {\"2M\": 5120, \"4K\": 0, \"1G\": 0}, \"1\": {\"2M\": 0, \"4K\": 0, \"1G\": 0}}",
         "cpu_info":{
            "arch":"x86_64",
            "model":"IvyBridge",
            "vendor":"Intel",
            "features":[
               "pge",
               "avx",
               "vmx",
               "clflush",
               "sep",
               "syscall",
               "vme",
               "dtes64",
               "tsc",
               "sse",
               "xsave",
               "xsaveopt",
               "erms",
               "xtpr",
               "cmov",
               "smep",
               "ssse3",
               "est",
               "pat",
               "monitor",
               "smx",
               "pcid",
               "lm",
               "msr",
               "nx",
               "fxsr",
               "tm",
               "sse4.1",
               "pae",
               "sse4.2",
               "pclmuldq",
               "acpi",
               "tsc-deadline",
               "popcnt",
               "mmx",
               "osxsave",
               "cx8",
               "mce",
               "de",
               "tm2",
               "ht",
               "dca",
               "pni",
               "pdcm",
               "mca",
               "pdpe1gb",
               "apic",
               "fsgsbase",
               "f16c",
               "pse",
               "ds",
               "invtsc",
               "lahf_lm",
               "aes",
               "sse2",
               "ss",
               "ds_cpl",
               "arat",
               "pbe",
               "fpu",
               "cx16",
               "pse36",
               "mtrr",
               "rdrand",
               "rdtscp",
               "x2apic"
            ],
            "topology":{
               "cores":10,
               "cells":2,
               "threads":2,
               "sockets":1
            }
         },
         "free_disk_gb":208,
         "memory_mb_used":5120,
         "vcpus_by_node":"{\"0\": 14, \"1\": 20}",
         "memory_mb_by_node":"{\"0\": {\"2M\": 21440, \"4K\": 0, \"1G\": 0}, \"1\": {\"2M\": 29088, \"4K\": 0, \"1G\": 0}}",
         "service":{
            "host":"compute-1",
            "disabled_reason":null,
            "id":8
         },
         "local_gb_used":9,
         "id":2,
         "current_workload":0,
         "state":"up",
         "vcpus_used_by_node":"{\"0\": {\"shared\": 0.125, \"dedicated\": 6}, \"1\": {\"shared\": 0.0, \"dedicated\": 0}}",
         "status":"enabled",
         "host_ip":"192.168.205.205",
         "hypervisor_hostname":"compute-1",
         "hypervisor_version":2006000,
         "disk_available_least":208,
         "local_gb":219,
         "free_ram_mb":45408,
         "vcpus_used":6.125,
         "hypervisor_type":"QEMU",
         "memory_mb":50528,
         "vcpus":34,
         "running_vms":7
      }
   }

This operation does not accept a request body.



