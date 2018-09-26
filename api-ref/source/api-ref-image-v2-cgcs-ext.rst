====================================================
Image API v2 StarlingX extensions
====================================================

StarlingX extensions to the OpenStack Image API to support additional
properties on images for customizing migration behaviour, supporting raw
cached images in ceph cluster, disabling VM auto-recovery and indicating
the backend where the image is stored.

The typical port used for the Image REST API is 9292. However, proper
technique would be to look up the image/glance service endpoint in
Keystone.

-------
Images
-------

The Image entity is extended by StarlingX to have additional
properties on images for customizing migration behaviour, supporting raw
cached images in ceph cluster, disabling VM auto-recovery and indicating
the backend where the image is stored.

******************
Creates an image
******************

.. rest_method:: POST /v2/images

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation. NOTE that the extensions listed here are
``additional properties`` which are supported in the Image API and
implemented in a Stack-specific manner.

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "cache_raw (Optional)", "plain", "xsd:bool", "On systems using Ceph storage, boot image files must be converted to RAW format before they can be used to create volumes. You can accelerate volume creation in StarlingX (and therefore instance launch time) by caching the RAW images as they are created. The cached images are maintained in the Glance image storage space and used for volume creation, eliminating conversion time. The default behaviour is to NOT cache the raw format."
   "sw_wrs_auto_recovery (Optional)", "plain", "xsd:bool", "Indicates whether auto recovery of failed virutal machine instances is enabled or not. The default is True."
   "hw_wrs_live_migration_timeout (Optional)", "plain", "xsd:integer", "Indicates the number of seconds to wait for a live migration to complete for a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the timeout value is provisioned in both the flavor and the image, the smaller value is used. The default is 800 seconds. The minimum timeout is 120 seconds and the maximum timeout value is 800 seconds. To disable the live migration timeout feature, set this value to 0."
   "hw_wrs_live_migration_max_downtime (Optional)", "plain", "xsd:integer", "Indicates the maximum amoutn of downtime to tolerate during a live migration of a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the max downtime value is provisioned in both the flavor and the image, the value from the flavor overrides the value from the image. The default is 500 milliseconds. The minimum timer value is 100 milliseconds."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "cache_raw (Optional)", "plain", "xsd:bool", "On systems using Ceph storage, boot image files must be converted to RAW format before they can be used to create volumes. You can accelerate volume creation in StarlingX (and therefore instance launch time) by caching the RAW images as they are created. The cached images are maintained in the Glance image storage space and used for volume creation, eliminating conversion time. The default behaviour is to NOT cache the raw format."
   "store (Optional)", "plain", "xsd:string", "Indicates which Glance backend the image is stored in; either ``file`` for the Controller Filesystem or ``rbd`` for the ceph backend."
   "sw_wrs_auto_recovery (Optional)", "plain", "xsd:bool", "Indicates whether auto recovery of failed virutal machine instances is enabled or not. The default is ``True``."
   "hw_wrs_live_migration_timeout (Optional)", "plain", "xsd:integer", "Indicates the number of seconds to wait for a live migration to complete for a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the timeout value is provisioned in both the flavor and the image, the smaller value is used. The default is 800 seconds. The minimum timeout is 120 seconds and the maximum timeout value is 800 seconds. To disable the live migration timeout feature, set this value to 0."
   "hw_wrs_live_migration_max_downtime (Optional)", "plain", "xsd:integer", "Indicates the maximum amoutn of downtime to tolerate during a live migration of a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the max downtime value is provisioned in both the flavor and the image, the value from the flavor overrides the value from the image. The default is 500 milliseconds. The minimum timer value is 100 milliseconds."

::

   {
      "hw_wrs_live_migration_timeout":"400",
      "name":"cirros",
      "container_format":"bare",
      "cache_raw":"True",
      "visibility":"public",
      "disk_format":"qcow2",
      "sw_wrs_auto_recovery":"False",
      "hw_wrs_live_migration_max_downtime":"350"
   }

