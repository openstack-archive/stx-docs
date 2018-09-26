====================================================
Networking API v2 StarlingX extensions
====================================================

This section describes changes made to the standard OpenStack Networking
API for the StarlingX. Some existing OpenStack API instances have
been enhanced to add new attributes, or update the semantics of existing
attributes. In other cases, entirely new API instances have been created
to expose StarlingX networking functionality via the standard
RESTful API.

The typical port used for the Networking REST API is 9696. However,
proper technique would be to look up the neutron service endpoint in
Keystone.

-----------
Extensions
-----------

The Extensions entity lists all available extensions; both open-source
extensions and StarlingX extensions.

*************************************
Lists all extensions - Networking API
*************************************

.. rest_method:: GET /v2.0/extensions

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

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
            "namespace" : "http://docs.windriver.org/tis/ext/wrs-provider/v1",
            "name" : "wrs-provider-network",
            "updated" : "2014-10-01T12:00:00-00:00",
            "description" : "WRS Provider Network Extensions.",
            "alias" : "wrs-provider",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.org/tis/ext/wrs-tenant/v1",
            "name" : "wrs-tenant-settings",
            "updated" : "2014-10-01T12:00:00-00:00",
            "description" : "WRS Tenant Network Settings Extensions.",
            "alias" : "wrs-tenant",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.org/tis/ext/wrs-tm/v1",
            "name" : "wrs-traffic-management",
            "updated" : "2014-10-01T12:00:00-00:00",
            "description" : "WRS Traffic Management Extensions.",
            "alias" : "wrs-tm",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.org/tis/ext/wrs-net/v1",
            "name" : "wrs-tenant-network",
            "updated" : "2014-10-01T12:00:00-00:00",
            "description" : "WRS Tenant Network Extensions.",
            "alias" : "wrs-net",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.org/tis/ext/wrs-binding/v1",
            "name" : "wrs-port-binding",
            "updated" : "2014-10-01T12:00:00-00:00",
            "description" : "WRS Port Binding Extensions.",
            "alias" : "wrs-binding",
            "links" : []
         },
         ...
      ]
   }

This operation does not accept a request body.

*************************************************************
Gets information about a specified extension - Networking API
*************************************************************

.. rest_method:: GET /v2.0/extensions/​{extension_alias}​

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
      "extensions" : {
         "namespace" : "http://docs.windriver.org/tis/ext/wrs-provider/v1",
         "name" : "wrs-provider-network",
         "updated" : "2014-10-01T12:00:00-00:00",
         "description" : "WRS Provider Network Extensions.",
         "alias" : "wrs-provider",
         "links" : []
      }
   }

   OR

   {
      "extensions" : {
         "namespace" : "http://docs.windriver.org/tis/ext/wrs-tenant/v1",
         "name" : "wrs-tenant-settings",
         "updated" : "2014-10-01T12:00:00-00:00",
         "description" : "WRS Tenant Network Settings Extensions.",
         "alias" : "wrs-tenant",
         "links" : []
      }
   }

   OR

   {
      "extensions" : {
         "namespace" : "http://docs.windriver.org/tis/ext/wrs-tm/v1",
         "name" : "wrs-traffic-management",
         "updated" : "2014-10-01T12:00:00-00:00",
         "description" : "WRS Traffic Management Extensions.",
         "alias" : "wrs-tm",
         "links" : []
      }
   }

   OR

   {
      "extensions" : {
         "namespace" : "http://docs.windriver.org/tis/ext/wrs-net/v1",
         "name" : "wrs-tenant-network",
         "updated" : "2014-10-01T12:00:00-00:00",
         "description" : "WRS Tenant Network Extensions.",
         "alias" : "wrs-net",
         "links" : []
      }
   }

   OR

   {
      "extensions" : {
         "namespace" : "http://docs.windriver.org/tis/ext/wrs-binding/v1",
         "name" : "wrs-port-binding",
         "updated" : "2014-10-01T12:00:00-00:00",
         "description" : "WRS Port Binding Extensions.",
         "alias" : "wrs-binding",
         "links" : []
      }
   }

This operation does not accept a request body.

-----------------
Provider Network
-----------------

The Provider Network entity is a new entity which was added to the
OpenStack API. It enables management of provider networks via the
RESTful API. The standard OpenStack API included no such entity;
instead, the end user was required to edit static configuration files
through the system to add, or update provider network information.

This entity and all of its operations are only available to
administrator level users.

*****************************
Lists all provider networks
*****************************

.. rest_method:: GET /v2.0/wrs-provider/providernets

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernets (Optional)", "plain", "xsd:list", "The list of provider networks."
   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network."
   "mtu (Optional)", "plain", "xsd:integer", "The maximum transmit unit (MTU) assigned to the provider network. Must be between 576 and 9216 bytes inclusively. The default value is 1500."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network."
   "ranges (Optional)", "plain", "xsd:list", "The list of segmentation ranges defined for this provider network. See the provider network range description for a description of range fields."
   "status (Optional)", "plain", "xsd:string", "The current status of the provider network. Returns ``ACTIVE`` if at least one compute node has a data interface associated to this provider network and is available. "

::

   {
       "providernets": [
           {
               "description": "Group0 provider networks for data1 interfaces",
               "id": "b67d40aa-3651-4dd6-886f-4bff5caa266e",
               "mtu": 1500,
               "name": "group0-data1",
               "ranges": [
                   {
                       "description": "tenant2 reserved networks",
                       "id": "a8184bf7-b683-4bc1-a70c-dae34391344c",
                       "maximum": 631,
                       "minimum": 616,
                       "name": "group0-tenant2",
                       "shared": false,
                       "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
                   }
               ],
               "status": "ACTIVE",
               "type": "vlan",
               "vlan_transparent": false,
           },
           {
               "description": "Group0 provider networks for data0 interfaces",
               "id": "c496c429-cb52-4d4b-9171-b4b31fa91a80",
               "mtu": 1500,
               "name": "group0-data0",
               "ranges": [
                   {
                       "description": "Shared internal networks",
                       "id": "f3e1bc29-29f7-4ee0-a78d-9c3d7a0f53e5",
                       "maximum": 731,
                       "minimum": 700,
                       "name": "group0-shared",
                       "shared": true,
                       "tenant_id": null
                   },
                   {
                       "description": "External network access",
                       "id": "35ef3460-700a-48a3-8df9-145eb68fcd31",
                       "maximum": 10,
                       "minimum": 10,
                       "name": "group0-external",
                       "shared": true,
                       "tenant_id": null
                   },
                   {
                       "description": "tenant1 reserved networks",
                       "id": "736b0c0d-945b-4a17-9fe4-cf02a5327132",
                       "maximum": 615,
                       "minimum": 600,
                       "name": "group0-tenant1",
                       "shared": false,
                       "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
                   }
               ],
               "status": "ACTIVE",
               "type": "vlan",
               "vlan_transparent": false,
           }
       ]
   }

This operation does not accept a request body.

**************************************************************
Shows detailed information about a specific provider network
**************************************************************

.. rest_method:: GET /v2.0/wrs-provider/providernets/​{providernet_id}​

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

   "providernet_id", "URI", "csapi:UUID", "The ID for a provider network."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network."
   "mtu (Optional)", "plain", "xsd:integer", "The maximum transmit unit (MTU) assigned to the provider network. Must be between 576 and 9216 bytes inclusively. The default value is 1500."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network."
   "ranges (Optional)", "plain", "xsd:list", "The list of segmentation ranges defined for this provider network. See the provider network range description for a description of range fields."
   "status (Optional)", "plain", "xsd:string", "The current status of the provider network. Returns ``ACTIVE`` if at least one compute node has a data interface associated to this provider network and is available. "

::

   {
       "providernet": {
           "description": "Group0 provider networks for data1 interfaces",
           "id": "b67d40aa-3651-4dd6-886f-4bff5caa266e",
           "mtu": 1500,
           "name": "group0-data1",
           "ranges": [
               {
                   "description": "tenant2 reserved networks",
                   "id": "a8184bf7-b683-4bc1-a70c-dae34391344c",
                   "maximum": 631,
                   "minimum": 616,
                   "name": "group0-tenant2",
                   "shared": false,
                   "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
               }
           ],
           "status": "ACTIVE",
           "type": "vlan",
           "vlan_transparent": false,
       }
   }

This operation does not accept a request body.

****************************
Creates a provider network
****************************

