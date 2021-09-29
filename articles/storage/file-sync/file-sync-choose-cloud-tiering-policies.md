---
title: Selección de directivas de nube por niveles de Azure File Sync | Microsoft Docs
description: Detalles sobre lo que hay que tener en cuenta al elegir directivas de nube por niveles de Azure File Sync.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 04/13/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 8cd8b51531fa28e3d698de47358c3b69e4f88ec2
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128665841"
---
# <a name="choose-cloud-tiering-policies"></a>Selección de directivas de nube por niveles

En este artículo se proporciona una guía para los usuarios que van a seleccionar y ajustar sus directivas de nube por niveles. Antes de leer este artículo, asegúrese de que comprende cómo funciona la nube por niveles. Para conocer los aspectos básicos, consulte [Comprender la nube por niveles de Azure File Sync](file-sync-cloud-tiering-overview.md). Puede encontrar una explicación detallada de las directivas de nube por niveles en [Directivas de nube por niveles de Azure File Sync](file-sync-cloud-tiering-policy.md).

## <a name="limitations"></a>Limitaciones

- La nube por niveles no se admite en el volumen del sistema de Windows.

- Sin embargo, puede habilitarla si tiene una cuota de FSRM en el nivel de volumen. Una vez que se establece una cuota de FSRM, las API de consulta de espacio libre que se llaman automáticamente notifican el espacio disponible en el volumen según la configuración de la cuota.

### <a name="minimum-file-size-for-a-file-to-tier"></a>Tamaño mínimo de archivo mejorado para almacenar un archivo por niveles

En las versiones del agente 9 y más recientes, el tamaño de archivo mínimo para organizar un archivo por niveles se basa en el tamaño del clúster del sistema de archivos. El tamaño mínimo del archivo que se selecciona para la realizar agrupación en niveles en la nube se calcula en función del doble del tamaño del clúster y en un mínimo de 8 KiB. En la tabla siguiente se muestran los tamaños de archivo mínimos que se pueden organizar por niveles, en función del tamaño del clúster del volumen:

|Tamaño del clúster del volumen  |Los archivos de este tamaño o mayores se pueden organizar por niveles  |
|----------------------------|---------|
|4 KiB o más pequeños (4096 bytes)      | 8 KiB    |
|8 KiB (8192 bytes)                 | 16 KiB   |
|16 KiB (16384 bytes)               | 32 KiB   |
|32 KiB (32768 bytes)               | 64 KiB   |
|64 KiB (65536 bytes)    | 128 KiB  |
|128 KiB (131072 bytes) | 256 KiB |
|256 KiB (262144 bytes) | 512 KiB |
|512 KiB (524288 bytes) | 1 MiB |
|1 MiB (1048576 bytes) | 2 MiB |
|2 MiB (2097152 bytes) | 4 MiB |

Los tamaños de clúster de hasta 2 MiB son compatibles con la versión 12 del agente de Azure File Sync pero, para tamaños mayores, la nube por niveles no funciona.

Todos los sistemas de archivos que usa Windows organizan el disco duro en función del tamaño del clúster (lo que también se conoce como tamaño de la unidad de asignación). El tamaño del clúster representa la cantidad más pequeña de espacio en disco que se puede usar para conservar un archivo. Cuando los tamaños de archivo no son un múltiplo par del tamaño del clúster, se debe usar espacio adicional para conservar el archivo (hasta el siguiente múltiplo del tamaño del clúster).

Azure File Sync se admite en volúmenes NTFS con Windows Server 2012 R2 y versiones más recientes. En la tabla siguiente se describen los tamaños de clúster predeterminados cuando se crea un volumen NTFS con Windows Server 2019.

|Tamaño del volumen    |Windows Server 2019             |
|---------------|--------------------------------|
|7 MiB – 16 TiB   | 4 KiB                |
|16 TiB – 32 TiB   | 8 KiB                |
|32 TiB – 64 TiB   | 16 KiB               |

Es posible que, después de crear el volumen, se le haya aplicado formato manualmente con otro tamaño de clúster. Si el volumen procede de una versión anterior de Windows, los tamaños de clúster predeterminados también pueden diferir. [En este artículo se ofrecen más detalles sobre los tamaños de clúster predeterminados.](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat) Incluso si elige un tamaño de clúster inferior a 4 KiB, se seguirá aplicando un límite de 8 KiB como el tamaño de archivo más pequeño que se puede organizar en niveles. (Aunque el tamaño doble del clúster técnicamente sea menor que 8 KiB).

El motivo del mínimo absoluto se encuentra en la forma en que NTFS almacena archivos extremadamente pequeños; por ejemplo, de 1 KiB a 4 KiB de tamaño. En función de otros parámetros del volumen, es posible que los archivos pequeños no se almacenen en un clúster del disco. Igualmente, es posible que sea más eficaz almacenar dichos archivos directamente en la tabla de archivos maestros del volumen o en "Registro de MFT". El punto de reanálisis de los niveles de la nube siempre se almacena en el disco y toma exactamente un clúster. La organización en niveles de estos pequeños archivos podría hacer que acabe sin espacio libre. Los casos extremos pueden incluso acabar usando más espacio con la opción de organizar la nube por niveles habilitada. Para evitar esto, el tamaño mínimo de un archivo que la nube por niveles distribuirá en niveles es de 8 KiB en un clúster de 4 KiB o inferior.

