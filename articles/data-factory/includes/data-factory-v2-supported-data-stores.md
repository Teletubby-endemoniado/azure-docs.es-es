---
title: Archivo de inclusión
description: archivo de inclusión
services: data-factory
author: jianleishen
ms.service: data-factory
ms.topic: include
ms.date: 09/29/2021
ms.author: jianleishen
ms.custom: include file
ms.openlocfilehash: 6bb98eee64707f43e486180f2a221689de1dc774
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132353880"
---
| Category | Almacén de datos | Se admite como origen | Se admite como receptor | Compatible con [IR de Azure](../concepts-integration-runtime.md#azure-integration-runtime) | Compatible con [IR autohospedado](../concepts-integration-runtime.md#self-hosted-integration-runtime) |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **Azure** |[Almacenamiento de blobs de Azure](../connector-azure-blob-storage.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Índice de Azure Cognitive Search](../connector-azure-search.md) | |✓ |✓ |✓  |
| &nbsp; |[Azure Cosmos DB (API de SQL)](../connector-azure-cosmos-db.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[API de Azure Cosmos DB para MongoDB](../connector-azure-cosmos-db-mongodb-api.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Data Explorer](../connector-azure-data-explorer.md) |✓ |✓ |✓ |✓ |
| &nbsp; |[Azure Data Lake Storage Gen1](../connector-azure-data-lake-store.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Data Lake Storage Gen2](../connector-azure-data-lake-storage.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Database for MariaDB](../connector-azure-database-for-mariadb.md) |✓ | |✓ |✓  |
| &nbsp; |[Azure Database for MySQL](../connector-azure-database-for-mysql.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Database para PostgreSQL](../connector-azure-database-for-postgresql.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Databricks Delta Lake](../connector-azure-databricks-delta-lake.md) |✓ |✓ |✓ |✓ |
| &nbsp; |[Archivos de Azure](../connector-azure-file-storage.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure SQL Database](../connector-azure-sql-database.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Instancia administrada de Azure SQL](../../azure-sql/managed-instance/sql-managed-instance-paas-overview.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Synapse Analytics](../connector-azure-sql-data-warehouse.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Table Storage](../connector-azure-table-storage.md) |✓ |✓ |✓ |✓  |
| **Base de datos** |[Amazon RDS para Oracle](../connector-amazon-rds-for-oracle.md) |✓ | |✓ |✓  |
| &nbsp; |[Amazon RDS para SQL Server](../connector-amazon-rds-for-sql-server.md) |✓ | |✓ |✓  |
| &nbsp; |[Amazon Redshift](../connector-amazon-redshift.md) |✓ | |✓ |✓  |
| &nbsp; |[DB2](../connector-db2.md) |✓ | |✓ |✓  |
| &nbsp; |[Drill](../connector-drill.md) |✓ | |✓ |✓  |
| &nbsp; |[Google BigQuery](../connector-google-bigquery.md) |✓ | |✓ |✓  |
| &nbsp; |[Greenplum](../connector-greenplum.md) |✓ | |✓ |✓  |
| &nbsp; |[HBase](../connector-hbase.md) |✓ | |✓ |✓  |
| &nbsp; |[Hive](../connector-hive.md) |✓ | |✓ |✓  |
| &nbsp; |[Apache Impala](../connector-impala.md) |✓ | |✓ |✓  |
| &nbsp; |[Informix](../connector-informix.md) |✓ |✓ | |✓  |
| &nbsp; |[MariaDB](../connector-mariadb.md) |✓ | |✓ |✓  |
| &nbsp; |[Microsoft Access](../connector-microsoft-access.md) |✓ |✓ | |✓  |
| &nbsp; |[MySQL](../connector-mysql.md) |✓ | |✓ |✓  |
| &nbsp; |[Netezza](../connector-netezza.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle](../connector-oracle.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Phoenix](../connector-phoenix.md) |✓ | |✓ |✓  |
| &nbsp; |[PostgreSQL](../connector-postgresql.md) |✓ | |✓ |✓  |
| &nbsp; |[Presto](../connector-presto.md) |✓ | |✓ |✓  |
| &nbsp; |[SAP Business Warehouse a través de Open Hub](../connector-sap-business-warehouse-open-hub.md) |✓ | | |✓  |
| &nbsp; |[SAP Business Warehouse a través de expresión multidimensional](../connector-sap-business-warehouse.md) |✓ | | |✓  |
| &nbsp; |[SAP HANA](../connector-sap-hana.md) |✓ | Receptor compatible solo con el [conector ODBC y el SAP HANA ODBC](../connector-sap-hana.md#sap-hana-sink) | |✓  |
| &nbsp; |[SAP Table](../connector-sap-table.md) |✓ | | |✓  |
| &nbsp; |[Snowflake](../connector-snowflake.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Spark](../connector-spark.md) |✓ | |✓ |✓  |
| &nbsp; |[SQL Server](../connector-sql-server.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Sybase](../connector-sybase.md) |✓ | | |✓  |
| &nbsp; |[Teradata](../connector-teradata.md) |✓ | |✓ |✓  |
| &nbsp; |[Vertica](../connector-vertica.md) |✓ | |✓ |✓  |
| **NoSQL** |[Cassandra](../connector-cassandra.md) |✓ | |✓ |✓  |
| &nbsp; |[Couchbase (versión preliminar)](../connector-couchbase.md) |✓ | |✓ |✓  |
| &nbsp; |[MongoDB](../connector-mongodb.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[MongoDB Atlas](../connector-mongodb-atlas.md) |✓ |✓ |✓ |✓  |
| **Archivo** |[Amazon S3](../connector-amazon-simple-storage-service.md) |✓ | |✓ |✓  |
| &nbsp; |[Almacenamiento compatible con Amazon S3](../connector-amazon-s3-compatible-storage.md) |✓ | |✓ |✓  |
| &nbsp; |[Sistema de archivos](../connector-file-system.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[FTP](../connector-ftp.md) |✓ | |✓ |✓  |
| &nbsp; |[Google Cloud Storage](../connector-google-cloud-storage.md) |✓ | |✓ |✓  |
| &nbsp; |[HDFS](../connector-hdfs.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle Cloud Storage](../connector-oracle-cloud-storage.md) |✓ | |✓ |✓  |
| &nbsp; |[SFTP](../connector-sftp.md) |✓ |✓ |✓ |✓  |
| **Protocolo genérico** |[HTTP genérico](../connector-http.md) |✓ | |✓ |✓  |
| &nbsp; |[OData genérico](../connector-odata.md) |✓ | |✓ |✓  |
| &nbsp; |[ODBC genérico](../connector-odbc.md) |✓ |✓ | |✓  |
| &nbsp; |[REST genérico](../connector-rest.md) |✓ | ✓ |✓ |✓  |
| **Servicios y aplicaciones** |[Servicio web de Amazon Marketplace](../connector-amazon-marketplace-web-service.md) |✓ | |✓ |✓  |
| &nbsp; |[Concur (versión preliminar)](../connector-concur.md) |✓ | |✓ |✓  |
| &nbsp; |[Dataverse](../connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Dynamics 365](../connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Dynamics AX](../connector-dynamics-ax.md) |✓ | |✓ |✓  |
| &nbsp; |[Dynamics CRM](../connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Google AdWords](../connector-google-adwords.md) |✓ | |✓ |✓  |
| &nbsp; |[HubSpot](../connector-hubspot.md) |✓ | |✓ |✓  |
| &nbsp; |[Jira](../connector-jira.md) |✓ | |✓ |✓  |
| &nbsp; |[Magento (versión preliminar)](../connector-magento.md) |✓ | |✓ |✓  |
| &nbsp; |[Marketo (versión preliminar)](../connector-marketo.md) |✓ | |✓ |✓  |
| &nbsp; |[Microsoft 365](../connector-office-365.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle Eloqua (versión preliminar)](../connector-oracle-eloqua.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle Responsys (versión preliminar)](../connector-oracle-responsys.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle Service Cloud (versión preliminar)](../connector-oracle-service-cloud.md) |✓ | |✓ |✓  |
| &nbsp; |[PayPal (versión preliminar)](../connector-paypal.md) |✓ | |✓ |✓  |
| &nbsp; |[QuickBooks (versión preliminar)](../connector-quickbooks.md) |✓ | |✓ |✓  |
| &nbsp; |[Salesforce](../connector-salesforce.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Servicio de Salesforce en la nube](../connector-salesforce-service-cloud.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Nube de marketing de Salesforce](../connector-salesforce-marketing-cloud.md) |✓ | |✓ |✓  |
| &nbsp; |[SAP Cloud for Customer (C4C)](../connector-sap-cloud-for-customer.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[SAP ECC](../connector-sap-ecc.md) |✓ | |✓ |✓ |
| &nbsp; |[ServiceNow](../connector-servicenow.md) |✓ | |✓ |✓  |
|  |[Lista de SharePoint Online](../connector-sharepoint-online-list.md) |✓ | |✓ |✓ |
| &nbsp; |[Shopify (versión preliminar)](../connector-shopify.md) |✓ | |✓ |✓  |
| &nbsp; |[Square (versión preliminar)](../connector-square.md) |✓ | |✓ |✓  |
| &nbsp; |[Tabla web (tabla HTML)](../connector-web-table.md) |✓ | | |✓  |
| &nbsp; |[Xero](../connector-xero.md) |✓ | |✓ |✓  |
| &nbsp; |[Zoho (versión preliminar)](../connector-zoho.md) |✓ | |✓ |✓  |

> [!NOTE]
> Si un conector está marcado como *versión preliminar*, puede probarlo y enviarnos sus comentarios. Si quiere tener una dependencia de los conectores de versión preliminar de la solución, póngase en contacto con el [servicio de soporte técnico de Azure](https://azure.microsoft.com/support/).
