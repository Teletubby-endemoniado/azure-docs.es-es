---
title: Migración de una Base de datos SQL Server a SQL Server en una máquina virtual | Microsoft Docs
description: Obtenga información sobre cómo migrar una base de datos de usuario local a SQL Server en una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: bluefooted
editor: ''
tags: azure-service-management
ms.assetid: 00fd08c6-98fa-4d62-a3b8-ca20aa5246b1
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.tgt_pltfrm: vm-windows-sql-server
ms.subservice: migration
ms.topic: how-to
ms.date: 08/18/2018
ms.author: pamela
ms.reviewer: mathoma
ms.openlocfilehash: 1d5454ad84baadf82ecea93793cc4cf22d4fb22f
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130165306"
---
# <a name="migrate-a-sql-server-database-to-sql-server-on-an-azure-virtual-machine"></a>Migre una base de datos de SQL Server a SQL Server en una máquina virtual de Azure

[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Existen varias formas de migrar una base de datos de usuario de SQL Server local a SQL Server en una máquina virtual (VM). En este artículo se tratan diversos métodos brevemente y se recomienda el mejor de ellos para diversos escenarios.


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

  > [!NOTE]
  > SQL Server 2008 y SQL Server 2008 R2 se aproximan al [final de su ciclo de vida de soporte técnico](https://www.microsoft.com/sql-server/sql-server-2008) para instancias locales. Para ampliar el soporte, puede migrar la instancia de SQL Server a una máquina virtual de Azure o comprar las actualizaciones de seguridad extendidas para mantenerlo en el entorno local. Para más información, vea [Ampliar la compatibilidad de SQL Server 2008 y SQL Server 2008 R2 con Azure](sql-server-2008-extend-end-of-support.md)

## <a name="what-are-the-primary-migration-methods"></a>¿Cuáles son los principales métodos de migración?

Los principales métodos de migración son:

* Realizar una copia de seguridad local con compresión y copiar manualmente el archivo de copia de seguridad en la máquina virtual de Azure.
* Realizar una copia de seguridad en una dirección URL y restaurarla en la máquina virtual de Azure desde dicha dirección URL.
* Desasociar los archivos de datos y de registro, copiarlos en Azure Blob Storage y, seguidamente, asociarlos a SQL Server en la máquina virtual de Azure desde la dirección URL.
* Convertir la máquina física de local a un VHD de Hyper-V, cargarla en Azure Blob Storage y, a continuación, implementarla como una máquina virtual nueva con el VHD cargado.
* Enviar la unidad de disco duro con el servicio Import/Export de Windows.
* Si tiene una implementación de grupos de disponibilidad AlwaysOn local, utilice el [Asistente para agregar una réplica de Azure](/previous-versions/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-onprem-availability) para crear una réplica en Azure, ejecute una conmutación por error y dirija a los usuarios a la instancia de base de datos de Azure.
* Utilice la [replicación transaccional](/sql/relational-databases/replication/transactional/transactional-replication) de SQL Server para configurar la instancia de Azure SQL Server como suscriptor, deshabilite la replicación y dirija a los usuarios a la instancia de base de datos de Azure.

> [!TIP]
> También puede utilizar estas mismas técnicas para mover bases de datos entre VM de SQL Server en Azure. Por ejemplo, no hay ninguna manera admitida para actualizar una VM de imagen de la galería de SQL Server desde una versión o edición a otra. En este caso, debe crear una nueva VM con SQL Server con la nueva versión o edición y luego usar una de las técnicas de migración de este artículo para mover las bases de datos. 

## <a name="choose-a-migration-method"></a>Elección de un método de migración

Para obtener el mejor rendimiento de transferencia de datos, migre los archivos de base de datos a la máquina virtual de Azure con un archivo de copia de seguridad comprimido.

Para minimizar el tiempo de inactividad durante el proceso de migración de la base de datos, utilice la opción AlwaysOn o la opción de replicación transaccional.

Si no es posible usar los métodos anteriores, migre la base de datos de forma manual. Por lo general, se empieza con una copia de seguridad de base de datos, se sigue con una copia de la copia de seguridad de base de datos en Azure y, después, se restaura la base de datos. También es posible copiar los archivos mismos de la base de datos en Azure y, luego, anexarlos. Existen varios métodos para llevar a cabo este proceso manual de migración de una base de datos a una VM de Azure.

> [!NOTE]
> Al actualizar a SQL Server 2014 o SQL Server 2016 desde versiones anteriores de SQL Server, es preciso considerar si es preciso realizar cambios. Como parte del proyecto de migración es aconsejable solucionar todas las dependencias de características no compatibles con la nueva versión de SQL Server. Para obtener más información sobre los escenarios y las ediciones compatibles, consulte [Actualización a SQL Server](/sql/database-engine/install-windows/upgrade-sql-server).

En la tabla siguiente se muestran los principales métodos de migración y se explica en qué circunstancias es más apropiado usar cada uno de ellos.

| Método | Versión de base de datos de origen | Versión de base de datos de destino | Restricción del tamaño de copia de seguridad de la base de datos de origen | Notas |
| --- | --- | --- | --- | --- |
| [Realizar una copia de seguridad local con compresión y copiar manualmente el archivo de copia de seguridad en la máquina virtual de Azure](#back-up-and-restore) |SQL Server 2005 o superior |SQL Server 2005 o superior |[Límite de almacenamiento de máquina virtual de Azure](../../../index.yml) | Se trata de una técnica sencilla y probada para mover bases de datos entre máquinas. |
| [Realizar una copia de seguridad a una dirección URL y restaurarla en la máquina virtual de Azure desde dicha dirección URL](#backup-to-url-and-restore-from-url) |SQL Server 2012 SP1 CU2 o superior | SQL Server 2012 SP1 CU2 o superior | < 12,8 TB para SQL Server 2016, de lo contrario < 1 TB | Este método es solo otra manera de mover el archivo de copia de seguridad a la VM con Azure Storage. |
| [Desasociar y, a continuación, copiar los archivos de datos y de registro en Azure Blob Storage y, a continuación, asociarlos a SQL Server en la máquina virtual de Azure desde una dirección URL](#detach-and-attach-from-a-url) | SQL Server 2005 o superior |SQL Server 2014 o superior | [Límite de almacenamiento de máquina virtual de Azure](../../../index.yml) | Este método se usa cuando se pretenden [almacenar estos archivos mediante el servicio de almacenamiento de blobs de Azure](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure) y adjuntarlos a SQL Server en una VM de Azure, especialmente con bases de datos muy grandes. |
| [Convertir máquina local en VHD de Hyper-V, cargar en el almacenamiento de blobs de Azure y, a continuación, implementar una nueva máquina virtual con el VHD cargado](#convert-to-a-vm-upload-to-a-url-and-deploy-as-a-new-vm) |SQL Server 2005 o superior |SQL Server 2005 o superior |[Límite de almacenamiento de máquina virtual de Azure](../../../index.yml) |Se usa cuando el usuario [tiene su propia licencia de SQL Server](../../../azure-sql/azure-sql-iaas-vs-paas-what-is-overview.md), cuando se migra una base de datos que se va a ejecutar en una versión anterior de SQL Server o cuando se migran bases de datos de usuario y del sistema conjuntamente como parte de la migración de base de datos dependiente de otras bases de datos de usuario o bases de datos del sistema. |
| [Envío de unidad de disco duro con el servicio Import/Export de Windows](#ship-a-hard-drive) |SQL Server 2005 o superior |SQL Server 2005 o superior |[Límite de almacenamiento de máquina virtual de Azure](../../../index.yml) |Use el [servicio de importación y exportación de Windows](../../../import-export/storage-import-export-service.md) cuando el método de copia manual sea demasiado lento, como por ejemplo, en el caso de bases de datos muy grandes. |
| [Usar el Asistente para agregar réplica de Azure](/previous-versions/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-onprem-availability) |SQL Server 2012 o superior |SQL Server 2012 o superior |[Límite de almacenamiento de máquina virtual de Azure](../../../index.yml) |Minimiza el tiempo de inactividad; úselo cuando tenga una implementación local de AlwaysOn. |
| [Uso de la replicación transaccional de SQL Server](/sql/relational-databases/replication/transactional/transactional-replication) |SQL Server 2005 o superior |SQL Server 2005 o superior |[Límite de almacenamiento de máquina virtual de Azure](../../../index.yml) |Úselo cuando necesite minimizar el tiempo de inactividad y no tenga una implementación local de AlwaysOn. |

## <a name="back-up-and-restore"></a>Copia de seguridad y restauración

Haga una copia de seguridad de la base de datos con compresión, copie la copia de seguridad a la VM y, luego, restaure dicha base de datos. Si el archivo de copia de seguridad es mayor que 1 TB, debe crear un conjunto distribuido porque el tamaño máximo de un disco de máquina virtual es 1 TB. Utilice los siguientes pasos generales para migrar una base de datos de usuario con este método manual:

1. Realice una copia de seguridad completa de la base de datos en una ubicación local.
2. Cree o cargue una máquina virtual con la versión de SQL Server deseada.
3. Configure la conectividad según sus requisitos. Consulte [Conexión a una máquina virtual de SQL Server en Azure (Resource Manager)](ways-to-connect-to-sql.md).
4. Copie los archivos de copia de seguridad en la máquina virtual con Escritorio remoto, el Explorador de Windows o el comando copy en un símbolo del sistema.

## <a name="backup-to-url-and-restore-from-url"></a>Copia de seguridad en una dirección URL y restauración desde la dirección URL

En lugar de hacer la copia de seguridad en un archivo local, puede usar la [copia de seguridad en una dirección URL](/sql/relational-databases/backup-restore/sql-server-backup-to-url) y luego restaurar desde la dirección URL en la máquina virtual. SQL Server 2016 admite conjuntos de copia de seguridad distribuidos. Se recomiendan para el rendimiento y deben superar los límites de tamaño por blob. En el caso de bases de datos muy grandes, se recomienda usar el [servicio Import/Export de Windows](../../../import-export/storage-import-export-service.md) .

## <a name="detach-and-attach-from-a-url"></a>Desasociación y asociación desde una dirección URL

Desasocie la base de datos y los archivos de registro y transfiéralos a [Azure Blob Storage](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure). Asocie la base de datos desde la dirección URL en la VM de Azure. Use este método si desea que los archivos de base de datos físicos residan en Blob Storage, lo que puede resultar útil para las bases de datos de gran tamaño. Utilice los siguientes pasos generales para migrar una base de datos de usuario con este método manual:

1. Desasocie los archivos de base de datos de la instancia de base de datos local.
2. Copie los archivos de base de datos desasociados en Azure Blob Storage con la [utilidad de línea de comandos AZCopy](../../../storage/common/storage-use-azcopy-v10.md).
3. Asocie los archivos de base de datos desde la dirección URL de Azure a la instancia de SQL Server en la máquina virtual de Azure.

## <a name="convert-to-a-vm-upload-to-a-url-and-deploy-as-a-new-vm"></a>Conversión a máquina virtual, carga en una dirección URL e implementación como máquina virtual nueva

Este método se usa para migrar todas las bases de datos de usuario y del sistema de una instancia de SQL Server local a una máquina virtual de Azure. Utilice los siguientes pasos generales para migrar una instancia completa de SQL Server con este método manual:

1. Convierta las máquinas físicas o virtuales en discos duros virtuales Hyper-V.
2. Cargue archivos VHD en Azure Storage mediante el [cmdlet Add-AzureVHD](/previous-versions/azure/dn495173(v=azure.100)).
3. Implemente una máquina virtual nueva mediante el VHD cargado.

> [!NOTE]
> Para migrar una aplicación completa, considere el uso de [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md).

## <a name="ship-a-hard-drive"></a>Envío de una unidad de disco duro

Use el [servicio Azure Import/Export](../../../import-export/storage-import-export-service.md) para transferir grandes cantidades de datos de archivo a Azure Blob Storage en aquellas situaciones en que el proceso de carga a través de la red sea demasiado caro o no sea viable. Con este servicio, se envían una o varias unidades de discos duros que contengan esos datos a un centro de datos de Azure, donde los datos se cargarán a su cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte [Introducción a SQL Server en máquinas virtuales de Azure](sql-server-on-azure-vm-iaas-what-is-overview.md).

> [!TIP]
> Si tiene alguna pregunta sobre las máquinas virtuales de SQL Server, consulte las [Preguntas más frecuentes](frequently-asked-questions-faq.yml).

Para obtener instrucciones sobre cómo crear SQL Server en una máquina virtual de Azure a partir de una imagen capturada, consulte [Sugerencias y trucos para la "clonación" de máquinas virtuales de SQL Azure a partir de imágenes capturadas](/archive/blogs/psssql/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images) en el blog de CSS SQL Server Engineers.