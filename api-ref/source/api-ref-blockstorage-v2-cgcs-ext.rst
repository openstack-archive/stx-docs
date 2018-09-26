====================================================
Block Storage API v2 Titanium extensions
====================================================

Titanium extensions to the OpenStack Block Storage API such as backup
status and export/import actions for volumes and snapshots.

The typical port used for the Block Storage REST API is 8776. However,
proper technique would be to look up the cinderv2 service endpoint in
keystone.

-----------
Extensions
-----------

The Extensions entity lists all available extensions

**********************
Lists all extensions
**********************

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

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."

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
            "namespace" : "http://docs.windriver.com/volume/ext/wrs-snapshot/api/v1.0",
            "name" : "WrsSnapshotExportAction",
            "updated" : "2014-08-16T00:00:00+00:00",
            "description" : "Enable snapshot export to file",
            "alias" : "wrs-snapshot",
            "links" : []
         },
         {
            "namespace" : "http://docs.windriver.com/volume/ext/wrs-volume/api/v1.0",
            "name" : "WrsVolumeExport",
            "updated" : "2014-08-11T00:00:00+00:00",
            "description" : "Enable volume export/import",
            "alias" : "wrs-volume",
            "links" : []
         },
         ...
      ]
   }

This operation does not accept a request body.

**********************************************
Gets information about a specified extension
**********************************************

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

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."
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
         "namespace" : "http://docs.windriver.com/volume/ext/wrs-volume/api/v1.0",
         "name" : "WrsVolumeExport",
         "updated" : "2014-08-11T00:00:00+00:00",
         "description" : "Enable volume export/import",
         "alias" : "wrs-volume",
         "links" : []
      }
   }

   OR

   {
      "extension" : {
         "namespace" : "http://docs.windriver.com/volume/ext/wrs-snapshot/api/v1.0",
         "name" : "WrsSnapshotExportAction",
         "updated" : "2014-08-16T00:00:00+00:00",
         "description" : "Enable snapshot export to file",
         "alias" : "wrs-snapshot",
         "links" : []
      }
   }

This operation does not accept a request body.

--------
Volumes
--------

Titanium extensions include export and import actions for performing
backup and restores of volumes, and a backup status attribute to
indicate the status of the new actions.

**************************************
Get information about system volumes
**************************************

.. rest_method:: GET /v2/​{tenant_id}​/volumes/detail

Preconditions

-  The specified volume must exist in all case.

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-volume:backup_status", "plain", "xsd:string", "Indicates backup status."

::

   {
      "volumes" : [
         {
            "wrs-volume:backup_status" : "Export completed at 2015-02-27 16:35:53.545339",
            "volume_type" : "None",
            "status" : "available",
            "size" : 1,
            "created_at" : "2015-02-27T16:26:08.164607",
            "id" : "b7db512f-463e-4720-8fbd-154c0f2bc2ae",
            "metadata" : {},
            "attachments" : [],
            "os-volume-replication:driver_data" : null,
            "os-vol-mig-status-attr:migstat" : null,
            "display_name" : null,
            "availability_zone" : "nova",
            "display_description" : null,
            "encrypted" : false,
            "os-vol-mig-status-attr:name_id" : null,
            "os-vol-host-attr:host" : "controller@lvm#lvm",
            "os-volume-replication:extended_status" : null,
            "snapshot_id" : null,
            "os-vol-tenant-attr:tenant_id" : "e0741109067649a8899936e9fefda95b",
            "bootable" : "false",
            "source_volid" : null
         },
         {
            "wrs-volume:backup_status" : "Import completed at 2015-02-27 15:04:29.135579",
            "volume_type" : "None",
            "status" : "available",
            "size" : 1,
            "created_at" : "2015-02-27T14:04:34.763953",
            "id" : "27080551-9d88-4cf0-aa85-c1392dbf38f4",
            "metadata" : {},
            "attachments" : [],
            "os-volume-replication:driver_data" : null,
            "os-vol-mig-status-attr:migstat" : null,
            "display_name" : null,
            "availability_zone" : "nova",
            "display_description" : null,
            "encrypted" : false,
            "os-vol-mig-status-attr:name_id" : null,
            "os-vol-host-attr:host" : "controller@lvm#lvm",
            "os-volume-replication:extended_status" : null,
            "snapshot_id" : null,
            "os-vol-tenant-attr:tenant_id" : "e0741109067649a8899936e9fefda95b",
            "bootable" : "false",
            "source_volid" : null
         },
         {
            "wrs-volume:backup_status" : "Snapshot export completed at 2015-02-27 20:57:29.323714",
            "volume_type" : "None",
            "status" : "available",
            "size" : 1,
            "created_at" : "2015-02-27T13:44:55.317995",
            "id" : "2c4f094b-f6d8-4ff6-800e-e5998cb4d6fa",
            "metadata" : {},
            "attachments" : [],
            "os-volume-replication:driver_data" : null,
            "os-vol-mig-status-attr:migstat" : null,
            "display_name" : null,
            "availability_zone" : "nova",
            "display_description" : null,
            "encrypted" : false,
            "os-vol-mig-status-attr:name_id" : null,
            "os-vol-host-attr:host" : "controller@lvm#lvm",
            "os-volume-replication:extended_status" : null,
            "snapshot_id" : null,
            "os-vol-tenant-attr:tenant_id" : "e0741109067649a8899936e9fefda95b",
            "bootable" : "false",
            "source_volid" : null
         }
      ]
   }