::

   {
      "hw_wrs_live_migration_timeout":"400",
      "disk_format":"qcow2",
      "min_ram":0,
      "updated_at":"2016-10-25T12:02:09Z",
      "file":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67/file",
      "owner":"b27359bcdb3e424db43a3e2255777f37",
      "id":"9d34ff07-6f66-4107-9363-e38b0b559a67",
      "size":null,
      "self":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67",
      "cache_raw":"True",
      "container_format":"bare",
      "schema":"/v2/schemas/image",
      "status":"queued",
      "tags":[

      ],
      "visibility":"public",
      "min_disk":0,
      "sw_wrs_auto_recovery":"False",
      "virtual_size":null,
      "hw_wrs_live_migration_max_downtime":"350",
      "name":"cirros",
      "checksum":null,
      "created_at":"2016-10-25T12:02:09Z",
      "protected":false
   }

******************
Lists all images
******************

.. rest_method:: GET /v2/images

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation. NOTE that the extensions listed here are
``additional properties`` which are supported in the Image API and
implemented in a Stack-specific manner.

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

   "cache_raw (Optional)", "plain", "xsd:bool", "On systems using Ceph storage, boot image files must be converted to RAW format before they can be used to create volumes. You can accelerate volume creation in StarlingX (and therefore instance launch time) by caching the RAW images as they are created. The cached images are maintained in the Glance image storage space and used for volume creation, eliminating conversion time. The default behaviour is to NOT cache the raw format."
   "store (Optional)", "plain", "xsd:string", "Indicates which Glance backend the image is stored in; either ``file`` for the Controller Filesystem or ``rbd`` for the ceph backend."
   "sw_wrs_auto_recovery (Optional)", "plain", "xsd:bool", "Indicates whether auto recovery of failed virutal machine instances is enabled or not. The default is ``True``."
   "hw_wrs_live_migration_timeout (Optional)", "plain", "xsd:integer", "Indicates the number of seconds to wait for a live migration to complete for a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the timeout value is provisioned in both the flavor and the image, the smaller value is used. The default is 800 seconds. The minimum timeout is 120 seconds and the maximum timeout value is 800 seconds. To disable the live migration timeout feature, set this value to 0."
   "hw_wrs_live_migration_max_downtime (Optional)", "plain", "xsd:integer", "Indicates the maximum amoutn of downtime to tolerate during a live migration of a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the max downtime value is provisioned in both the flavor and the image, the value from the flavor overrides the value from the image. The default is 500 milliseconds. The minimum timer value is 100 milliseconds."

::

   {
      "images":[
         {
            "status":"active",
            "virtual_size":null,
            "name":"sample-guest",
            "tags":[

            ],
            "container_format":"bare",
            "created_at":"2016-10-24T22:50:17Z",
            "size":688914432,
            "disk_format":"raw",
            "updated_at":"2016-10-24T22:50:26Z",
            "visibility":"public",
            "self":"/v2/images/ac22a842-27c6-40a5-8475-f15f12e94202",
            "min_disk":0,
            "protected":false,
            "id":"ac22a842-27c6-40a5-8475-f15f12e94202",
            "file":"/v2/images/ac22a842-27c6-40a5-8475-f15f12e94202/file",
            "checksum":"29514837240a4bb80df0e2362644ae17",
            "owner":"b27359bcdb3e424db43a3e2255777f37",
            "direct_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/ac22a842-27c6-40a5-8475-f15f12e94202/snap",
            "min_ram":0,
            "store":"rbd",
            "schema":"/v2/schemas/image"
         },
         {
            "hw_wrs_live_migration_timeout":"400",
            "cache_raw":"True",
            "min_ram":0,
            "updated_at":"2016-10-25T12:06:19Z",
            "file":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67/file",
            "owner":"b27359bcdb3e424db43a3e2255777f37",
            "id":"9d34ff07-6f66-4107-9363-e38b0b559a67",
            "size":13287936,
            "self":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67",
            "disk_format":"qcow2",
            "cache_raw_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/9d34ff07-6f66-4107-9363-e38b0b559a67_raw/snap",
            "container_format":"bare",
            "direct_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/9d34ff07-6f66-4107-9363-e38b0b559a67/snap",
            "store":"rbd",
            "schema":"/v2/schemas/image",
            "status":"active",
            "cache_raw_size":"41126400",
            "cache_raw_status":"Cached",
            "tags":[

            ],
            "visibility":"public",
            "min_disk":0,
            "sw_wrs_auto_recovery":"False",
            "virtual_size":null,
            "hw_wrs_live_migration_max_downtime":"350",
            "name":"cirros",
            "checksum":"ee1eca47dc88f4879d8a229cc70a07c6",
            "created_at":"2016-10-25T12:02:09Z",
            "protected":false
         }
      ],
      "schema":"/v2/schemas/images",
      "first":"/v2/images?sort_key=name&sort_dir=asc&limit=20"
   }

