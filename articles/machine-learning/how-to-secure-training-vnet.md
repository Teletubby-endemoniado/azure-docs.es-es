---
title: Protección de entornos de entrenamiento con redes virtuales
titleSuffix: Azure Machine Learning
description: Use una instancia aislada de Azure Virtual Network para proteger el entorno de entrenamiento de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: enterprise-readiness
ms.topic: how-to
ms.reviewer: larryfr
ms.author: jhirono
author: jhirono
ms.date: 11/05/2021
ms.custom: contperf-fy20q4, tracking-python, contperf-fy21q1, references_regions
ms.openlocfilehash: ec3a5feefa41a1bd0bb0448321fc87f7e11a55d9
ms.sourcegitcommit: 838413a8fc8cd53581973472b7832d87c58e3d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132136285"
---
# <a name="secure-an-azure-machine-learning-training-environment-with-virtual-networks"></a>Protección de un entorno de entrenamiento de Azure Machine Learning con redes virtuales

En este artículo, aprenderá a proteger los entornos de entrenamiento con una red virtual en Azure Machine Learning.

> [!TIP]
> Este artículo forma parte de una serie sobre la protección de un flujo de trabajo de Azure Machine Learning. Consulte los demás artículos de esta serie:
>
> * [Información general sobre redes virtuales](how-to-network-security-overview.md)
> * [Protección de los recursos de un área de trabajo](how-to-secure-workspace-vnet.md)
> * [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
> * [Habilitación de la función de Studio](how-to-enable-studio-virtual-network.md)
> * [Uso de un DNS personalizado](how-to-custom-dns.md)
> * [Uso de un firewall](how-to-access-azureml-behind-firewall.md)
>
> Para ver un tutorial sobre cómo crear un área de trabajo segura, un clúster de proceso y una instancia de proceso, vea [Tutorial: Creación de un área de trabajo segura](tutorial-create-secure-workspace.md).

En este artículo aprenderá a proteger los siguientes recursos de proceso de entrenamiento en una red virtual:
> [!div class="checklist"]
> - Clúster de proceso de Azure Machine Learning
> - Instancia de proceso de Azure Machine Learning
> - Azure Databricks
> - Máquina virtual
> - Clúster de HDInsight

## <a name="prerequisites"></a>Requisitos previos

+ Lea el artículo [Introducción a la seguridad de red](how-to-network-security-overview.md) para comprender los escenarios comunes de redes virtuales y la arquitectura de red virtual general.

+ Una red virtual y una subred existentes que se usarán con los recursos de proceso.

+ Para implementar recursos en una red virtual o subred, la cuenta de usuario debe tener permisos para realizar las siguientes acciones en los controles de acceso basados en roles de Azure (Azure RBAC):

    - "Microsoft.Network/virtualNetworks/*/read" en el recurso de red virtual. Esto no es necesario para las implementaciones de plantillas de Azure Resource Manager (ARM).
    - "Microsoft.Network/virtualNetworks/subnet/join/action" en el recurso de subred.

    Para obtener más información sobre Azure RBAC con redes, consulte los [roles integrados de redes](../role-based-access-control/built-in-roles.md#networking).

### <a name="azure-machine-learning-compute-clusterinstance"></a>Clúster o instancia de proceso de Azure Machine Learning

* La red virtual debe estar en la misma suscripción que el área de trabajo de Azure Machine Learning.
* La subred usada para la instancia o clúster de proceso debe tener suficientes direcciones IP no asignadas.

    * Un clúster de proceso puede modificar la escala dinámicamente. Si no hay suficientes direcciones IP sin asignar, el clúster se asignará parcialmente.
    * Una instancia de proceso solo requiere una dirección IP.

* Para crear una instancia o un clúster de proceso [sin una dirección IP pública](#no-public-ip) (una característica en vista previa), el área de trabajo debe usar un punto de conexión privado para conectarse a la red virtual. Para obtener más información, consulte [Configuración de un punto de conexión privado para un área de trabajo de Azure Machine Learning](how-to-configure-private-link.md).
* Asegúrese de que no hay directivas de seguridad ni bloqueos que restrinjan los permisos para administrar la red virtual. Al comprobar si hay directivas o bloqueos, busque en la suscripción y el grupo de recursos de la red virtual.
* Compruebe si sus directivas de seguridad o bloqueos del grupo de recursos o la suscripción de la red virtual restringen los permisos para administrar las redes virtuales. 
* Si tiene previsto proteger la red virtual restringiendo el tráfico, vea la sección [Acceso obligatorio a una red de Internet pública](#required-public-internet-access).
* La subred que se usa para implementar la instancia o el clúster de proceso no se debe delegar a ningún otro servicio. Por ejemplo, no debe delegarse a ACI.

### <a name="azure-databricks"></a>Azure Databricks

* La red virtual debe estar en la misma suscripción y región que el área de trabajo de Azure Machine Learning.
* Si las cuentas de Azure Storage del área de trabajo también están protegidas en una red virtual, deben estar en la misma red virtual que el clúster de proceso de Azure Databricks.

## <a name="limitations"></a>Limitaciones

### <a name="azure-machine-learning-compute-clusterinstance"></a>Clúster o instancia de proceso de Azure Machine Learning

* Si coloca varios clústeres o instancias de proceso en una red virtual, puede que tenga que solicitar un aumento de cuota para uno o varios de los recursos. El clúster o la instancia de proceso de Machine Learning asigna automáticamente recursos de red adicionales __al grupo de recursos que contiene la red virtual__. Para cada clúster o instancia de proceso, el servicio asigna los recursos siguientes:

    * Un grupo de seguridad de red (NSG). Este NSG contiene las reglas siguientes, que son específicas para el clúster y la instancia de proceso:

        * Permitir el tráfico TCP entrante en los puertos 29876-29877 desde la etiqueta de servicio `BatchNodeManagement`.
        * Permitir el tráfico TCP entrante en el puerto 44224 desde la etiqueta de servicio `AzureMachineLearning`.

        En la captura de pantalla siguiente se muestra un ejemplo de estas reglas:

        :::image type="content" source="./media/how-to-secure-training-vnet/compute-instance-cluster-network-security-group.png" alt-text="Captura de pantalla de un NSG":::


        > [!TIP]
        > Si la instancia o el clúster de proceso no usa una dirección IP pública (una característica en vista previa), estas reglas de NSG de entrada no son necesarias. 
        
    * En el caso de la instancia o el clúster de proceso, ahora es posible quitar la dirección IP pública (una característica en vista previa). Si tiene asignaciones de Azure Policy que prohíben la creación de la dirección IP pública, la implementación de la instancia o el clúster de proceso se realizará correctamente.

    * Un equilibrador de carga

    En el caso de los clústeres de proceso, estos recursos se eliminan cada vez que se reduce el clúster verticalmente hasta 0 nodos y se crean al escalar verticalmente.

    En el caso de una instancia de proceso, estos recursos se mantienen hasta que se elimina la instancia. Detener la instancia no hace que se quiten los recursos. 

    > [!IMPORTANT]
    > Estos recursos están limitados por las [cuotas de recursos](../azure-resource-manager/management/azure-subscription-service-limits.md) de la suscripción. Si el grupo de recursos de red virtual está bloqueado, se producirá un error en la eliminación de la instancia o el clúster de proceso. No se puede eliminar el equilibrador de carga hasta que se elimine la instancia o el clúster de proceso. Asegúrese también de que no haya ninguna asignación de Azure Policy que prohíba la creación de grupos de seguridad de red.

* Si las cuentas de Azure Storage del área de trabajo también están en la red virtual, use las instrucciones siguientes sobre las limitaciones de subred:

    * Si tiene previsto usar __Estudio__ de Azure Machine Learning para visualizar datos o usar el diseñador, la cuenta de almacenamiento debe estar __en la misma subred que la instancia o el clúster de proceso__.
    * Si tiene previsto usar el __SDK__, la cuenta de almacenamiento puede estar en una subred diferente.

    > [!NOTE]
    > Agregar una instancia de recurso para su área de trabajo o seleccionar la casilla "Permitir que los servicios de Microsoft de confianza accedan a esta cuenta" no es suficiente para permitir la comunicación desde el proceso.

* Cuando el área de trabajo usa un punto de conexión privado, solo se puede acceder a la instancia de proceso desde dentro de la red virtual. Si usa un DNS personalizado o un archivo de hosts, agregue una entrada para `<instance-name>.<region>.instances.azureml.ms`. Asigne esta entrada a la dirección IP privada del punto de conexión privado del área de trabajo. Para más información, consulte el artículo [DNS personalizado](./how-to-custom-dns.md).
* Las directivas de punto de conexión de servicio de red virtual no funcionan para las cuentas de almacenamiento del sistema de la instancia o el clúster de proceso.
* Si el almacenamiento y la instancia de proceso se encuentran en regiones diferentes, es posible que vea tiempos de espera intermitentes.
* Si la instancia de Azure Container Registry del área de trabajo usa un punto de conexión privado para conectarse a la red virtual, no puede usar una identidad administrada para la instancia de proceso. Para usar una identidad administrada con la instancia de proceso, no ponga el registro de contenedor en la red virtual.
* Si quiere usar Jupyter Notebooks en una instancia de proceso:

    * No deshabilite la comunicación de websocket. Asegúrese de que la red permite la comunicación de websocket con `*.instances.azureml.net` y `*.instances.azureml.ms`.
    * Asegúrese de que el cuaderno se ejecuta en un recurso de proceso detrás de la misma red virtual y subred que los datos. Al crear la instancia de proceso, use **Configuración avanzada** > **Configurar red virtual** para seleccionar la red y la subred.

* Los __clústeres de proceso__ se pueden crear en una región diferente a la del área de trabajo. Esta función está en __versión preliminar__ y solo está disponible para __clústeres de proceso__, no para instancias de proceso. Cuando se usa una región diferente para el clúster, se aplican las siguientes limitaciones:

    * Si los recursos asociados al área de trabajo, como el almacenamiento, se encuentran en una red virtual diferente a la del clúster, configure el emparejamiento de red virtual global entre las redes. Para más información, consulte [Emparejamiento de redes virtuales](../virtual-network/virtual-network-peering-overview.md).
    * Es posible que vea un aumento de la latencia de red y de los costos de transferencia de datos. La latencia y los costos pueden producirse al crear el clúster y al ejecutar trabajos en él.

    Instrucciones como el uso de reglas de NSG, rutas definidas por el usuario y requisitos de entrada/salida, se aplican de la forma habitual cuando se usa una región diferente a la del área de trabajo.

    > [!WARNING]
    > Si usa un __área de trabajo habilitada para un punto de conexión privado__, __no se admite__ la creación del clúster en otra región.

### <a name="azure-databricks"></a>Azure Databricks

* Además de las subredes __databricks-private__ y __databricks-public__ utilizadas por Azure Databricks, también se requiere la subred __predeterminada__ creada para la red virtual.
* Azure Databricks no usa un punto de conexión privado para comunicarse con la red virtual.

Para obtener más información sobre el uso de Azure Databricks en una red virtual, vea [Implementación de Azure Databricks en la red virtual de Azure](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject).

### <a name="azure-hdinsight-or-virtual-machine"></a>Azure HDInsight o máquina virtual

* Azure Machine Learning solo admite máquinas virtuales que ejecuten Ubuntu.

## <a name="required-public-internet-access"></a>Acceso obligatorio a una red de Internet pública

[!INCLUDE [machine-learning-required-public-internet-access](../../includes/machine-learning-public-internet-access.md)]

Para obtener información sobre el uso de una solución de firewall, vea [Uso de un firewall con Azure Machine Learning](how-to-access-azureml-behind-firewall.md).

## <a name="compute-clusters"></a><a name="compute-cluster"></a>Clústeres de proceso

Use las pestañas siguientes para seleccionar cómo tiene previsto crear un clúster de proceso:

# <a name="studio"></a>[Estudio](#tab/azure-studio)

Siga los pasos siguientes para crear un clúster de proceso en Estudio de Azure Machine Learning:

1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com/) y, después, seleccione la suscripción y el área de trabajo.
1. Seleccione __Proceso__ a la izquierda, __Clústeres de proceso__ en el centro y, luego, seleccione __+ Nuevo__.

    :::image type="content" source="./media/how-to-enable-virtual-network/create-compute-cluster.png" alt-text="Captura de pantalla de creación de un clúster":::

1. En el cuadro de diálogo __Create compute cluster__ (Crear clúster de proceso), seleccione el tamaño y la configuración de la máquina virtual que necesita y, después, seleccione __Siguiente__.

    :::image type="content" source="./media/how-to-enable-virtual-network/create-compute-cluster-vm.png" alt-text="Captura de pantalla de la configuración de la máquina virtual":::

1. En la sección __Parámetros de configuración__, establezca los valores de __Nombre del proceso__, __Red virtual__ y __Subred__.

    :::image type="content" source="media/how-to-enable-virtual-network/create-compute-cluster-config.png" alt-text="Captura de pantalla que muestra la definición del nombre del proceso, la red virtual y la subred.":::

    > [!TIP]
    > Si el área de trabajo usa un punto de conexión privado para conectarse a la red virtual, el campo de selección __Red virtual__ está atenuado.
    > 

1. Seleccione __Crear__ para crear el clúster de proceso.

# <a name="python"></a>[Python](#tab/python)

El código siguiente crea un nuevo clúster de Proceso de Machine Learning en la `default` subred de una red virtual denominada `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           location="westus2",
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

---

Cuando finaliza el proceso de creación, el modelo se entrena mediante el clúster en un experimento. Para más información, consulte [Selección y uso de un destino de proceso para entrenamiento](how-to-set-up-training-targets.md).

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

### <a name="no-public-ip-for-compute-clusters-preview"></a><a name="no-public-ip-amlcompute"></a>Ninguna dirección IP pública para clústeres de proceso (versión preliminar)

Al habilitar la opción **Ninguna dirección IP pública**, el clúster de proceso no usa una dirección IP pública para la comunicación con ninguna dependencia. En su lugar, se comunica únicamente dentro de la red virtual mediante un ecosistema de Azure Private Link y puntos de conexión privados o del servicio, de modo que se elimina la necesidad de una dirección IP pública por completo. La opción Ninguna dirección IP pública quita el acceso y la detectabilidad de nodos de clúster de proceso de Internet. De esta forma, se elimina un vector de amenaza significativo. Los clústeres de **Ninguna dirección IP pública** ayudan a cumplir las directivas de IP pública que muchas empresas tienen. 

Un clúster de proceso con la opción **Ninguna dirección IP pública** habilitada **no tiene requisitos de comunicación entrantes** de la red pública de Internet, en comparación con los del clúster de proceso de la IP pública. En concreto, no se requiere ninguna regla de NSG de entrada (`BatchNodeManagement`, `AzureMachineLearning`). Todavía debe permitir la entrada desde el origen de **VirtualNetwork** y cualquier origen de puerto al destino de **VirtualNetwork** y puerto de destino de **29876, 29877**.

Los clústeres de la opción **Ninguna dirección IP pública** dependen de [Azure Private Link](how-to-configure-private-link.md) para el área de trabajo de Azure Machine Learning. Un clúster de proceso con la opción **Ninguna dirección IP pública** también requiere que deshabilite las directivas de red de los puntos de conexión privados y las directivas de red del servicio Private Link. Estos requisitos proceden de los puntos de conexión privados y del servicio Azure Private Link y no son específicos de Azure Machine Learning. Siga las instrucciones indicadas en [Deshabilitación de directivas de red para el servicio Private Link](../private-link/disable-private-link-service-network-policy.md) para establecer los parámetros `disable-private-endpoint-network-policies` y `disable-private-link-service-network-policies` en la subred de la red virtual.

Para que las **conexiones de salida** funcionen, debe configurar un firewall de salida, como Azure Firewall, con rutas definidas por el usuario. Por ejemplo, puede usar un firewall definido con la [configuración de entrada o salida](how-to-access-azureml-behind-firewall.md) y enrutar el tráfico a él. Para ello, defina una tabla de rutas en la subred en la que se implementa el clúster de proceso. La entrada de la tabla de rutas puede configurar el próximo salto de la dirección IP privada del firewall con el prefijo de dirección 0.0.0.0/0.

Puede usar un punto de conexión de servicio o un punto de conexión privado para el registro de contenedor de Azure y el almacenamiento de Azure en la subred en la que se implementa el clúster.

Para crear un clúster de proceso sin dirección IP pública (una característica en vista previa) en Studio, marque la casilla **Ninguna dirección IP pública** en la sección de la red virtual.
También puede crear un clúster de proceso sin dirección IP pública mediante una plantilla de ARM. En la plantilla de ARM, establezca el parámetro enableNodePublicIP en false.

[!INCLUDE [no-public-ip-info](../../includes/machine-learning-no-public-ip-availibility.md)]

**Solución de problemas**

* Si recibe este mensaje de error durante la creación del clúster "La subred especificada tiene habilitado PrivateLinkServiceNetworkPolicies o PrivateEndpointNetworkEndpoints", siga las instrucciones de [Deshabilitar directivas de red para un servicio de Private Link](../private-link/disable-private-link-service-network-policy.md) y [Deshabilitar directivas de red para el punto de conexión privado](../private-link/disable-private-endpoint-network-policy.md).

* Si se produce un error en la ejecución del trabajo con problemas de conexión a ACR o Azure Storage, compruebe que el cliente ha agregado puntos de conexión privados o de servicio de ACR y Azure Storage a la subred y que ACR/Azure Storage permite el acceso desde la subred.

* Para asegurarse de que ha creado un clúster sin dirección IP pública, en Studio, al consultar los detalles del clúster, verá que la propiedad **No Public IP** (Ninguna dirección IP pública) está establecida en **true** en las propiedades del recurso.

## <a name="compute-instance"></a>Instancia de proceso

Para obtener los pasos sobre cómo crear una instancia de proceso implementada en una red virtual, consulte [Creación y administración de una instancia de proceso de Azure Machine Learning](how-to-create-manage-compute-instance.md).

### <a name="no-public-ip-for-compute-instances-preview"></a><a name="no-public-ip"></a>Ninguna dirección IP pública para instancias de proceso (versión preliminar)

Al habilitar la opción **Ninguna dirección IP pública**, la instancia de proceso no usa una dirección IP pública para la comunicación con ninguna dependencia. En su lugar, se comunica únicamente dentro de la red virtual mediante un ecosistema de Azure Private Link y puntos de conexión privados o del servicio, de modo que se elimina la necesidad de una dirección IP pública por completo. La opción Ninguna dirección IP pública quita el acceso y la detectabilidad del nodo de la instancia de proceso de Internet. De esta forma, se elimina un vector de amenaza significativo. Las instancias de proceso también realizarán el filtrado de paquetes para rechazar cualquier tráfico de fuera de la red virtual. Las instancias de la opción **Ninguna dirección IP pública** dependen de [Azure Private Link](how-to-configure-private-link.md) para el área de trabajo de Azure Machine Learning. 

Para que las **conexiones de salida** funcionen, debe configurar un firewall de salida, como Azure Firewall, con rutas definidas por el usuario. Por ejemplo, puede usar un firewall definido con la [configuración de entrada o salida](how-to-access-azureml-behind-firewall.md) y enrutar el tráfico a él. Para ello, defina una tabla de rutas en la subred en la que se implementa la instancia de proceso. La entrada de la tabla de rutas puede configurar el próximo salto de la dirección IP privada del firewall con el prefijo de dirección 0.0.0.0/0.

Una instancia de proceso con la opción **Ninguna dirección IP pública** habilitada **no tiene requisitos de comunicación entrantes** de la red pública de Internet, en comparación con los de la instancia de proceso de la IP pública. En concreto, no se requiere ninguna regla de NSG de entrada (`BatchNodeManagement`, `AzureMachineLearning`). Todavía debe permitir la entrada desde el origen de **VirtualNetwork**, cualquier origen de puerto, destino de **VirtualNetwork** y puerto de destino de **29876, 29877, 44224**.

Una instancia de proceso con la opción **Ninguna dirección IP pública** también requiere que deshabilite las directivas de red de los puntos de conexión privados y las directivas de red del servicio Private Link. Estos requisitos proceden de los puntos de conexión privados y del servicio Azure Private Link y no son específicos de Azure Machine Learning. Siga las instrucciones indicadas en [Deshabilitación de directivas de red para la dirección IP de origen del servicio Private Link](../private-link/disable-private-link-service-network-policy.md) para establecer los parámetros `disable-private-endpoint-network-policies` y `disable-private-link-service-network-policies` en la subred de la red virtual.

Para crear una instancia de proceso sin dirección IP pública (una característica en vista previa) en Studio, marque la casilla **Ninguna dirección IP pública** en la sección de la red virtual.
También puede crear una instancia de proceso sin dirección IP pública mediante una plantilla de ARM. En la plantilla de ARM, establezca el parámetro enableNodePublicIP en false.

Pasos siguientes:
* [Uso de un DNS personalizado](how-to-custom-dns.md)
* [Uso de un firewall](how-to-access-azureml-behind-firewall.md)

[!INCLUDE [no-public-ip-info](../../includes/machine-learning-no-public-ip-availibility.md)]

## <a name="inbound-traffic"></a>Tráfico entrante

[!INCLUDE [udr info for computes](../../includes/machine-learning-compute-user-defined-routes.md)]

A fin de obtener más información sobre los requisitos de tráfico de entrada y salida para Azure Machine Learning, vea [Uso de un área de trabajo detrás de un firewall](how-to-access-azureml-behind-firewall.md).

## <a name="azure-databricks"></a>Azure Databricks

Para obtener información específica sobre el uso de Azure Databricks con una red virtual, consulte [Implementar Azure Databricks en su red virtual de Azure](/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject).

<a id="vmorhdi"></a>

## <a name="virtual-machine-or-hdinsight-cluster"></a>Máquina virtual o clúster de HDInsight

En esta sección aprenderá a usar una máquina virtual o un clúster de Azure HDInsight en una red virtual con su área de trabajo.

### <a name="create-the-vm-or-hdinsight-cluster"></a>Creación de la máquina virtual o el clúster de HDInsight

Cree una máquina virtual o un clúster de HDInsight mediante Azure Portal o la CLI de Azure y colóquelo en una red virtual de Azure. Para más información, consulte los siguientes artículos.
* [Creación y administración de redes virtuales de Azure para máquinas virtuales Linux](../virtual-machines/linux/tutorial-virtual-network.md)

* [Extender HDInsight mediante una red virtual de Azure](../hdinsight/hdinsight-plan-virtual-network-deployment.md)

### <a name="configure-network-ports"></a>Configuración de los puertos de red 

Permita que Azure Machine Learning se comunique con el puerto SSH en la máquina virtual o el clúster y configure una entrada de origen para el grupo de seguridad de red. El puerto SSH suele ser el 22. Para permitir el tráfico desde este origen, realice las acciones siguientes:

1. En la lista desplegable __Origen__, seleccione __Etiqueta de servicio__.

1. En la lista desplegable __Etiqueta de servicio de origen__ , seleccione __AzureMachineLearning__.

    ![Reglas de entrada para realizar la experimentación en una máquina virtual o un clúster de HDInsight dentro de una red virtual](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

1. En la lista desplegable __Intervalos de puertos de origen__, seleccione __*__ .

1. En la lista desplegable __Destino__, seleccione __Cualquiera__.

1. En la lista desplegable __Intervalos de puertos de destino__, seleccione __22__.

1. En __Protocolo__, seleccione __Cualquiera__.

1. En __Acción__, seleccione __Permitir__.

Conserve las reglas de salida predeterminadas para el grupo de seguridad de red. Para más información, consulte las reglas de seguridad predeterminadas en [Grupos de seguridad](../virtual-network/network-security-groups-overview.md#default-security-rules).

Si no quiere usar las reglas de salida predeterminadas y quiere limitar el acceso de salida de la red virtual, vea la sección [Acceso obligatorio a una red de Internet pública](#required-public-internet-access).

### <a name="attach-the-vm-or-hdinsight-cluster"></a>Conexión de la máquina virtual o el clúster de HDInsight

Conecte el clúster de HDInsight o de máquina virtual a su área de trabajo de Azure Machine Learning. Para más información, consulte [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md).

## <a name="next-steps"></a>Pasos siguientes

Este artículo forma parte de una serie sobre la protección de un flujo de trabajo de Azure Machine Learning. Consulte los demás artículos de esta serie:

* [Información general sobre redes virtuales](how-to-network-security-overview.md)
* [Protección de los recursos de un área de trabajo](how-to-secure-workspace-vnet.md)
* [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
* [Habilitación de la función de Studio](how-to-enable-studio-virtual-network.md)
* [Uso de un DNS personalizado](how-to-custom-dns.md)
* [Uso de un firewall](how-to-access-azureml-behind-firewall.md)