## <a name="selecting-your-initial-policies"></a>Selección de las directivas iniciales

Por lo general, cuando se habilita la nube por niveles en un punto de conexión de servidor, se debe crear una unidad virtual local para cada punto de conexión de servidor individual. Al aislar el punto de conexión de servidor, es más fácil comprender cómo funciona la nube por niveles y ajustar las directivas en consecuencia. Sin embargo, Azure File Sync funciona incluso si tiene varios puntos de conexión de servidor en la misma unidad; para más información, consulte la sección [Varios puntos de conexión de servidor en el volumen local](file-sync-cloud-tiering-policy.md#multiple-server-endpoints-on-a-local-volume). También se recomienda que, cuando se habilite por primera vez la nube por niveles, mantenga la directiva de fecha deshabilitada y la directiva de espacio disponible del volumen en torno al 10 % o 20 % como máximo. Con la mayoría de los volúmenes del servidor de archivos, el 20 % de espacio disponible en el volumen suele ser la mejor opción.

Para simplificar y entender claramente cómo se van a almacenar los elementos por niveles, se recomienda ajustar sobre todo la directiva de espacio disponible del volumen y mantener la directiva de fecha deshabilitada a menos que sea necesario. El motivo es que a la mayoría de los clientes le resulta de utilidad rellenar la caché local con tantos archivos activos como sea posible y almacenar por niveles el resto en la nube. Sin embargo, la directiva de fecha puede ser beneficiosa si quiere liberar espacio en el disco local de forma anticipada y sabe que los archivos de ese punto de conexión de servidor a los que se ha accedido después del número de días especificado en esa directiva no necesitan mantenerse localmente. De esta forma, se libera una valiosa capacidad de disco local para que otros puntos de conexión del mismo volumen almacenen en caché más archivos.

Después de establecer las directivas, supervise la salida y ajuste ambas directivas en consecuencia. Se recomienda examinar específicamente las métricas de **tamaño de recuperación de la nube por niveles** y **tamaño de recuperación de la nube por niveles por aplicación** en Azure Monitor. Para saber sobre cómo supervisar la salida, consulte [Supervisión de la nube por niveles](file-sync-monitor-cloud-tiering.md).

## <a name="adjusting-your-policies"></a>Ajuste de las directivas

Si la cantidad de archivos que se recuperan constantemente de Azure es mayor de la deseada, es posible que tenga más archivos activos que espacio para guardarlos en el volumen del servidor local. Si es posible, aumente el tamaño del volumen local o reduzca el porcentaje de la directiva de espacio disponible del volumen en incrementos pequeños. Reducir demasiado el porcentaje de espacio disponible en el volumen también puede tener consecuencias negativas. Para una mayor renovación del conjunto de archivos, se necesita más espacio disponible para los nuevos archivos y la recuperación de los archivos "pasivos". El almacenamiento por niveles se inicia con un retraso de hasta una hora y, luego, necesita tiempo de procesamiento, que es el motivo de que siempre tenga que haber bastante espacio disponible en el volumen.

Mantener más datos locales conlleva menores costos de salida, ya que se recuperarán menos archivos de Azure, pero también requiere una mayor cantidad de almacenamiento local, que tiene su propio costo.

Al ajustar la directiva de espacio disponible del volumen, la cantidad de datos que se deben mantener de forma local viene determinada por los siguientes factores: el ancho de banda, el patrón de acceso del conjunto de datos y el presupuesto. Si tiene una conexión con un ancho de banda bajo, sería conveniente mantener más datos en el entorno local para garantizar retrasos mínimos para los usuarios. En caso contrario, puede basarlo en la tasa de renovación durante un período determinado. Por ejemplo, si sabe que accede activamente al 10 % del conjunto de datos de 1 TiB cada mes, o que este porcentaje de datos cambia, le interesa mantener en el entorno local 100 GiB para no tener que recuperar archivos con frecuencia. Si el volumen es de 2 TiB, será mejor mantener el 5 % (o 100 GiB) en el entorno local, lo que significa que el 95 % restante es el porcentaje de espacio disponible en el volumen. No obstante, debería agregar un búfer de cara a los períodos de mayor renovación, es decir, comenzar con un porcentaje de espacio disponible en el volumen más grande y ajustarlo posteriormente si es necesario.

## <a name="standard-operating-procedures"></a>Procedimientos operativos estándar

- Al migrar por primera vez a Azure Files a través de Azure File Sync, la nube por niveles depende de la carga inicial.
- La nube por niveles comprueba el cumplimiento de las directivas de espacio disponible del volumen y de fecha cada 60 minutos.
- El uso del modificador/LFSM en Robocopy al migrar archivos permitirá que estos se sincronicen y se organicen en la nube por niveles para hacerles sitio durante la carga inicial.
- Si el almacenamiento por niveles se produce antes de que se forme un mapa térmico, los archivos se almacenarán por niveles según la marca de tiempo de la última modificación

## <a name="next-steps"></a>Pasos siguientes

- [Planeamiento de una implementación de Azure File Sync](file-sync-planning.md)
