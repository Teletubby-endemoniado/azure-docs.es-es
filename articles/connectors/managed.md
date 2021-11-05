---
title: Operaciones de conectores administrados
description: Use acciones y desencadenadores administrados por Microsoft para crear flujos de trabajo automatizados que integren otras aplicaciones, datos, servicios y sistemas mediante Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: conceptual
ms.date: 05/16/2021
ms.openlocfilehash: 4f0bb89992f76e7a8eb9311d733a3b791b73114a
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131086609"
---
# <a name="managed-connectors-in-azure-logic-apps"></a>Conectores administrados en Azure Logic Apps

Los [conectores administrados](apis-list.md) proporcionan maneras de obtener acceso a otros servicios y sistemas en los que los [desencadenadores y las acciones integrados](built-in.md) no están disponibles. Puede usar estos desencadenadores y acciones para crear flujos de trabajo que integren datos, aplicaciones, servicios basados en la nube y sistemas locales. En comparación con los desencadenadores y las acciones integrados, estos conectores suelen estar vinculados a un servicio o sistema específico, como Azure Blob Storage, Office 365, SQL, Salesforce o los servidores SFTP. Administrados por Microsoft y hospedados en Azure, los conectores administrados normalmente requieren que primero se cree una conexión desde el flujo de trabajo y autentique su identidad. Tanto los desencadenadores basados en la periodicidad como los basados en un webhook están disponibles, por lo que si usa un desencadenador basado en la periodicidad, revise la [Información general sobre el comportamiento de la periodicidad](apis-list.md#recurrence-behavior).

Para un pequeño número de servicios, sistemas y protocolos, Azure Logic Apps proporciona operaciones integradas junto con sus [versiones de conector administrado](managed.md). El número y el intervalo disponibles varían en función de si se crea un recurso de aplicación lógica basado en el plan de consumo que se ejecute en Azure Logic Apps multiinquilino o un recurso de aplicación lógica basado en planes estándar que se ejecute en Azure Logic Apps de un solo inquilino. Para más información, revise [Entorno de servicio de integración (ISE): inquilino único o multiinquilino](../logic-apps/single-tenant-overview-compare.md). En cambio, en la mayoría de los casos, la versión integrada proporciona un mejor rendimiento, funcionalidades y precios, entre otras cosas.

Por ejemplo, si crea una aplicación lógica de un solo inquilino, las operaciones integradas están disponibles para Azure Service Bus, Azure Event Hubs, SQL Server y MQ. En algunos casos, una versión integrada y una versión del conector administrado están disponibles. En cambio, en la mayoría de los casos, la versión integrada proporciona un mejor rendimiento, funcionalidades y precios, entre otras cosas. Si crea una aplicación lógica multiinquilino, las operaciones integradas están disponibles para Azure Functions, Azure App Services y Azure API Management.

Algunos conectores administrados de Azure Logic Apps pertenecen a varias subcategorías. Por ejemplo, el conector SAP es tanto un [conector empresarial](#enterprise-connectors) como un [conector local](#on-premises-connectors).

* Los [conectores estándar](#standard-connectors) proporcionan acceso a servicios como Azure Blob Storage, Office 365, SharePoint, Salesforce, Power BI, OneDrive y muchos más.
* Los [conectores empresariales](#enterprise-connectors) proporcionan acceso a sistemas empresariales como SAP, IBM MQ e IBM 3270.
* Los [conectores locales](#on-premises-connectors) proporcionan acceso a sistemas locales como SQL Server, SharePoint Server, SAP, Oracle DB, recursos compartidos de archivos y otros.
* Los [conectores de cuenta de integración](#integration-account-connectors) le permiten transformar y validar el código XML, codificar y descodificar archivos sin formato y procesar mensajes de negocio a negocio (B2B) con protocolos AS2, EDIFACT y X12.
* Los [conectores del entorno del servicio de integración](#ise-connectors) están diseñados para ejecutarse específicamente en un ISE y ofrecen ventajas con respecto a las versiones de estos que no son ISE.

## <a name="standard-connectors"></a>Conectores estándar

Azure Logic Apps proporciona estos conectores estándar populares para crear flujos de trabajo automatizados mediante estos servicios y sistemas. Algunos conectores estándar también admiten [sistemas locales](#on-premises-connectors) o [cuentas de integración](#integration-account-connectors).

:::row:::
    :::column:::
        [![Icono de Azure Service Bus][azure-service-bus-icon]][azure-service-bus-doc]
        \
        \
        [**Azure Service Bus**][azure-service-bus-doc]
        \
        \
        Administre mensajes asincrónicos, sesiones y suscripciones a temas con el conector más usado en Logic Apps.
    :::column-end:::
    :::column:::
        [![Icono de SQL Server][sql-server-icon]][sql-server-doc]
        \
        \
        [**SQL Server**][sql-server-doc]
        \
        \
        Conéctese a SQL Server en el entorno local o a una base de datos de Azure SQL Database en la nube para poder administrar los registros, ejecutar procedimientos almacenados o realizar consultas.
    :::column-end:::
    :::column:::
        [![Icono de Azure Blob Storage][azure-blob-storage-icon]][azure-blob-storage-doc]
        \
        \
        [**Azure Blob Storage**][azure-blob-storage-doc]
        \
        \
        Conéctese a su cuenta de Azure Storage para crear y administrar el contenido de los blobs.
    :::column-end:::
    :::column:::
        [![Icono de Office 365 Outlook][office-365-outlook-icon]][office-365-outlook-doc]
        \
        \
        [**Office 365 Outlook**][office-365-outlook-doc]
        \
        \
        Conéctese a su cuenta de correo electrónico profesional o educativa para crear y administrar correos electrónicos, tareas, eventos de calendario y reuniones, contactos, solicitudes y más.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de STFP-SSH][sftp-ssh-icon]][sftp-ssh-doc]
        \
        \
        [**STFP-SSH**][sftp-ssh-doc]
        \
        \
        Conéctese a servidores SFTP accesibles desde internet mediante SSH para trabajar con archivos y carpetas.
    :::column-end:::
    :::column:::
        [![Icono de SharePoint Online][sharepoint-online-icon]][sharepoint-online-doc]
        \
        \
        [**SharePoint Online**][sharepoint-online-doc]
        \
        \
        Conéctese a SharePoint Online para administrar archivos, datos adjuntos, carpetas y mucho más.
    :::column-end:::
    :::column:::
        [![Icono de Azure Queues
    ][azure-queues-icon]][azure-queues-doc]    \
        \
        [**Azure Queues**][azure-queues-doc]
        \
        \
        Conéctese a su cuenta de Azure Storage para poder crear y administrar colas y mensajes.
    :::column-end:::
    :::column:::
        [![Icono de FTP][ftp-icon]][ftp-doc]
        \
        \
        [**FTP**][ftp-doc]
        \
        \
        Conéctese a servidores FTP accesibles desde internet para trabajar con archivos y carpetas.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de sistema de archivos][file-system-icon]][file-system-doc]
        \
        \
        [**Sistema de archivos**][file-system-doc]
        \
        \
        Conéctese al recurso compartido de archivos local para crear y administrar archivos.
    :::column-end:::
    :::column:::
        [![Icono de Azure Event Hubs][azure-event-hubs-icon]][azure-event-hubs-doc]
        \
        \
        [**Azure Event Hubs**][azure-event-hubs-doc]
        \
        \
        Consuma y publique eventos en un centro de eventos. Por ejemplo, obtenga una salida de su aplicación lógica con Event Hubs y enviarla luego a un proveedor de análisis en tiempo real.
    :::column-end:::
    :::column:::
        [![Icono de Azure Event Grid][azure-event-grid-icon]][azure-event-grid-doc]
        \
        \
        [**Azure Event Grid**][azure-event-grid-doc]
        \
        \
        Supervise los eventos publicados por Event Grid, por ejemplo, cuando cambian los recursos de Azure o los recursos de terceros.
    :::column-end:::
    :::column:::
        [![Icono de Salesforce][salesforce-icon]][salesforce-doc]
        \
        \
        [**Salesforce**][salesforce-doc]
        \
        \
        Conéctese a su cuenta de Salesforce para crear y administrar elementos tales como registros, trabajos, objetos y mucho más.
    :::column-end:::
:::row-end:::

## <a name="on-premises-connectors"></a>Conectores locales

Para poder crear una conexión a un sistema local, primero debe [descargar, instalar y configurar una puerta de enlace de datos local][gateway-doc]. Esta puerta de enlace proporciona un canal de comunicación seguro sin tener que configurar la infraestructura de red necesaria.

Los siguientes conectores suelen ser los [conectores estándar](#standard-connectors) más usados que proporciona Azure Logic Apps para acceder a datos y recursos en sistemas locales. Consulte la lista de conectores locales en [Orígenes de datos admitidos](../logic-apps/logic-apps-gateway-connection.md#supported-connections).

:::row:::
    :::column:::
        [![Icono de Biztalk Server][biztalk-server-icon]][biztalk-server-doc]
        \
        \
        [**Biztalk Server**][biztalk-server-doc]
    :::column-end:::
    :::column:::
        [![Icono de sistema de archivos][file-system-icon]][file-system-doc]
        \
        \
        [**Sistema de archivos**][file-system-doc]
    :::column-end:::
    :::column:::
        [![Icono de IBM DB2][ibm-db2-icon]][ibm-db2-doc]
        \
        \
        [**IBM DB2**][ibm-db2-doc]
    :::column-end:::
    :::column:::
        [![Icono de IBM Informix][ibm-informix-icon]][ibm-informix-doc]
        \
        \
        [**IBM Informix**][ibm-informix-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de MySQL][mysql-icon]][mysql-doc]
        \
        \
        [**MySQL**][mysql-doc]
    :::column-end:::
    :::column:::
        [![Icono de Oracle DB][oracle-db-icon]][oracle-db-doc]
        \
        \
        [**Oracle DB**][oracle-db-doc]
    :::column-end:::
    :::column:::
        [![Icono de PostgreSQL][postgre-sql-icon]][postgre-sql-doc]
        \
        \
        [**PostgreSQL**][postgre-sql-doc]
    :::column-end:::
    :::column:::
        [![Icono de SharePoint Server][sharepoint-server-icon]][sharepoint-server-doc]
        \
        \
        [**SharePoint Server**][sharepoint-server-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de SQL Server][sql-server-icon]][sql-server-doc]
        \
        \
        [**SQL Server**][sql-server-doc]
    :::column-end:::
    :::column:::
        [![Icono de Teradata][teradata-icon]][teradata-doc]
        \
        \
        [**Teradata**][teradata-doc]
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Conectores de la cuenta de integración

Los conectores de cuentas de integración admiten específicamente [escenarios de comunicación de negocio a negocio (B2B)](../logic-apps/logic-apps-enterprise-integration-overview.md) en Azure Logic Apps. Después de [crear una cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) y definir los artefactos B2B, como asociados comerciales, acuerdos, mapas y esquemas, puede usar los conectores de las cuentas de integración para codificar y descodificar mensajes, transformar contenido, etc.

Por ejemplo, si usa Microsoft BizTalk Server, puede crear una conexión desde el flujo de trabajo mediante el [conector local de BizTalk Server](#on-premises-connectors). A continuación, puede ampliar o realizar operaciones de BizTalk en el flujo de trabajo mediante los conectores de la cuenta de integración.

> [!NOTE]
> Para poder usar conectores de cuenta de integración en una instancia de Azure Logic Apps multiinquilino basada en planes de consumo, debe [vincular el recurso de la aplicación lógica a una cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md). 

:::row:::
    :::column:::
        [![Icono de descodificación de AS2][as2-icon]][as2-doc]
        \
        \
        [**descodificación de AS2**][as2-doc]
    :::column-end:::
    :::column:::
        [![Icono de codificación de AS2][as2-icon]][as2-doc]
        \
        \
        [**Codificación de AS2**][as2-doc]
    :::column-end:::
    :::column:::
        [![Icono de descodificación de EDIFACT][edifact-icon]][edifact-decode-doc]
        \
        \
        [**Descodificación de EDIFACT**][edifact-decode-doc]
    :::column-end:::
    :::column:::
        [![Icono de codificación de EDIFACT][edifact-icon]][edifact-encode-doc]
        \
        \
        [**Codificación de EDIFACT**][edifact-encode-doc]
    :::column-end:::
    :::column:::
        [![Icono de descodificación de X12][x12-icon]][x12-decode-doc]
        \
        \
        [**Descodificación de X12**][x12-decode-doc]
    :::column-end:::
    :::column:::
        [![Icono de codificación de X12][x12-icon]][x12-encode-doc]
        \
        \
        [**Codificación de X12**][x12-encode-doc]
    :::column-end:::
:::row-end:::

## <a name="enterprise-connectors"></a>Conectores de empresa

Los conectores siguientes proporcionan acceso a los sistemas empresariales por un costo adicional:

:::row:::
    :::column:::
        [![Icono de IBM 3270][ibm-3270-icon]][ibm-3270-doc]
        \
        \
        [**IBM 3270**][ibm-3270-doc]
    :::column-end:::
    :::column:::
        [![Icono de IBM MQ][ibm-mq-icon]][ibm-mq-doc]
        \
        \
        [**IBM MQ**][ibm-mq-doc]
    :::column-end:::
    :::column:::
        [![Icono de SAP][sap-icon]][sap-connector-doc]
        \
        \
        [**SAP**][sap-connector-doc]
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="ise-connectors"></a>Conectores del ISE

En un entorno deL servicio de integración (ISE), estos conectores administrados también tienen [versiones de ISE](apis-list.md#ise-and-connectors), que tienen funcionalidades diferentes a las de sus versiones multiinquilino:

> [!NOTE]
> Las aplicaciones lógicas que se ejecutan en un ISE, así como sus conectores, independientemente de la ubicación en que se ejecuten dichos conectores, usan un plan de precios fijo frente a un plan de precios basado en el consumo. Para más información, vea [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md) y [Detalles de precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

:::row:::
    :::column:::
        [![Icono de ISE de AS2][as2-icon]][as2-doc]
        \
        \
        [**ISE de** AS2][as2-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Automation][azure-automation-icon]][azure-automation-doc]
        \
        \
        [**ISE de** Azure Automation][azure-automation-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Blob Storage][azure-blob-storage-icon]][azure-blob-storage-doc]
        \
        \
        [**ISE de** Azure Blob Storage][azure-blob-storage-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Cosmos DB][azure-cosmos-db-icon]][azure-cosmos-db-doc]
        \
        \
        [**ISE de** Azure Cosmos DB][azure-cosmos-db-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de ISE de Azure Event Hubs][azure-event-hubs-icon]][azure-event-hubs-doc]
        \
        \
        [**ISE de** Azure Event Hubs][azure-event-hubs-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Event Grid][azure-event-grid-icon]][azure-event-grid-doc]
        \
        \
        [**ISE de** Azure Event Grid][azure-event-grid-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Files][azure-file-storage-icon]][azure-file-storage-doc]
        \
        \
        [**ISE de**  Azure Files][azure-file-storage-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Key Vault][azure-key-vault-icon]][azure-key-vault-doc]
        \
        \
        [**ISE de** Azure Key Vault][azure-key-vault-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de ISE de registros de Azure Monitor][azure-monitor-logs-icon]][azure-monitor-logs-doc]
        \
        \
        [**ISE de** registros de Azure Monitor][azure-monitor-logs-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Service Bus][azure-service-bus-icon]][azure-service-bus-doc]
        \
        \
        [**ISE de** Azure Service Bus][azure-service-bus-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Synapse Analytics][azure-sql-data-warehouse-icon]][azure-sql-data-warehouse-doc]
        \
        \
        [**ISE de** Azure Synapse Analytics][azure-sql-data-warehouse-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de Azure Table Storage][azure-table-storage-icon]][azure-table-storage-doc]
        \
        \
        [**ISE de** Azure Table Storage][azure-table-storage-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de ISE de Azure Queues][azure-queues-icon]][azure-queues-doc]
        \
        \
        [**ISE de** Azure Queues][azure-queues-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de EDIFACT][edifact-icon]][edifact-doc]
        \
        \
        [**ISE de** EDIFACT][edifact-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE del sistema de archivos][file-system-icon]][file-system-doc]
        \
        \
        [**ISE del** Sistema de archivos][file-system-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de FTP][ftp-icon]][ftp-doc]
        \
        \
        [**ISE de** FTP][ftp-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de ISE de IBM 3270][ibm-3270-icon]][ibm-3270-doc]
        \
        \
        [**ISE de** IBM 3270][ibm-3270-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de IBM DB2][ibm-db2-icon]][ibm-db2-doc]
        \
        \
        [**ISE de** IBM DB2][ibm-db2-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de IBM MQ][ibm-mq-icon]][ibm-mq-doc]
        \
        \
        [**ISE de** IBM MQ][ibm-mq-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de SAP][sap-icon]][sap-connector-doc]
        \
        \
        [**ISE de** SAP][sap-connector-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Icono de ISE de SFTP-SSH][sftp-ssh-icon]][sftp-ssh-doc]
        \
        \
        [**ISE de** SFTP-SSH][sftp-ssh-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de SMTP][smtp-icon]][smtp-doc]
        \
        \
        [**ISE de** SMTP][smtp-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de SQL Server][sql-server-icon]][sql-server-doc]
        \
        \
        [**ISE de** SQL Server][sql-server-doc]
    :::column-end:::
    :::column:::
        [![Icono de ISE de X12][x12-icon]][x12-doc]
        \
        \
        [**ISE de** X12][x12-doc]
    :::column-end:::