.. rest_method:: POST /v2.0/wrs-provider/providernets

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network."
   "type (Optional)", "plain", "xsd:string", "The encapsulation type of the provider network. Valid values are: ``vlan``, ``flat``"
   "vlan_transparent (Optional)", "plain", "xsd:bool", "Specifies whether VLAN transparent network are supported on this provider network."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network."
   "mtu (Optional)", "plain", "xsd:integer", "The maximum transmit unit (MTU) assigned to the provider network. Must be between 576 and 9216 bytes inclusively. The default value is 1500."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network."
   "ranges (Optional)", "plain", "xsd:list", "The list of segmentation ranges defined for this provider network. See the provider network range description for a description of range fields."
   "status (Optional)", "plain", "xsd:string", "The current status of the provider network. Returns ``ACTIVE`` if at least one compute node has a data interface associated to this provider network and is available. "

::

   {
       "providernet": {
           "description": "A sample provider network",
           "name": "test",
           "type": "vlan",
           "vlan_transparent": false,
       }
   }

::

   {
       "providernet": {
           "description": "A sample provider network",
           "id": "4da9e42c-e556-470c-8e92-cbd19bcc6a10",
           "mtu": 1500,
           "name": "test",
           "ranges": [],
           "status": "DOWN",
           "type": "vlan",
           "vlan_transparent": false,
       }
   }

**************************************
Modifies a specific provider network
**************************************

.. rest_method:: PUT /v2.0/wrs-provider/providernets/​{providernet_id}​

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernet_id", "URI", "csapi:UUID", "The ID for a provider network."
   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network."
   "vlan_transparent (Optional)", "plain", "xsd:bool", "Specifies whether VLAN transparent network are supported on this provider network."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network."
   "mtu (Optional)", "plain", "xsd:integer", "The maximum transmit unit (MTU) assigned to the provider network. Must be between 576 and 9216 bytes inclusively. The default value is 1500."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network."
   "ranges (Optional)", "plain", "xsd:list", "The list of segmentation ranges defined for this provider network. See the provider network range description for a description of range fields."
   "status (Optional)", "plain", "xsd:string", "The current status of the provider network. Returns ``ACTIVE`` if at least one compute node has a data interface associated to this provider network and is available. "

::

   {
       "providernet": {
           "description": "Another sample provider network"
       }
   }

::

   {
       "providernet": {
           "description": "Another sample provider network",
           "id": "4da9e42c-e556-470c-8e92-cbd19bcc6a10",
           "mtu": 1500,
           "name": "test",
           "ranges": [],
           "status": "DOWN",
           "type": "vlan",
           "vlan_transparent": false,
       }
   }

*************************************
Deletes a specific provider network
*************************************

.. rest_method:: DELETE /v2.0/wrs-provider/providernets/​{providernet_id}​

**Normal response codes**

204

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernet_id", "URI", "csapi:UUID", "The ID for a provider network."

This operation does not accept a request body.

*****************************************************************************************************************************************
Lists networks that are implemented by a given provider network. Each network is listed with its assigned provider network segmentation
*****************************************************************************************************************************************

.. rest_method:: GET /v2.0/wrs-provider/providernets/​{providernet_id}​/providernet-bindings

identifier. If the network has any tagged subnets then they will be
listed as separate entities with their corresponding provider network
segmentation identifier.

**Normal response codes**

200

**Error response codes**

itemNotFound (401)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernet_id", "URI", "csapi:UUID", "The ID for a provider network."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "networks (Optional)", "plain", "xsd:list", "The list of tenant networks."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the tenant network."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the tenant network."
   "providernet_type (Optional)", "plain", "xsd:string", "The encapsulation type of the provider network."
   "segmentation_id (Optional)", "plain", "xsd:integer", "The provider network segmentation identifier that is assigned to this tenant network. If the ``vlan_id`` attribute is non-zero then the ``segmentation_id`` represents that identifier which has been associated to a tagged subnet on the listed tenant network."
   "vlan_id (Optional)", "plain", "xsd:integer", "The VLAN identifier which has been configured on the tenant subnet."

::

   {
       "networks": [
           {
               "id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
               "name": "tenant1-net0",
               "providernet_type": "vlan",
               "segmentation_id": 601,
               "vlan_id": 0
           },
           {
               "id": "7e5ed852-a990-4fc5-89a2-b17093ca1982",
               "name": "internal0-net0",
               "providernet_type": "vlan",
               "segmentation_id": 700,
               "vlan_id": 0
           },
           {
               "id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "name": "external-net0",
               "providernet_type": "vlan",
               "segmentation_id": 10,
               "vlan_id": 0
           },
           {
               "id": "f652780a-7a9d-4667-8df4-5c8632728be9",
               "name": "tenant1-mgmt-net",
               "providernet_type": "vlan",
               "segmentation_id": 600,
               "vlan_id": 0
           }
       ]
   }

This operation does not accept a request body.

-----------------------
Provider Network Range
-----------------------

The Provider Network Range entity is a new entity which was added to the
OpenStack API. It enables management of provider network segmentation
ranges via the RESTful API. The standard OpenStack API included no such
entity; instead, the end user was required to edit static configuration
files through the system to add, or update provider network segmentation
ranges.

This entity and all of its operations are only available to
administrator level users.

***********************************
Lists all provider network ranges
***********************************

.. rest_method:: GET /v2.0/wrs-provider/providernet-ranges

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernetranges (Optional)", "plain", "xsd:list", "The list of provider network ranges."
   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network segmentation range."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network segmentation range."
   "maximum (Optional)", "plain", "xsd:integer", "The upper bound of the segmentation range (inclusive)."
   "minimum (Optional)", "plain", "xsd:integer", "The lower bound of the segmentation range (inclusive)."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network segmentation range."
   "providernet_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the parent provider network."
   "providernet_name (Optional)", "plain", "xsd:string", "The user defined name of the parent provider network."
   "shared (Optional)", "plain", "xsd:bool", "The shared attribute indicates that the range is available to any tenant."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the range. Only valid if the shared attribute is False."

::

   {
       "providernet_ranges": [
           {
               "description": "Shared internal networks",
               "id": "f3e1bc29-29f7-4ee0-a78d-9c3d7a0f53e5",
               "maximum": 731,
               "minimum": 700,
               "name": "group0-shared",
               "providernet_id": "c496c429-cb52-4d4b-9171-b4b31fa91a80",
               "providernet_name": "group0-data0",
               "shared": true,
               "tenant_id": null
           },
           {
               "description": "External network access",
               "id": "35ef3460-700a-48a3-8df9-145eb68fcd31",
               "maximum": 10,
               "minimum": 10,
               "name": "group0-external",
               "providernet_id": "c496c429-cb52-4d4b-9171-b4b31fa91a80",
               "providernet_name": "group0-data0",
               "shared": true,
               "tenant_id": null
           },
           {
               "description": "tenant1 reserved networks",
               "id": "736b0c0d-945b-4a17-9fe4-cf02a5327132",
               "maximum": 615,
               "minimum": 600,
               "name": "group0-tenant1",
               "providernet_id": "c496c429-cb52-4d4b-9171-b4b31fa91a80",
               "providernet_name": "group0-data0",
               "shared": false,
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "description": "tenant2 reserved networks",
               "id": "a8184bf7-b683-4bc1-a70c-dae34391344c",
               "maximum": 631,
               "minimum": 616,
               "name": "group0-tenant2",
               "providernet_id": "b67d40aa-3651-4dd6-886f-4bff5caa266e",
               "providernet_name": "group0-data1",
               "shared": false,
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           },
           {
               "description": "A sample provider network segmentation range",
               "id": "bdf07406-a867-42e5-9533-5100c4a3f2ba",
               "maximum": 100,
               "minimum": 1,
               "name": "test-range-0",
               "providernet_id": "239ffb19-bad8-4b05-9194-aa8399816a36",
               "providernet_name": "test",
               "shared": true,
               "tenant_id": null
           }
       ]
   }

This operation does not accept a request body.

*********************************************************************
Shows detailed information about a specific provider network range
*********************************************************************

.. rest_method:: GET /v2.0/wrs-provider/providernet-ranges/​{providernet-range_id}​

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

   "providernetrange_id", "URI", "csapi:UUID", "The ID for a provider network segmentation range."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network segmentation range."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network segmentation range."
   "maximum (Optional)", "plain", "xsd:integer", "The upper bound of the segmentation range (inclusive)."
   "minimum (Optional)", "plain", "xsd:integer", "The lower bound of the segmentation range (inclusive)."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network segmentation range."
   "providernet_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the parent provider network."
   "providernet_name (Optional)", "plain", "xsd:string", "The user defined name of the parent provider network."
   "shared (Optional)", "plain", "xsd:bool", "The shared attribute indicates that the range is available to any tenant."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the range. Only valid if the shared attribute is False."

