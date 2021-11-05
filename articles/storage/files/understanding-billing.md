---
title: Descripción de la facturación de Azure Files | Microsoft Docs
description: Aprenda a interpretar los modelos de facturación aprovisionado y de pago por uso para recursos compartidos de archivos de Azure.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 08/17/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 6cac0f689592aa6840520c87438add4bda987b32
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131019103"
---
# <a name="understand-azure-files-billing"></a>Descripción de la facturación de Azure Files
Azure Files proporciona dos modelos de facturación distintos: aprovisionado y pago por uso. El modelo aprovisionado solo está disponible para los recursos compartidos de archivos prémium, que son recursos compartidos de archivos implementados en el tipo de cuenta de almacenamiento **FileStorage**. El modelo de pago por uso solo está disponible para los recursos compartidos de archivos estándar, que son recursos compartidos de archivos implementados en el tipo de cuenta de almacenamiento de **uso general, versión 2 (GPv2)** . En este artículo se explica cómo funcionan ambos modelos con el fin de ayudarle a entender la factura mensual de Azure Files.

:::row:::
    :::column:::
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/m5_-GsKv4-o" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    :::column-end:::
    :::column:::
        En la entrevista de este vídeo se analizan los aspectos básicos del modelo de facturación de Azure Files, incluida la optimización de los recursos compartidos de archivos de Azure para lograr los costos más bajos posibles y la comparación de Azure Files con otras ofertas de almacenamiento de archivos locales y en la nube.
   :::column-end:::
:::row-end:::