This operation does not accept a request body.

***************************************************
Shows detailed information about a specific image
***************************************************

.. rest_method:: GET /v2/images/​{image_id}​

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation. NOTE that the extensions listed here are
``additional properties`` which are supported in the Image API and
implemented in a Stack-specific manner.

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

   "image_id", "URI", "xsd:string", "The name for the image."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "cache_raw (Optional)", "plain", "xsd:bool", "On systems using Ceph storage, boot image files must be converted to RAW format before they can be used to create volumes. You can accelerate volume creation in StarlingX (and therefore instance launch time) by caching the RAW images as they are created. The cached images are maintained in the Glance image storage space and used for volume creation, eliminating conversion time. The default behaviour is to NOT cache the raw format."
   "store (Optional)", "plain", "xsd:string", "Indicates which Glance backend the image is stored in; either ``file`` for the Controller Filesystem or ``rbd`` for the ceph backend."
   "sw_wrs_auto_recovery (Optional)", "plain", "xsd:bool", "Indicates whether auto recovery of failed virutal machine instances is enabled or not. The default is ``True``."
   "hw_wrs_live_migration_timeout (Optional)", "plain", "xsd:integer", "Indicates the number of seconds to wait for a live migration to complete for a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the timeout value is provisioned in both the flavor and the image, the smaller value is used. The default is 800 seconds. The minimum timeout is 120 seconds and the maximum timeout value is 800 seconds. To disable the live migration timeout feature, set this value to 0."
   "hw_wrs_live_migration_max_downtime (Optional)", "plain", "xsd:integer", "Indicates the maximum amoutn of downtime to tolerate during a live migration of a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the max downtime value is provisioned in both the flavor and the image, the value from the flavor overrides the value from the image. The default is 500 milliseconds. The minimum timer value is 100 milliseconds."

::

   {
      "hw_wrs_live_migration_timeout":"400",
      "cache_raw":"True",
      "min_ram":0,
      "updated_at":"2016-10-25T12:06:19Z",
      "file":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67/file",
      "owner":"b27359bcdb3e424db43a3e2255777f37",
      "id":"9d34ff07-6f66-4107-9363-e38b0b559a67",
      "size":13287936,
      "self":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67",
      "disk_format":"qcow2",
      "cache_raw_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/9d34ff07-6f66-4107-9363-e38b0b559a67_raw/snap",
      "container_format":"bare",
      "direct_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/9d34ff07-6f66-4107-9363-e38b0b559a67/snap",
      "store":"rbd",
      "schema":"/v2/schemas/image",
      "status":"active",
      "cache_raw_size":"41126400",
      "cache_raw_status":"Cached",
      "tags":[

      ],
      "visibility":"public",
      "min_disk":0,
      "sw_wrs_auto_recovery":"False",
      "virtual_size":null,
      "hw_wrs_live_migration_max_downtime":"350",
      "name":"cirros",
      "checksum":"ee1eca47dc88f4879d8a229cc70a07c6",
      "created_at":"2016-10-25T12:02:09Z",
      "protected":false
   }

This operation does not accept a request body.

***************************
Modifies a specific image
***************************

.. rest_method:: PUT /v2/images/​{image_id}​

This is an existing OpenStack API. The documentation that follows lists
only the fields that are new or modified. For a detailed description of
existing and unmodified fields please refer to the standard OpenStack
API documentation. NOTE that the extensions listed here are
``additional properties`` which are supported in the Image API and
implemented in a Stack-specific manner.

**Normal response codes**

200

**Error response codes**