::

   {
       "providernet_range": {
           "description": "A sample provider network segmentation range",
           "id": "bdf07406-a867-42e5-9533-5100c4a3f2ba",
           "maximum": 100,
           "minimum": 1,
           "name": "test-range-0",
           "providernet_id": "239ffb19-bad8-4b05-9194-aa8399816a36",
           "providernet_name": "test",
           "shared": true,
           "tenant_id": null
       }
   }

This operation does not accept a request body.

**********************************
Creates a provider network range
**********************************

.. rest_method:: POST /v2.0/wrs-provider/providernet-ranges

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network segmentation range."
   "maximum (Optional)", "plain", "xsd:integer", "The upper bound of the segmentation range (inclusive)."
   "minimum (Optional)", "plain", "xsd:integer", "The lower bound of the segmentation range (inclusive)."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network segmentation range."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network segmentation range."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network segmentation range."
   "maximum (Optional)", "plain", "xsd:integer", "The upper bound of the segmentation range (inclusive)."
   "minimum (Optional)", "plain", "xsd:integer", "The lower bound of the segmentation range (inclusive)."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network segmentation range."
   "providernet_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the parent provider network."
   "providernet_name (Optional)", "plain", "xsd:string", "The user defined name of the parent provider network."
   "shared (Optional)", "plain", "xsd:bool", "The shared attribute indicates that the range is available to any tenant."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the range. Only valid if the shared attribute is False."

::

   {
       "providernet_range": {
           "description": "A sample provider network segmentation range",
           "maximum": "100",
           "minimum": "1",
           "name": "test-range-0",
           "providernet_id": "239ffb19-bad8-4b05-9194-aa8399816a36",
           "shared": true
       }
   }

::

   {
       "providernet_range": {
           "description": "A sample provider network segmentation range",
           "id": "bdf07406-a867-42e5-9533-5100c4a3f2ba",
           "maximum": "100",
           "minimum": "1",
           "name": "test-range-0",
           "providernet_id": "239ffb19-bad8-4b05-9194-aa8399816a36",
           "providernet_name": "test",
           "shared": true,
           "tenant_id": null
       }
   }

*********************************************
Modifies a specific provider network range
*********************************************

.. rest_method:: PUT /v2.0/wrs-provider/providernet-ranges/​{providernet-range_id}​

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernetrange_id", "URI", "csapi:UUID", "The ID for a provider network segmentation range."
   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network segmentation range."
   "maximum (Optional)", "plain", "xsd:integer", "The upper bound of the segmentation range (inclusive)."
   "minimum (Optional)", "plain", "xsd:integer", "The lower bound of the segmentation range (inclusive)."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "User defined description of the provider network segmentation range."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the provider network segmentation range."
   "maximum (Optional)", "plain", "xsd:integer", "The upper bound of the segmentation range (inclusive)."
   "minimum (Optional)", "plain", "xsd:integer", "The lower bound of the segmentation range (inclusive)."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the provider network segmentation range."
   "providernet_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the parent provider network."
   "providernet_name (Optional)", "plain", "xsd:string", "The user defined name of the parent provider network."
   "shared (Optional)", "plain", "xsd:bool", "The shared attribute indicates that the range is available to any tenant."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the range. Only valid if the shared attribute is False."

::

   {
       "providernet_range": {
           "maximum": "1099",
           "minimum": "1000",
           "description": "VLAN identifiers reserved for tenant1"
       }
   }

::

   {
       "providernet_range": {
           "description": null,
           "id": "fe24481a-303f-4cd9-a0ac-76c2e4a9bcc8",
           "maximum": "1099",
           "minimum": "1000",
           "name": "test-range-0",
           "providernet_id": "c496c429-cb52-4d4b-9171-b4b31fa91a80",
           "providernet_name": "group0-data0",
           "shared": false,
           "tenant_id": "206f147dcf72421fa6829e33bfb34637"
       }
   }

********************************************
Deletes a specific provider network range
********************************************

.. rest_method:: DELETE /v2.0/wrs-provider/providernet-ranges/​{providernet-range_id}​

**Normal response codes**

204

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernetrange_id", "URI", "csapi:UUID", "The ID for a provider network segmentation range."

This operation does not accept a request body.

----------------------
Provider Network Type
----------------------

The Provider Network Type entity is a new entity which was added to the
OpenStack API. It exists simply to allow the end user to query which
provider network types are supported by the system.

This entity and all of its operations are only available to
administrator level users.

***************************************
Lists all supported providernet types
***************************************

.. rest_method:: GET /v2.0/wrs-provider/providernet-types

Insert extra description here, if required.

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernettypes (Optional)", "plain", "xsd:list", "The list of supported providernet types."
   "description (Optional)", "plain", "xsd:string", "System description of the provider network type."
   "type (Optional)", "plain", "xsd:string", "The encapsulation type of the provider network. Valid values are: ``vlan``, ``flat``"

::

   {
       "providernet_types": [
           {
               "description": "Ethernet network without additional encapsulation",
               "type": "flat"
           },
           {
               "description": "802.1q encapsulated Ethernet network",
               "type": "vlan"
           }
       ]
   }

This operation does not accept a request body.

-----------------------------------
Provider Network Connectivity Test
-----------------------------------

The Provider Network Connectivity Test entity is a new entity which was
added to the OpenStack API. It enables the verification of provider
network connectivity between compute nodes.

This entity and all of its operations are only available to
administrator level users.

*************************************************
Lists results of providernet connectivity tests
*************************************************

.. rest_method:: GET /v2.0/wrs-provider/providernet-connectivity-tests

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernet_connectivity_tests (Optional)", "plain", "xsd:list", "List of providernet connectivity test results."

::

   {
      "providernet_connectivity_tests":[
         {
            "status":"PASS",
            "segmentation_id":"10",
            "updated_at":"2016-04-12 17:11:34.515416",
            "host_name":"compute-1",
            "providernet_id":"fc210630-7bb5-4ad2-a7e3-a4b752a8377b",
            "host_id":"da6e8822-49ed-43f7-a5e4-90db837ffb2e",
            "providernet_name":"physnet0",
            "audit_uuid":"c3278c0b-660b-4152-a756-4eab241c1627",
            "type":"vlan",
            "message":""
         },
         {
            "status":"PASS",
            "segmentation_id":"10",
            "updated_at":"2016-04-12 17:11:34.511279",
            "host_name":"compute-0",
            "providernet_id":"fc210630-7bb5-4ad2-a7e3-a4b752a8377b",
            "host_id":"3c349ef5-d5f1-4bb1-9742-3538b6e9a352",
            "providernet_name":"physnet0",
            "audit_uuid":"c3278c0b-660b-4152-a756-4eab241c1627",
            "type":"vlan",
            "message":""
         }
      ]
   }

This operation does not accept a request body.

*****************************************************************************
Schedule providernet connectivity test to be run, and return scheduled UUID
*****************************************************************************

.. rest_method:: POST /v2.0/wrs-provider/providernet-connectivity-tests

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "providernet_name (Optional)", "plain", "xsd:string", "Run audit for a given providernet identified by name."
   "providernet_id (Optional)", "plain", "xsd:string", "Run audit for a given providernet identified by ID."
   "host_name (Optional)", "plain", "xsd:string", "Run audit for all providernets on a given host identified by name."
   "host_id (Optional)", "plain", "xsd:string", "Run audit for all providernets on a given host identified by ID."
   "segmentation_id (Optional)", "plain", "xsd:string", "Restrict audit to these segmentation IDs."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "audit_uuid (Optional)", "plain", "xsd:string", "Unique ID assigned to the audit."

::

   {
      "providernet_connectivity_test":{
         "segmentation_id":null,
         "host_name":null,
         "providernet_id":null
      }
   }

::

   {
      "providernet_connectivity_test":{
         "audit_uuid":"12a9f9c2-ca3b-4b56-b463-5755022c8d16"
      }
   }

----------------
Tenant Settings
----------------

The Tenant Settings entity is a new entity which was added to the
OpenStack API. It enables management of features or system behaviours on
a per-tenant basis by the administrator.

This entity and all of its operations are only available to
administrator level users.

***********************************
Lists all tenant network settings
***********************************

.. rest_method:: GET /v2.0/wrs-tenant/settings

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "settings (Optional)", "plain", "xsd:list", "The list of tenant network settings."
   "mac_filtering (Optional)", "plain", "xsd:bool", "The state of the source MAC filtering feature for the specified tenant. The current state of the feature only affects newly launched VM instances."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant."

::

   {
       "settings": [
           {
               "mac_filtering": false,
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "mac_filtering": false,
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           }
       ]
   }

This operation does not accept a request body.

********************************************************************
Shows detailed information about a specific tenant network setting
********************************************************************

