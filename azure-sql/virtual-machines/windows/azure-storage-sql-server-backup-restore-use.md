---
title: How to use Azure Storage for SQL Server backup and restore
description: Learn how to back up SQL Server to Azure Storage. Explains the benefits of backing up SQL databases to Azure Storage.
author: adbadram
ms.author: adbadram
ms.reviewer: mathoma
ms.date: 06/18/2024
ms.service: virtual-machines-sql
ms.subservice: backup
ms.topic: conceptual
tags: azure-service-management
---
# Use Azure Storage for SQL Server backup and restore
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Starting with SQL Server 2012 SP1 CU2, you can now write back up SQL Server databases directly to Azure Blob storage. Use this functionality to back up to and restore from Azure Blob storage. Back up to the cloud offers benefits of availability, limitless geo-replicated off-site storage, and ease of migration of data to and from the cloud. You can issue `BACKUP` or `RESTORE` statements by using Transact-SQL or SMO.

## Overview
SQL Server 2016 introduces new capabilities; you can use [file-snapshot backup](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) to perform nearly instantaneous backups and incredibly quick restores.

This topic explains why you might choose to use Azure Storage for SQL Server backups and then describes the components involved. You can use the resources provided at the end of the article to access walk-throughs and additional information to start using this service with your SQL Server backups.

## Benefits of using Azure Blob storage for SQL Server backups
There are several challenges that you face when backing up SQL Server. These challenges include storage management, risk of storage failure, access to off-site storage, and hardware configuration. Many of these challenges are addressed by using Azure Blob storage for SQL Server backups. Consider the following benefits:

* **Ease of use**: Storing your backups in Azure blobs can be a convenient, flexible, and easy to access off-site option. Creating off-site storage for your SQL Server backups can be as easy as modifying your existing scripts/jobs to use the **BACKUP TO URL** syntax. Off-site storage should typically be far enough from the production database location to prevent a single disaster that might impact both the off-site and production database locations. By choosing to [geo-replicate your Azure blobs](/azure/storage/common/storage-redundancy), you have an extra layer of protection in the event of a disaster that could affect the whole region.
* **Backup archive**: Azure Blob storage offers a better alternative to the often used tape option to archive backups. Tape storage might require physical transportation to an off-site facility and measures to protect the media. Storing your backups in Azure Blob storage provides an instant, highly available, and a durable archiving option.
* **Managed hardware**: There is no overhead of hardware management with Azure services. Azure services manage the hardware and provide geo-replication for redundancy and protection against hardware failures.
* **Unlimited storage**: By enabling a direct backup to Azure blobs, you have access to virtually unlimited storage. Alternatively, backing up to an Azure virtual machine disk has limits based on machine size. There is a limit to the number of disks you can attach to an Azure virtual machine for backups. This limit is 16 disks for an extra large instance and fewer for smaller instances.
* **Backup availability**: Backups stored in Azure blobs are available from anywhere and at any time and can easily be accessed for restores to a SQL Server instance, without the need for database attach/detach or downloading and attaching the VHD.
* **Cost**: Pay only for the service that is used. Can be cost-effective as an off-site and backup archive option. See the [Azure pricing calculator](https://go.microsoft.com/fwlink/?LinkId=277060 "Pricing Calculator"), and the [Azure Pricing article](https://go.microsoft.com/fwlink/?LinkId=277059 "Pricing article") for more information.
* **Storage snapshots**: When database files are stored in an Azure blob and you are using SQL Server 2016, you can use [file-snapshot backup](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) to perform nearly instantaneous backups and incredibly quick restores.

For more details, see [SQL Server Backup and Restore with Azure Blob storage](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service).

The following two sections introduce Azure Blob storage, including the required SQL Server components. It is important to understand the components and their interaction to successfully use backup and restore from Azure Blob storage.

## Azure Blob storage components
The following Azure components are used when backing up to Azure Blob storage.

| Component | Description |
| --- | --- |
| **Storage account** |The storage account is the starting point for all storage services. To access Azure Blob storage, first create an Azure Storage account. SQL Server is agnostic to the type of storage redundancy used. Backup to Page blobs and block blobs is supported for every storage redundancy (LRS\ZRS\GRS\RA-GRS\RA-GZRS\etc.). For more information about Azure Blob storage, see [How to use Azure Blob storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet). |
| **Container** |A container provides a grouping of a set of blobs, and can store an unlimited number of Blobs. To write a SQL Server backup to Azure Blob storage, you must have at least the root container created. |
| **Blob** |A file of any type and size. Blobs are addressable using the following URL format: `https://<storageaccount>.blob.core.windows.net/<container>/<blob>`. For more information about page Blobs, see [Understanding Block and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) |

## SQL Server components
The following SQL Server components are used when backing up to Azure Blob storage.

| Component | Description |
| --- | --- |
| **URL** |A URL specifies a Uniform Resource Identifier (URI) to a unique backup file. The URL provides the location and name of the SQL Server backup file. The URL must point to an actual blob, not just a container. If the blob does not exist, Azure creates it. If an existing blob is specified, the backup command fails, unless the `WITH FORMAT` option is specified. The following is an example of the URL you would specify in the BACKUP command: `https://<storageaccount>.blob.core.windows.net/<container>/<FILENAME.bak>`.<br><br> HTTPS is recommended but not required. |
| **Credential** |The information that is required to connect and authenticate to Azure Blob storage is stored as a credential. In order for SQL Server to write backups to an Azure Blob or restore from it, a SQL Server credential must be created. For more information, see [SQL Server Credential](/sql/t-sql/statements/create-credential-transact-sql). |

> [!NOTE]
> SQL Server 2016 has been updated to support block blobs. Please see [Tutorial: Use Microsoft Azure Blob Storage with SQL Server databases](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) for more details.
> 

## Next steps

1. Create an Azure account if you don't already have one. If you are evaluating Azure, consider the [free trial](https://azure.microsoft.com/free/).
2. Then go through one of the following tutorials that walk you through creating a storage account and performing a restore.
   
   * **SQL Server 2014**: [Tutorial: SQL Server 2014 Backup and Restore to Microsoft Azure Blob storage](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url).
   * **SQL Server 2016**: [Tutorial: Using the Microsoft Azure Blob Storage with SQL Server databases](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016)
3. Review additional documentation starting with [SQL Server Backup and Restore with Microsoft Azure Blob storage](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service).

If you have any problems, review the topic [SQL Server Backup to URL Best Practices and Troubleshooting](/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).

For other SQL Server backup and restore options, see [Backup and Restore for SQL Server on Azure Virtual Machines](backup-restore.md).
