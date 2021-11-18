---
title: Protección contra amenazas avanzada
titleSuffix: Azure SQL Database, SQL Managed Instance, & Azure Synapse Analytics
description: Advanced Threat Protection detecta actividades anómalas en la base de datos que indican posibles amenazas de seguridad en Azure SQL Database, Azure SQL Managed Instance y Azure Synapse Analytics.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.custom: sqldbrb=2
ms.topic: conceptual
author: davidtrigano
ms.author: datrigan
ms.reviewer: vanto, sstein
ms.date: 06/09/2021
tags: azure-synapse
ms.openlocfilehash: 5377af3720f8666f23bfc956c86fdc5df667b432
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132301862"
---
# <a name="sql-advanced-threat-protection"></a>SQL Advanced Threat Protection
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)] :::image type="icon" source="../media/applies-to/yes.png" border="false":::SQL Server en máquina virtual de Azure :::image type="icon" source="../media/applies-to/yes.png" border="false":::SQL Server habilitado para Azure Arc

Advanced Threat Protection para [Azure SQL Database](sql-database-paas-overview.md), [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md), [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md), [SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) y [SQL Server habilitado para Azure Arc](/sql/sql-server/azure-arc/overview) detecta actividades anómalas que indican intentos inusuales y potencialmente perjudiciales de aprovechar vulnerabilidades de bases de datos o acceder a ellas.

Protección contra amenazas avanzada forma parte de la oferta [Microsoft Defender para SQL](../../security-center/defender-for-sql-introduction.md), que es un paquete unificado de funcionalidades de seguridad avanzadas de SQL. Se puede acceder a Protección contra amenazas avanzada y administrarlo a través del portal central de Microsoft Defender para SQL.

## <a name="overview"></a>Información general

Advanced Threat Protection proporciona una nueva capa de seguridad, que permite a los clientes detectar amenazas potenciales y responder a ellas cuando se producen, gracias a las alertas de seguridad sobre actividades anómalas. Los usuarios reciben una alerta sobre actividades sospechosas en las bases de datos, posibles vulnerabilidades y ataques por inyección de código SQL, así como sobre los patrones de acceso y consultas a las bases de datos anómalos. Protección contra amenazas avanzada integra las alertas con [Microsoft Defender for Cloud](https://azure.microsoft.com/services/security-center/), que incluye detalles de actividades sospechosas y acciones recomendadas sobre cómo investigar y mitigar la amenaza. Advanced Threat Protection facilita la solución de posibles amenazas a la base de datos sin necesidad de ser un experto en seguridad ni tener que administrar sistemas de supervisión de seguridad avanzada.

Para obtener una experiencia de investigación completa, se recomienda habilitar la auditoría, que escribe los eventos de las bases de datos en un registro de auditoría en su cuenta de Azure Storage.  Para habilitar la auditoría, consulte [Auditoría para Azure SQL Database y Azure Synapse](../../azure-sql/database/auditing-overview.md) o [Auditoría para Instancia administrada de Azure SQL](../managed-instance/auditing-configure.md).

## <a name="alerts"></a>Alertas

Advanced Threat Protection detecta actividades anómalas que indican intentos poco habituales y posiblemente dañinos de acceder a las bases de datos o aprovecharse de ellas. Para ver una lista de las alertas, consulte [Alertas de SQL Database y Azure Synapse Analytics en Microsoft Defender for Cloud](../../security-center/alerts-reference.md#alerts-sql-db-and-warehouse).

## <a name="explore-detection-of-a-suspicious-event"></a>Exploración de la detección de un evento sospechoso

Cuando se detecten actividades anómalas en las bases de datos, recibirá una notificación por correo electrónico. El correo electrónico proporciona información sobre el evento de seguridad sospechoso, en la que se incluyen la naturaleza de las actividades anómalas, el nombre de la base de datos, el nombre del servidor, el nombre de la aplicación y la hora del evento. Además, el correo electrónico proporciona información sobre las posibles causas y las medidas recomendadas para investigar y mitigar la amenaza potencial para la base de datos.

![Informes de actividades anómalas](./media/threat-detection-overview/anomalous_activity_report.png)

1. Haga clic en el vínculo **Ver las alertas recientes de SQL** del correo electrónico para iniciar Azure Portal y mostrar la página de alertas de Microsoft Defender for Cloud, que proporciona información general sobre amenazas activas detectadas en la base de datos.

   ![Amenazas de actividad](./media/threat-detection-overview/active_threats.png)

1. Haga clic en una alerta específica para obtener detalles y acciones adicionales para investigar esta amenaza y solucionar amenazas futuras.

   Por ejemplo, la inyección de código SQL es uno de los problemas de seguridad habituales entre las aplicaciones web en Internet y se usa para atacar aplicaciones controladas por datos. Los atacantes aprovechan las vulnerabilidades de la aplicación para inyectar instrucciones SQL malintencionadas en los campos de entrada de la aplicación, con el fin de infringir la seguridad o modificar datos en la base de datos. Para las alertas de inyección de código SQL, los detalles de la alerta incluyen la instrucción SQL vulnerable que se ha aprovechado.

   ![Alerta específica](./media/threat-detection-overview/specific_alert.png)

## <a name="explore-alerts-in-the-azure-portal"></a>Exploración de alertas en Azure Portal

Protección contra amenazas avanzada integra sus alertas con [Microsoft Defender for Cloud](https://azure.microsoft.com/services/security-center/). Los iconos de Protección contra amenazas avanzada de SQL de la base de datos y las hojas de Microsoft Defender for Cloud de SQL en Azure Portal muestran el seguimiento del estado de las amenazas activas.

Haga clic en la **alerta de Protección contra amenazas avanzada** para iniciar la página de alertas de Microsoft Defender for Cloud y obtener información general de las amenazas de SQL activas detectadas en la base de datos.

:::image type="content" source="media/azure-defender-for-sql/advanced-threat-protection-alerts.png" alt-text="Información general sobre las alertas de Protección contra amenazas avanzada en la base de datos":::

:::image type="content" source="media/azure-defender-for-sql/advanced-threat-protection.png" alt-text="Protección contra amenazas avanzada en Defender para SQL":::

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [Advanced Threat Protection en Azure SQL Database y Azure Synapse](threat-detection-configure.md).
- Obtenga más información sobre [Advanced Threat Protection en Instancia administrada de Azure SQL](../managed-instance/threat-detection-configure.md).
- Más información sobre [Microsoft Defender para SQL](azure-defender-for-sql.md).
- Más información sobre las [auditorías de Azure SQL Database](../../azure-sql/database/auditing-overview.md)
- Más información sobre [Microsoft Defender for Cloud](../../security-center/security-center-introduction.md). Para más información sobre los precios, consulte la [página de precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