.. rest_method:: GET /v2.0/wrs-tenant/settings/​{tenant_id}​

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

   "tenant_id", "URI", "csapi:UUID", "The ID for a tenant."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "mac_filtering (Optional)", "plain", "xsd:bool", "The state of the source MAC filtering feature for the specified tenant. The current state of the feature only affects newly launched VM instances."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant."

::

   {
       "setting": {
           "mac_filtering": false
       }
   }

This operation does not accept a request body.

********************************************
Modifies a specific tenant network setting
********************************************

.. rest_method:: PUT /v2.0/wrs-tenant/settings/​{tenant_id}​

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for a tenant."
   "mac_filtering (Optional)", "plain", "xsd:bool", "The state of the source MAC filtering feature for the specified tenant. The current state of the feature only affects newly launched VM instances."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "mac_filtering (Optional)", "plain", "xsd:bool", "The state of the source MAC filtering feature for the specified tenant. The current state of the feature only affects newly launched VM instances."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant."

::

   {
       "setting": {
           "mac_filtering": true
       }
   }

::

   {
       "setting": {
           "mac_filtering": true
       }
   }

*******************************************
Deletes a specific tenant network setting
*******************************************

.. rest_method:: DELETE /v2.0/wrs-tenant/settings/​{tenant_id}​

**Normal response codes**

204

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for a tenant."

This operation does not accept a request body.

-------------
QOS Policies
-------------

The QOS entity is a new entity which was added to the OpenStack API. It
enables management of Quality of Service policies and profiles via the
RESTful API. QOS policies can be created and maintained by the
administrator.

************************
Lists all QOS policies
************************

.. rest_method:: GET /v2.0/wrs-tm/qoses

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "Qoses (Optional)", "plain", "xsd:list", "The list of QOS policies."
   "description (Optional)", "plain", "xsd:string", "The user defined description of the QoS policy."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the QoS policy."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the QoS policy."
   "policies (Optional)", "plain", "xsd:dict", "The set of scheduler policies and weights for the QoS policy."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant to which this policy is assigned."

::

   {
       "qoses": [
           {
               "description": "tenant1 Management Network Policy",
               "id": "102c64e4-ad26-4610-ae39-f59e15fcb80c",
               "name": "tenant1-mgmt-qos",
               "policies": {
                   "scheduler": {
                       "weight": "8"
                   }
               },
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "description": "tenant2 Management Network Policy",
               "id": "62970a9a-b093-4747-92dd-9de25616036a",
               "name": "tenant2-mgmt-qos",
               "policies": {
                   "scheduler": {
                       "weight": "8"
                   }
               },
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           },
           {
               "description": "External Network Policy",
               "id": "d28e697c-c290-4895-b57c-ac7d38db9003",
               "name": "external-qos",
               "policies": {
                   "scheduler": {
                       "weight": "16"
                   }
               },
               "tenant_id": "206f147dcf72421fa6829e33bfb34637"
           }
       ]
   }

This operation does not accept a request body.

********************************************************
Shows detailed information about a specific QOS policy
********************************************************

.. rest_method:: GET /v2.0/wrs-tm/qoses/​{qos_id}​

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

   "qos_id", "URI", "csapi:UUID", "The ID for a QOS Policy."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "The user defined description of the QoS policy."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the QoS policy."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the QoS policy."
   "policies (Optional)", "plain", "xsd:dict", "The set of scheduler policies and weights for the QoS policy."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant to which this policy is assigned."

::

   {
       "qos": {
           "description": "tenant1 Management Network Policy",
           "id": "102c64e4-ad26-4610-ae39-f59e15fcb80c",
           "name": "tenant1-mgmt-qos",
           "policies": {
               "scheduler": {
                   "weight": "8"
               }
           },
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
       }
   }

This operation does not accept a request body.

**********************
Creates a QOS policy
**********************

.. rest_method:: POST /v2.0/wrs-tm/qoses

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "The user defined description of the QoS policy."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the QoS policy."
   "policies (Optional)", "plain", "xsd:dict", "The set of scheduler policies and weights for the QoS policy."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant to which this policy is assigned."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "The user defined description of the QoS policy."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the QoS policy."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the QoS policy."
   "policies (Optional)", "plain", "xsd:dict", "The set of scheduler policies and weights for the QoS policy."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant to which this policy is assigned."

::

   {
       "qos": {
           "description": "A sample QoS profile",
           "name": "test-qos-0",
           "policies": {
               "scheduler": {
                   "weight": "8"
               }
           },
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
       }
   }

::

   {
       "qos": {
           "description": "A sample QoS profile",
           "id": "5e1841fa-c106-47ee-a736-e527478f1239",
           "name": "test-qos-0",
           "policies": {
               "scheduler": {
                   "weight": "8"
               }
           },
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
       }
   }

********************************
Modifies a specific QOS policy
********************************

.. rest_method:: PUT /v2.0/wrs-tm/qoses/​{qos_id}​

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "qos_id", "URI", "csapi:UUID", "The ID for a QOS Policy."
   "description (Optional)", "plain", "xsd:string", "The user defined description of the QoS policy."
   "policies (Optional)", "plain", "xsd:dict", "The set of scheduler policies and weights for the QoS policy."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "description (Optional)", "plain", "xsd:string", "The user defined description of the QoS policy."
   "id (Optional)", "plain", "csapi:UUID", "The unique UUID value of the QoS policy."
   "name (Optional)", "plain", "xsd:string", "The user defined name of the QoS policy."
   "policies (Optional)", "plain", "xsd:dict", "The set of scheduler policies and weights for the QoS policy."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant to which this policy is assigned."

::

   {
       "qos": {
           "description": "Another sample QoS profile",
           "policies": {
               "scheduler": {
                   "weight": "16"
               }
           }
       }
   }

::

   {
       "qos": {
           "description": "Another sample QoS profile",
           "id": "5e1841fa-c106-47ee-a736-e527478f1239",
           "name": "test-qos-0",
           "policies": {
               "scheduler": {
                   "weight": "16"
               }
           },
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
       }
   }

*******************************
Deletes a specific QOS policy
*******************************

.. rest_method:: DELETE /v2.0/wrs-tm/qoses/​{qos_id}​

**Normal response codes**

204

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "qos_id", "URI", "csapi:UUID", "The ID for a QOS Policy."

This operation does not accept a request body.

--------
Network
--------

The Network entity is an existing OpenStack API. It has been extended to
add the following StarlingX functionality.

-  A QOS policy can optionally be associated to a tenant network

-  The maximum transmit unit (MTU) of each tenant network is inherited
   from its associated provider network

-  The status of each tenant network is derived from the state of the
   DHCP server which services its subnets

***************************************************************************
Lists networks that are accessible to the tenant who submits the reequest
***************************************************************************

.. rest_method:: GET /v2.0/networks

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "networks (Optional)", "plain", "xsd:list", "The list of tenant networks."
   "wrs-tm:qos (Optional)", "plain", "csapi:UUID", "The unique UUID of the assigned QoS policy."
   "status (Optional)", "plain", "xsd:string", "Indicates whether the tenant network is ``ACTIVE`` or ``DOWN``. If the network is DHCP enabled then it can only be active if at least 1 DHCP agent is servicing the network. StarlingX corrected the reporting of this status."