:::row-end:::

Para más información, consulte los temas siguientes:

* [Acceso a recursos de Azure Virtual Network desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md)
* [Conexión a redes virtuales de Azure desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de API personalizadas que se pueden llamar desde Logic Apps](../logic-apps/logic-apps-create-api-app.md)

<!--Managed connector icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-cosmos-db-icon]: ./media/apis-list/azure-cosmos-db.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-devops-icon]: ./media/apis-list/azure-devops.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-file-storage-icon]: ./media/apis-list/azure-file-storage.png
[azure-key-vault-icon]: ./media/apis-list/azure-key-vault.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-monitor-logs-icon]: ./media/apis-list/azure-monitor-logs.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[azure-sql-data-warehouse-icon]: ./media/apis-list/azure-sql-data-warehouse.png
[azure-table-storage-icon]: ./media/apis-list/azure-table-storage.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-ssh-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[twilio-icon]: ./media/apis-list/twilio.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[youtube-icon]: ./media/apis-list/youtube.png

<!--Managed connector doc links-->
[azure-automation-doc]: /connectors/azureautomation/ "Creación y administración de trabajos de Automation para su infraestructura local y en la nube"
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Administrar archivos del contenedor de blobs con el conector de Azure Blob Storage"
[azure-cosmos-db-doc]: ./connectors-create-api-cosmos-db.md "Conectar a Azure Cosmos DB para que pueda acceder a los documentos de Azure Cosmos DB y administrarlos"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Supervisar los eventos publicados por Event Grid, por ejemplo, cuando hay cambios en los recursos de Azure o de terceros"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Conectarse a Azure Event Hubs para recibir y enviar eventos entre aplicaciones lógicas y Event Hubs"
[azure-file-storage-doc]: /connectors/azurefile/ "Conectarse a su cuenta de Azure Storage para crear, actualizar, obtener y eliminar archivos"
[azure-key-vault-doc]: /connectors/keyvault/ "Conexión a la instancia de Azure Key Vault para administrar sus secretos y claves"
[azure-monitor-logs-doc]: /connectors/azuremonitorlogs/ "Ejecución de consultas en los registros de Azure Monitor de las áreas de trabajo de Log Analytics y los componentes de Application Insights"
[azure-queues-doc]: /connectors/azurequeues/ "Conectarse a su cuenta de Azure Storage para crear y administrar colas y mensajes"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Administrar mensajes de Service Bus, temas y suscripciones a temas"
[azure-sql-data-warehouse-doc]: /connectors/sqldw/ "Conectarse a Azure Synapse Analytics para visualizar los datos"
[azure-table-storage-doc]: /connectors/azuretables/ "Conectarse a su cuenta de Azure Storage para crear, actualizar y consultar tablas y más"
[biztalk-server-doc]: /connectors/biztalk/ "Conectarse a su instancia de BizTalk Server para ejecutar aplicaciones basadas en BizTalk en paralelo con Azure Logic Apps"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Conectar con un sistema de archivos local"
[ftp-doc]: ./connectors-create-api-ftp.md "Conectar con un servidor FTP/FTPS para tareas de FTP, incluidas la carga, la obtención y la eliminación de archivos, entre otras"
[github-doc]: ./connectors-create-api-github.md "Conectar con GitHub y realizar un seguimiento de los problemas"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Conectarse con Google Calendar y administrar el calendario"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Conectarse con Google Sheets para modificar sus hojas"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Conectarse con Google Tasks para administrar las tareas"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "Conexión a aplicaciones de 3270 en sistemas centrales de IBM"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Conectar con una instancia de IBM DB2 en la nube o local. Actualizar una fila, obtener una tabla, etc."
[ibm-informix-doc]: ./connectors-create-api-informix.md "Conectar con una instancia de Informix en la nube o local. Leer una fila, enumerar las tablas, etc."
[ibm-mq-doc]: ./connectors-create-api-mq.md "Conectarse a IBM MQ local o en Azure y enviar y recibir mensajes"
[instagram-doc]: ./connectors-create-api-instagram.md "Conectar con Instagram. Desencadenar eventos o realizar acciones en ellos"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Conectar con Mandrill para realizar comunicaciones"
[mysql-doc]: /connectors/mysql/ "Conectarse con su base de datos de MySQL local para que pueda leer y escribir datos"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Conectarse a su cuenta profesional o educativa para enviar y recibir correos electrónicos, administrar su calendario y contactos y mucho más"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Conectarse a la instancia personal de Microsoft OneDrive para cargar, eliminar, mostrar archivos y mucho más"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Conectarse a la instancia de Microsoft OneDrive de su empresa para cargar, eliminar, mostrar archivos y mucho más"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Conectarse a una base de datos de Oracle para agregar, insertar, eliminar filas y mucho más"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Conectarse a su buzón de correo de Outlook para administrar sus correos, calendarios, contactos y mucho más"
[postgre-sql-doc]: /connectors/postgresql/ "Conectarse a su base de datos de PostgreSQL para leer datos de las tablas"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Conectar con su cuenta de Salesforce. Administrar cuentas, clientes potenciales, oportunidades, etc."
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Conectar con un servidor SAP local"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Conectar con SendGrid. Enviar correo electrónico y administrar listas de destinatarios"
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "Conectarse con su cuenta SFTP mediante SSH. Cargar, obtener y eliminar archivos, entre otras operaciones."
[sharepoint-server-doc]: ./connectors-create-api-sharepoint.md "Conectarse al servidor local de SharePoint. Administrar documentos, elementos de lista, etc."
[sharepoint-online-doc]: ./connectors-create-api-sharepoint.md "Conectar con SharePoint Online. Administrar documentos, elementos de lista, etc."
[slack-doc]: ./connectors-create-api-slack.md "Conectar con Slack y publicar mensajes en los canales de Slack"
[smtp-doc]: ./connectors-create-api-smtp.md "Conectar con un servidor SMTP y enviar correo con datos adjuntos"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Conectar con SparkPost para la comunicación"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Conectar con Azure SQL Database o SQL Server. Crear, actualizar, obtener y eliminar entradas en una tabla de base de datos SQL"
[teradata-doc]: /connectors/teradata/ "Conectarse a su base de datos de Teradata para leer datos de las tablas"
[twilio-doc]: ./connectors-create-api-twilio.md "Conectar con Twilio. Enviar y recibir mensajes, obtener los números disponibles, administrar los números de teléfono entrantes y mucho más."
[youtube-doc]: ./connectors-create-api-youtube.md "Conectar con YouTube. Administrar sus vídeos y canales"

<!--Integration account connector icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[x12-icon]: ./media/apis-list/x12.png

<!-- Integration account connector docs -->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Codificar y descodificar mensajes que usan el protocolo AS2"
[edifact-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "Codificar y descodificar mensajes que usan el protocolo EDIFACT"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "Descodificar mensajes que usan el protocolo EDIFACT"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "Codificar mensajes que usan el protocolo EDIFACT"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Codificar y descodificar mensajes que usan el protocolo X12"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Descodificar mensajes que usan el protocolo X12"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Codificar mensajes que usan el protocolo X12"

<!--Other doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Conexión a orígenes de datos locales desde aplicaciones lógicas con una puerta de enlace de datos local"