---
title: "Custom roles for SQL Server to Azure SQL Database migrations in Azure Data Studio"
titleSuffix: Azure Database Migration Service
description: Learn how to use custom roles for SQL Server to Azure SQL Database migrations in Azure Data Studio.
author: abhims14
ms.author: abhishekum
ms.reviewer: randolphwest
ms.date: 06/26/2024
ms.service: dms
ms.topic: conceptual
ms.custom:
  - sql-migration-content
---

# Custom roles for SQL Server to Azure SQL Database migrations in Azure Data Studio

This article explains how to set up a custom role in Azure for SQL Server database migrations. The custom role only has the permissions necessary to create and run an instance of Azure Database Migration Service with Azure SQL Database as a target.

Use the `AssignableScopes` section of the role definition JSON string to control where the permissions appear in the **Add role assignment** UI in the Azure portal. To avoid cluttering the UI with extra roles, you might want to define the role at the level of the resource group, or even the level of the resource. The resource that the custom role applies to doesn't perform the actual role assignment.

```json
{
    "properties": {
        "roleName": "DmsCustomRoleDemoForSqlDB",
        "description": "",
        "assignableScopes": [
            "/subscriptions/<SQLDatabaseSubscription>/resourceGroups/<SQLDatabaseResourceGroup>",
            "/subscriptions/<DatabaseMigrationServiceSubscription>/resourceGroups/<DatabaseMigrationServiceResourceGroup>"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.Sql/servers/read",
                    "Microsoft.Sql/servers/write",
                    "Microsoft.Sql/servers/databases/read",
                    "Microsoft.Sql/servers/databases/write",
                    "Microsoft.Sql/servers/databases/delete",
                    "Microsoft.DataMigration/locations/operationResults/read",
                    "Microsoft.DataMigration/locations/operationStatuses/read",
                    "Microsoft.DataMigration/locations/sqlMigrationServiceOperationResults/read",
                    "Microsoft.DataMigration/databaseMigrations/write",
                    "Microsoft.DataMigration/databaseMigrations/read",
                    "Microsoft.DataMigration/databaseMigrations/delete",
                    "Microsoft.DataMigration/databaseMigrations/cancel/action",
                    "Microsoft.DataMigration/sqlMigrationServices/write",
                    "Microsoft.DataMigration/sqlMigrationServices/delete",
                    "Microsoft.DataMigration/sqlMigrationServices/read",
                    "Microsoft.DataMigration/sqlMigrationServices/listAuthKeys/action",
                    "Microsoft.DataMigration/sqlMigrationServices/regenerateAuthKeys/action",
                    "Microsoft.DataMigration/sqlMigrationServices/deleteNode/action",
                    "Microsoft.DataMigration/sqlMigrationServices/listMonitoringData/action",
                    "Microsoft.DataMigration/sqlMigrationServices/listMigrations/read",
                    "Microsoft.DataMigration/sqlMigrationServices/MonitoringData/read"
                ],
                "notActions": [],
                "dataActions": [],
                "notDataActions": []
            }
        ]
    }
}
```

You can use either the Azure portal, Azure PowerShell, Azure CLI, or Azure REST API to create the roles.

For more information, see [Create or update Azure custom roles using the Azure portal](/azure/role-based-access-control/custom-roles-portal) and [Azure custom roles](/azure/role-based-access-control/custom-roles).

## Permissions required to migrate to Azure SQL Database

| Permission action | Description |
| --- | --- |
| `Microsoft.Sql/servers/read` | Return the list of SQL database resources or get the properties for the specified SQL database. |
| `Microsoft.Sql/servers/write` | Create a SQL database with the specified parameters or update the properties or tags for the specified SQL database. |
| `Microsoft.Sql/servers/databases/read` | Get an existing SQL database. |
| `Microsoft.Sql/servers/databases/write` | Create a new database or update an existing database. |
| `Microsoft.Sql/servers/databases/delete` | Delete an existing SQL database. |
| `Microsoft.DataMigration/locations/operationResults/read` | Get the results of a long-running operation related to a 202 Accepted response. |
| `Microsoft.DataMigration/locations/operationStatuses/read` | Get the status of a long-running operation related to a 202 Accepted response. |
| `Microsoft.DataMigration/locations/sqlMigrationServiceOperationResults/read` | Retrieve service operation results. |
| `Microsoft.DataMigration/databaseMigrations/write` | Create or update a database migration resource. |
| `Microsoft.DataMigration/databaseMigrations/read` | Retrieve a database migration resource. |
| `Microsoft.DataMigration/databaseMigrations/delete` | Delete a database migration resource. |
| `Microsoft.DataMigration/databaseMigrations/cancel/action` | Stop ongoing migration for the database. |
| `Microsoft.DataMigration/sqlMigrationServices/write` | Create a new service or change the properties of an existing service. |
| `Microsoft.DataMigration/sqlMigrationServices/delete` | Delete an existing service. |
| `Microsoft.DataMigration/sqlMigrationServices/read` | Retrieve the details of the migration service. |
| `Microsoft.DataMigration/sqlMigrationServices/listAuthKeys/action` | Retrieve the list of authentication keys. |
| `Microsoft.DataMigration/sqlMigrationServices/regenerateAuthKeys/action` | Regenerate authentication keys. |
| `Microsoft.DataMigration/sqlMigrationServices/deleteNode/action` | Deregister the integration runtime node. |
| `Microsoft.DataMigration/sqlMigrationServices/listMonitoringData/action` | List the monitoring data for all migrations. |
| `Microsoft.DataMigration/sqlMigrationServices/listMigrations/read` | Lists the migrations for the user. |
| `Microsoft.DataMigration/sqlMigrationServices/MonitoringData/read` | Retrieve the monitoring data. |

You can get a sample script to create a login and provision it with the necessary permissions, for [VMware](/azure/migrate/migrate-support-matrix-vmware?pivots=sql-server-instance-database-discovery-requirements#sql-server-instance-and-database-discovery-requirements), [Hyper-V](/azure/migrate/migrate-support-matrix-hyper-v#sql-server-instance-and-database-discovery-requirements), or [physical servers](/azure/migrate/migrate-support-matrix-physical#sql-server-instance-and-database-discovery-requirements), using Windows authentication or SQL Server authentication.

## Role assignment

To assign a role to a user or an app ID:

1. In the Azure portal, go to the resource.

1. In the left menu, select **Access control (IAM)**, and then scroll to find the custom roles you created.

1. Select the roles to assign, select the user or app ID, and then save the changes.

   The user or app ID now appears on the **Role assignments** tab.

## Related content

- [Database Migration Guide](../../index.yml)