::

   {
       "networks": [
           {
               "admin_state_up": true,
               "id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "name": "external-net0",
               "mtu": 1500,
               "provider:network_type": "vlan",
               "provider:physical_network": "group0-data0",
               "provider:segmentation_id": 10,
               "wrs-tm:qos": "d28e697c-c290-4895-b57c-ac7d38db9003",
               "router:external": true,
               "shared": true,
               "status": "ACTIVE",
               "subnets": [
                   "b282ef86-2584-4a02-9b58-69d6233952a2"
               ],
               "tenant_id": "206f147dcf72421fa6829e33bfb34637"
           },
           {
               "admin_state_up": true,
               "id": "f652780a-7a9d-4667-8df4-5c8632728be9",
               "name": "tenant1-mgmt-net",
               "mtu": 1500,
               "provider:network_type": "vlan",
               "provider:physical_network": "group0-data0",
               "provider:segmentation_id": 600,
               "wrs-tm:qos": "102c64e4-ad26-4610-ae39-f59e15fcb80c",
               "router:external": false,
               "shared": false,
               "status": "ACTIVE",
               "subnets": [
                   "34efd537-7a72-4fcd-b837-9874caf34117"
               ],
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "id": "9472a8ab-9205-43ef-a460-5f01f031791a",
               "name": "tenant2-mgmt-net",
               "mtu": 1500,
               "provider:network_type": "vlan",
               "provider:physical_network": "group0-data1",
               "provider:segmentation_id": 616,
               "wrs-tm:qos": "62970a9a-b093-4747-92dd-9de25616036a",
               "router:external": false,
               "shared": false,
               "status": "ACTIVE",
               "subnets": [
                   "9aa900f4-522b-4b83-ba93-57f7d92da5d5"
               ],
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           },
           {
               "admin_state_up": true,
               "id": "7e5ed852-a990-4fc5-89a2-b17093ca1982",
               "name": "internal0-net0",
               "mtu": 1500,
               "provider:network_type": "vlan",
               "provider:physical_network": "group0-data0",
               "provider:segmentation_id": 700,
               "router:external": false,
               "shared": true,
               "status": "ACTIVE",
               "subnets": [
                   "ad791a3e-33cf-4d8d-b80f-91c87f97745e"
               ],
               "tenant_id": "206f147dcf72421fa6829e33bfb34637",
               "vlan_transparent": false,
           },
           {
               "admin_state_up": true,
               "id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
               "name": "tenant1-net0",
               "mtu": 1500,
               "provider:network_type": "vlan",
               "provider:physical_network": "group0-data0",
               "provider:segmentation_id": 601,
               "router:external": false,
               "shared": false,
               "status": "ACTIVE",
               "subnets": [
                   "bc269028-1862-4dde-ba2e-62a67d1af4e4",
                   "837aebc9-6c78-43e9-8124-168ba16adbc7"
               ],
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
               "vlan_transparent": false,
           },
           {
               "admin_state_up": true,
               "id": "7a77e654-794a-4e19-9679-ac2733e19876",
               "name": "tenant2-net0",
               "mtu": 1500,
               "provider:network_type": "vlan",
               "provider:physical_network": "group0-data1",
               "provider:segmentation_id": 617,
               "router:external": false,
               "shared": false,
               "status": "ACTIVE",
               "subnets": [
                   "985806f5-9fd7-4d47-9da6-cb0c1316e63d"
               ],
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
               "vlan_transparent": false,
           }
       ]
   }

This operation does not accept a request body.

*******************************************
Shows information for a specified network
*******************************************

