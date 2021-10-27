---
title: Administración de recursos y cuotas
titleSuffix: Azure Machine Learning
description: Obtenga información sobre las cuotas y límites de los recursos de Azure Machine Learning y cómo solicitar aumentos de cuota.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: SimranArora904
ms.author: siarora
ms.date: 06/14/2021
ms.topic: how-to
ms.custom: troubleshooting,contperf-fy20q4, contperf-fy21q2
ms.openlocfilehash: bbfcf7fd3edb483415041a4706e7a5cd22e2d76a
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129997126"
---
# <a name="manage-and-increase-quotas-for-resources-with-azure-machine-learning"></a>Administración y aumento de las cuotas de los recursos con Azure Machine Learning

Azure usa límites y cuotas para evitar rebasamientos presupuestarios por fraudes y para respetar las restricciones de capacidad de Azure. Tenga en cuenta estos límites a medida que escala para cargas de trabajo de producción. En este artículo, aprenderá lo siguiente:

> [!div class="checklist"]
> + Límites predeterminados de los recursos de Azure relacionados con [Azure Machine Learning](overview-what-is-azure-machine-learning.md).
> + Creación de cuotas de nivel de área de trabajo.
> + Visualización de las cuotas y los límites.
> + Solicitud de aumentos de cuota.

Además de administrar cuotas, puede obtener información sobre el [Planeamiento y administración de los costos de Azure Machine Learning](concept-plan-manage-cost.md) o sobre los [Límites de servicio en Azure Machine Learning](resource-limits-quotas-capacity.md).

## <a name="special-considerations"></a>Consideraciones especiales