******************************************
Get information about a specified volume
******************************************

.. rest_method:: GET /v2/​{tenant_id}​/volumes/​{volume_id}​

Preconditions

-  The specified volume must exist in all case.

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."
   "volume_id", "URI", "csapi:UUID", "The ID for the volume to list."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-volume:backup_status", "plain", "xsd:string", "Indicates backup status."

::

   {
      "volumes" : [
         {
            "wrs-volume:backup_status" : "Import completed at 2015-02-27 15:04:29.135579",
            "volume_type" : "None",
            "status" : "available",
            "size" : 1,
            "created_at" : "2015-02-27T14:04:34.763953",
            "id" : "27080551-9d88-4cf0-aa85-c1392dbf38f4",
            "metadata" : {},
            "attachments" : [],
            "os-volume-replication:driver_data" : null,
            "os-vol-mig-status-attr:migstat" : null,
            "display_name" : null,
            "availability_zone" : "nova",
            "display_description" : null,
            "encrypted" : false,
            "os-vol-mig-status-attr:name_id" : null,
            "os-vol-host-attr:host" : "controller@lvm#lvm",
            "os-volume-replication:extended_status" : null,
            "snapshot_id" : null,
            "os-vol-tenant-attr:tenant_id" : "e0741109067649a8899936e9fefda95b",
            "bootable" : "false",
            "source_volid" : null
         },
      ]
   }

******************************************************************
Executes the specified action or command on the specified volume
******************************************************************

.. rest_method:: POST /v2/​{tenant_id}​/volumes/​{volume_id}​/action

Preconditions

-  The specified volume must exist in all case.

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."
   "volume_id", "URI", "csapi:UUID", "The ID for the volume to list."
   "wrs-volume:os-volume_export", "plain", "xsd:string", "Export volume to a file"
   "wrs-volume:os-volume_import", "plain", "xsd:string", "Import a volume from a file <ul><li>file_name: ""VolumeExportName.tgz"". </li></ul>"

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "volume_type", "plain", "xsd:string", "Indicates the volume type."
   "updated_at", "plain", "xsd:string", "Indicates when the action was performed."
   "status", "plain", "xsd:string", "Indicates the state of the export or import action."
   "id", "plain", "csapi:UUID", "Indicates the volume UUID."
   "display_description", "plain", "xsd:string", "Volume descrition if any."
   "size", "plain", "xsd:int", "Indicates the volume size in Gbyte."

::

   {
      'wrs-volume:os-volume_export' : {
         'volume_type' : null,
         'updated_at' : '2015-02-27T14:04:35.201969',
         'status' : 'exporting',
         'id' : '27080551-9d88-4cf0-aa85-c1392dbf38f4',
         'display_description' : null,
         'size' : 1
      }
   }
   or
   {
      'wrs-volume:os-volume_import' : {
         'volume_type' : null,
         'updated_at' : '2015-02-27T15:03:54.045796',
         'status' : 'importing',
         'id' : '27080551-9d88-4cf0-aa85-c1392dbf38f4',
         'display_description' : null,
         'size' : 1
      }
   }

----------
Snapshots
----------

Titanium extensions include export actions for performing backup volumes
already attached to a VM, and a backup status attribute to indicate the
status of the new actions.

