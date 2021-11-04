---
title: Información general de SQL Server en Azure Virtual Machines para Linux | Microsoft Docs
description: Aprenda a ejecutar las ediciones completas de SQL Server en Azure Virtual Machines para Linux. Obtenga vínculos directos a todas las imágenes de máquina virtual de SQL Server y al contenido relacionado.
services: virtual-machines-sql
documentationcenter: ''
author: MashaMSFT
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: service-overview
ms.topic: overview
ms.workload: iaas-sql-server
ms.date: 10/26/2021
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 7bcf2e64643d27822f2f58e56b054e25d3adaca5
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131080961"
---
# <a name="overview-of-sql-server-on-linux-azure-virtual-machines"></a>Introducción a SQL Server en máquinas virtuales de Azure en Linux
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
> * [Linux](sql-server-on-linux-vm-what-is-iaas-overview.md)

SQL Server en Azure Virtual Machines le permite usar versiones completas de SQL Server en la nube sin tener que administrar ningún hardware local. Las máquinas virtuales con SQL Server también simplifican los costos de licencia cuando se paga por uso.

Las máquinas virtuales de Azure se ejecutan en distintas [regiones geográficas](https://azure.microsoft.com/regions/) en todo el mundo. También ofrecen diversos [tamaños de máquina](../../../virtual-machines/sizes.md). La galería de imágenes de máquina virtual le permite crear una máquina virtual con SQL Server con la versión, la edición y el sistema operativo correctos. Esto hace que las máquinas virtuales sean una buena opción para muchas cargas de trabajo de SQL Server diferentes. 

Si no está familiarizado con Azure SQL, consulte el vídeo de *introducción a SQL Server en una máquina virtual de Azure*  en la detallada [serie de vídeos sobre Azure SQL](https://channel9.msdn.com/Series/Azure-SQL-for-Beginners?WT.mc_id=azuresql4beg_azuresql-ch9-niner):
> [!VIDEO https://channel9.msdn.com/Series/Azure-SQL-for-Beginners/SQL-Server-on-Azure-VM-Overview-4-of-61/player]

## <a name="get-started-with-sql-server-vms"></a><a id="create"></a> Introducción a máquinas virtuales con SQL Server

Para empezar, elija una imagen de máquina virtual con SQL Server con la versión, la edición y el sistema operativo requeridos. Las secciones siguientes proporcionan vínculos directos a Azure Portal para las imágenes de la galería de máquinas virtuales de SQL Server.

> [!TIP]
> Para más información sobre cómo entender los precios de imágenes de SQL Server; consulte la [página de precios de máquinas virtuales Linux que ejecutan SQL Server](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

| Versión | Sistema operativo | Edición |
| --- | --- | --- |
| **SQL Server 2019** | Ubuntu 18.04 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804enterprise-ARM), [Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804standard-ARM), [Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804web-ARM), [Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-ubuntu1804sqldev-ARM) | 
| **SQL Server 2019** | Red Hat Enterprise Linux (RHEL) 8 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8enterprise-ARM), [Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8standard-ARM), [Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8web-ARM), [Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-rhel8sqldev-ARM)|
| **SQL Server 2019** | SUSE Linux Enterprise Server (SLES) v12 SP5 | [Enterprise](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5enterprise-ARM), [Standard](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5standard-ARM), [Web](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5web-ARM), [Developer](https://ms.portal.azure.com/#create/microsoftsqlserver.sql2019-sles12sp5sqldev-ARM)|
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> Para ver las imágenes de máquinas virtuales con SQL Server para Windows disponibles, consulte [Introducción a SQL Server en máquinas virtuales Windows de Azure](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

## <a name="installed-packages"></a><a id="packages"></a> Paquetes instalados

Al configurar SQL Server en Linux, instale el paquete del motor de base de datos y, después, varios paquetes opcionales en función de los requisitos. Las imágenes de máquina virtual Linux para SQL Server instalan automáticamente la mayoría de los paquetes. La siguiente tabla muestra qué paquetes se instalan para cada distribución.

| Distribución | [Motor de base de datos](/sql/linux/sql-server-linux-setup) | [Herramientas](/sql/linux/sql-server-linux-setup-tools) | [Agente SQL Server](/sql/linux/sql-server-linux-setup-sql-agent) | [Búsqueda de texto completo](/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](/sql/linux/sql-server-linux-setup-ssis) | [Complemento de alta disponibilidad](/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![RHEL y motor de base de datos](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL y herramientas](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL y agente de SQL Server](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL y búsqueda de texto completo](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL y SSIS](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![RHEL y complemento de alta disponibilidad](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |
| SLES | ![SLES y motor de base de datos](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES y herramientas](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES y agente de SQL Server](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES y búsqueda de texto completo](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![SLES y SSIS](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) | ![SLES y complemento de alta disponibilidad](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png)|
| Ubuntu | ![Ubuntu y motor de base de datos](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu y herramientas](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu y agente de SQL Server](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu y búsqueda de texto completo](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu y SSIS](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![Ubuntu y complemento de alta disponibilidad](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |


> [!NOTE]
> La extensión de SQL IaaS Agent para SQL Server en máquinas virtuales Linux de Azure solo está disponible para la distribución Ubuntu Linux.

## <a name="related-products-and-services"></a>Productos y servicios relacionados

### <a name="linux-virtual-machines"></a>Máquinas virtuales Linux

* [Introducción a las máquinas virtuales de Azure Virtual Machines](../../../virtual-machines/linux/overview.md)

### <a name="storage"></a>Storage

* [Introducción a Microsoft Azure Storage](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Redes

* [Información general sobre Virtual Network](../../../virtual-network/virtual-networks-overview.md)
* [Direcciones IP en Azure](../../../virtual-network/ip-services/public-ip-addresses.md)
* [Crear un nombre de dominio completo en el Portal de Azure](../../../virtual-machines/create-fqdn.md)

### <a name="sql"></a>SQL

* [Documentación de SQL Server en Linux](/sql/linux)
* [Comparación de Azure SQL Database](../../azure-sql-iaas-vs-paas-what-is-overview.md)

## <a name="next-steps"></a>Pasos siguientes

Introducción a SQL Server en máquinas virtuales Linux:

* [Creación de una máquina virtual con SQL Server en Azure Portal](sql-vm-create-portal-quickstart.md)

Obtenga respuestas a las preguntas más habituales acerca de las máquinas virtuales con SQL Server en Linux:

* [Preguntas más frecuentes sobre SQL Server en Azure Virtual Machines](frequently-asked-questions-faq.yml)