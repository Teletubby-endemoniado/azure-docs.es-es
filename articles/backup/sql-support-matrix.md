---
title: Matriz de compatibilidad de Azure Backup para la copia de seguridad de SQL Server en VM de Azure
description: Proporciona un resumen de opciones de compatibilidad y limitaciones para realizar copias de seguridad de SQL Server en VM de Azure con el servicio Azure Backup.
ms.topic: conceptual
ms.date: 10/22/2021
ms.custom: references_regions
ms.openlocfilehash: c0b46a1c75c47b85985946646bf3216c9ab719f5
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130219269"
---
# <a name="support-matrix-for-sql-server-backup-in-azure-vms"></a>Matriz de compatibilidad para la copia de seguridad de SQL Server en VM de Azure

Puede usar Azure Backup para realizar copias de seguridad de bases de datos de SQL Server en VM de Azure hospedadas en la plataforma de nube Microsoft Azure. En este artículo se resumen las configuraciones y limitaciones de compatibilidad generales para los escenarios e implementaciones de copia de seguridad de SQL Server en VM de Azure.

## <a name="scenario-support"></a>Compatibilidad con los escenarios

**Soporte técnico** | **Detalles**
--- | ---
**Implementaciones admitidas** | Se admiten máquinas virtuales de Azure de SQL Marketplace y que no son de Marketplace (SQL Server instalado manualmente).
**Regiones admitidas** | Azure Backup para bases de datos de SQL Server está disponible en todas las regiones, excepto Sur de Francia (FRS), Norte de Reino Unido (UKN), Sur de Reino Unido 2 (UKS2), UG IOWA (UGI) y Alemania (Selva Negra).
**Sistemas operativos compatibles** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux no se admite actualmente.
**Versiones admitidas de SQL Server** | SQL Server 2019, SQL Server 2017 tal como se detalla en la [página de búsqueda del ciclo de vida del producto](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017), SQL Server 2016 y los SP tal como se detalla en la [página de búsqueda del ciclo de vida del producto](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack), SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Enterprise, Standard, Web, Developer, Express.<br><br>No se admiten las versiones de base de datos local rápidas.
**Versiones de .NET compatibles** | .NET Framework 4.5.2 o posterior instalado en la máquina virtual
**Implementaciones admitidas** | Se admiten máquinas virtuales de Azure del Marketplace de SQL y que no son de Marketplace (SQL Server instalado manualmente). La compatibilidad con instancias independientes siempre está en los [grupos de disponibilidad](backup-sql-server-on-availability-groups.md).

## <a name="feature-considerations-and-limitations"></a>Consideraciones y limitaciones de las características

|Configuración  |Límite máximo |
|---------|---------|
|Número de bases de datos que se pueden proteger en un servidor (y en un almacén)    |   2000      |
|Tamaño de la base de datos compatible (más allá de esto, pueden aparecer problemas de rendimiento)   |   6 TB*      |
|Número de archivos admitidos en una base de datos    |   1000      |
|Número de copias de seguridad completas admitidas al día    |    Una copia de seguridad programada. <br><br> Tres copias de seguridad a petición. <br><br> Se recomienda no desencadenar más de tres copias de seguridad al día. Sin embargo, para permitir reintentos del usuario en caso de intentos fallidos, el límite máximo de copias de seguridad a petición se establece en nueve intentos. |

_*El límite de tamaño de la base de datos depende no solo de la velocidad de transferencia de datos que se admita, sino también de la configuración del límite de tiempo de copia de seguridad. No es un límite rígido. [Más información](#backup-throughput-performance) sobre el rendimiento de la copia de seguridad._

* La copia de seguridad de SQL Server se puede configurar en Azure Portal o **PowerShell**. No se admite la CLI.
* La solución es compatible con ambos tipos de [implementaciones](../azure-resource-manager/management/deployment-models.md): las máquinas virtuales de Azure Resource Manager y las máquinas virtuales clásicas.
* Se admiten todos los tipos de copia de seguridad (completas, diferenciales y de registro) y los modelos de recuperación (simple, completo o registros de operaciones masivas).
* En el caso de las bases de datos de **solo lectura**: las copias de seguridad completas y de solo copia son los únicos tipos de copia de seguridad admitidos.
* La compresión nativa de SQL es compatible si el usuario la habilita explícitamente en la directiva de copia de seguridad. Azure Backup invalida los valores predeterminados de nivel de instancia con la cláusula COMPRESSION/NO_COMPRESSION según el valor de este control establecido por el usuario.
* Se admite la copia de seguridad de base de datos habilitada para TDE. Para restaurar una base de datos cifrada TDE en otra de SQL Server, primero debe [restaurar el certificado en el servidor de destino](/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server). Está disponible la compresión de copia de seguridad para las bases de datos habilitadas para TDE en SQL Server 2016 y versiones más recientes, pero con un tamaño de transferencia inferior, tal y como se explica [aquí](https://techcommunity.microsoft.com/t5/sql-server/backup-compression-for-tde-enabled-databases-important-fixes-in/ba-p/385593).
* No se admiten las operaciones de copia de seguridad y restauración para las bases de datos reflejadas y las instantáneas de base de datos.
* La **instancia del clúster de conmutación por error (FCI)** de SQL Server no se admite.

## <a name="backup-throughput-performance"></a>Rendimiento de la copia de seguridad

Azure Backup admite una velocidad de transferencia de datos constante de 200 Mbps en las copias de seguridad completas y diferenciales de bases de datos SQL grandes (de 500 GB). Para utilizar el rendimiento óptimo, asegúrese de que:

- La máquina virtual subyacente (que contiene la instancia de SQL Server, que hospeda la base de datos) se configura con el rendimiento de red necesario. Si el rendimiento máximo de la máquina virtual es inferior a 200 Mbps, Azure Backup no puede transferir datos a una velocidad óptima.<br>Además, el disco que contiene los archivos de base de datos debe tener suficiente rendimiento aprovisionado. [Más información](../virtual-machines/disks-performance.md) sobre el rendimiento de discos en máquinas virtuales de Azure. 
- Los procesos, que se ejecutan en la máquina virtual, no consumen el ancho de banda de la máquina virtual. 
- Las programaciones de las copias de seguridad se reparten entre un subconjunto de bases de datos. Si se ejecutan varias copias de seguridad simultáneamente en una máquina virtual, la tasa de consumo de red se compartirá entre todas ellas. [Más información](faq-backup-sql-server.yml#can-i-control-how-many-concurrent-backups-run-on-the-sql-server-) sobre cómo controlar el número de copias de seguridad simultáneas.

>[!NOTE]
> [Descargue el planeador de recursos detallado](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx) para calcular el número aproximado de bases de datos protegidas que se recomiendan por servidor en función de los recursos de la máquina virtual, el ancho de banda y la directiva de copia de seguridad.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [hacer una copia de seguridad de una base de datos de SQL Server](backup-azure-sql-database.md) que se ejecuta en una máquina virtual de Azure.