.. rest_method:: GET /v2.0/networks/​{network_id}​

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401), unauthorized (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "network_id", "URI", "csapi:UUID", "The UUID for a network."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-tm:qos (Optional)", "plain", "csapi:UUID", "The unique UUID of the assigned QoS policy."
   "status (Optional)", "plain", "xsd:string", "Indicates whether the tenant network is ``ACTIVE`` or ``DOWN``. If the network is DHCP enabled then it can only be active if at least 1 DHCP agent is servicing the network. StarlingX corrected the reporting of this status."

::

   {
       "network": {
           "admin_state_up": true,
           "id": "e87e7438-8a07-4e82-a472-862bb7fa93ac",
           "name": "test-net-0",
           "mtu": 1500,
           "provider:network_type": "vlan",
           "provider:physical_network": "group0-data0",
           "provider:segmentation_id": 602,
           "wrs-tm:qos": "102c64e4-ad26-4610-ae39-f59e15fcb80c",
           "router:external": false,
           "shared": false,
           "status": "ACTIVE",
           "subnets": [],
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10",
           "vlan_transparent": false,
       }
   }

This operation does not accept a request body.

-------
Subnet
-------

The Subnet entity is an existing OpenStack API. It has been extended to
add the following StarlingX functionality.

-  A subnet can be configured to allow VLAN tagging by the VM instance.

*************************************************************************
Lists subnets that are accessible to the tenant who submits the request
*************************************************************************

.. rest_method:: GET /v2.0/subnets

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "subnets (Optional)", "plain", "xsd:list", "The list of subnets."
   "wrs-net:managed (Optional)", "plain", "xsd:bool", "Indicates whether IP address allocation is managed by the system or by the customer. If ``true`` then the system allocates IP addresses when ports are created and attached to VM instances. If ``false`` then the system will not assign any IP addresses automatically. This implies that if the system cannot allocate any IP addresses that it also cannot allocate a DHCP server, manage allocation pools, or server DNS nameservers or static routers."
   "wrs-provider:network_type (Optional)", "plain", "xsd:string", "The type of the provider network to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-provider:physical_name (Optional)", "plain", "xsd:string", "The name of the provider network to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-provider:segmentation_id (Optional)", "plain", "xsd:integer", "The provider network segmentation id to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-net:vlan_id (Optional)", "plain", "xsd:integer", "The VLAN ID to be used in the VM instance. If the VLAN ID is 0 then all packets originated from the VM instance are expected to be untagged. If the VLAN ID value is non zero than it is expected that all packets originated by the VM must be tagged with the corresponding VLAN ID value. Any other value will be discarded by the host vswitch."

::

   {
       "subnets": [
           {
               "allocation_pools": [
                   {
                       "end": "192.168.1.254",
                       "start": "192.168.1.2"
                   }
               ],
               "cidr": "192.168.1.0/24",
               "dns_nameservers": [],
               "enable_dhcp": false,
               "gateway_ip": "192.168.1.1",
               "host_routes": [],
               "id": "b282ef86-2584-4a02-9b58-69d6233952a2",
               "ip_version": 4,
               "wrs-net:managed": true,
               "name": "external-subnet0",
               "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "wrs-provider:network_type": "vlan",
               "wrs-provider:physical_network": "group0-data0",
               "wrs-provider:segmentation_id": 10,
               "tenant_id": "206f147dcf72421fa6829e33bfb34637",
               "wrs-net:vlan_id": 0
           },
           {
               "allocation_pools": [
                   {
                       "end": "192.168.201.50",
                       "start": "192.168.201.2"
                   }
               ],
               "cidr": "192.168.201.0/24",
               "dns_nameservers": [],
               "enable_dhcp": true,
               "gateway_ip": "192.168.201.1",
               "host_routes": [],
               "id": "9aa900f4-522b-4b83-ba93-57f7d92da5d5",
               "ip_version": 4,
               "wrs-net:managed": true,
               "name": "tenant2-mgmt-subnet",
               "network_id": "9472a8ab-9205-43ef-a460-5f01f031791a",
               "wrs-provider:network_type": "vlan",
               "wrs-provider:physical_network": "group0-data1",
               "wrs-provider:segmentation_id": 616,
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3",
               "wrs-net:vlan_id": 0
           },
           {
               "allocation_pools": [
                   {
                       "end": "192.168.101.50",
                       "start": "192.168.101.2"
                   }
               ],
               "cidr": "192.168.101.0/24",
               "dns_nameservers": [],
               "enable_dhcp": true,
               "gateway_ip": "192.168.101.1",
               "host_routes": [],
               "id": "34efd537-7a72-4fcd-b837-9874caf34117",
               "ip_version": 4,
               "wrs-net:managed": true,
               "name": "tenant1-mgmt-subnet",
               "network_id": "f652780a-7a9d-4667-8df4-5c8632728be9",
               "wrs-provider:network_type": "vlan",
               "wrs-provider:physical_network": "group0-data0",
               "wrs-provider:segmentation_id": 600,
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10",
               "wrs-net:vlan_id": 0
           },
           {
               "allocation_pools": [],
               "cidr": "10.0.0.0/24",
               "dns_nameservers": [],
               "enable_dhcp": false,
               "gateway_ip": null,
               "host_routes": [],
               "id": "ad791a3e-33cf-4d8d-b80f-91c87f97745e",
               "ip_version": 4,
               "wrs-net:managed": false,
               "name": "internal0-subnet0-0",
               "network_id": "7e5ed852-a990-4fc5-89a2-b17093ca1982",
               "wrs-provider:network_type": "vlan",
               "wrs-provider:physical_network": "group0-data0",
               "wrs-provider:segmentation_id": 700,
               "tenant_id": "206f147dcf72421fa6829e33bfb34637",
               "wrs-net:vlan_id": 0
           },
           {
               "allocation_pools": [],
               "cidr": "172.16.0.0/24",
               "dns_nameservers": [],
               "enable_dhcp": false,
               "gateway_ip": null,
               "host_routes": [],
               "id": "bc269028-1862-4dde-ba2e-62a67d1af4e4",
               "ip_version": 4,
               "wrs-net:managed": false,
               "name": "tenant1-subnet0",
               "network_id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
               "wrs-provider:network_type": "vlan",
               "wrs-provider:physical_network": "group0-data0",
               "wrs-provider:segmentation_id": 601,
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10",
               "wrs-net:vlan_id": 0
           },
           {
               "allocation_pools": [],
               "cidr": "172.18.0.0/24",
               "dns_nameservers": [],
               "enable_dhcp": false,
               "gateway_ip": null,
               "host_routes": [],
               "id": "985806f5-9fd7-4d47-9da6-cb0c1316e63d",
               "ip_version": 4,
               "wrs-net:managed": false,
               "name": "tenant2-subnet0",
               "network_id": "7a77e654-794a-4e19-9679-ac2733e19876",
               "wrs-provider:network_type": "vlan",
               "wrs-provider:physical_network": "group0-data1",
               "wrs-provider:segmentation_id": 617,
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3",
               "wrs-net:vlan_id": 0
           }
       ]
   }

This operation does not accept a request body.

******************************************
Shows information for a specified subnet
******************************************

.. rest_method:: GET /v2.0/subnets/​{subnet_id}​

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

201

**Error response codes**

itemNotFound (401), unauthorized (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "subnet_id", "URI", "csapi:UUID", "The UUID for a subnet."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-net:managed (Optional)", "plain", "xsd:bool", "Indicates whether IP address allocation is managed by the system or by the customer. If ``true`` then the system allocates IP addresses when ports are created and attached to VM instances. If ``false`` then the system will not assign any IP addresses automatically. This implies that if the system cannot allocate any IP addresses that it also cannot allocate a DHCP server, manage allocation pools, or server DNS nameservers or static routers."
   "wrs-provider:network_type (Optional)", "plain", "xsd:string", "The type of the provider network to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-provider:physical_name (Optional)", "plain", "xsd:string", "The name of the provider network to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-provider:segmentation_id (Optional)", "plain", "xsd:integer", "The provider network segmentation id to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-net:vlan_id (Optional)", "plain", "xsd:integer", "The VLAN ID to be used in the VM instance. If the VLAN ID is 0 then all packets originated from the VM instance are expected to be untagged. If the VLAN ID value is non zero than it is expected that all packets originated by the VM must be tagged with the corresponding VLAN ID value. Any other value will be discarded by the host vswitch."

::

   {
       "subnet": {
           "allocation_pools": [],
           "cidr": "1.2.3.0/24",
           "dns_nameservers": [],
           "enable_dhcp": false,
           "gateway_ip": null,
           "host_routes": [],
           "id": "837aebc9-6c78-43e9-8124-168ba16adbc7",
           "ip_version": 4,
           "wrs-net:managed": false,
           "name": "test-subnet-0",
           "network_id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
           "wrs-provider:network_type": "vlan",
           "wrs-provider:physical_network": "group0-data0",
           "wrs-provider:segmentation_id": 615,
           "tenant_id": "206f147dcf72421fa6829e33bfb34637",
           "wrs-net:vlan_id": 99
       }
   }

This operation does not accept a request body.

*****************************************
Creates a subnet on a specified network
*****************************************

.. rest_method:: POST /v2.0/subnets

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

201

**Error response codes**

badRequest (400), itemNotFound (401), forbidden (403), unauthorized
(404), buildInProgress (409)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-net:managed (Optional)", "plain", "xsd:bool", "Indicates whether IP address allocation is managed by the system or by the customer. If ``true`` then the system allocates IP addresses when ports are created and attached to VM instances. If ``false`` then the system will not assign any IP addresses automatically. This implies that if the system cannot allocate any IP addresses that it also cannot allocate a DHCP server, manage allocation pools, or server DNS nameservers or static routers."
   "wrs-net:vlan_id (Optional)", "plain", "xsd:integer", "The VLAN ID to be used in the VM instance. If the VLAN ID is 0 then all packets originated from the VM instance are expected to be untagged. If the VLAN ID value is non zero than it is expected that all packets originated by the VM must be tagged with the corresponding VLAN ID value. Any other value will be discarded by the host vswitch."
   "wrs-provider:network_type (Optional)", "plain", "xsd:string", "The type of physical network that maps to this subnet resource. For example, ``flat``, ``vlan``, or ``vxlan``. The value specified must match the equivalent attribute value on the parent network resource. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only available to admin users.</emphasis>"
   "wrs-provider:physical_network (Optional)", "plain", "xsd:string", "The physical network where this subnet object is implemented. The value specified must match the equivalent attribute value on the parent network resource. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only available to admin users.</emphasis>"
   "wrs-provider:segmentation_id (Optional)", "plain", "xsd:string", "An isolated segment on the physical network reserved for this subnet resource. The ``network_type`` attribute defines the segmentation model. For example, if the ``network_type`` is vlan, this ID is a vlan identifier. If the ``network_type`` value is vxlan, this ID is a vxlan VNI value. All subnets on a specific network that share the same ``wrs-net:vlan_id`` attribute value must have the same ``segmentation_id`` attribute value. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only available to admin users.</emphasis>"

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-net:managed (Optional)", "plain", "xsd:bool", "Indicates whether IP address allocation is managed by the system or by the customer. If ``true`` then the system allocates IP addresses when ports are created and attached to VM instances. If ``false`` then the system will not assign any IP addresses automatically. This implies that if the system cannot allocate any IP addresses that it also cannot allocate a DHCP server, manage allocation pools, or server DNS nameservers or static routers."
   "wrs-provider:network_type (Optional)", "plain", "xsd:string", "The type of the provider network to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-provider:physical_name (Optional)", "plain", "xsd:string", "The name of the provider network to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-provider:segmentation_id (Optional)", "plain", "xsd:integer", "The provider network segmentation id to which this subnet is assigned. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"
   "wrs-net:vlan_id (Optional)", "plain", "xsd:integer", "The VLAN ID to be used in the VM instance. If the VLAN ID is 0 then all packets originated from the VM instance are expected to be untagged. If the VLAN ID value is non zero than it is expected that all packets originated by the VM must be tagged with the corresponding VLAN ID value. Any other value will be discarded by the host vswitch."

::

   {
       "subnet": {
           "cidr": "1.2.3.0/24",
           "enable_dhcp": false,
           "ip_version": 4,
           "wrs-net:managed": false,
           "name": "test-subnet-0",
           "network_id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
           "wrs-provider:network_type": "vlan",
           "wrs-provider:physical_network": "group0-data0",
           "wrs-provider:segmentation_id": "615",
           "wrs-net:vlan_id": 99
       }
   }

::

   {
       "subnet": {
           "allocation_pools": [],
           "cidr": "1.2.3.0/24",
           "dns_nameservers": [],
           "enable_dhcp": false,
           "gateway_ip": null,
           "host_routes": [],
           "id": "837aebc9-6c78-43e9-8124-168ba16adbc7",
           "ip_version": 4,
           "wrs-net:managed": false,
           "name": "test-subnet-0",
           "network_id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
           "wrs-provider:network_type": "vlan",
           "wrs-provider:physical_network": "group0-data0",
           "wrs-provider:segmentation_id": 615,
           "tenant_id": "206f147dcf72421fa6829e33bfb34637",
           "wrs-net:vlan_id": 99
       }
   }

-----
Port
-----

The Port entity is an existing OpenStack API. It has been extended to
add the following StarlingX functionality.

-  The network interface type (vif_model) is recorded when attached to a
   VM instance.

-  Source MAC address filtering is enabled when created for a tenant
   which has this feature enabled by the administrator.

-  The MAC address automatically updates to reflect changes to PCI
   passthrough devices for VM instances.

-  The maximum transmit unit (MTU) attribute is a reflection of the MTU
   value of the attached tenant network.

********************************************
Lists ports to which the tenant has access
********************************************

.. rest_method:: GET /v2.0/ports

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "ports (Optional)", "plain", "xsd:list", "The list of ports."
   "wrs-binding:mac_filtering (Optional)", "plain", "xsd:bool", "The state of source MAC address filtering on the port. If this is ``true`` then the attached vswitch enforces that all ingress packets have a source MAC address that matches the port MAC address."
   "wrs-binding:mtu (Optional)", "plain", "xsd:integer", "The maximum transmit unit (MTU). This value is inherited from the tenant network that attaches to this port."
   "wrs-binding:vif_model (Optional)", "plain", "xsd:string", "The type of virtual networking device that is presented to the VM instance. This value is only visible if the device_owner is a VM instance port (i.e., device_owner=""compute:nova"")."

::

   {
       "ports": [
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-0",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "ce58c529-ba73-47f7-a409-a438ceced112",
               "device_owner": "network:router_interface",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.201.1",
                       "subnet_id": "9aa900f4-522b-4b83-ba93-57f7d92da5d5"
                   }
               ],
               "id": "e44d4c03-0add-4b1a-bd36-8f6e7ccd48d9",
               "mac_address": "fa:16:3e:a6:27:3f",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "9472a8ab-9205-43ef-a460-5f01f031791a",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-0",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "ea2baef7-d84b-44c4-82e7-f274ab5e8b6f",
               "device_owner": "network:router_gateway",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.1.2",
                       "subnet_id": "b282ef86-2584-4a02-9b58-69d6233952a2"
                   }
               ],
               "id": "0ebefe26-de93-4d9e-b31a-b4620a6743e8",
               "mac_address": "fa:16:3e:29:11:9b",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": ""
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-0",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "ea2baef7-d84b-44c4-82e7-f274ab5e8b6f",
               "device_owner": "network:router_interface",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.101.1",
                       "subnet_id": "34efd537-7a72-4fcd-b837-9874caf34117"
                   }
               ],
               "id": "cf8c4f0a-f615-4437-a87a-39f2b87b7662",
               "mac_address": "fa:16:3e:fb:4b:89",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "f652780a-7a9d-4667-8df4-5c8632728be9",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-0",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "dhcp596d6b96-7696-5200-a782-fa1c60fe4171-f652780a-7a9d-4667-8df4-5c8632728be9",
               "device_owner": "network:dhcp",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.101.9",
                       "subnet_id": "34efd537-7a72-4fcd-b837-9874caf34117"
                   }
               ],
               "id": "30a92b3d-3353-4c24-9963-d0177b38ad59",
               "mac_address": "fa:16:3e:f5:db:a6",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "f652780a-7a9d-4667-8df4-5c8632728be9",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-1",
               "wrs-binding:mtu": 1500,
               "wrs-binding:vif_model": "virtio",
               "binding:vif_type": "bridge",
               "device_id": "2e934b37-772e-451a-b64a-cd68d9f8ae42",
               "device_owner": "compute:nova",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.101.11",
                       "subnet_id": "34efd537-7a72-4fcd-b837-9874caf34117"
                   }
               ],
               "id": "3ec39233-315c-474b-9f08-482e70c264c7",
               "mac_address": "fa:16:3e:23:0c:bc",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "f652780a-7a9d-4667-8df4-5c8632728be9",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "2c5c2553-36f2-4db4-8769-6ab71b6e2b1e",
               "device_owner": "network:floatingip",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.1.4",
                       "subnet_id": "b282ef86-2584-4a02-9b58-69d6233952a2"
                   }
               ],
               "id": "5e5a6728-81af-4b2d-b068-8a8e56847a1e",
               "mac_address": "fa:16:3e:67:22:12",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "security_groups": [],
               "status": "UNKNOWN",
               "tenant_id": ""
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-1",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "2e934b37-772e-451a-b64a-cd68d9f8ae42",
               "device_owner": "compute:nova",
               "fixed_ips": [],
               "id": "8c99a79f-212c-45e5-89e6-8aa9ed3b5fcb",
               "mac_address": "fa:16:3e:46:47:46",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "2c0896cf-d118-4dca-9760-b4d97e3c7ec3",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-0",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "ce58c529-ba73-47f7-a409-a438ceced112",
               "device_owner": "network:router_gateway",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.1.3",
                       "subnet_id": "b282ef86-2584-4a02-9b58-69d6233952a2"
                   }
               ],
               "id": "10ce8655-96de-45f4-952c-a830ddf1f0d9",
               "mac_address": "fa:16:3e:a4:1d:67",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": ""
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "f2ade26f-5a16-4fef-b6f7-9bd9769ea1f4",
               "device_owner": "network:floatingip",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.1.5",
                       "subnet_id": "b282ef86-2584-4a02-9b58-69d6233952a2"
                   }
               ],
               "id": "573e154f-ec13-4e39-afcc-8e79d2089589",
               "mac_address": "fa:16:3e:23:37:59",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876",
               "security_groups": [],
               "status": "UNKNOWN",
               "tenant_id": ""
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-1",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "2e934b37-772e-451a-b64a-cd68d9f8ae42",
               "device_owner": "compute:nova",
               "fixed_ips": [],
               "id": "9c9b1182-9f7d-4e21-910c-c4441a23d397",
               "mac_address": "fa:16:3e:75:5f:61",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "7e5ed852-a990-4fc5-89a2-b17093ca1982",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "binding:capabilities": {
                   "port_filter": true
               },
               "binding:host_id": "compute-0",
               "wrs-binding:mtu": 1500,
               "binding:vif_type": "bridge",
               "device_id": "dhcp596d6b96-7696-5200-a782-fa1c60fe4171-9472a8ab-9205-43ef-a460-5f01f031791a",
               "device_owner": "network:dhcp",
               "fixed_ips": [
                   {
                       "ip_address": "192.168.201.6",
                       "subnet_id": "9aa900f4-522b-4b83-ba93-57f7d92da5d5"
                   }
               ],
               "id": "62872b6d-3e74-4838-9cf3-99736d0f3919",
               "mac_address": "fa:16:3e:92:0b:14",
               "wrs-binding:mac_filtering": false
               "name": "",
               "network_id": "9472a8ab-9205-43ef-a460-5f01f031791a",
               "security_groups": [],
               "status": "ACTIVE",
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           }
       ]
   }