+ Una cuota es un límite de crédito, no una garantía de capacidad. Si tiene necesidades de capacidad a gran escala, [póngase en contacto con el soporte técnico de Azure para incrementar la cuota](#request-quota-increases).

+ Una cuota se comparte entre todos los servicios de las suscripciones, incluido Azure Machine Learning. Calcule el uso en todos los servicios al evaluar la capacidad.
 
  El proceso de Azure Machine Learning es una excepción. Tiene una cuota independiente de la cuota de proceso base. 

+ Los límites predeterminados varían según el tipo de categoría de la oferta, por ejemplo, evaluación gratuita, pago por uso, y la serie de la máquina virtual (VM), como Dv2, F y G.

## <a name="default-resource-quotas"></a>Cuotas de recursos predeterminadas

En esta sección, obtendrá información acerca de los límites de cuota predeterminados y máximos para los siguientes recursos:

+ Recursos de Azure Machine Learning
  + Proceso de Azure Machine Learning
  + Canalizaciones de Azure Machine Learning
+ Máquinas virtuales
+ Azure Container Instances
+ Azure Storage

> [!IMPORTANT]
> Los límites están sujetos a cambios. Para obtener la información más reciente, consulte [Límites de servicio en Azure Machine Learning](resource-limits-quotas-capacity.md).



### <a name="azure-machine-learning-assets"></a>Recursos de Azure Machine Learning
En cada área de trabajo se aplican los límites siguientes a los recursos. 

| **Recurso** | **Límite máximo** |
| --- | --- |
| Conjuntos de datos | 10 millones |
| Ejecuciones | 10 millones |
| Modelos | 10 millones|
| Artifacts | 10 millones |

Además, el **tiempo de ejecución** máximo es de 30 días y el número máximo de **métricas registradas por ejecución** es de 1 millón.

### <a name="azure-machine-learning-compute"></a>Proceso de Azure Machine Learning
[Proceso de Azure Machine Learning](concept-compute-target.md#azure-machine-learning-compute-managed) tiene un límite de cuota predeterminado sobre el número de núcleos (dividido entre la familia de máquinas virtuales y los núcleos totales acumulados) y sobre el número de recursos de proceso únicos permitidos por región en una suscripción. Esta cuota es independiente de la cuota de núcleos de máquina virtual indicada en la sección anterior, ya que solo se aplica a los recursos de proceso administrados de Azure Machine Learning.

[Solicite un aumento de la cuota](#request-quota-increases) para incrementar los límites de las cuotas de núcleos de la familia de máquinas virtuales, las cuotas de núcleos totales de la suscripción y los recursos de esta sección.

Recursos disponibles:
+ Los **núcleos dedicados por región** tienen un límite predeterminado de 24 a 300, según el tipo de oferta de la suscripción. Es posible aumentar el número de núcleos dedicados por suscripción de cada familia de máquinas virtuales. Las familias de máquinas virtuales especializadas, como las series NCv2, NCv3 o ND, comienzan con un valor predeterminado de cero núcleos.

+ Los **núcleos de baja prioridad por región** tienen un límite predeterminado de 100 a 3000, según el tipo de oferta de la suscripción. El número de núcleos de baja prioridad por suscripción se puede aumentar y es un valor único en todas las familias de máquinas virtuales.

+ Los **clústeres por región** tienen un límite predeterminado de 200. Se comparten entre un clúster de entrenamiento y una instancia de proceso. (Una instancia de proceso se considera un clúster de un solo nodo para los fines de la cuota).

> [!TIP]
> Para obtener más información sobre la familia de máquinas virtuales para la que se va a solicitar un aumento de cuota, vea [Tamaños de las máquinas virtuales en Azure](../virtual-machines/sizes.md). Por ejemplo, las familias de máquinas virtuales de GPU comienzan por una "N" en su nombre de familia (por ejemplo, serie NCv3)

En la siguiente tabla se muestran los límites adicionales en la plataforma. Póngase en contacto con el equipo del producto de AzureML por medio de una incidencia de soporte **técnico** para solicitar una excepción.

| **Recurso o acción** | **Límite máximo** |
| --- | --- |
| Número de áreas de trabajo por grupo de recursos | 800 |
| Nodos de una única configuración de **clúster** de Proceso de Azure Machine Learning (AmlCompute) como un grupo no habilitado para la comunicación (es decir, que no puede ejecutar trabajos de MPI) | 100 nodos, pero configurable hasta 65 000 nodos |
| Los nodos en un único paso de ejecución paralelo se **ejecutan** en un clúster de Proceso de Azure Machine Learning (AmlCompute) | 100 nodos, pero configurable hasta 65 000 nodos si el clúster está configurado para escalar por encima |
| Nodos de una única configuración de **clúster** de Proceso de Azure Machine Learning (AmlCompute) como un grupo habilitado para la comunicación | 300 nodos, pero configurable hasta 4000 nodos |
| Nodos de una única configuración de **clúster** de Proceso de Azure Machine Learning (AmlCompute) como un grupo habilitado para la comunicación en una familia de máquinas virtuales habilitada para RDMA | 100 nodos |
| Los nodos en una única interfaz de paso de mensajes se **ejecutan** en un clúster de Proceso de Azure Machine Learning (AmlCompute) | 100 nodos, pero se puede aumentar a 300 nodos |
| Número de procesos MPI de GPU por nodo | 1-4 |
| Número de trabajos de GPU por nodo | 1-4 |
| Vigencia del trabajo | 21 días<sup>1</sup> |
| Vigencia del trabajo en un nodo de prioridad baja | 7 días<sup>2</sup> |
| Número de servidores de parámetro por nodo | 1 |

<sup>1</sup> La vigencia máxima es la duración entre el inicio y la finalización de una ejecución. Las ejecuciones completadas se mantienen de forma indefinida. Los datos de las ejecuciones no completadas dentro de la duración máxima no son accesibles.

<sup>2</sup> Los trabajos en un nodo de prioridad baja pueden adelantarse siempre que haya una restricción de capacidad. Se recomienda implementar puntos de comprobación en el trabajo.

### <a name="azure-machine-learning-managed-online-endpoints-preview"></a>Puntos de conexión en línea administrados (versión preliminar) de Azure Machine Learning.
[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

Los puntos de conexión en línea administrados de Azure Machine Learning tienen los siguientes límites:

| **Recurso** | **Límite** |
| --- | --- |
| El nombre del punto de conexión| Los nombres de los puntos de conexión deben <li> Empezar con una letra <li> Tener entre 3 y 32 caracteres de longitud  <li> Consistir solo en letras y números <sup>1</sup> |
| Nombre de implementación| Los nombres de implementación deben <li> Empezar con una letra <li> Tener entre 3 y 32 caracteres de longitud  <li>  Consistir solo en letras y números <sup>1</sup> |
| Número de puntos de conexión por suscripción | 50 |
| Número de implementaciones por suscripción | 200 |
| Número de implementaciones por punto de conexión | 20 |
| Número de instancias por implementación | 20 |
| Tamaño máximo de carga en el nivel de punto de conexión | 1,5 MB |
| Tiempo de espera máximo de la solicitud en el nivel de punto de conexión  | 60 segundos |
| Total de QPS en el nivel de punto de conexión para todas las implementaciones | 100 |

<sup>1</sup> Los guiones únicos, como `my-endpoint-name`, se aceptan en los nombres de punto de conexión e implementación.

### <a name="azure-machine-learning-pipelines"></a>Canalizaciones de Azure Machine Learning
Las [canalizaciones de Azure Machine Learning](concept-ml-pipelines.md) tienen los siguientes límites.

| **Recurso** | **Límite** |
| --- | --- |
| Pasos en una canalización | 30,000 |
| Número de áreas de trabajo por grupo de recursos | 800 |

### <a name="virtual-machines"></a>Máquinas virtuales
Cada suscripción de Azure tiene un límite en el número de máquinas virtuales que pueden tenerse entre todos los servicios. Los núcleos de las máquinas virtuales tienen un límite regional total y un límite regional por serie de tamaño. Ambos límites se aplican por separado.

Por ejemplo, considere una suscripción con un límite total de núcleos de máquinas virtuales de Este de EE. UU. de 30, un límite de núcleos de serie A de 30 y un límite de núcleos de serie D de 30. Esta suscripción podría volver a implementar 30 máquinas virtuales A1, 30 máquinas virtuales D1 o una combinación de las dos que no superen los 30 núcleos.

No se pueden aumentar los límites de las máquinas virtuales por encima de los valores que se muestran en la siguiente tabla.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="container-instances"></a>Azure Container Instances

Para más información, consulte [Límites de Container Instances](../azure-resource-manager/management/azure-subscription-service-limits.md#container-instances-limits).

### <a name="storage"></a>Storage
Azure Storage tiene un límite de 250 cuentas de almacenamiento por región y suscripción. Este límite incluye las cuentas de almacenamiento Estándar y Premium.

En el caso de que quisiera aumentar el límite, envíe una solicitud a través del [Soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). El equipo de Azure Storage revisa el caso y puede aprobar hasta 250 cuentas de almacenamiento para una región.


## <a name="workspace-level-quotas"></a>Cuotas de nivel de área de trabajo

Use las cuotas de nivel de área de trabajo para administrar la asignación de destino de proceso de Azure Machine Learning entre varias [áreas de trabajo](concept-workspace.md) de la misma suscripción.

De manera predeterminada, todas las áreas de trabajo comparten la misma cuota que la cuota de nivel de suscripción de las familias de máquinas virtuales. Sin embargo, puede establecer una cuota máxima para familias de máquinas virtuales individuales en áreas de trabajo de una suscripción. Esto permite compartir capacidad y evitar problemas de contención de recursos.

1. Vaya a cualquier área de trabajo de la suscripción.
1. En el panel izquierdo, seleccione **Uso y cuotas**.
1. Seleccione la pestaña **Configurar cuotas** para ver las cuotas.
1. Expanda una familia de máquinas virtuales.
1. Establezca un límite de cuota en cualquier área de trabajo que aparezca en esa familia de máquinas virtuales.

No se puede establecer un valor negativo ni un valor superior a la cuota de nivel de suscripción.

[![Captura de pantalla que muestra una cuota de nivel de área de trabajo de Azure Machine Learning.](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)

> [!NOTE]
> Se necesitan permisos de nivel de suscripción para establecer una cuota en el nivel de área de trabajo.

## <a name="view-your-usage-and-quotas"></a>Visualización del uso y las cuotas

Para ver la cuota de diversos recursos de Azure, como máquinas virtuales, almacenamiento o red, use Azure Portal:

1. En el panel izquierdo seleccione **Todos los servicios** y luego **Suscripciones** en la categoría **General**.

2. En la lista de suscripciones seleccione la suscripción cuya cuota busca.

3. Seleccione **Uso y cuotas** para ver los límites y el uso actuales de la cuota. Use los filtros para seleccionar el proveedor y las ubicaciones. 

La cuota de proceso de Azure Machine Learning de la suscripción se administra de forma independiente de otras cuotas de Azure: 

1. Vaya al área de trabajo de **Azure Machine Learning** en Azure Portal.

2. En el panel izquierdo, en la sección **Soporte técnico y solución de problemas**, seleccione **Uso y cuotas** para ver el uso y los límites de cuota actuales.

3. Seleccione una suscripción para ver los límites de cuota. Filtre por la región que le interesa.

4. Puede alternar entre una vista de nivel de suscripción y una de nivel de área de trabajo.

## <a name="request-quota-increases"></a>Solicitud de aumento de cuota

Para aumentar el límite o la cuota por encima del límite predeterminado, [abra una solicitud de soporte técnico al cliente en línea sin cargo alguno](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/).

No se pueden aumentar los límites por encima de los valores máximos mostrados en las tablas anteriores. Si no hay ningún límite máximo, no se puede ajustar el límite del recurso.

Cuando solicite un aumento de cuota, seleccione el servicio que tiene en mente. Por ejemplo, seleccione Azure Machine Learning, Container Instances o Storage. En el caso del proceso de Azure Machine Learning, puede seleccionar el botón **Solicitar cuota** mientras ve la cuota en los pasos anteriores.

> [!NOTE]
> Las [suscripciones de evaluación gratuita](https://azure.microsoft.com/offers/ms-azr-0044p) no son aptas para aumentos de límite ni cuota. Si tiene una suscripción de evaluación gratuita, puede actualizar a una suscripción de [pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/). Para obtener más información, vea [Actualización de evaluación gratuita de Azure a pago por uso](../cost-management-billing/manage/upgrade-azure-subscription.md) y [Preguntas más frecuentes sobre la cuenta gratuita de Azure](https://azure.microsoft.com/free/free-account-faq).

## <a name="next-steps"></a>Pasos siguientes

+ [Planeamiento y administración de los costos de Azure Machine Learning](concept-plan-manage-cost.md)
+ [Límites de servicio en Azure Machine Learning](resource-limits-quotas-capacity.md)
+ [Solución de problemas de implementación y puntuación de puntos de conexión en línea administrados (versión preliminar)](how-to-troubleshoot-managed-online-endpoints.md)
