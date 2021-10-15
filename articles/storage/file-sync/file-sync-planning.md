---
title: Planeamiento de una implementación de Azure File Sync |Microsoft Docs
description: Planee sus implementaciones con Azure File Sync, que es un servicio que permite almacenar en caché varios recursos compartidos de archivos de Azure en una VM en la nube o en una instancia local de Windows Server.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 04/13/2021
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions, devx-track-azurepowershell
ms.openlocfilehash: c4429e0410fb9511d511ce5841876d5fbca173f5
ms.sourcegitcommit: 7bd48cdf50509174714ecb69848a222314e06ef6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2021
ms.locfileid: "129388103"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Planeamiento de una implementación de Azure Files Sync

:::row:::
    :::column:::
        [![Entrevista y demostración al presentar Azure File Sync: haga clic para reproducir.](./media/storage-sync-files-planning/azure-file-sync-interview-video-snapshot.png)](https://www.youtube.com/watch?v=nfWLO7F52-s)
    :::column-end:::
    :::column:::
        Azure File Sync es un servicio que permite almacenar en caché varios recursos compartidos de archivos de Azure en una VM en la nube o en una instancia local de Windows Server. 
        
        En este artículo se explican los conceptos y características de Azure File Sync. Una vez que esté familiarizado con Azure File Sync, considere la posibilidad de seguir la [guía de implementación de Azure File Sync](file-sync-deployment-guide.md) para probar este servicio.        
    :::column-end:::
:::row-end:::

Los archivos se almacenarán en la nube en [recursos compartidos de archivos de Azure](../files/storage-files-introduction.md). Se pueden usar recursos compartidos de archivos de Azure de dos formas: montando directamente los recursos compartidos de archivos de Azure sin servidor (SMB) o almacenando en caché recursos compartidos de archivos de Azure localmente mediante Azure File Sync. La opción de implementación que elija cambiará los aspectos que debe tener en cuenta a la hora de planear la implementación. 

- **Montaje directo de un recurso compartido de archivos de Azure**: dado que Azure Files proporciona acceso SMB, puede montar recursos compartidos de archivos de Azure locales o en la nube mediante el cliente SMB estándar disponible en Windows, macOS y Linux. Dado que los recursos compartidos de archivos de Azure no tienen servidor, la implementación en escenarios de producción no requiere la administración de un servidor de archivos o un dispositivo NAS, lo que significa que no tiene que aplicar revisiones de software ni intercambiar discos físicos. 

- **Almacenamiento en caché de recursos compartidos de archivos de Azure localmente con Azure File Sync**: Azure File Sync le permite centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma una instancia de Windows Server local (o en la nube) en una caché rápida de su recurso compartido de archivos de Azure. 

## <a name="management-concepts"></a>Conceptos de administración
Las implementaciones de Azure File Sync tiene tres objetos de administración fundamentales:

- **Recurso compartido de archivos de Azure**: Un recurso compartido de archivos de Azure es un recurso compartido de archivos en la nube sin servidor, que proporciona el *punto de conexión en la nube* de una relación de sincronización de Azure File Sync. A los archivos de un recurso compartido de archivos de Azure se puede acceder directamente con SMB o el protocolo FileREST, aunque se recomienda acceder a ellos principalmente mediante la memoria caché de Windows Server cuando el recurso compartido de archivos de Azure se vaya a usar con Azure File Sync. Ello se debe a que en la actualidad, Azure Files carece de un mecanismo de detección de cambios eficaz como el de Windows Server, por lo que los cambios directos en el recurso compartido de archivos de Azure tardarán un tiempo en propagarse en los puntos de conexión del servidor.
- **Punto de conexión de servidor**: la ruta de acceso de Windows Server que se sincroniza con un recurso compartido de archivos de Azure. Puede ser una carpeta concreta de un volumen o la raíz del volumen. Si sus espacios de nombres no se superponen, pueden existir varios puntos de conexión de servidor en el mismo volumen.
- **Grupo de sincronización**: el objeto que define la relación de sincronización entre un **punto de conexión en la nube**, o un recurso compartido de archivos de Azure, y un punto de conexión de servidor. Los puntos de conexión dentro de un grupo de sincronización se mantienen sincronizados entre sí. Si, por ejemplo, tiene dos conjuntos distintos de archivos que desea administrar con Azure File Sync, podría crear dos grupos de sincronización y agregar a cada uno puntos de conexión diferentes.

### <a name="azure-file-share-management-concepts"></a>Conceptos de administración de recursos compartidos de archivos de Azure
[!INCLUDE [storage-files-file-share-management-concepts](../../../includes/storage-files-file-share-management-concepts.md)]

### <a name="azure-file-sync-management-concepts"></a>Conceptos de administración de Azure File Sync
Los grupos de sincronización se implementan en los **servicios de sincronización del almacenamiento**, que son objetos de primer nivel que registran servidores para que se usen con Azure File Sync y contienen las relaciones del grupo de sincronización. El recurso del servicio de sincronización de almacenamiento es un homólogo del recurso de la cuenta de almacenamiento, y se puede implementar igualmente en grupos de recursos de Azure. Los servicios de sincronización del almacenamiento pueden crear grupos de sincronización que contengan recursos compartido de archivos de Azure en varias cuentas de almacenamiento y varios servidores Windows Server registrados.

Para poder crear un grupo de sincronización en un servicio de sincronización de almacenamiento, antes es preciso registrar un servidor Windows Server en el servicio. De esta forma se crea un objeto de **servidor registrado**, que representa una relación de confianza entre el servidor o clúster y el servicio de sincronización de almacenamiento. Para registrar un servicio de sincronización de almacenamiento, antes hay que instalar el agente de Azure File Sync en el servidor. Un servidor o un clúster individuales no se puede registrar con varios servicios de sincronización de almacenamiento simultáneos.

Un grupo de sincronización contiene un punto de conexión en la nube, o un recurso compartido de archivos de Azure, y al menos un punto de conexión de servidor. El objeto de punto de conexión de servidor contiene los valores que configuran la funcionalidad de **nube por niveles**, que proporciona la funcionalidad de almacenamiento en caché de Azure File Sync. Para realizar la sincronización con un recurso compartido de archivos de Azure, la cuenta de almacenamiento que contiene el recurso compartido de archivos de Azure debe estar en la misma región de Azure que el servicio de sincronización de almacenamiento.

> [!Important]  
> Puede realizar cambios en el espacio de nombres de cualquier punto de conexión en la nube o punto de conexión de servidor en el grupo de sincronización y sincronizar los archivos con los demás puntos de conexión del grupo de sincronización. Si realiza algún cambio directamente en el punto de conexión en la nube (recurso compartido de archivos de Azure), tenga en cuenta que un trabajo de detección de cambios de Azure File Sync deberá detectar primero esos cambios. Se inicia un trabajo de detección de cambios para un punto de conexión en la nube solo una vez cada 24 horas. Para obtener más información, consulte [Preguntas más frecuentes de Azure Files](../files/storage-files-faq.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json#afs-change-detection).

### <a name="consider-the-count-of-storage-sync-services-needed"></a>Tener en cuenta el recuento de servicios de sincronización de almacenamiento necesarios
En una sección anterior se describe el recurso principal que se va a configurar para Azure File Sync: un *servicio de sincronización de almacenamiento*. Windows Server solo se puede registrar en un servicio de sincronización de almacenamiento. Por lo tanto, a menudo es mejor implementar solo un servicio de sincronización de almacenamiento y registrar todos los servidores. 

Solo debe crear varios servicios de sincronización de almacenamiento si tiene:
* distintos conjuntos de servidores que nunca deben intercambiar datos. En este caso, debe diseñar el sistema para excluir determinados conjuntos de servidores que se van a sincronizar con un recurso compartido de archivos de Azure que ya está en uso como punto de conexión en la nube en un grupo de sincronización de un servicio de sincronización de almacenamiento diferente. Otra forma de ver esto es que los servidores de Windows Server registrados en un servicio de sincronización de almacenamiento distinto no se pueden sincronizar con el mismo recurso compartido de archivos de Azure.
* necesidad de tener más grupos de sincronización o servidores registrados de los que un único servicio de sincronización de almacenamiento puede admitir. Para más información, revise los [Objetivos de escalabilidad de Azure File Sync](../files/storage-files-scale-targets.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json#azure-file-sync-scale-targets).

## <a name="plan-for-balanced-sync-topologies"></a>Plan para topologías de sincronización equilibrada
Antes de implementar los recursos, es importante planear lo que va a sincronizar en un servidor local, con el recurso compartido de archivos de Azure. La realización de un plan le ayudará a determinar el número de cuentas de almacenamiento, recursos compartidos de archivos de Azure y recursos de sincronización que necesitará. Estas consideraciones siguen siendo pertinentes, incluso si los datos no residen actualmente en Windows Server o en el servidor que quiere usar a largo plazo. La [sección de migración](#migration) puede ayudar a determinar las rutas de migración adecuadas para su situación.

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

## <a name="windows-file-server-considerations"></a>Consideraciones sobre el servidor de archivos de Windows
Para habilitar la funcionalidad de sincronización en Windows Server, es preciso instalar el agente de Azure File Sync, que se puede descargar. Este agente proporciona dos componentes principales: `FileSyncSvc.exe`, el servicio de Windows en segundo plano responsable de supervisar los cambios en los puntos de conexión del servidor e iniciar sesiones de sincronización, y `StorageSync.sys`, un filtro del sistema de archivos que permite la organización de la nube por niveles y una rápida recuperación ante desastres.  

### <a name="operating-system-requirements"></a>Requisitos de sistema operativo
Azure File Sync es compatible con las siguientes versiones de Windows Server:

| Versión | SKU compatibles | Opciones de implementación compatibles |
|---------|----------------|------------------------------|
| Windows Server 2019 | Datacenter, Standard e IoT | Full y Core |
| Windows Server 2016 | Datacenter, Standard y Storage Server | Full y Core |
| Windows Server 2012 R2 | Datacenter, Standard y Storage Server | Full y Core |

Las versiones futuras de Windows Server se agregarán tan pronto como se publiquen.

> [!Important]  
> Se recomienda mantener sincronizadas todas las instancias que use con Azure File Sync con las actualizaciones más recientes de Windows Update. 

### <a name="minimum-system-resources"></a>Recursos mínimos del sistema
Azure File Sync requiere un servidor, ya sea físico o virtual, con un mínimo de una CPU y de 2 GiB de memoria.

> [!Important]  
> Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina debe configurarse con un mínimo de 2048 MiB de memoria.

En la mayoría de las cargas de trabajo de producción, no se recomienda configurar un servidor de Azure File Sync que tenga los requisitos mínimos. Para más información, consulte el artículo en el que se indican los [recursos del sistema recomendados](#recommended-system-resources).

### <a name="recommended-system-resources"></a>Recursos del sistema recomendados
Al igual que cualquier característica del servidor o aplicación, los requisitos de los recursos del sistema para Azure File Sync los determina la escala de la implementación; cuanto mayores sean las implementaciones en un servidor más recursos del sistema requerirán. En el caso de Azure File Sync, la escala la determinada el número de objetos en los puntos de conexión del servidor y en la renovación en el conjunto de datos. Un servidor individual puede tener puntos de conexión de servidor en varios grupos de sincronización y el número de objetos enumerados en la tabla siguiente tiene en cuenta el espacio de nombres completo al que está asociado un servidor. 

Por ejemplo, el punto de conexión del servidor A con 10 millones de objetos + el punto de conexión de servidor B con 10 millones de objetos = 20 millones de objetos. Para esa implementación de ejemplo, se recomienda usar 8 CPU y 16 GiB de memoria para el estado estable y, si es posible, 48 GiB de memoria para la migración inicial.
 
Los datos del espacio de nombres se almacenan en la memoria por motivos de rendimiento. Por eso, los espacios de nombres más grandes requieren más memoria para mantener un buen rendimiento, y una mayor renovación requiere más CPU para procesar. 
 
En la tabla siguiente, se proporcionan tanto el tamaño del espacio de nombres como una conversión a la capacidad para los recursos compartidos de archivos de uso general típicos, donde el tamaño medio de los archivos es de 512 KiB. Si el tamaño de los archivos es menor, considere la posibilidad de agregar memoria adicional para la misma cantidad de capacidad. Base la configuración de memoria en el tamaño del espacio de nombres.

| Tamaño del espacio de nombres: archivos y directorios (millones)  | Capacidad típica (TiB)  | Núcleos de CPU  | Memoria recomendada (GiB) |
|---------|---------|---------|---------|
| 3        | 1.4     | 2        | 8 (sincronización inicial)/2 (renovación típica)      |
| 5        | 2.3     | 2        | 16 (sincronización inicial)/4 (renovación típica)    |
| 10       | 4,7     | 4        | 32 (sincronización inicial)/8 (renovación típica)   |
| 30       | 14,0    | 8        | 48 (sincronización inicial)/16 (renovación típica)   |
| 50       | 23,3    | 16       | 64 (sincronización inicial)/32 (renovación típica)  |
| 100*     | 46,6    | 32       | 128 (sincronización inicial)/32 (renovación típica)  |

\*Actualmente no se recomienda sincronizar más de 100 millones de archivos y directorios. Este límite es flexible y se basa en los umbrales que hemos probado. Para más información, consulte [Objetivos de escalabilidad de Azure File Sync](../files/storage-files-scale-targets.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json#azure-file-sync-scale-targets).

> [!TIP]
> La sincronización inicial de un espacio de nombres es una operación intensiva y se recomienda asignar más memoria hasta que se complete la sincronización inicial. Esto no es necesario, pero puede acelerar la sincronización inicial. 
> 
> La renovación típica es el 0,5 % del espacio de nombres que cambia por día. Para mayores niveles de renovación, considere la posibilidad de agregar más CPU. 

- Un volumen asociado localmente con formato del sistema de archivos NTFS.

### <a name="evaluation-cmdlet"></a>Cmdlet de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante el cmdlet de evaluación de Azure File Sync. Este cmdlet busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Las comprobaciones incluyen la mayoría de las características que se mencionan a continuación, pero no todas; se recomienda que lea el resto de esta sección detenidamente para asegurarse de que la implementación se realiza sin problemas. 

El cmdlet de evaluación se puede instalar mediante el módulo Az de PowerShell, que se puede instalar siguiendo estas instrucciones: [Instale y configure Azure PowerShell](/powershell/azure/install-Az-ps).

#### <a name="usage"></a>Uso  
Puede invocar la herramienta de evaluación de varias maneras diferentes: puede realizar las comprobaciones del sistema, las comprobaciones del conjunto de datos o ambas. Para realizar las comprobaciones del sistema y el conjunto de datos: 

```powershell
Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Para probar solo el conjunto de datos:
```powershell
Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Para probar solo los requisitos del sistema:
```powershell
Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name> -SkipNamespaceChecks
```
 
Para mostrar los resultados en CSV:
```powershell
$validation = Invoke-AzStorageSyncCompatibilityCheck C:\DATA
$validation.Results | Select-Object -Property Type, Path, Level, Description, Result | Export-Csv -Path C:\results.csv -Encoding utf8
```

### <a name="file-system-compatibility"></a>Compatibilidad del sistema de archivos
Azure File Sync solo se admite en volúmenes NTFS conectados directamente. El almacenamiento conectado directamente, o DAS, en Windows Server significa que el sistema operativo Windows Server es el propietario del sistema de archivos. DAS se puede proporcionar mediante discos conectados físicamente al servidor de archivos, para lo que se conectan discos virtuales a una máquina virtual servidor de archivos (como una máquina virtual hospedada por Hyper-V), o incluso mediante ISCSI.

Solo se admiten volúmenes NTFS; ReFS, FAT, FAT32 y otros sistema de archivos no se admiten.

La siguiente tabla muestra el estado de interoperabilidad de las características del sistema de archivos NTFS: 

| Característica | Compatibilidad con el estado | Notas |
|---------|----------------|-------|
| Listas de control de acceso (ACL) | totalmente compatible | Azure File Sync conserva las listas de control de acceso discrecional con estilo Windows y Windows Server las exige en los puntos de conexión de servidor. También se pueden exigir listas de control de acceso cuando el recurso compartido de archivos de se monta directamente; sin embargo, esto requiere configuración adicional. Para más información, consulte la [sección acerca de la identidad](#identity). |
| Vínculos físicos | Omitido | |
| Vínculos simbólicos | Omitido | |
| Puntos de montaje | Compatibilidad parcial | Los puntos de montaje podrían ser la raíz de un punto de conexión de servidor, pero se omiten si están incluidos en el espacio de nombres del punto de conexión de servidor. |
| Uniones | Omitido | Por ejemplo, las carpetas DfrsrPrivate y DFSRoots del Sistema de archivos distribuido. |
| Puntos de repetición de análisis | Omitido | |
| Compresión NTFS | totalmente compatible | |
| Archivos dispersos | totalmente compatible | Los archivos dispersos se sincronizan (no se bloquean), pero lo hacen con la nube como un archivo completo. Si se cambia el contenido del archivo en la nube (o en otro servidor), el archivo ya no estará disperso cuando el cambio se haya descargado. |
| Flujos de datos alternativos (ADS) | Conservados, pero no sincronizados | Por ejemplo, las etiquetas de clasificación creadas por la infraestructura de clasificación de archivos no están sincronizadas. Las etiquetas de clasificación existentes en los archivos en cada uno de los puntos de conexión del servidor se dejan como están. |

<a id="files-skipped"></a>Azure File Sync también omitirá ciertos archivos temporales y carpetas del sistema:

| Archivo/carpeta | Nota: |
|-|-|
| pagefile.sys | Archivo específico del sistema |
| Desktop.ini | Archivo específico del sistema |
| thumbs.db | Archivo temporal para miniaturas |
| ehthumbs.db | Archivo temporal para miniaturas de elementos multimedia |
| ~$\*.\* | Archivo temporal de Office |
| \*.tmp | Archivo temporal |
| \*.laccdb | Archivo de bloqueo de base de datos de Access|
| 635D02A9D91C401B97884B82B3BCDAEA.* | Archivo de sincronización interna|
| \\System Volume Information | Carpeta específica del volumen |
| $RECYCLE.BIN| Carpeta |
| \\SyncShareState | Carpeta para sincronización |

### <a name="consider-how-much-free-space-you-need-on-your-local-disk"></a>Tenga en cuenta la cantidad de espacio disponible que necesita en el disco local.
Al planear el uso de Azure File Sync, tenga en cuenta la cantidad de espacio disponible que necesita en el disco local en el que piensa tener un punto de conexión de servidor.

Con Azure File Sync, deberá tener en cuenta lo siguiente para tener espacio en el disco local:
- Con la nube por niveles habilitada:
    - Puntos de repetición de análisis para archivos en capas
    - Base de datos de metadatos de Azure File Sync
    - Almacén térmico de Azure File Sync
    - Archivos totalmente descargados en la caché de acceso frecuente (de existir)
    - Requisitos de la directiva de espacio disponible del volumen

- Con la nube por niveles deshabilitada:  
    - Archivos totalmente descargados
    - Almacén térmico de Azure File Sync
    - Base de datos de metadatos de Azure File Sync

Usaremos un ejemplo para ilustrar cómo calcular la cantidad de espacio disponible que se necesita en el disco local. Supongamos que ha instalado el agente de Azure File Sync en una VM Windows de Azure y tiene pensado crear un punto de conexión de servidor en el disco F. Tiene 1 millón de archivos y le gustaría crear niveles de todos ellos, 100 000 directorios y un tamaño de clúster de disco de 4 KiB. El tamaño del disco es de 1000 GiB. Quiere habilitar la nube por niveles y establecer la directiva de espacio libre del volumen en el 20 %. 

1. NTFS asigna un tamaño de clúster para cada uno de los archivos en capas. 1 millón de archivos * tamaño de clúster de 4 KiB = 4 000 000 KiB (4 GiB)
> [!Note]  
> NTFS asigna el espacio ocupado por los archivos en capas. Por lo tanto, no se mostrará en ninguna interfaz de usuario.
3. Los metadatos de sincronización ocupan un tamaño de clúster por elemento. (1 millón de archivos + 100 000 directorios) * tamaño del clúster de 4 KiB = 4 400 000 KiB (4,4 GiB)
4. El almacén térmico de Azure File Sync ocupa 1,1 KiB por archivo. 1 millón de archivos * 1,1 KiB = 1 100 000 KiB (1,1 GiB)
5. La directiva de espacio disponible del volumen es del 20 %. 1000 GiB * 0,2 = 200 GiB

En este caso, Azure File Sync necesitaría unos 209 500 000 KiB (209,5 GiB) de espacio para este espacio de nombres. Agregue esta cantidad a cualquier espacio disponible adicional que desee para averiguar cuánto espacio libre se necesita para este disco.

### <a name="failover-clustering"></a>Clústeres de conmutación por error
1. La característica de clústeres de conmutación por error de Windows es compatible con Azure File Sync en la opción de implementación "Servidor de archivos para uso general". 
2. El único escenario admitido por Azure File Sync es el clúster de conmutación por error de Windows Server con discos en clúster.
3. La característica de clústeres de conmutación por error no se admite en "Servidor de archivos de escalabilidad horizontal para datos de aplicación" (SOFS) o en volúmenes compartidos de clúster (CSV) o discos locales.

> [!Note]  
> El agente de Azure File Sync debe estar instalado en cada nodo de un clúster de conmutación por error para que la sincronización funcione correctamente.

### <a name="data-deduplication"></a>Desduplicación de datos
**Windows Server 2016 y Windows Server 2019**   
Ahora se admite la desduplicación de datos independientemente de si la nube por niveles está habilitada o deshabilitada en uno o varios puntos de conexión de servidor del volumen en Windows Server 2016 y Windows Server 2019. Habilitar la desduplicación de datos en un volumen con la nube por niveles habilitada, le permite almacenar en caché más archivos en el entorno local sin necesidad de aprovisionar más almacenamiento. 

Cuando la desduplicación de datos está habilitada en un volumen con la nube por niveles habilitada, los archivos optimizados para desduplicación dentro de la ubicación del punto de conexión del servidor se organizan en niveles de forma similar a un archivo normal en función de la configuración de la directiva de la nube por niveles. Una vez que los archivos optimizados para la desduplicación se han organizado en niveles, el trabajo de recolección de elementos no utilizados de desduplicación de datos se ejecutará automáticamente para recuperar el espacio en disco mediante la eliminación de fragmentos innecesarios a los que ya no hacen referencia otros archivos del volumen.

Tenga en cuenta que el ahorro de volumen solo se aplica al servidor; los datos del recurso compartido de Azure no se desduplicarán.

> [!Note]  
> Para admitir la desduplicación de datos en volúmenes con una nube por niveles habilitada en Windows Server 2019, se debe instalar la actualización de Windows [KB4520062, de octubre de 2019](https://support.microsoft.com/help/4520062) o una actualización acumulativa mensual posterior, y se requiere el agente de Azure File Sync versión 12.0.0.0 o posterior.

**Windows Server 2012 R2**  
Azure File Sync no admite la desduplicación de datos y la nube por niveles en el mismo volumen en Windows Server 2012 R2. Si la desduplicación de datos está habilitada en un volumen, se debe deshabilitar la nube por niveles. 

**Notas**
- Si la desduplicación de datos está instalada antes de instalar el agente de Azure File Sync, es necesario reiniciar para que se admita en el mismo volumen la desduplicación de datos y la nube por niveles.
- Si habilita la desduplicación de datos en un volumen después de haber habilitado la nube por niveles, el trabajo inicial de optimización por desduplicación optimiza los archivos del volumen que no están aún en niveles, y tendrá la siguiente repercusión en la nube por niveles:
    - La directiva de espacio disponible seguirá colocando los archivos en niveles según el espacio libre en el volumen mediante el uso del mapa térmico.
    - La directiva de fecha omitirá la organización en niveles de los archivos, que podrían haber sido en otra situación aptos para niveles, ya que el trabajo de optimización por desduplicación tiene acceso a los archivos.
- Para los trabajos de optimización por desduplicación en curso, el valor de desduplicación de datos [MinimumFileAgeDays](/powershell/module/deduplication/set-dedupvolume), retrasará la nube por niveles con directiva de fecha, si el archivo no está colocado ya en un nivel. 
    - Ejemplo: Si el valor MinimumFileAgeDays es de siete días y la directiva de fecha de nube por niveles es de 30 días, la directiva de fecha colocará los archivos en niveles pasados 37 días.
    - Nota: Una vez que Azure File Sync haya colocado un archivo en un nivel, el trabajo de optimización por desduplicación omitirá el archivo.
- Si un servidor que ejecuta Windows Server 2012 R2 y que tiene instalado el agente de Azure File Sync se actualiza a Windows Server 2016 o Windows Server 2019, es necesario realizar los pasos siguientes para que se pueda admitir en el mismo volumen la desduplicación de datos y la nube por niveles:  
    - Desinstalar al agente de Azure File Sync para Windows Server 2012 R2 y reiniciar el servidor.
    - Descargar al agente de Azure File Sync para la versión de sistema operativo del nuevo servidor (Windows Server 2016 o Windows Server 2019).
    - Instalar el agente de Azure File Sync y reiniciar el servidor.  
    
    Nota: Los valores de configuración de Azure File Sync en el servidor se conservan cuando el agente se desinstala y reinstala.

### <a name="distributed-file-system-dfs"></a>Sistema de archivos distribuido (DFS)
Azure File Sync admite la interoperabilidad con espacios de nombres DFS (DFS-N) y la replicación DFS (DFS-R).

**Espacios de nombres DFS (DFS-N)** : Azure File Sync es totalmente compatible con servidores de DFS-N. Puede instalar el agente de Azure File Sync en uno o varios miembros DFS-N para sincronizar datos entre los puntos de conexión del servidor y el punto de conexión en la nube. Para más información, consulte [Información general de Espacios de nombres DFS](/windows-server/storage/dfs-namespaces/dfs-overview).
 
**Replicación DFS (DFS-R)** : puesto que DFS-R y Azure File Sync son soluciones de replicación, en la mayoría de los casos, se recomienda reemplazar DFS-R por Azure File Sync. Hay, sin embargo, varios escenarios donde puede que desee usar DFS-R y Azure File Sync conjuntamente:

- Va a migrar desde una implementación de DFS-R a una implementación de Azure File Sync. Para más información, consulte [Migrate a DFS Replication (DFS-R) deployment to Azure File Sync](file-sync-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync) (Migración de una implementación de la replicación DFS (DFS-R) a Azure File Sync).
- No todos los servidores locales que necesitan una copia de los datos de archivo pueden estar conectados directamente a Internet.
- Los servidores de sucursales consolidan los datos en un único servidor central, para el que le gustaría utilizar Servidores de sucursales consolidan los datos en un único servidor concentrador, le gustaría utilizar Azure File Sync.

Para que Azure File Sync y DFS-R trabajen en paralelo:

1. Los niveles de nube de Azure File Sync deben deshabilitarse en volúmenes con carpetas replicadas DFS-R.
2. Los puntos de conexión de servidor no se deben configurar en carpetas de replicación de solo lectura DFS-R.

Para más información, consulte [Introducción a Espacios de nombres DFS y Replicación DFS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)).

### <a name="sysprep"></a>Sysprep
No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y esto puede provocar resultados inesperados. La instalación del agente y el registro del servidor se deben realizar después de implementar la imagen del servidor y completar la instalación mínima de sysprep.

### <a name="windows-search"></a>Windows Search
Si en un punto de conexión de un servidor están habilitados los niveles en la nube, Windows Search omite y no indexa los archivos que están en capas. Los archivos que no están en capas se indexan correctamente.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Otras soluciones de administración de almacenamiento jerárquico (HSM)
No deben utilizarse otras soluciones HSM deben utilizarse con Azure File Sync.

## <a name="performance-and-scalability"></a>Escalabilidad y rendimiento

Dado que el agente de Azure File Sync se ejecuta en una máquina con Windows Server que se conecta a los recursos compartidos de archivos de Azure, el rendimiento de sincronización efectivo depende de una serie de factores de su infraestructura: Windows Server y la configuración del disco subyacente, el ancho de banda de la red entre el servidor y Azure Storage, el tamaño del archivo, el tamaño total del conjunto de datos y la actividad en el conjunto de datos. Dado que Azure File Sync funciona en el nivel de archivos, las características de rendimiento de una solución basada en Azure File Sync se mide mejor en el número de objetos (archivos y directorios) que se procesan por segundo.

Los cambios realizados en el recurso compartido de archivos de Azure mediante Azure Portal o SMB no se detectan y replican de forma inmediata como cambios en el punto de conexión del servidor. Azure Files aún no dispone de registros en diario o notificaciones, por lo que no hay manera de iniciar automáticamente una sesión de sincronización cuando se cambian los archivos. En Windows Server, Azure File Sync usa el [registro en diario de USN de Windows](/windows/win32/fileio/change-journals) para iniciar automáticamente una sesión de sincronización cuando cambian los archivos.

Para detectar cambios en el recurso compartido de archivos de Azure, Azure File Sync tiene un trabajo programado que se denomina trabajo de detección de cambios. Un trabajo de detección de cambios enumera todos los archivos del recurso compartido de archivos y, a continuación, los compara con la versión de sincronización correspondiente. Cuando el trabajo de detección de cambios determina qué archivos han cambiado, Azure File Sync inicia una sesión de sincronización. El trabajo de detección de cambios se inicia cada 24 horas. Dado que el trabajo de detección de cambios enumera todos los archivos del recurso compartido de archivos de Azure, la detección de cambios tarda más en los espacios de nombres más largos que los espacios de nombres más cortos. En el caso de los espacios de nombres largos, es posible que sea necesario determinar más de una vez cada 24 horas qué archivos han cambiado.

Para obtener más información, consulte [Métricas de rendimiento de Azure File Sync](../files/storage-files-scale-targets.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json#azure-file-sync-performance-metrics) y [Objetivos de escalabilidad de Azure File Sync](../files/storage-files-scale-targets.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json#azure-file-sync-scale-targets).

## <a name="identity"></a>Identidad
Azure File Sync funciona con su identidad basada en AD estándar sin ninguna configuración especial más allá de configurar la sincronización. Cuando se usa Azure File Sync, la expectativa general es que la mayor parte de los accesos atraviesen los servidores de almacenamiento en caché de Azure File Sync, en lugar del recurso compartido de archivos de Azure. Dado que los puntos de conexión del servidor se encuentran en Windows Server y que Windows Server ha admitido listas de control de acceso de estilo AD y Windows durante mucho tiempo, lo único que se necesita es asegurarse de que los servidores de archivos de Windows registrados en el servicio de sincronización de almacenamiento están unidos a un dominio. Azure File Sync almacenará las listas de control de acceso en los archivos del recurso compartido de archivos de Azure y los replicará en todos los puntos de conexión del servidor.

Aunque los cambios que se realizan directamente en el recurso compartido de archivos de Azure tardarán más tiempo en sincronizarse con los puntos de conexión del servidor del grupo de sincronización, es posible que también desee asegurarse de que puede exigir sus permisos de AD en su recurso compartido de archivos directamente en la nube también. Para ello, debe unir a un dominio su cuenta de almacenamiento a su AD local de la misma forma que los servidores de archivos de Windows se unen a un dominio. Para más información acerca de la unión a un dominio de una cuenta de almacenamiento a una instancia de Active Directory propiedad de un cliente, consulte la [introducción a Active Directory de Azure Files](../files/storage-files-active-directory-overview.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json).

> [!Important]  
> La unión a un dominio de una cuenta de almacenamiento a Active Directory no es necesaria para implementar Azure File Sync correctamente. Este es un paso estrictamente opcional que permite al recurso compartido de archivos de Azure exigir listas de control de acceso locales cuando los usuarios montan el recurso compartido de archivos de Azure directamente.

## <a name="networking"></a>Redes
El agente de Azure File Sync se comunica con su servicio de sincronización del almacenamiento y el recurso compartido de archivos de Azure mediante los protocolos REST y FileREST de Azure File Sync, ambos usan siempre HTTPS sobre el puerto 443. SMB nunca se usa para cargar o descargar datos entre Windows Server y el recurso compartido de archivos de Azure. Dado que la mayoría de las organizaciones permiten el tráfico HTTPS sobre el puerto 443, como requisito para visitar la mayor parte de sitios web, normalmente no se requiere ninguna configuración especial de la red para implementar Azure File Sync.

En función de los requisitos normativos únicos y de la directiva de su organización, puede requerir una comunicación más restrictiva con Azure y, por consiguiente, Azure File Sync proporciona varios mecanismos para que configure la red. En función de sus requisitos, puede:

- Tunnel sync and file upload/download traffic sobre ExpressRoute o Azure VPN. 
- Usar las características de Azure Files y Redes de Azure como los puntos de conexión de servicio y los puntos de conexión privados.
- Configurar Azure File Sync para que admita su proxy en su entorno.
- Limitar la actividad de la red desde Azure File Sync.

> [!Important]  
> Azure File Sync no admite el enrutamiento de Internet. La opción de enrutamiento de red predeterminada, el enrutamiento de Microsoft, es compatible con Azure File Sync.

Para más información sobre Azure File Sync y las redes, vea [Consideraciones de redes para Azure File Sync](file-sync-networking-overview.md).

## <a name="encryption"></a>Cifrado
Cuando se usa Azure File Sync, hay que tener en cuenta tres capas diferentes de cifrado: cifrado en el almacenamiento en reposo de Windows Server, cifrado en tránsito entre el agente de Azure File Sync y Azure, y cifrado en reposo de los datos del recurso compartido de archivos de Azure. 

### <a name="windows-server-encryption-at-rest"></a>Cifrado en reposo de Windows Server 
Hay dos estrategias para cifrar datos en Windows Server que funcionan habitualmente con Azure File Sync: el cifrado debajo del sistema de archivos, de forma que tanto el sistema de archivos como todos los datos escritos en ella estén cifrados y el cifrado del propio formato de archivo. Estos métodos no son mutuamente excluyentes; se pueden usar conjuntamente si se desea, ya que el fin del cifrado es diferente.

Para proporcionar cifrado debajo del sistema de archivos, Windows Server ofrece la Bandeja de entrada de BitLocker. BitLocker es totalmente transparente para Azure File Sync. La razón principal para usar un mecanismo de cifrado como BitLocker es evitar la filtración física de los datos de un centro de datos local por parte de alguien que robe los discos y evitar la transferencia local de un sistema operativo no autorizado para realizar lecturas y escritura de los datos. Para más información sobre BitLocker, consulte [Introducción a BitLocker](/windows/security/information-protection/bitlocker/bitlocker-overview).

Los productos de terceros que funcionan de forma similar a BitLocker, en que se usan al lado del volumen NTFS, deberían ser completamente transparentes para Azure File Sync. 

El otro método principal para cifrar datos es cifrar el flujo de datos del archivo cuando la aplicación lo guarde. Algunas aplicaciones pueden hacerlo de forma nativa, sin embargo, normalmente no sucede. Un ejemplo de un método para cifrar el flujo de datos del archivo es Azure Information Protection (AIP)/Azure Rights Management Services (Azure RMS)/Active Directory RMS. La principal razón para usar un mecanismo de cifrado como AIP/RMS es impedir la filtración de datos de un recurso compartido de archivos por parte de personas que los copien a ubicaciones alternativas, como una unidad flash, o que los envíen por correo electrónico a una persona no autorizada. Cuando el flujo de datos de un archivo se cifra como parte del formato de archivo, el archivo seguirá estando cifrado en el recurso compartido de archivos de Azure. 

Azure File Sync no interopera con el Sistema de cifrado de archivos NTFS (NTFS EFS) ni con soluciones de cifrado de terceros que se encuentran por encima del sistema de archivos, pero por debajo del flujo de datos del archivo. 

### <a name="encryption-in-transit"></a>Cifrado en tránsito

> [!NOTE]
> El servicio Azure File Sync quitará la compatibilidad con TLS 1.0 y 1.1 a partir del 1 de agosto de 2020. Todas las versiones compatibles del agente de Azure File Sync ya usan TLS 1.2 de forma predeterminada. El uso de una versión anterior de TLS podría producirse si TLS 1.2 estaba deshabilitado en el servidor o se utiliza un proxy. Si usa un proxy, se recomienda que compruebe la configuración del proxy. Las regiones del servicio Azure File Sync agregadas después del 1 de mayo de 2020 solo admitirán TLS 1.2 y la compatibilidad con TLS 1.0 y 1.1 se quitará de las regiones existentes el 1 de agosto de 2020.  Para más información, consulte la [guía para la solución de problemas](file-sync-troubleshoot.md#tls-12-required-for-azure-file-sync).

El agente de Azure File Sync se comunica con su servicio de sincronización del almacenamiento y el recurso compartido de archivos de Azure mediante los protocolos REST y FileREST de Azure File Sync, ambos usan siempre HTTPS sobre el puerto 443. Azure File Sync no envía solicitudes sin cifrar a través de HTTP. 

Las cuentas de Azure Storage contienen un modificador para requerir el cifrado en tránsito, que está habilitado de manera predeterminada. Aunque el modificador del nivel de la cuenta de almacenamiento esté deshabilitado, lo que significa que son posibles las conexiones no cifradas con los recursos compartidos de Azure, Azure File Sync solo usará canales cifrados para acceder al recurso compartido de archivos.

La razón principal para deshabilitar el cifrado en tránsito para la cuenta de almacenamiento es admitir una aplicación heredada que debe ejecutarse en un sistema operativo anterior, como Windows Server 2008 R2 o una distribución de Linux anterior, que se comunique con un recurso compartido de archivos de Azure directamente. Si la aplicación heredada se comunica con la caché de Windows Server del recurso compartido de archivos, el hecho de alternar este valor no tendrá efecto alguno. 

Se recomienda encarecidamente asegurarse de que está habilitado el cifrado de los datos en tránsito.

Para obtener más información sobre el cifrado en tránsito, consulte [Requerir transferencia segura en Azure Storage](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="azure-file-share-encryption-at-rest"></a>Cifrado en reposo del recurso compartido de archivos de Azure
[!INCLUDE [storage-files-encryption-at-rest](../../../includes/storage-files-encryption-at-rest.md)]

## <a name="storage-tiers"></a>Niveles de almacenamiento
[!INCLUDE [storage-files-tiers-overview](../../../includes/storage-files-tiers-overview.md)]

#### <a name="regional-availability"></a>Disponibilidad regional
[!INCLUDE [storage-files-tiers-large-file-share-availability](../../../includes/storage-files-tiers-large-file-share-availability.md)]

## <a name="azure-file-sync-region-availability"></a>Disponibilidad en regiones de Azure File Sync

Para la disponibilidad regional, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=storage).

Las siguientes regiones requieren que solicite acceso a Azure Storage antes de poder usar Azure File Sync en ellas:

- Sur de Francia
- Oeste de Sudáfrica
- Centro de Emiratos Árabes Unidos

Para solicitar acceso a estas regiones, siga el proceso de [este documento](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="redundancy"></a>Redundancia
[!INCLUDE [storage-files-redundancy-overview](../../../includes/storage-files-redundancy-overview.md)]

> [!Important]  
> Tanto el almacenamiento con redundancia geográfica como el almacenamiento con redundancia de zona geográfica tienen la capacidad de realizar la conmutación por error manual en la región secundaria. Se recomienda no hacerlo si no se produce un desastre al usar Azure File Sync debido a la mayor probabilidad de pérdida de datos. En caso de desastre en el que desee iniciar una conmutación por error manual del almacenamiento, necesitará abrir un caso de soporte técnico con Microsoft para reanudar la sincronización de Azure File Sync con el punto de conexión secundario.

## <a name="migration"></a>Migración
Si tiene un servidor de archivos de Windows 2012R2 o de una versión posterior, Azure File Sync se puede instalar directamente en su lugar, sin necesidad de trasladar los datos a un nuevo servidor. Si planea migrar a un nuevo servidor de archivos de Windows como parte de la adopción de Azure File Sync, o si los datos se encuentran actualmente en el almacenamiento conectado a la red (NAS), existen varios enfoques posibles de migración para usar Azure File Sync con estos datos. El método de migración que debe elegir depende de dónde residan los datos actualmente. 

Consulte el artículo [Información general sobre la migración de recursos compartidos de archivos de Azure File Sync y Azure](../files/storage-files-migration-overview.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json), donde puede encontrar instrucciones detalladas para su escenario.

## <a name="antivirus"></a>Antivirus
Dado que lo que hace un antivirus es examinar los archivos en busca de código malintencionado conocido, puede provocar la recuperación de archivos por niveles, lo que da lugar a cargos elevados de salida. En las versiones 4.0 y posteriores del agente de Azure File Sync, los archivos en niveles tienen establecido el atributo seguro de Windows FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS. Se recomienda consultar con el proveedor de software cómo configurar su solución para omitir la lectura de archivos que tengan establecido este atributo (muchas realizan la omisión automáticamente). 

Las soluciones de antivirus internas de Microsoft, Windows Defender y System Center Endpoint Protection (SCEP), omiten de forma automática la lectura de archivos que tienen establecido dicho atributo. Hemos probado ambas soluciones e identificamos un problema menor: al agregar un servidor a un grupo de sincronización existente, se recuperan (descargan) los archivos de menos de 800 bytes en el nuevo servidor. Estos archivos permanecerán en el nuevo servidor y no se organizarán en niveles ya que no cumplen con el requisito de tamaño de niveles (>64 kb).

> [!Note]  
> Los proveedores de software antivirus pueden comprobar la compatibilidad entre sus productos y Azure File Sync con [Azure File Sync Antivirus Compatibility Test Suite](https://www.microsoft.com/download/details.aspx?id=58322), que está disponible para su descarga en el Centro de descarga de Microsoft.

## <a name="backup"></a>Copia de seguridad 
Si está habilitada la nube por niveles, no se deben usar soluciones que realicen copias de seguridad directamente del punto de conexión de servidor o de una máquina virtual en la que se encuentre este. La nube por niveles hace que solo un subconjunto de los datos se almacene en el punto de conexión de servidor, y que el conjunto de datos completo resida en el recurso compartido de archivos de Azure. En función de la solución de copia de seguridad usada, los archivos por niveles se omitirán y no se realizará una copia de seguridad de ellos (porque tienen el conjunto de atributos FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS), o se recuperarán en el disco, lo que provocará cargos elevados de salida. Se recomienda usar una solución de copia de seguridad en la nube para realizar la copia de seguridad del recurso compartido de archivos de Azure directamente. Para más información, consulte [Acerca de la copia de seguridad de recursos compartidos de archivos de Azure](../../backup/azure-file-share-backup-overview.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) o póngase en contacto con el proveedor de copias de seguridad para ver si admite la copia de seguridad de recursos compartidos de Azure.

Si prefiere usar una solución de copia de seguridad local, las copias de seguridad deben realizarse en un servidor del grupo de sincronización que tenga deshabilitada la nube por niveles. Al realizar una restauración, use las opciones de restauración de nivel de volumen o de archivo. Los archivos restaurados con la opción de restauración a nivel de archivo se sincronizarán con todos los puntos de conexión del grupo de sincronización y los archivos existentes se reemplazarán con la versión restaurada desde la copia de seguridad.  Las restauraciones a nivel de volumen no reemplazarán las versiones de archivo más recientes en el recurso compartido de archivos de Azure u otros puntos de conexión del servidor.

> [!WARNING]
> Si necesita usar Robocopy /B con un agente de Azure File Sync que se ejecuta en el servidor de origen o de destino, actualice al agente de Azure File Sync versión v12.0 o posterior. El uso de Robocopy /B con versiones de agente inferiores a v12.0 dará lugar a daños en los archivos en niveles durante la copia.

> [!Note]  
> La reconstrucción completa (BMR) puede causar resultados inesperados y actualmente no se admite.

> [!Note]  
> Con la versión 9 del agente de Azure File Sync, las instantáneas de VSS (incluida la pestaña Versiones anteriores) ya se admiten en los volúmenes que tienen habilitada la nube por niveles. Sin embargo, debe habilitar la compatibilidad con la versión anterior a través de PowerShell. [Más información](file-sync-deployment-guide.md#self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service).

## <a name="data-classification"></a>Clasificación de datos
Si tiene instalado software de clasificación de datos, habilitar la nube por niveles puede aumentar el costo por dos motivos:

1. Con la nube por niveles habilitada, los archivos de uso más frecuentes se almacenan en la memoria caché local y los archivos de acceso esporádico se organizan por niveles en el recurso compartido de archivos de Azure en la nube. Si la clasificación de datos examina periódicamente todos los archivos del recurso compartido de archivos, los archivos en niveles de la nube deben volver a llamarse cada vez que se examinan. 

2. Si el software de clasificación de datos utiliza los metadatos del flujo de datos de un archivo, este debe volver a llamarse por completo para que el software pueda ver la clasificación. 

El aumento del número de llamadas y de la cantidad de datos que se llaman puede aumentar los costos.

## <a name="azure-file-sync-agent-update-policy"></a>Directiva de actualización del agente de Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Tenga en cuenta los valores de proxy y firewall](file-sync-firewall-and-proxy.md)
* [Implementación de Azure Files](../files/storage-how-to-create-file-share.md?toc=%2fazure%2fstorage%2ffilesync%2ftoc.json)
* [Implementación de Azure File Sync](file-sync-deployment-guide.md)
* [Supervisión de Azure File Sync](file-sync-monitoring.md)