badMediaType (415), NetworkNotFound (400)

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "image_id", "URI", "xsd:string", "The name for the image."
   "sw_wrs_auto_recovery (Optional)", "plain", "xsd:bool", "Indicates whether auto recovery of failed virutal machine instances is enabled or not. The default is True."
   "hw_wrs_live_migration_timeout (Optional)", "plain", "xsd:integer", "Indicates the number of seconds to wait for a live migration to complete for a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the timeout value is provisioned in both the flavor and the image, the smaller value is used. The default is 800 seconds. The minimum timeout is 120 seconds and the maximum timeout value is 800 seconds. To disable the live migration timeout feature, set this value to 0."
   "hw_wrs_live_migration_max_downtime (Optional)", "plain", "xsd:integer", "Indicates the maximum amoutn of downtime to tolerate during a live migration of a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the max downtime value is provisioned in both the flavor and the image, the value from the flavor overrides the value from the image. The default is 500 milliseconds. The minimum timer value is 100 milliseconds."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "cache_raw (Optional)", "plain", "xsd:bool", "On systems using Ceph storage, boot image files must be converted to RAW format before they can be used to create volumes. You can accelerate volume creation in StarlingX (and therefore instance launch time) by caching the RAW images as they are created. The cached images are maintained in the Glance image storage space and used for volume creation, eliminating conversion time. The default behaviour is to NOT cache the raw format."
   "store (Optional)", "plain", "xsd:string", "Indicates which Glance backend the image is stored in; either ``file`` for the Controller Filesystem or ``rbd`` for the ceph backend."
   "sw_wrs_auto_recovery (Optional)", "plain", "xsd:bool", "Indicates whether auto recovery of failed virutal machine instances is enabled or not. The default is ``True``."
   "hw_wrs_live_migration_timeout (Optional)", "plain", "xsd:integer", "Indicates the number of seconds to wait for a live migration to complete for a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the timeout value is provisioned in both the flavor and the image, the smaller value is used. The default is 800 seconds. The minimum timeout is 120 seconds and the maximum timeout value is 800 seconds. To disable the live migration timeout feature, set this value to 0."
   "hw_wrs_live_migration_max_downtime (Optional)", "plain", "xsd:integer", "Indicates the maximum amoutn of downtime to tolerate during a live migration of a VM created with this image. Note that this can be specified as an extraspec of the flavor as well. If the max downtime value is provisioned in both the flavor and the image, the value from the flavor overrides the value from the image. The default is 500 milliseconds. The minimum timer value is 100 milliseconds."

::

   [
      {
         "path":"/hw_wrs_live_migration_timeout",
         "value":"500",
         "op":"replace"
      },
      {
         "path":"/sw_wrs_auto_recovery",
         "value":"True",
         "op":"replace"
      },
      {
         "path":"/hw_wrs_live_migration_max_downtime",
         "value":"300",
         "op":"replace"
      }
   ]

::

   {
      "hw_wrs_live_migration_timeout":"500",
      "cache_raw":"True",
      "min_ram":0,
      "updated_at":"2016-10-25T12:15:41Z",
      "file":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67/file",
      "owner":"b27359bcdb3e424db43a3e2255777f37",
      "id":"9d34ff07-6f66-4107-9363-e38b0b559a67",
      "size":13287936,
      "self":"/v2/images/9d34ff07-6f66-4107-9363-e38b0b559a67",
      "disk_format":"qcow2",
      "cache_raw_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/9d34ff07-6f66-4107-9363-e38b0b559a67_raw/snap",
      "container_format":"bare",
      "direct_url":"rbd://840f16bc-3238-4adb-8700-6b9876c23462/images/9d34ff07-6f66-4107-9363-e38b0b559a67/snap",
      "store":"rbd",
      "schema":"/v2/schemas/image",
      "status":"active",
      "cache_raw_size":"41126400",
      "cache_raw_status":"Cached",
      "tags":[

      ],
      "visibility":"public",
      "min_disk":0,
      "sw_wrs_auto_recovery":"True",
      "virtual_size":null,
      "hw_wrs_live_migration_max_downtime":"300",
      "name":"cirros",
      "checksum":"ee1eca47dc88f4879d8a229cc70a07c6",
      "created_at":"2016-10-25T12:02:09Z",
      "protected":false
   }