This operation does not accept a request body.

****************************************
Shows information for a specified port
****************************************

.. rest_method:: GET /v2.0/ports/​{port_id}​

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401), unauthorized (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "port_id", "URI", "csapi:UUID", "The UUID for a port."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-binding:mac_filtering (Optional)", "plain", "xsd:bool", "The state of source MAC address filtering on the port. If this is ``true`` then the attached vswitch enforces that all ingress packets have a source MAC address that matches the port MAC address."
   "wrs-binding:mtu (Optional)", "plain", "xsd:integer", "The maximum transmit unit (MTU). This value is inherited from the tenant network that attaches to this port."
   "wrs-binding:vif_model (Optional)", "plain", "xsd:string", "The type of virtual networking device that is presented to the VM instance. This value is only visible if the device_owner is a VM instance port (i.e., device_owner=""compute:nova"")."

::

   {
       "port": {
           "admin_state_up": true,
           "binding:capabilities": {
               "port_filter": true
           },
           "binding:host_id": "compute-1",
           "wrs-binding:mtu": 1500,
           "wrs-binding:vif_model": "virtio",
           "binding:vif_type": "bridge",
           "device_id": "2e934b37-772e-451a-b64a-cd68d9f8ae42",
           "device_owner": "compute:nova",
           "fixed_ips": [
               {
                   "ip_address": "192.168.101.11",
                   "subnet_id": "34efd537-7a72-4fcd-b837-9874caf34117"
               }
           ],
           "id": "3ec39233-315c-474b-9f08-482e70c264c7",
           "mac_address": "fa:16:3e:23:0c:bc",
           "wrs-binding:mac_filtering": false
           "name": "",
           "network_id": "f652780a-7a9d-4667-8df4-5c8632728be9",
           "security_groups": [],
           "status": "ACTIVE",
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
       }
   }

This operation does not accept a request body.

-------
Router
-------

The Router entity is an existing OpenStack API. It has been extended to
add the following StarlingX functionality.

-  The host attribute has been added to reflect which compute host
   implements the virtual router.

*********************************************************************************
Lists logical routers that are accessible to the tenant who submits the request
*********************************************************************************

.. rest_method:: GET /v2.0/routers

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "routers (Optional)", "plain", "xsd:list", "The list of virtual routers."
   "wrs-net:host (Optional)", "plain", "xsd:string", "The host node where this virtual router is implemented. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"

::

   {
       "routers": [
           {
               "admin_state_up": true,
               "external_gateway_info": {
                   "enable_snat": true,
                   "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876"
               },
               "wrs-net:host": "compute-0",
               "id": "ea2baef7-d84b-44c4-82e7-f274ab5e8b6f",
               "name": "tenant1-router",
               "routes": [],
               "status": "ACTIVE",
               "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
           },
           {
               "admin_state_up": true,
               "external_gateway_info": {
                   "enable_snat": true,
                   "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876"
               },
               "wrs-net:host": "compute-0",
               "id": "ce58c529-ba73-47f7-a409-a438ceced112",
               "name": "tenant2-router",
               "routes": [],
               "status": "ACTIVE",
               "tenant_id": "d8753af85cef49a4bf5f95208c4957f3"
           }
       ]
   }

This operation does not accept a request body.

**************************************
Shows details for a specified router
**************************************

.. rest_method:: GET /v2.0/routers/​{router_id}​

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation.

**Normal response codes**

200

**Error response codes**

itemNotFound (401), forbidden (403), unauthorized (404)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "router_id", "URI", "csapi:UUID", "The UUID for a router."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-net:host (Optional)", "plain", "xsd:string", "The host node where this virtual router is implemented. <emphasis xmlns=""http://docbook.org/ns/docbook"" role=""bold"">Only visible to admin users.</emphasis>"

::

   {
       "router": {
           "admin_state_up": true,
           "external_gateway_info": {
               "enable_snat": true,
               "network_id": "b9475152-11d3-4bda-95c7-fb26a3ad3876"
           },
           "wrs-net:host": "compute-0",
           "id": "ea2baef7-d84b-44c4-82e7-f274ab5e8b6f",
           "name": "tenant1-router",
           "routes": [],
           "status": "ACTIVE",
           "tenant_id": "0590d9fa3dd74bfe9bdf7ed4e5331a10"
       }
   }

This operation does not accept a request body.

----------------
Port Forwarding
----------------

The portfowarding entity is an upstream subproject that is not yet part
of the existing OpenStack API. It provides virtual router port
forwarding functionality. It has been included to add the functionality
to the StarlingX.

************************************************
Lists all virtual router port forwarding rules
************************************************

.. rest_method:: GET /v2.0/portforwardings

**Normal response codes**

200

**Error response codes**

computeFault (400, 500, ...), serviceUnavailable (503), badRequest (400),
unauthorized (401), forbidden (403), badMethod (405), overLimit (413),
itemNotFound (404)

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "portforwardings (Optional)", "plain", "xsd:list", "The list of virtual router port forwarding rules."
   "router_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the router to which the rule is associated."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the rule."
   "inside_addr (Optional)", "plain", "xsd:string", "The internal (private) address which is the final destination of the forwarding rule."
   "inside_port (Optional)", "plain", "xsd:integer", "The internal (private) layer4 protocol port number."
   "outside_port (Optional)", "plain", "xsd:integer", "The external (public) address which is the initial destination of the forwarding rule."
   "protocol (Optional)", "plain", "xsd:string", "The layer4 protocol name; valid values are: ``udp``, ``tcp``, ``udp-lite``, ``sctp``, ``dccp``."
   "description (Optional)", "plain", "xsd:string", "A user defined description string."

::

   {
       "portforwardings": [{
           "router_id": "3303b254-404c-47eb-b02c-81bf0d602682",
           "inside_addr": "192.168.101.63",
           "protocol": "tcp",
           "outside_port": 1234,
           "tenant_id": "6d8ff9303a45465492566fb936fe3b1d",
           "description": "sample port forwarding rule",
           "id": "66ebb891-f883-4fbd-9daf-15b367429d39",
           "inside_port": 80
       },
       {
           "router_id": "3303b254-404c-47eb-b02c-81bf0d602682",
           "inside_addr": "192.168.101.62",
           "protocol": "tcp",
           "outside_port": 1235,
           "tenant_id": "6d8ff9303a45465492566fb936fe3b1d",
           "description": "another sample port forwarding rule",
           "id": "f8e22185-07f2-43e7-9213-abf84ef0eb58",
           "inside_port": 80
       }]
   }

This operation does not accept a request body.

************************************************************
Creates a record for a virtual router port forwarding rule
************************************************************

.. rest_method:: POST /v2.0/portforwardings

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "router_id (Optional)", "plain", "xsd:string", "The virtual router on which to create the port forwarding rule"
   "inside_addr (Optional)", "plain", "xsd:string", "The internal (private) address which is the final destination of the forwarding rule."
   "inside_port (Optional)", "plain", "xsd:integer", "The internal (private) layer4 protocol port number."
   "outside_port (Optional)", "plain", "xsd:integer", "The external (public) address which is the initial destination of the forwarding rule."
   "protocol (Optional)", "plain", "xsd:string", "The layer4 protocol name; valid values are: ``udp``, ``tcp``, ``udp-lite``, ``sctp``, ``dccp``."
   "description (Optional)", "plain", "xsd:string", "A user defined description string."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "router_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the router to which the rule is associated."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the rule."
   "inside_addr (Optional)", "plain", "xsd:string", "The internal (private) address which is the final destination of the forwarding rule."
   "inside_port (Optional)", "plain", "xsd:integer", "The internal (private) layer4 protocol port number."
   "outside_port (Optional)", "plain", "xsd:integer", "The external (public) address which is the initial destination of the forwarding rule."
   "protocol (Optional)", "plain", "xsd:string", "The layer4 protocol name; valid values are: ``udp``, ``tcp``, ``udp-lite``, ``sctp``, ``dccp``."
   "description (Optional)", "plain", "xsd:string", "A user defined description string."

::

   {
       "portforwarding": {
           "router_id": "3303b254-404c-47eb-b02c-81bf0d602682",
           "inside_addr": "192.168.101.63",
           "protocol": "tcp",
           "outside_port": "1234",
           "inside_port": "80",
           "description": "sample port forwarding rule"
       }
   }

::

   {
       "portforwarding": {
           "router_id": "3303b254-404c-47eb-b02c-81bf0d602682",
           "inside_addr": "192.168.101.63",
           "protocol": "tcp",
           "outside_port": "1234",
           "tenant_id": "6d8ff9303a45465492566fb936fe3b1d",
           "description": "sample port forwarding rule",
           "id": "66ebb891-f883-4fbd-9daf-15b367429d39",
           "inside_port": "80"
       }
   }

*********************************************************************************
Shows detailed information about a specific virtual router port forwarding rule
*********************************************************************************

.. rest_method:: GET /v2.0/portforwardings/​{portforwarding_id}​

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

   "portforwarding_id", "URI", "csapi:UUID", "The ID for a virtual router port forwarding rule."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "router_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the router to which the rule is associated."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the rule."
   "inside_addr (Optional)", "plain", "xsd:string", "The internal (private) address which is the final destination of the forwarding rule."
   "inside_port (Optional)", "plain", "xsd:integer", "The internal (private) layer4 protocol port number."
   "outside_port (Optional)", "plain", "xsd:integer", "The external (public) address which is the initial destination of the forwarding rule."
   "protocol (Optional)", "plain", "xsd:string", "The layer4 protocol name; valid values are: ``udp``, ``tcp``, ``udp-lite``, ``sctp``, ``dccp``."
   "description (Optional)", "plain", "xsd:string", "A user defined description string."

::

   {
       "portforwarding": {
           "router_id": "3303b254-404c-47eb-b02c-81bf0d602682",
           "inside_addr": "192.168.101.63",
           "protocol": "tcp",
           "outside_port": 1234,
           "tenant_id": "6d8ff9303a45465492566fb936fe3b1d",
           "description": "sample port forwarding rule",
           "id": "66ebb891-f883-4fbd-9daf-15b367429d39",
           "inside_port": 80
       }
   }

This operation does not accept a request body.

***********************************************
Updates a virtual router port forwarding rule
***********************************************

.. rest_method:: PUT /v2.0/portforwardings/​{portforwarding_id}​

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "portforwarding_id", "URI", "csapi:UUID", "The ID for a virtual router port forwarding rule."
   "inside_addr (Optional)", "plain", "xsd:string", "The internal (private) address which is the final destination of the forwarding rule."
   "inside_port (Optional)", "plain", "xsd:integer", "The internal (private) layer4 protocol port number."
   "outside_port (Optional)", "plain", "xsd:integer", "The external (public) address which is the initial destination of the forwarding rule."
   "protocol (Optional)", "plain", "xsd:string", "The layer4 protocol name; valid values are: ``udp``, ``tcp``, ``udp-lite``, ``sctp``, ``dccp``."
   "description (Optional)", "plain", "xsd:string", "A user defined description string."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "router_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the router to which the rule is associated."
   "tenant_id (Optional)", "plain", "csapi:UUID", "The unique UUID of the tenant which owns the rule."
   "inside_addr (Optional)", "plain", "xsd:string", "The internal (private) address which is the final destination of the forwarding rule."
   "inside_port (Optional)", "plain", "xsd:integer", "The internal (private) layer4 protocol port number."
   "outside_port (Optional)", "plain", "xsd:integer", "The external (public) address which is the initial destination of the forwarding rule."
   "protocol (Optional)", "plain", "xsd:string", "The layer4 protocol name; valid values are: ``udp``, ``tcp``, ``udp-lite``, ``sctp``, ``dccp``."
   "description (Optional)", "plain", "xsd:string", "A user defined description string."

::

   {
       "portforwarding": {
           "description": "updated sample port forwarding rule"
       }
   }

::

   {
       "portforwarding": {
           "router_id": "3303b254-404c-47eb-b02c-81bf0d602682",
           "inside_addr": "192.168.101.63",
           "protocol": "tcp",
           "outside_port": 1234,
           "tenant_id": "6d8ff9303a45465492566fb936fe3b1d",
           "description": "updated sample port forwarding rule",
           "id": "66ebb891-f883-4fbd-9daf-15b367429d39",
           "inside_port": 80
       }
   }

********************************************************
Deletes a specific virtual router port forwarding rule
********************************************************

.. rest_method:: DELETE /v2.0/portforwardings/​{portforwarding_id}​

**Normal response codes**

204

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "portforwarding_id", "URI", "csapi:UUID", "The ID for a virtual router port forwarding rule."

This operation does not accept a request body.