***********************************************
Get information about system volume snapshots
***********************************************

.. rest_method:: GET /v2/​{tenant_id}​/snapshots/detail

Preconditions

-  The specified volume snapshot must exist in all case.

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-snapshot:backup_status", "plain", "xsd:string", "Indicates backup status."

::

   {
      "snapshots" : [
         {
            "volume_id" : "f15dcbfb-8b41-4fff-adb8-77a4162a318b",
            "status" : "available",
            "display_description" : null,
            "display_name" : null,
            "size" : 1,
            "created_at" : "2015-02-27T13:19:02.380453",
            "os-extended-snapshot-attributes:project_id" : "e0741109067649a8899936e9fefda95b",
            "wrs-snapshot:backup_status" : "Export completed at 2015-02-27 13:19:48.914344",
            "id" : "7b220cb7-212f-411e-a8cd-41e6bdbac724",
            "metadata" : {},
            "os-extended-snapshot-attributes:progress" : "100%"
         },
         {
            "volume_id" : "2c4f094b-f6d8-4ff6-800e-e5998cb4d6fa",
            "status" : "available",
            "display_description" : null,
            "display_name" : null,
            "size" : 1,
            "created_at" : "2015-02-27T20:56:32.033427",
            "os-extended-snapshot-attributes:project_id" : "e0741109067649a8899936e9fefda95b",
            "wrs-snapshot:backup_status" : "Export completed at 2015-02-27 20:57:29.279574",
            "id" : "0aa45e0c-74ea-433e-b8f3-0dc778d3972b",
            "metadata" : {},
            "os-extended-snapshot-attributes:progress" : "100%"
         }
      ]
   }

***********************************************
Get information of a specific volume snapshot
***********************************************

.. rest_method:: GET /v2/​{tenant_id}​/snapshots/​{snapshot_id}​

Preconditions

-  The specified volume snapshot must exist in all case.

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."
   "snapshot_id", "URI", "csapi:UUID", "The ID for the snapshot to list."

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "wrs-snapshot:backup_status", "plain", "xsd:string", "Indicates backup status."

::

   {
      "snapshot" : {
         "volume_id" : "2c4f094b-f6d8-4ff6-800e-e5998cb4d6fa",
         "status" : "available",
         "display_description" : null,
         "display_name" : null,
         "size" : 1,
         "created_at" : "2015-02-27T20:56:32.033427",
         "os-extended-snapshot-attributes:project_id" : "e0741109067649a8899936e9fefda95b",
         "wrs-snapshot:backup_status" : "Export completed at 2015-02-27 20:57:29.279574",
         "id" : "0aa45e0c-74ea-433e-b8f3-0dc778d3972b",
         "metadata" : {},
         "os-extended-snapshot-attributes:progress" : "100%"
      }
   }

***************************************************************************
Executes the specified action or command on the specified volume snapshot
***************************************************************************

.. rest_method:: POST /v2/​{tenant_id}​/snapshots/​{snapshot_id}​/action

Preconditions

-  The specified volume snapshot must exist in all case.

**Normal response codes**

200

**Request parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "tenant_id", "URI", "csapi:UUID", "The ID for the tenant or account in a multi-tenancy cloud."
   "snapshot_id", "URI", "csapi:UUID", "The ID for the snapshot to list."
   "wrs-snapshot:os-snapshot_export", "plain", "xsd:string", "Export volume snapshot to a file"

**Response parameters**

.. csv-table::
   :header: "Parameter", "Style", "Type", "Description"
   :widths: 20, 20, 20, 60

   "volume_type", "plain", "xsd:string", "Indicates the volume type."
   "updated_at", "plain", "xsd:string", "Indicates when the action was performed."
   "status", "plain", "xsd:string", "Indicates the state of the volume snapshot export action."
   "id", "plain", "csapi:UUID", "Indicates the volume UUID."
   "display_description", "plain", "xsd:string", "Volume descrition if any."
   "volume_size", "plain", "xsd:int", "Indicates the volume size in Gbyte."

::

   {
      "wrs-snapshot:os-export_snapshot" : {
         "volume_type" : null,
         "updated_at" : "2015-03-03T15:32:31.386661",
         "status" : "exporting",
         "volume_size" : 1,
         "id" : "9ad36199-c5b3-44bf-9273-c298ab7a0a2b",
         "display_description" : null
      }
   }


