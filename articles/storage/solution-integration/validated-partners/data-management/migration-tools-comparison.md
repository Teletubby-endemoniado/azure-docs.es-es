---
title: 'Comparación de herramientas de migración de Azure Storage: datos no estructurados'
description: Funcionalidad básica y comparación entre las herramientas usadas para la migración de datos no estructurados
author: dukicn
ms.author: nikoduki
ms.topic: conceptual
ms.date: 08/04/2021
ms.service: storage
ms.subservice: partner
ms.openlocfilehash: d266f059869bb0f25df10dcc4fad317d3d3da7c3
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123426713"
---
# <a name="comparison-matrix"></a>Tabla comparativa

En la siguiente matriz de comparación se muestra la funcionalidad básica de las distintas herramientas que se pueden usar para la migración de datos no estructurados. 

## <a name="supported-azure-services"></a>Servicios de Azure compatibles

|    | [Microsoft](https://www.microsoft.com/) | [Datadobi](https://www.datadobi.com) | [Data Dynamics](https://www.datadynamicsinc.com/) | [Komprise](https://www.komprise.com/) |
|--- |-----------------------------------------|--------------------------------------|---------------------------------------------------|---------------------------------------|
|  **Nombre de la solución**  | [Azure File Sync](../../../file-sync/file-sync-deployment-guide.md) | [DobiMigrate](https://azuremarketplace.microsoft.com/marketplace/apps/datadobi1602192408529.datadobi-dobimigrate?tab=Overview)              | [Movilidad y migración de datos](https://azuremarketplace.microsoft.com/marketplace/apps/datadynamicsinc1581991927942.vm_4?tab=PlansAndPrice)      | [Administración de datos inteligentes](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management?tab=Overview)    |
| **Compatibilidad con Azure Files (todos los niveles)** | Sí                          | Sí                      | Sí            | Sí                            |
| **Compatibilidad con Azure NetApp Files**      | No                           | Sí                      | Sí            | Sí                            |
| **Compatibilidad con el acceso frecuente y esporádico de blob de Azure**   | No                           | Sí (a través de la versión preliminar de NFS)    | Sí            | Sí                            |
| **Compatibilidad con el nivel de archivo de blob de Azure** | No                           | No                       | No             | Sí (como destino de la migración) |
| **Compatibilidad con Azure Data Lake Storage** | No                           | No                       | No             | No                             |
| **Orígenes compatibles**      | Windows Server 2012 R2 y versiones posteriores | Sistemas de archivos de NAS y en la nube | Cualquier NAS y S3 | NAS, blob, S3                  |

## <a name="supported-protocols-source--destination"></a>Protocolos compatibles (origen y destino)

|    | [Microsoft](https://www.microsoft.com/) | [Datadobi](https://www.datadobi.com) | [Data Dynamics](https://www.datadynamicsinc.com/) | [Komprise](https://www.komprise.com/) |
|--- |-----------------------------------------|--------------------------------------|---------------------------------------------------|---------------------------------------|
| **Nombre de la solución**   | [Azure File Sync](../../../file-sync/file-sync-deployment-guide.md) | [DobiMigrate](https://azuremarketplace.microsoft.com/marketplace/apps/datadobi1602192408529.datadobi-dobimigrate?tab=Overview )              | [Movilidad y migración de datos](https://azuremarketplace.microsoft.com/marketplace/apps/datadynamicsinc1581991927942.vm_4?tab=PlansAndPrice)      | [Administración de datos inteligentes](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management?tab=Overview)    |
| **SMB 2.1**       | Sí | Sí | Sí | Sí |
| **SMB 3.0**       | Sí | Sí | Sí | Sí |
| **SMB 3.1**       | Sí | Sí | Sí | Sí |
| **NFS v3**        | No  | Sí | Sí | Sí |
| **NFS v4.1**      | No  | Sí | No  | Sí |
| **API de REST de blob** | No  | No  | Sí | Sí |
| **S3**            | No  | Sí | Sí | Sí |

## <a name="extended-features"></a>Características ampliadas

|    | [Microsoft](https://www.microsoft.com/) | [Datadobi](https://www.datadobi.com) | [Data Dynamics](https://www.datadynamicsinc.com/) | [Komprise](https://www.komprise.com/) |
|--- |-----------------------------------------|--------------------------------------|---------------------------------------------------|---------------------------------------|
|  **Nombre de la solución**  | [Azure File Sync](../../../file-sync/file-sync-deployment-guide.md) | [DobiMigrate](https://azuremarketplace.microsoft.com/marketplace/apps/datadobi1602192408529.datadobi-dobimigrate?tab=Overview )              | [Movilidad y migración de datos](https://azuremarketplace.microsoft.com/marketplace/apps/datadynamicsinc1581991927942.vm_4?tab=PlansAndPrice)      | [Administración de datos inteligentes](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management?tab=Overview)    |
| **Reasignación de UID/SID**                   | No  | Sí                        | Sí | No                             |
| **Reasignación de ACL de protocolo**                | No  | No                         | No  | No                             |
| **Compatibilidad con DFS**                           | Sí | Sí                        | Sí | Sí                            |
| **Limitaciones de compatibilidad**                    | Sí | Sí                        | Sí | Sí                            |
| **Exclusiones de patrón de archivo**               | No  | Sí                        | Sí | Sí (mediante la funcionalidad de copia) |
| **Compatibilidad con atributos de archivo selectivos** | Sí | Sí                        | Sí | Sí (para atributos ampliados)  |
| **Eliminación de propagaciones**                   | Sí | Sí                        | Sí | Sí                            |
| **Seguimiento de las uniones de NTFS**                 | No  | Sí                        | No  | Sí                            |
| **Invalidación del propietario de SMB y del propietario del grupo**    | Sí | Sí                        | Sí | No                             |
| **Cadena de informes de custodia**            | No  | Sí                        | No  | Sí                            |
| **Compatibilidad con flujos de datos alternativos**    | No  | Sí                        | Sí | No                             |
| **Programación de la migración**              | No  | Sí                        | Sí | Sí                            |
| **Conservación de ACL**                        | Sí  | Sí                        | Sí | Sí                            |
| **Compatibilidad con DACL**                          | Sí | Sí                        | Sí | Sí                            |
| **Compatibilidad con SACL**                          | Sí | Sí                        | Sí | No                             |
| **Conservación de la hora de acceso**                | Sí | Sí                        | Sí | Sí                            |
| **Conservación de la hora de modificación**              | Sí | Sí                        | Sí | Sí                            |
| **Conservación de la hora de creación**              | Sí  | Sí                        | Sí | Sí                            |
| **Compatibilidad con Azure Data Box**       | Sí | Sí                        | No  | No                             |
| **Ubicación de instantáneas**                | No  | Manual                     | Sí | No                             |
| **Compatibilidad con los vínculos simbólicos**                 | No  | Sí                        | No  | Sí                            |
| **Compatibilidad con vínculos físicos**                     | No  | Se migran como archivos independientes | Sí | Sí                            |
| **Compatibilidad con archivos abiertos o bloqueados**       | Sí | Sí                        | Sí | Sí                            |
| **Migración incremental**                 | Sí | Sí                        | Sí | Sí                            |
| **Compatibilidad con la conmutación**                    | No  | Sí                        | Sí | No (solo manual)               |
| **[Otras características](#other-features)**         | [Vínculo](#azure-file-sync)| [Vínculo](#datadobi-dobimigrate) | [Vínculo](#data-dynamics-data-mobility-and-migration) | [Vínculo](#komprise-intelligent-data-management)                |

## <a name="assessment-and-reporting"></a>Evaluación e informes

|    | [Microsoft](https://www.microsoft.com/) | [Datadobi](https://www.datadobi.com) | [Data Dynamics](https://www.datadynamicsinc.com/) | [Komprise](https://www.komprise.com/) |
|--- |-----------------------------------------|--------------------------------------|---------------------------------------------------|---------------------------------------|
| **Nombre de la solución**   | [Azure File Sync](../../../file-sync/file-sync-deployment-guide.md) | [DobiMigrate](https://azuremarketplace.microsoft.com/marketplace/apps/datadobi1602192408529.datadobi-dobimigrate?tab=Overview )              | [Movilidad y migración de datos](https://azuremarketplace.microsoft.com/marketplace/apps/datadynamicsinc1581991927942.vm_4?tab=PlansAndPrice)      | [Administración de datos inteligentes](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management?tab=Overview)    |
| **Capacity**                        | No      | Sí | Sí | Sí            |
| **Número de archivos o carpetas**            | No      | Sí | Sí | Sí            |
| **Distribución de vencimiento a lo largo del tiempo**      | No      | Sí | Sí | Sí            |
| **Hora de acceso**                     | No      | Sí | Sí | Sí            |
| **Hora de modificación**                   | No      | Sí | Sí | Sí            |
| **Hora de creación**                   | No      | Sí | Sí | Sí            |
| **Estado del informe por archivo u objeto** | Parcial | Sí | Sí | Sí            |

## <a name="licensing"></a>Licencias

|    | [Microsoft](https://www.microsoft.com/) | [Datadobi](https://www.datadobi.com) | [Data Dynamics](https://www.datadynamicsinc.com/) | [Komprise](https://www.komprise.com/) |
|--- |-----------------------------------------|--------------------------------------|---------------------------------------------------|---------------------------------------|
| **Nombre de la solución**   | [Azure File Sync](../../../file-sync/file-sync-deployment-guide.md) | [DobiMigrate](https://azuremarketplace.microsoft.com/marketplace/apps/datadobi1602192408529.datadobi-dobimigrate?tab=Overview )              | [Movilidad y migración de datos](https://azuremarketplace.microsoft.com/marketplace/apps/datadynamicsinc1581991927942.vm_4?tab=PlansAndPrice)      | [Administración de datos inteligentes](https://azuremarketplace.microsoft.com/marketplace/apps/komprise_inc.intelligent_data_management?tab=Overview)    |
| **BYOL**             | N/A | Sí | Sí | Sí |
| **Compromiso de Azure** | Sí   | Sí | Sí | Sí |

## <a name="other-features"></a>Otras características

### <a name="azure-file-sync"></a>Azure File Sync

- Validación de hash interna

> [!TIP]
> Azure File Sync está diseñado como una solución híbrida permanente para el almacenamiento en caché local o la sincronización de varios recursos compartidos de archivos de Azure. En esa función, proporciona migración a la nube sin tiempo de inactividad. Salvo que tenga previsto almacenar en caché los recursos compartidos de archivos de Azure de forma local, no es recomendable que utilice Azure File Sync como herramienta de migración. Consulte [Información general sobre la migración de recursos compartidos de archivos de Azure](../../../files/storage-files-migration-overview.md) u otras herramientas de asociados que se describen en este artículo.

### <a name="datadobi-dobimigrate"></a>Datadobi DobiMigrate

- Comprobaciones previas a la migración
- Planeamiento de la migración
- Simulacro de prueba de migración total
- Detección y alerta de la actividad de usuario en el lado de destino antes de la migración total
- Migraciones controladas por directivas
- Iteraciones de copia programadas
- Opciones configurables para controlar la seguridad del directorio raíz
- Ejecuciones de comprobación a petición
- Comprobación de la lectura de datos en el origen y el destino
- Flujo de trabajo de control de errores interactivo y gráfico
- Capacidad de restringir ciertas operaciones de propagación como eliminaciones y actualizaciones
- Capacidad de conservar la hora de acceso en el origen (además del destino)
- Capacidad de ejecutar la reversión al origen durante la conmutación de migración
- Capacidad de migrar los atributos de archivo SMB seleccionados
- Capacidad de limpiar los descriptores de seguridad de NTFS
- Capacidad de invalidar los permisos de NFSv3 y escribir nuevos bits de modo en el destino
- Capacidad de convertir las ACL de borrador POSIX de NFSv3 a ACL de NFSv4
- SMB 1 (CIFS)
- Soporte técnico 24 x 7 x 365

### <a name="data-dynamics-data-mobility-and-migration"></a>Migración y movilidad de datos de Data Dynamics

- Validación de hash

### <a name="komprise-intelligent-data-management"></a>Administración de datos inteligentes de Komprise

- Migraciones basadas en proyectos o directorios
- Reintento automático de errores
- Evaluación o informe: tipos de archivo, tamaño de archivo, basado en proyecto
- Evaluación o informe: búsquedas personalizadas basadas en metadatos
- Solución completa de administración del ciclo de vida de datos para el archivado, la replicación y el análisis
- Análisis basado en la hora de acceso a datos S3, blob
- Etiquetado
- Soporte técnico 24 x 7 x 365
- Validación de hash

*La lista se comprobó por última vez el 31 de marzo de 2021.*

## <a name="see-also"></a>Consulte también

- [Información general sobre la migración del almacenamiento](../../../common/storage-migration-overview.md)
- [Elección de la solución de Azure para la transferencia de datos](../../../common/storage-choose-data-transfer-solution.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Migración a recursos compartidos de archivos de Azure](../../../files/storage-files-migration-overview.md)
- [Migración a Data Lake Storage con WANdisco LiveData Platform para Azure](../../../blobs/migrate-gen2-wandisco-live-data-platform.md)
- [Copia o movimiento de datos a Azure Storage con AzCopy](../../../common/storage-use-azcopy-v10.md)
- [Migración de grandes conjuntos de datos a Azure Blob Storage con AzReplicate (aplicación de ejemplo)](/samples/azure/azreplicate/azreplicate/)