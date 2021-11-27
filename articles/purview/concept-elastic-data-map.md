---
title: Mapa de datos elástico
description: En este artículo se explican los conceptos del servicio Mapa de datos elástico en Azure Purview
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-map
ms.topic: conceptual
ms.date: 08/18/2021
ms.custom: template-concept
ms.openlocfilehash: fd667abab0802d6d11fc855a620bf51f8419b978
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132555829"
---
# <a name="elastic-data-map-in-azure-purview"></a>Mapa de datos elástico en Azure Purview

Mapa de datos de Azure Purview proporciona la base para la detección de datos y la gobernanza de estos. Captura metadatos sobre los datos empresariales presentes en análisis, software como servicio (SaaS) y sistemas operativos en entornos híbridos, locales y de varias nubes. Mapa de datos de Purview se actualiza automáticamente con su sistema de clasificación y examen integrado. Con la interfaz de usuario, los desarrolladores pueden interactuar más mediante programación con Mapa de datos utilizando las API de Apache Atlas de código abierto.

## <a name="elastic-data-map"></a>Mapa de datos elástico

Todas las cuentas de Azure Purview tienen un servicio Mapa de datos que puede crecer elásticamente a partir de una unidad de capacidad. Se escalan y reducen verticalmente en función de la carga de solicitudes dentro de la ventana de elasticidad ([compruebe los límites actuales](how-to-manage-quotas.md)). Estos límites deben cubrir la mayoría de los escenarios de datos. Pero si necesita mayor capacidad, [puede crear una incidencia de soporte técnico](#request-capacity).

## <a name="data-map-capacity-unit"></a>Unidad de capacidad de Mapa de datos

El servicio Mapa de datos elástico incluye componentes de almacenamiento y rendimiento de operación que se representan como unidad de capacidad (CU). De forma predeterminada, todas las cuentas de Azure Purview, se proporcionan con una unidad de capacidad y aumentan elásticamente en función del uso. Cada unidad de capacidad de Mapa de datos incluye un rendimiento de 25 operaciones por segundo y 2 GB de límite de almacenamiento de metadatos.  

### <a name="operations"></a>Operaciones

Las operaciones son la medida de rendimiento de Mapa de datos de Purview. Incluyen las operaciones Crear, Leer, Escribir, Actualizar y Eliminar en los metadatos almacenados en Mapa de datos. A continuación se enumeran algunos ejemplos de operaciones:

- Creación de un recurso en Mapa de datos
- Agregue una relación a un recurso como propietario, administrador, primario, linaje, etc.
- Edite un recurso para agregar metadatos empresariales, como la descripción, el término del glosario, etc.
- Búsqueda de palabras clave que devuelve resultados a la página de resultados de búsqueda.

### <a name="storage"></a>Almacenamiento

El almacenamiento es el segundo componente de Mapa de datos e incluye metadatos técnicos, empresariales, operativos y semánticos.

Los metadatos técnicos incluyen el esquema, el tipo de datos, las columnas, etc., que se detectan en el [examen](concept-scans-and-ingestion.md) de Purview. Los metadatos empresariales incluyen el etiquetado automatizado (por ejemplo, promocionado a partir de conjuntos de datos de Power BI o descripciones de tablas de SQL) y el etiquetado manual de descripciones, términos del glosario, etc. Algunos ejemplos de metadatos semánticos son la asignación de colecciones a orígenes de datos o clasificaciones. Los metadatos operativos incluyen el estado de ejecución de la actividad de flujo de datos y copia de Data Factory, y el tiempo de ejecución.

## <a name="work-with-elastic-data-map"></a>Trabajo con el mapa de datos elástico

- **Mapa de datos elástico con escalado automático**: comenzará con un servicio Mapa de datos tan bajo como una unidad de capacidad que se pueda escalar automáticamente en función de la carga. Para la mayoría de las organizaciones, esta característica dará lugar a un mayor ahorro y un menor precio para iniciar proyectos de gobernanza de datos. Esta característica afectará a los precios.

- **Análisis mejorado e ingesta**: puede realizar un seguimiento y controlar la población de los recursos de datos, así como la clasificación y el linaje en los procesos de examen e ingesta. Esta característica afectará a los precios.

## <a name="scenario"></a>Escenario

Claudia es una administradora de Azure en Contoso que quiere aprovisionar una nueva cuenta de Azure Purview desde Azure Portal. Durante el aprovisionamiento, no conoce el tamaño necesario del Mapa de datos de Purview para admitir el estado futuro de la plataforma. Sin embargo, sabe que Mapa de datos de Purview se factura por unidades de capacidad, lo que se ve afectado por el rendimiento del almacenamiento y las operaciones. Quiere aprovisionar el servicio Mapa de datos más pequeño para mantener el costo bajo y aumentar el tamaño de dicho servicio de forma elástica en función del consumo.  

Puede crear una cuenta de Purview con el tamaño predeterminado del servicio de Mapa de datos de 1 unidad de capacidad que se puede escalar y reducir verticalmente automáticamente. La característica de escalado automático también permite optimizar la capacidad en función de ráfagas de datos intermitentes o planeadas durante períodos específicos. Claudia sigue los pasos que se indican a continuación en la experiencia de aprovisionamiento para definir la configuración de red y completa el aprovisionamiento.  

En la página de métricas de Azure Monitor, Claudia puede ver el consumo de almacenamiento y el rendimiento de las operaciones de Mapa de datos. Puede incluso configurar una alerta cuando el almacenamiento o el rendimiento de las operaciones alcance un límite determinado para supervisar el consumo y la facturación de la nueva cuenta de Purview.  

## <a name="data-map-billing"></a>Facturación del mapa de datos

A los clientes se les factura una unidad de capacidad (25 operaciones por segundo y 2 GB) y la facturación adicional se basa en el consumo de cada unidad de capacidad adicional que se acumula a la hora. Las operaciones de Mapa de datos se escalan en incrementos de 25 operaciones por segundo y el almacenamiento de metadatos se escala en incrementos de 2 GB de tamaño. Mapa de datos de Purview puede escalarse y reducirse verticalmente de forma automática dentro de la ventana de elasticidad ([compruebe los límites actuales](how-to-manage-quotas.md)). Sin embargo, para obtener el siguiente nivel de ventana de elasticidad, es necesario crear una incidencia de soporte técnico.

Las unidades de capacidad de Mapa de datos incluyen un extremo en el rendimiento de las operaciones y el almacenamiento. Si el almacenamiento supera la unidad de capacidad actual, se cobra a los clientes por la siguiente unidad de capacidad, incluso si no se usa el rendimiento de las operaciones. En la tabla siguiente se muestran los intervalos de la unidad de capacidad de Mapa de datos. Póngase en contacto con el soporte técnico si la unidad de capacidad de Mapa de datos supera las 100 unidades de capacidad.

|Unidad de capacidad de Mapa de datos  |Rendimiento de operaciones por segundo   |Capacidad de almacenamiento en GB|
|----------|-----------|------------|
|1    |25      |2     |
|2    |50      |4     |
|3    |75      |6     |
|4    |100     |8     |
|5    |125     |10    |
|6    |150     |12   |
|7    |175      |14     |
|8    |200     |16    |
|9    |225      |18    |
|10    |250     |20    |
|..   |..      |..     |
|100    |2.500     |200   |

### <a name="billing-examples"></a>Ejemplos de facturación

- El rendimiento de las operaciones de Mapa de datos de Purview durante la hora determinada es menor o igual que 25 operaciones por segundo y el tamaño de almacenamiento es de 1 GB. A los clientes se les factura una unidad de capacidad.

- El rendimiento de las operaciones de Mapa de datos de Purview durante la hora determinada es menor o igual que 25 operaciones por segundo y el tamaño de almacenamiento es de 3 GB. A los clientes se les factura dos unidades de capacidad.

- El rendimiento de las operaciones de Mapa de datos de Purview durante la hora determinada es de 50 operaciones por segundo y el tamaño de almacenamiento es de 1 GB. A los clientes se les factura dos unidades de capacidad.

- El rendimiento de las operaciones de Mapa de datos de Purview durante la hora determinada es de 50 operaciones por segundo y el tamaño de almacenamiento es de 5 GB. A los clientes se les factura tres unidades de capacidad.

- El rendimiento de las operaciones de Mapa de datos de Purview durante la hora determinada es de 250 operaciones por segundo y el tamaño de almacenamiento es de 5 GB. A los clientes se les factura diez unidades de capacidad.

### <a name="detailed-billing-example"></a>Ejemplo detallado de facturación

El ejemplo de facturación de Mapa de datos siguiente muestra un Mapa de datos con almacenamiento de metadatos creciente y operaciones variables por segundo en un período de seis horas, de 12 p. m. a 6 p. m. La línea roja del gráfico es el consumo de operaciones por segundo y la línea de puntos azul es el consumo de almacenamiento de metadatos durante este período de seis horas:

:::image type="content" source="./media/concept-elastic-data-map/operations-and-metadata.png" alt-text="Gráfico que muestra el número de operaciones y el crecimiento de metadatos a lo largo del tiempo.":::

Cada unidad de capacidad de Mapa de datos admite 25 operaciones por segundo y 2 GB de almacenamiento de metadatos. Mapa de datos se factura por hora. Se le facturará las unidades de capacidad de Mapa de datos máximas necesarias durante una hora. En ocasiones, es posible que necesite más operaciones por segundo en el plazo de una hora, lo que aumentará el número de unidades de capacidad necesarias en esa hora. En otras ocasiones, el uso de operaciones por segundo puede ser bajo, pero es posible que siga necesitado un gran volumen de almacenamiento de metadatos. El almacenamiento de metadatos es lo que determina cuántas unidades de capacidad necesita en el plazo de una hora.

En la tabla siguiente se muestra el número máximo de operaciones por segundo y el almacenamiento de metadatos usado por hora para este ejemplo de facturación: 

:::image type="content" source="./media/concept-elastic-data-map/billing-table.png" alt-text="Tabla que muestra el número máximo de operaciones y el crecimiento de metadatos a lo largo del tiempo.":::

En función del consumo de almacenamiento de metadatos y de las operaciones por segundo de Mapa de datos en este período, esta instancia de Mapa de datos se facturaría durante 20 horas de unidad de capacidad durante este período de seis horas (1 + 3 + 4 + 3 + 6 + 3):

:::image type="content" source="./media/concept-elastic-data-map/billing-capacity-hours.png" alt-text="Tabla que muestra el número de horas de CU a lo largo del tiempo.":::

>[!Important]
>Mapa de datos de Purview puede escalarse y reducirse verticalmente de forma automática dentro de la ventana de elasticidad ([compruebe los límites actuales](how-to-manage-quotas.md)). Para obtener el siguiente nivel de ventana de elasticidad, es necesario crear una incidencia de soporte técnico.

## <a name="request-capacity"></a>Solicitud de capacidad

Si trabaja con conjuntos de datos muy grandes o un entorno masivo y necesita mayor capacidad para el mapa de datos elástico, puede solicitar más capacidad de la ventana de elasticidad mediante la [creación de una incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Seleccione **Límites de servicio y suscripción (cuotas)** y siga las instrucciones en pantalla; para ello, elija la cuenta de Purview para la que quiere solicitar más capacidad.

:::image type="content" source="./media/concept-elastic-data-map/increase-limit.png" alt-text="Pantalla que muestra la creación de una incidencia de soporte técnico, con las opciones de aumento de límites seleccionadas.":::

En la descripción proporcione tanta información relevante como pueda sobre el entorno y la capacidad adicional que le gustaría solicitar.

## <a name="monitoring-the-elastic-data-map"></a>Supervisión del mapa de datos elásticos

Las métricas de las _unidades de capacidad del mapa de datos_ y del _tamaño de almacenamiento del mapa de datos_ se pueden supervisar para comprender el tamaño del estado de los datos y la facturación.

1. Vaya a [Azure Portal](https://portal.azure.com), vaya a la página **Cuentas de Purview** y seleccione su _cuenta de Purview_.

2. Haga clic en **Información general** y desplácese hacia abajo para observar la sección **Supervisión** de las métricas de las _unidades de capacidad del mapa de datos_ y las del _tamaño de almacenamiento del mapa de datos_ en diferentes períodos de tiempo.

    :::image type="content" source="./media/concept-elastic-data-map/data-map-metrics.png" alt-text="Captura de pantalla del menú que muestra la página de información general de las métricas del mapa de datos elásticos.":::

3. Para ver configuraciones adicionales, vaya a **Supervisión -> Métricas** para observar las **Unidades de capacidad del mapa de datos** y el **Tamaño de almacenamiento del mapa de datos**.

    :::image type="content" source="./media/concept-elastic-data-map/elastic-data-map-metrics.png" alt-text="Captura de pantalla del menú que muestra las métricas.":::

4. Haga clic en **Unidades de capacidad del mapa de datos** para ver el uso de la unidad de capacidad del mapa de datos durante las últimas 24 horas. Observe que al mantener el mouse sobre el gráfico de líneas, se indicarán las unidades de capacidad del mapa de datos consumidas a esa hora concreta en un día determinado.

    :::image type="content" source="./media/concept-elastic-data-map/data-map-capacity-default.png" alt-text="Captura de pantalla del menú que muestra las unidades de capacidad del mapa de datos consumidas durante 24 horas.":::

5. Haga clic en **Hora local: últimas 24 horas (automático: 1 hora)** en la parte superior derecha de la pantalla para modificar el intervalo de tiempo que se muestra para el gráfico.

    :::image type="content" source="./media/concept-elastic-data-map/data-map-capacity-custom.png" alt-text="Captura de pantalla del menú que muestra las unidades de capacidad del mapa de datos consumidas durante un intervalo de tiempo personalizado.":::

    :::image type="content" source="./media/concept-elastic-data-map/data-map-capacity-time-range.png" alt-text="Captura de pantalla del menú que muestra las unidades de capacidad del mapa de datos consumidas durante un intervalo de tiempo de tres días.":::

6. Para personalizar el tipo de gráfico, haga clic en la opción como se indica a continuación.

    :::image type="content" source="./media/concept-elastic-data-map/data-map-capacity-graph-type.png" alt-text="Captura de pantalla del menú que muestra las opciones para modificar el tipo de gráfico.":::

7. Haga clic en el **gráfico nuevo** para agregar el gráfico del tamaño de almacenamiento del mapa de datos.

    :::image type="content" source="./media/concept-elastic-data-map/data-map-storage-size.png" alt-text="Captura de pantalla del menú que muestra el tamaño de almacenamiento del mapa de datos usado.":::

## <a name="summary"></a>Resumen

Con el servicio Mapa de datos elástico, Purview proporciona una barrera de bajo costo para que los clientes inicien su recorrido en la gobernanza de datos.
DataMap de Purview puede crecer elásticamente con el modelo de pago por uso a partir de tan solo una unidad de capacidad.
Los clientes no tienen que preocuparse por elegir el tamaño de Mapa de datos correcto para su patrimonio de datos en tiempo de aprovisionamiento ni tratar con las migraciones de plataforma en un futuro debido a los límites de tamaño.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una cuenta de Azure Purview](create-catalog-portal.md)
- [Precios de Purview](https://azure.microsoft.com/pricing/details/azure-purview/)