Para obtener información sobre los precios de Azure Files, vea la [página de precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="applies-to"></a>Se aplica a
| Tipo de recurso compartido de archivos | SMB | NFS |
|-|:-:|:-:|
| Recursos compartidos de archivos Estándar (GPv2), LRS/ZRS | ![Sí](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |
| Recursos compartidos de archivos Estándar (GPv2), GRS/GZRS | ![Sí](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |
| Recursos compartidos de archivos Premium (FileStorage), LRS/ZRS | ![Sí](../media/icons/yes-icon.png) | ![Sí](../media/icons/yes-icon.png) |

## <a name="storage-units"></a>Unidades de almacenamiento    
Azure Files usa unidades de medida de base 2 para representar la capacidad de almacenamiento: KiB, MiB, GiB y TiB. El sistema operativo puede o no usar la misma unidad de medida o sistema de recuento.

### <a name="windows"></a>Windows
Tanto el sistema operativo Windows como Azure Files miden la capacidad de almacenamiento mediante el sistema de recuento de base 2, pero hay una diferencia al etiquetar las unidades. Azure Files etiqueta su capacidad de almacenamiento con unidades de medida de base 2, mientras que Windows etiqueta su capacidad de almacenamiento en unidades de medida de base 10. Al notificar la capacidad de almacenamiento, Windows no convierte su capacidad de almacenamiento de base 2 a base 10.

| Acrónimo | Definición                         | Unidad     | Windows se muestra como |
|---------|------------------------------------|----------|---------------------|
| KiB     | 1024 bytes                        | kibibyte | KB (kilobyte)       |
| MiB     | 1024 KiB (1 048 576 bytes)        | mebibyte | MB (megabyte)       |
| GiB     | 1024 MiB (1 073 741 824 bytes)     | gibibyte | GB (gigabyte)       |
| TiB     | 1024 GiB (1 099 511 627 776 bytes) | tebibyte | TB (terabyte)       |

### <a name="macos"></a>macOS
Vea [Cómo se indica la capacidad de almacenamiento en iOS y macOS](https://support.apple.com/HT201402) en el sitio web de Apple para determinar qué sistema de recuento se usa.

### <a name="linux"></a>Linux
Cada sistema operativo o cada parte individual del software podrían usar un sistema de recuento diferente. Consulte su documentación para determinar cómo informan de la capacidad de almacenamiento.

## <a name="reserve-capacity"></a>Capacidad de reserva
Azure Files admite reservas de la capacidad de almacenamiento, lo que le permite lograr un descuento en el almacenamiento mediante la confirmación previa del uso del almacenamiento. Debe considerar la posibilidad de comprar instancias reservadas para cualquier carga de trabajo de producción o cargas de trabajo de desarrollo y pruebas con superficies coherentes. Al comprar capacidad reservada, la reserva debe especificar las dimensiones siguientes:

- **Tamaño de capacidad:** las reservas de capacidad pueden ser de 10 TiB o 100 TiB, con descuentos más significativos para comprar una reserva de capacidad mayor. Puede comprar varias reservas, incluso reservas de diferentes tamaños de capacidad para satisfacer los requisitos de la carga de trabajo. Por ejemplo, si la implementación de producción tiene 120 TiB de recursos compartidos de archivos, podría comprar una reserva de 100 TiB y dos reservas de 10 TiB para satisfacer los requisitos de capacidad total.
- **Período**: las reservas se pueden comprar durante un período de un año o tres años, con descuentos más significativos por comprar un período de reserva más largo. 
- **Nivel**: el nivel de Azure Files para la reserva de capacidad. Las reservas de Azure Files están disponibles actualmente para los niveles de acceso prémium, frecuente y esporádico.
- **Ubicación**: la región de Azure para la reserva de capacidad. Las reservas de capacidad están disponibles en un subconjunto de regiones de Azure.
- **Redundancia**: la redundancia de almacenamiento para la reserva de capacidad. Las reservas se admiten para todas los redundancias que admite Azure Files, como LRS, ZRS, GRS y GZRS.

Una vez que compre una reserva de capacidad, el uso de almacenamiento existente la consumirá automáticamente. Si utiliza más espacio de almacenamiento del que ha reservado, pagará el precio de venta del saldo que no esté cubierto por la reserva de capacidad. Los cargos por transferencia de datos, ancho de banda y transacciones no se incluyen en la reserva.

Para obtener más información sobre cómo comprar reservas de almacenamiento, consulte [Optimización de costos para Azure Files con capacidad reservada](files-reserve-capacity.md).

## <a name="provisioned-model"></a>Modelo aprovisionado
Azure Files usa un modelo aprovisionado para los recursos compartidos de archivos prémium. En un modelo de negocio aprovisionado, debe especificar de forma proactiva cuáles son los requisitos de almacenamiento del servicio Azure Files, en lugar de que se le aplique una factura basada en lo que usa. Esto es similar a la compra de hardware local, ya que cuando se aprovisiona un recurso compartido de archivos de Azure con una determinada cantidad de almacenamiento, se paga por ese almacenamiento independientemente de si se usa o no, de la misma manera que no se empiezan a pagar los costos de los soportes físicos locales al empezar a usar el espacio. A diferencia de la compra de soportes físicos locales, los recursos compartidos de archivos aprovisionados se pueden escalar o reducir verticalmente de forma dinámica en función de las características de rendimiento de almacenamiento y de E/S.

El tamaño aprovisionado del recurso compartido de archivos puede aumentar en cualquier momento, pero solo se puede reducir una vez transcurridas 24 horas desde el último aumento. Después de esperar 24 horas sin un aumento de cuota, puede reducir el recurso compartido tantas veces como quiera hasta que lo vuelva a aumentar. Los cambios de escala de IOPS/rendimiento se aplicarán minutos después del cambio de tamaño aprovisionado.

Es posible reducir el tamaño del recurso compartido aprovisionados por debajo de sus GiB usados. Si lo hace, no se perderán datos, pero se le seguirá facturando el tamaño usado y seguirá recibiendo el rendimiento (IOPS de línea de base, rendimiento e IOPS de ráfaga) del recurso compartido aprovisionado, no del tamaño utilizado.

### <a name="provisioning-method"></a>Método de aprovisionamiento
Al aprovisionar un recurso compartido de archivos prémium, se especifica la cantidad de GiB que requiere la carga de trabajo. Cada GiB aprovisionado permite beneficiarse de IOPS y rendimiento adicionales con una proporción fija. Además de la IOPS de línea de base garantizada, cada recurso compartido de archivos prémium admite ráfagas dentro de lo posible. Las fórmulas de IOPS y rendimiento son las siguientes:

| Elemento | Valor |
|-|-|
| Tamaño máximo de un recurso compartido de archivos | 100 GiB |
| Unidad de aprovisionamiento | 1 GiB |
| Fórmula de IOPS de línea de base | `MIN(400 + 1 * ProvisionedGiB, 100000)` |
| Límite de aumento | `MIN(MAX(4000, 3 * ProvisionedGiB), 100000)` |
| Créditos de ráfaga | `(BurstLimit - BaselineIOPS) * 3600` |
| Velocidad de entrada | `40 MiB/sec + 0.04 * ProvisionedGiB` |
| Velocidad de salida | `60 MiB/sec + 0.06 * ProvisionedGiB` |

En la tabla siguiente se ilustran algunos ejemplos de estas fórmulas para los tamaños de recursos compartidos aprovisionados:

| Capacidad (GiB) | IOPS base | IOPS de ráfaga | Créditos de ráfaga | Entrada (MiB/s) | Salida (MiB/s) |
|-|-|-|-|-|-|
| 100 | 500 | Hasta 4000 | 12,600,000 | 44 | 66 |
| 500 | 900 | Hasta 4000 | 11,160,000 | 60 | 90 |
| 1024 | 1424 | Hasta 4000 | 10,713,600 | 81 | 122 |
| 5120 | 5520 | Hasta 15 360 | 35,424,000 | 245 | 368 |
| 10 240 | 10 640 | Hasta 30 720 | 72,288,000 | 450 | 675 |
| 33 792 | 34 192 | Hasta 100 000 | 236,908,800 | 1392 | 2088 |
| 51 200 | 51 600 | Hasta 100 000 | 174,240,000 | 2088 | 3132 |
| 102 400 | 100 000 | Hasta 100 000 | 0 | 4136 | 6204 |

El rendimiento efectivo de los recursos compartidos de archivos está sujeto a los límites de red de la máquina, el ancho de banda de red disponible, los tamaños de E/S y el paralelismo, entre muchos otros factores. Por ejemplo, en función de las pruebas internas con tamaños de e/s de lectura/escritura de 8 KiB, una sola máquina virtual Windows sin SMB multicanal habilitado (*F16s_v2 estándar*) conectada al recurso compartido de archivos Premium a través de SMB podría alcanzar un valor de hasta 20 000 IOPS de lectura y 15 000 IOPS de escritura. Con tamaños de e/s de lectura/escritura de 512 MiB, la misma máquina virtual puede alcanzar un rendimiento de salida de 1,1 GiB/s y 370 MiB/s de entrada. El mismo cliente puede lograr un rendimiento hasta \~ tres veces superior si SMB multicanal está habilitado en los recursos compartidos Premium. Para lograr una escala de rendimiento máxima, [habilite SMB multicanal](files-smb-protocol.md#smb-multichannel) y distribuya la carga entre varias máquinas virtuales. Consulte en el artículo sobre [rendimiento de SMB multicanal](storage-files-smb-multichannel-performance.md) y en la [ guía de solución de problemas](storage-troubleshooting-files-performance.md) algunos problemas de rendimiento comunes y sus soluciones alternativas.

### <a name="bursting"></a>Creación de ráfagas
Si la carga de trabajo necesita un rendimiento adicional para satisfacer los picos de demanda, el recurso compartido puede usar créditos de aumento para superar el límite de IOPS de la línea base del recurso compartido, con el fin de ofrecer el rendimiento de los recursos compartidos que necesita para cubrir la demanda. Los recursos compartidos de archivos Premium pueden aumentar su IOPS hasta 4000 o hasta un factor de tres, el valor que sea más alto. La creación de ráfagas está automatizada y funciona de acuerdo con un sistema de crédito. La creación de ráfagas funciona en la medida de lo posible y el límite de ráfaga no es una garantía.

Cada vez que el tráfico para el recurso compartido de archivos se encuentra por debajo del valor de IOPS de la línea de base, se acumulan créditos en un cubo de ráfagas. Por ejemplo, un recurso compartido de 100 GiB tiene un IOPS de línea base de 500. Si el tráfico real del recurso compartido era de 100 IOPS durante un intervalo específico de 1 segundo, el valor de 400 IOPS sin usar se agrega a un cubo de aumentos. Del mismo modo, un recurso compartido de 1 TiB inactivo acumula crédito de aumento en 1424 IOPS. Luego, estos créditos se usan más tarde si las operaciones superan el valor de IOPS de línea base.

Cada vez que un recurso compartido supera el valor de IOPS de línea base y tiene créditos en un cubo de aumentos, este aumentará hasta alcanzar la velocidad máxima permitida. Los recursos compartidos pueden seguir aumentando siempre y cuando queden créditos, pero esto se basa en el número de créditos de aumento acumulados. Cada E/S que supere el valor de IOPS de línea base consume un crédito y, una vez que se consumen todos los créditos, el recurso compartido volvería al valor de IOPS de la línea base.

Los créditos de recursos compartidos tienen tres estados:

- Acumulado: cuando el recurso compartido de archivos usa un valor inferior al de IOPS de línea de base.
- Rechazado: cuando el recurso compartido de archivos usa más que el valor de IOPS de la línea base y en el modo de aumento.
- Constante: cuando el recurso compartido de archivos usa exactamente el valor de IOPS de línea de base, no hay créditos acumulados ni usados.

Los nuevos recursos compartidos de archivo empiezan con la cantidad total de créditos del cubo de ráfagas. Los créditos de ráfaga no se acumularán si el valor de IOPS del recurso compartido cae por debajo del valor de IOPS de la línea de base debido a una limitación del servidor.

## <a name="pay-as-you-go-model"></a>Modelo de pago por uso
Azure Files usa un modelo de negocio de pago por uso para los recursos compartidos de archivos estándar. En un modelo de negocio de pago por uso, la cantidad que se paga se determina según el volumen que se usa realmente, en lugar de basarse en una cantidad aprovisionada. En un nivel alto, se paga un costo por la cantidad de datos lógicos almacenados y, a continuación, un conjunto adicional de transacciones en función del uso de esos datos. Un modelo de pago por uso puede ser rentable, ya que no es necesario sobreaprovisionar para tener en cuenta los requisitos futuros de crecimiento o rendimiento ni el desaprovisionamiento si la carga de trabajo tiene una superficie de datos que varía con el tiempo. Por otro lado, un modelo de pago por uso también puede ser difícil de planear como parte de un proceso de presupuesto, porque este modelo se basa en el consumo del usuario final.

### <a name="differences-in-standard-tiers"></a>Diferencias en los niveles estándar
Al crear un recurso compartido de archivos estándar, puede elegir entre los niveles de transacción optimizada, acceso frecuente y acceso esporádico. Los tres niveles se almacenan en el mismo hardware de almacenamiento estándar. La principal diferencia de estos tres niveles son los precios de almacenamiento de datos en reposo, que son menores en los niveles de acceso esporádico, y los precios de las transacciones, que son más altos en estos mismos niveles. Esto significa lo siguiente:

- La transacción optimizada, como su nombre implica, optimiza el precio de las cargas de trabajo de mucha transacción. La transacción optimizada tiene el precio de almacenamiento de datos en reposo más alto, pero los precios de transacción más bajos.
- El acceso frecuente es para cargas de trabajo activas que no implican un gran número de transacciones, y tiene un precio de almacenamiento de datos en reposo ligeramente inferior, pero los precios de transacción son algo mayores, en comparación con la transacción optimizada. Considérelo como el punto medio entre los niveles de transacción optimizada y acceso esporádico.
- El acceso esporádico optimiza el precio de las cargas de trabajo que no tienen mucha actividad y ofrece el precio más bajo de datos en reposo, pero el más alto en las transacciones.

Si coloca una carga de trabajo a la que se accede con poca frecuencia en el nivel de transacción optimizada, no pagará casi nada por las pocas horas del mes en que realiza transacciones en el recurso compartido, pero pagará una cantidad elevada por los costos de almacenamiento de datos. Si tuviera que trasladar este mismo recurso compartido al nivel de acceso esporádico, tampoco pagaría casi nada por los costos de transacción, simplemente porque no realiza transacciones con mucha frecuencia en esta carga de trabajo, pero el nivel de acceso esporádico ofrece un precio de almacenamiento de datos mucho más barato. La selección del nivel adecuado para su caso de uso le permite reducir considerablemente los costos.

Del mismo modo, si coloca en el nivel de acceso esporádico una carga de trabajo a la que accede con mucha frecuencia, incurrirá en muchos más costos por las transacciones, pero pagará menos por el almacenamiento de datos. Esto puede derivar en una situación en la que el aumento de los costos por los precios de las transacciones sobrepasan el ahorro obtenido por el precio más reducido del almacenamiento de datos, de tal forma que pagará más dinero en el nivel de acceso esporádico en comparación con el de transacción optimizada. Puede que, para algunos niveles de uso, mientras que el nivel de acceso frecuente será el nivel más rentable, el de acceso esporádico será más caro que el de transacción optimizada.

El nivel de la carga de trabajo y de la actividad determinarán el nivel más rentable del recurso compartido de archivos estándar. En la práctica, la mejor manera de elegir el nivel más rentable es examinar el consumo de recursos real del recurso compartido (datos almacenados, transacciones de escritura, etc.).

### <a name="logical-size-versus-physical-size"></a>Tamaño lógico frente a tamaño físico
El cargo de capacidad de datos en reposo de Azure Files se factura en función del tamaño lógico, denominado coloquialmente "tamaño" o "longitud del contenido" del archivo. El tamaño lógico del archivo es distinto del tamaño físico del archivo en el disco, a menudo denominado "tamaño en disco" o "tamaño usado". El tamaño físico del archivo puede ser mayor o menor que el tamaño lógico.

### <a name="what-are-transactions"></a>¿Qué son las transacciones?
Las transacciones son operaciones o solicitudes realizadas en Azure Files para cargar, descargar o manipular de otro modo el contenido del recurso compartido de archivos. Todas las acciones realizadas en un recurso compartido de archivos se traducen en una o varias transacciones, y en recursos compartidos estándar que usan el modelo de facturación de pago por uso, que genera costos por transacciones.

Hay cinco categorías de transacción básicas: escritura, lista, lectura, otras y eliminar. Todas las operaciones realizadas con la API de REST o SMB se agrupan en una de las cuatro categorías siguientes:

| Cubo de transacciones | Operaciones de administración | Operaciones de datos |
|-|-|-|
| Transacciones de escritura | <ul><li>`CreateShare`</li><li>`SetFileServiceProperties`</li><li>`SetShareMetadata`</li><li>`SetShareProperties`</li><li>`SetShareACL`</li></ul> | <ul><li>`CopyFile`</li><li>`Create`</li><li>`CreateDirectory`</li><li>`CreateFile`</li><li>`PutRange`</li><li>`PutRangeFromURL`</li><li>`SetDirectoryMetadata`</li><li>`SetFileMetadata`</li><li>`SetFileProperties`</li><li>`SetInfo`</li><li>`Write`</li><li>`PutFilePermission`</li></ul> |
| Transacciones de lista | <ul><li>`ListShares`</li></ul> | <ul><li>`ListFileRanges`</li><li>`ListFiles`</li><li>`ListHandles`</li></ul> |
| Transacciones de lectura | <ul><li>`GetFileServiceProperties`</li><li>`GetShareAcl`</li><li>`GetShareMetadata`</li><li>`GetShareProperties`</li><li>`GetShareStats`</li></ul> | <ul><li>`FilePreflightRequest`</li><li>`GetDirectoryMetadata`</li><li>`GetDirectoryProperties`</li><li>`GetFile`</li><li>`GetFileCopyInformation`</li><li>`GetFileMetadata`</li><li>`GetFileProperties`</li><li>`QueryDirectory`</li><li>`QueryInfo`</li><li>`Read`</li><li>`GetFilePermission`</li></ul> |
| Otras transacciones | | <ul><li>`AbortCopyFile`</li><li>`Cancel`</li><li>`ChangeNotify`</li><li>`Close`</li><li>`Echo`</li><li>`Ioctl`</li><li>`Lock`</li><li>`Logoff`</li><li>`Negotiate`</li><li>`OplockBreak`</li><li>`SessionSetup`</li><li>`TreeConnect`</li><li>`TreeDisconnect`</li><li>`CloseHandles`</li><li>`AcquireFileLease`</li><li>`BreakFileLease`</li><li>`ChangeFileLease`</li><li>`ReleaseFileLease`</li></ul> |
| Transacciones de eliminación | <ul><li>`DeleteShare`</li></ul> | <ul><li>`ClearRange`</li><li>`DeleteDirectory`</li></li>`DeleteFile`</li></ul> |  

> [!Note]  
> NFS 4.1 solo está disponible para los recursos compartidos de archivos prémium, que usan el modelo de facturación aprovisionado. Las transacciones no afectan a la facturación de los recursos compartidos de archivos prémium.

## <a name="value-add-services"></a>Servicios de valor agregado

### <a name="azure-file-sync"></a>Azure File Sync
Si está pensando en usar Azure File Sync, tenga en cuenta lo siguiente al evaluar el costo:

#### <a name="server-fee"></a>Cuota del servidor
Por cada servidor que se haya conectado a un grupo de sincronización, hay un cargo adicional de 5 USD. Esto es independiente del número de puntos de conexión de servidor. Por ejemplo, si tuviera un servidor con tres puntos de conexión de servidor diferentes, solo tendría un cargo de 5 USD. Un servidor de sincronización es gratuito por servicio de sincronización de almacenamiento. 

#### <a name="data-cost"></a>Costo de los datos
El costo de los datos en reposo depende del nivel de facturación que elija. Este es el costo de almacenar datos en el recurso compartido de archivos de Azure en la nube, incluido el almacenamiento de instantáneas.  

#### <a name="cloud-enumeration-scans-cost"></a>Costo de los exámenes de enumeración en la nube
Azure File Sync enumera el recurso compartido de archivos de Azure en la nube una vez al día para detectar los cambios que se realizaron directamente en el recurso compartido para que puedan sincronizarse con los puntos de conexión del servidor. Este examen genera transacciones que se facturan a la cuenta de almacenamiento a una tasa de dos transacciones LIST por directorio al día. Puede poner este número en la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para calcular el costo del examen.  

> [!Tip]  
> Si no está seguro de cuántas carpetas tiene, consulte la herramienta TreeSize de JAM Software GmbH.

#### <a name="churn-and-tiering-costs"></a>Costos de renovación y almacenamiento por niveles
A medida que los archivos cambian en los puntos de conexión del servidor, los cambios se cargan en el recurso compartido en la nube, lo que genera transacciones. Cuando se habilita la nube por niveles, se generan transacciones adicionales para administrar los archivos en niveles, incluida la E/S que se está produciendo en los archivos en niveles, además de los costos de salida. La cantidad y el tipo de transacciones son difíciles de predecir debido a las tasas de renovación y la eficacia de la memoria caché, pero puede usar los patrones de transacción anteriores para predecir los costos futuros si solo tiene un recurso compartido de archivos en la cuenta de almacenamiento. Consulte [Elección de un nivel de facturación](#choosing-a-billing-tier) para obtener más información sobre cómo ver las transacciones anteriores.  

#### <a name="choosing-a-billing-tier"></a>Elección de un nivel de facturación
Para clientes de Azure File Sync, se recomienda elegir recursos compartidos de archivos estándar en lugar de recursos compartidos de archivos Premium. Esto se debe a que con Azure File Sync los clientes obtienen esa baja latencia local que siempre tenían, por lo que el mayor rendimiento que proporcionan los recursos compartidos de archivos Premium no es necesario. Al migrar por primera vez a Azure Files a través de Azure File Sync, se recomienda el nivel Optimizado para transacciones debido al gran número de transacciones en las que se incurre durante la migración. Una vez completada la migración, puede indicar las transacciones anteriores en la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) para averiguar qué nivel es más adecuado para la carga de trabajo. 

Para ver las transacciones anteriores:
1. Vaya a la cuenta de almacenamiento y seleccione **Métricas** en la barra de navegación izquierda.
2. Seleccione **Ámbito** como nombre de la cuenta de almacenamiento, **Espacio de nombres de métricas** como "Archivo", **Métrica** como "Transacciones" y **Agregación** como "Suma".
3. Haga clic en **Aplicar división**.
4. Seleccione **Valores** como "Nombre de API". Seleccione los valores deseados en **Límite** y **Orden**.
5. Seleccione el período de tiempo deseado.

> [!Note]  
> Asegúrese de ver las transacciones durante un período de tiempo para obtener una mejor idea del número medio de transacciones. Asegúrese de que el período de tiempo elegido no se superponga con el aprovisionamiento inicial. Multiplique el número medio de transacciones durante este período de tiempo para obtener las transacciones estimadas durante todo un mes.

## <a name="file-storage-comparison-checklist"></a>Lista de comprobación para la comparación del almacenamiento de archivos
Para evaluar correctamente el costo de Azure Files en comparación con otras opciones de almacenamiento de archivos, tenga en cuenta las siguientes preguntas:

- **¿Cómo se paga por el almacenamiento, IOPS y el ancho de banda?**  
    Con Azure Files, el modelo de facturación que use depende de si va a implementar recursos compartidos de archivos [prémium](#provisioned-model) o [estándar](#pay-as-you-go-model). La mayoría de las soluciones en la nube tienen modelos en consonancia con los principios del almacenamiento aprovisionado (determinismo de precios, simplicidad) o el almacenamiento de pago por uso (pagar solo por el uso real). En el caso de los modelos provisionados, son de especial interés el tamaño mínimo de la cuota aprovisionada, la unidad de aprovisionamiento y la posibilidad de aumentar y disminuir el aprovisionamiento. 

- **¿Cómo se consigue la resistencia y la redundancia del almacenamiento?**  
    Con Azure Files, la resistencia y la redundancia del almacenamiento están integradas en la oferta del producto. Todos los niveles y opciones de redundancia garantizan una alta disponibilidad de los datos y el acceso a al menos tres copias de estos. Al considerar otras opciones de almacenamiento de archivos, tenga en cuenta si la resistencia y la redundancia del almacenamiento están integradas o es algo que debe ensamblar personalmente. 

- **¿Qué tiene que administrar?**  
    Con Azure Files, la unidad básica de administración es una cuenta de almacenamiento. Otras soluciones pueden requerir administración adicional, como actualizaciones del sistema operativo o administración de recursos virtuales (máquinas virtuales, discos, direcciones IP de red, etc.).

- **¿Cuáles son los costos de copia de seguridad?**  
    Con Azure Files, la integración de Azure Backup se habilita fácilmente y el almacenamiento de copia de seguridad se factura como parte de la cuota de costos (las copias de seguridad se almacenan como instantáneas diferenciales). Otras soluciones pueden requerir licencias de software de copia de seguridad e implicar costos adicionales de almacenamiento de copia de seguridad.

## <a name="see-also"></a>Consulte también
- [Página de precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/)
- [Planeamiento de una implementación de Azure Files](storage-files-planning.md) y [Planeamiento de una implementación de Azure File Sync](../file-sync/file-sync-planning.md)
- [Creación de un recurso compartido de archivos](storage-how-to-create-file-share.md) e [Implementación de Azure File Sync](../file-sync/file-sync-deployment-guide.md)
