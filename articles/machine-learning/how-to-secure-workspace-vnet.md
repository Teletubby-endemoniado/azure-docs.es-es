---
title: Protección de un área de trabajo de Azure Machine Learning con redes virtuales
titleSuffix: Azure Machine Learning
description: Use una instancia aislada de Azure Virtual Network para proteger el área de trabajo de Azure Machine Learning y los recursos asociados.
services: machine-learning
ms.service: machine-learning
ms.subservice: enterprise-readiness
ms.reviewer: larryfr
ms.author: jhirono
author: jhirono
ms.date: 09/22/2021
ms.topic: how-to
ms.custom: contperf-fy20q4, tracking-python, contperf-fy21q1, security
ms.openlocfilehash: 61e5bda5722d343aae2fc6be80312f13a21c415a
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129658200"
---
# <a name="secure-an-azure-machine-learning-workspace-with-virtual-networks"></a>Protección de un área de trabajo de Azure Machine Learning con redes virtuales

En este artículo, aprenderá a proteger un área de trabajo de Azure Machine Learning y sus recursos asociados en una red virtual.

> [!TIP]
> Este artículo forma parte de una serie sobre la protección de un flujo de trabajo de Azure Machine Learning. Consulte los demás artículos de esta serie:
>
> * [Información general sobre redes virtuales](how-to-network-security-overview.md)
> * [Protección del entorno de entrenamiento](how-to-secure-training-vnet.md)
> * [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
> * [Habilitación de la función de Studio](how-to-enable-studio-virtual-network.md)
> * [Uso de un DNS personalizado](how-to-custom-dns.md)
> * [Uso de un firewall](how-to-access-azureml-behind-firewall.md)
>
> Para ver un tutorial sobre cómo crear un área de trabajo segura, vea [Tutorial: Creación de un área de trabajo segura](tutorial-create-secure-workspace.md).

En este artículo aprenderá a habilitar los siguientes recursos de áreas de trabajo en una red virtual:
> [!div class="checklist"]
> - Área de trabajo de Azure Machine Learning
> - Cuentas de Azure Storage
> - Conjuntos de datos y almacenes de datos de Azure Machine Learning
> - Azure Key Vault
> - Azure Container Registry

## <a name="prerequisites"></a>Requisitos previos

+ Lea el artículo [Introducción a la seguridad de red](how-to-network-security-overview.md) para comprender los escenarios comunes de redes virtuales y la arquitectura de red virtual general.

+ Lea el artículo [Procedimientos recomendados de Azure Machine Learning para la seguridad empresarial](/azure/cloud-adoption-framework/ready/azure-best-practices/ai-machine-learning-enterprise-security) para información sobre los procedimientos recomendados.

+ Una red virtual y una subred existentes que se usarán con los recursos de proceso.

    > [!TIP]
    > Si tiene previsto usar Azure Container Instances en la red virtual (para implementar modelos), el área de trabajo y la red virtual deben estar en el mismo grupo de recursos. De lo contrario, podrán estar en grupos diferentes.

+ Para implementar recursos en una red virtual o subred, la cuenta de usuario debe tener permisos para realizar las siguientes acciones en los controles de acceso basados en roles de Azure (Azure RBAC):

    - "Microsoft.Network/virtualNetworks/join/action" en el recurso de red virtual.
    - "Microsoft.Network/virtualNetworks/subnet/join/action" en el recurso de subred.

    Para obtener más información sobre Azure RBAC con redes, consulte los [roles integrados de redes](../role-based-access-control/built-in-roles.md#networking).

### <a name="azure-container-registry"></a>Azure Container Registry

* La instancia de Azure Container Registry debe tener la versión Prémium. Para más información sobre la actualización, vea [Cambio de SKU](../container-registry/container-registry-skus.md#changing-tiers).

* La instancia de Azure Container Registry debe estar en la misma red virtual y subred que la cuenta de almacenamiento y los destinos de proceso utilizados para entrenamiento o inferencia.

* El área de trabajo de Azure Machine Learning debe contener un [clúster de proceso de Azure Machine Learning](how-to-create-attach-compute-cluster.md).

## <a name="limitations"></a>Limitaciones

### <a name="azure-storage-account"></a>Cuenta de Azure Storage

Si el área de trabajo de Azure Machine Learning y la cuenta de Azure Storage usan un punto de conexión privado para conectarse a la red virtual, las dos deben estar dentro de la misma subred.

### <a name="azure-container-registry"></a>Azure Container Registry

Cuando la instancia de ACR está detrás de una red virtual, Azure Machine Learning no puede usarla para compilar imágenes de Docker directamente. En su lugar, se usa el clúster de proceso para compilar las imágenes.

> [!IMPORTANT]
> El clúster de proceso que se usa para compilar imágenes de Docker debe poder acceder a los repositorios de paquetes que se usan para entrenar e implementar los modelos. Es posible que tenga que agregar reglas de seguridad de red que permitan el acceso a repositorios públicos, [usar paquetes de Python privados](how-to-use-private-python-packages.md) o usar [imágenes de Docker personalizadas](how-to-train-with-custom-image.md) que ya incluyan los paquetes.

> [!WARNING]
> Si Azure Container Registry usa un punto de conexión privado para comunicarse con la red virtual, no puede usar una identidad administrada con un clúster de proceso de Azure Machine Learning. Para usar una identidad administrada con un clúster de proceso, use un punto de conexión de servicio con Azure Container Registry para el área de trabajo.

## <a name="required-public-internet-access"></a>Acceso obligatorio a una red de Internet pública

[!INCLUDE [machine-learning-required-public-internet-access](../../includes/machine-learning-public-internet-access.md)]

Para obtener información sobre el uso de una solución de firewall, vea [Uso de un firewall con Azure Machine Learning](how-to-access-azureml-behind-firewall.md).

## <a name="secure-the-workspace-with-private-endpoint"></a>Protección del área de trabajo con punto de conexión privado

Azure Private Link le permite conectarse a su área de trabajo mediante un punto de conexión privado. El punto de conexión privado es un conjunto de direcciones IP privadas dentro de la red virtual. Después, puede limitar el acceso al área de trabajo para que solo se produzca en las direcciones IP privadas. Un punto de conexión privado ayuda a reducir el riesgo de una filtración de datos.

Para obtener más información sobre cómo configurar un punto de conexión privado para el área de trabajo, vea [Procedimientos para configurar un punto de conexión privado](how-to-configure-private-link.md).

> [!WARNING]
> La protección de un área de trabajo con puntos de conexión privados no garantiza la seguridad de un extremo a otro por sí misma. Debe seguir los pasos que se describen en el resto de este artículo y la serie de redes virtuales para proteger componentes individuales de la solución. Por ejemplo, si usa un punto de conexión privado para el área de trabajo, pero la cuenta de Azure Storage no está detrás de la red virtual, el tráfico entre el área de trabajo y el almacenamiento no usa la red virtual por motivos de seguridad.

## <a name="secure-azure-storage-accounts"></a>Protección de cuentas de Azure Storage

Azure Machine Learning admite cuentas de almacenamiento configuradas para usar un punto de conexión privado o un punto de conexión de servicio. 

# <a name="private-endpoint"></a>[Punto de conexión privado](#tab/pe)

1. En Azure Portal, seleccione la cuenta de Azure Storage.
1. Use la información de [Uso de puntos de conexión privados para Azure Storage](../storage/common/storage-private-endpoints.md#creating-a-private-endpoint) para agregar puntos de conexión privados para los siguientes subrecursos de almacenamiento:

    * **Blob**
    * **Archivo**
    * **Cola**: solo es necesario si tiene previsto usar [ParallelRunStep](./tutorial-pipeline-batch-scoring-classification.md) en una canalización de Azure Machine Learning.
    * **Tabla**: solo es necesario si tiene previsto usar [ParallelRunStep](./tutorial-pipeline-batch-scoring-classification.md) en una canalización de Azure Machine Learning.

    :::image type="content" source="./media/how-to-enable-studio-virtual-network/configure-storage-private-endpoint.png" alt-text="Captura de pantalla que muestra la página de configuración de los puntos de conexión privados con opciones de blob y archivo":::

    > [!TIP]
    > Al configurar una cuenta de almacenamiento que **no** sea el almacenamiento predeterminado, seleccione el tipo **Subrecurso de destino** correspondiente a la cuenta de almacenamiento que quiere agregar.

1. Después de crear los puntos de conexión privados para los subrecursos, seleccione la pestaña __Firewalls y redes virtuales__ en __Redes__ en la cuenta de almacenamiento.
1. Seleccione __Redes seleccionadas__ y, a continuación, en __Instancias de recursos__, seleccione `Microsoft.MachineLearningServices/Workspace` como __Tipo de recurso__. Seleccione el área de trabajo mediante __Nombre de instancia__. Para más información, consulte [Acceso de confianza basado en la identidad administrada asignada por el sistema](/azure/storage/common/storage-network-security#trusted-access-based-on-system-assigned-managed-identity).

    > [!TIP]
    > O bien, seleccione __Allow Azure services on the trusted services list to access this storage account__ (Permitir que los servicios de Azure de la lista de servicios de confianza accedan a esta cuenta de almacenamiento) para permitir un mayor acceso de los servicios de confianza. Para más información, vea [Configuración de Firewalls y redes virtuales de Azure Storage](../storage/common/storage-network-security.md#trusted-microsoft-services).

    :::image type="content" source="./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-no-vnet.png" alt-text="Área de redes de la página de Azure Storage en Azure Portal al usar un punto de conexión privado":::

1. Para guardar la configuración, seleccione __Guardar__.

> [!TIP]
> Al usar un punto de conexión privado, también puede deshabilitar el acceso público. Para obtener más información, consulte la sección sobre cómo [deshabilitar el acceso de lectura público](../storage/blobs/anonymous-read-access-configure.md#allow-or-disallow-public-read-access-for-a-storage-account).

# <a name="service-endpoint"></a>[Punto de conexión de servicio](#tab/se)

1. En Azure Portal, seleccione la cuenta de Azure Storage.

1. En la sección __Seguridad y redes__ de la izquierda de la página, seleccione __Redes__ y, luego, elija la pestaña __Firewalls y redes virtuales__.

1. Seleccione __Redes seleccionadas__. En __Redes virtuales__, seleccione el vínculo __Agregar red virtual existente__ y luego la red virtual que el área de trabajo usa.

    > [!IMPORTANT]
    > La cuenta de almacenamiento debe estar en la misma red virtual y subred que las instancias de proceso o clústeres usados para entrenamiento o inferencia.

1. En la sección __Resource instances__ (Instancias de recursos), seleccione `Microsoft.MachineLearningServices/Workspace` como __Tipo de recurso__ y seleccione el área de trabajo mediante __Nombre de instancia__. Para más información, consulte [Acceso de confianza basado en la identidad administrada asignada por el sistema](/azure/storage/common/storage-network-security#trusted-access-based-on-system-assigned-managed-identity).

    > [!TIP]
    > O bien, seleccione __Allow Azure services on the trusted services list to access this storage account__ (Permitir que los servicios de Azure de la lista de servicios de confianza accedan a esta cuenta de almacenamiento) para permitir un mayor acceso de los servicios de confianza. Para más información, vea [Configuración de Firewalls y redes virtuales de Azure Storage](../storage/common/storage-network-security.md#trusted-microsoft-services).

    :::image type="content" source="./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png" alt-text="Área de redes de la página de Azure Storage en Azure Portal":::

1. Para guardar la configuración, seleccione __Guardar__.

> [!TIP]
> Al usar un punto de conexión de servicio, también puede deshabilitar el acceso público. Para obtener más información, consulte la sección sobre cómo [deshabilitar el acceso de lectura público](../storage/blobs/anonymous-read-access-configure.md#allow-or-disallow-public-read-access-for-a-storage-account).

---

## <a name="secure-azure-key-vault"></a>Protección de Azure Key Vault

Azure Machine Learning usa una instancia de Key Vault asociada para almacenar las siguientes credenciales:
* La cadena de conexión de cuenta de almacenamiento asociada
* Contraseñas para las instancias de Azure Container Repository
* Cadenas de conexión a almacenes de datos

Azure Key Vault se puede configurar para usar un punto de conexión privado o un punto de conexión de servicio. Para usar las funcionalidades de experimentación de Azure Machine Learning con Azure Key Vault detrás de una red virtual, siga los pasos siguientes:

> [!TIP]
> Independientemente de si usa un punto de conexión privado o un punto de conexión de servicio, el almacén de claves debe estar en la misma red que el punto de conexión privado del área de trabajo.

# <a name="private-endpoint"></a>[Punto de conexión privado](#tab/pe)

Para información sobre cómo usar un punto de conexión privado con Azure Key Vault, consulte [Integración de Key Vault con Azure Private Link](/azure/key-vault/general/private-link-service#establish-a-private-link-connection-to-key-vault-using-the-azure-portal).


# <a name="service-endpoint"></a>[Punto de conexión de servicio](#tab/se)

1. Vaya a la instancia de Key Vault asociada al área de trabajo.

1. En la página de __Key Vault__, en el panel izquierdo, seleccione __Redes__.

1. En la pestaña __Firewalls y redes virtuales__, realice las acciones siguientes:
    1. En __Permitir el acceso desde__, seleccione __Punto de conexión privado y redes seleccionadas__.
    1. En __Redes virtuales__, seleccione __Agregar redes virtuales existentes__ para agregar la red virtual donde reside el proceso de experimentación.
    1. En __¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?__ , seleccione __Sí__.

    :::image type="content" source="./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png" alt-text="Sección sobre firewalls y redes virtuales del panel de Key Vault":::

Para más información, vea [Configuración de redes de Azure Key Vault](/azure/key-vault/general/how-to-azure-key-vault-network-security).

---

## <a name="enable-azure-container-registry-acr"></a>Habilitación de un registro de Azure Container Registry (ACR)

> [!TIP]
> Si no usó una instancia de Azure Container Registry existente al crear el área de trabajo, es posible que no exista ninguna. De forma predeterminada, el área de trabajo no creará ninguna instancia de ACR hasta que necesite una. Para forzar la creación de una, entrene o implemente un modelo mediante el área de trabajo antes de seguir los pasos de esta sección.

Puede configurar Azure Container Registry para usar un punto de conexión privado. Siga estos pasos para configurar el área de trabajo a fin de que use ACR cuando se encuentre en la red virtual:

1. Busque el nombre de la instancia de Azure Container Registry para el área de trabajo mediante alguno de los métodos siguientes:

    __Azure Portal__

    En la sección de información general del área de trabajo, el valor __Registro__ se vincula a la instancia de Azure Container Registry.

    :::image type="content" source="./media/how-to-enable-virtual-network/azure-machine-learning-container-registry.png" alt-text="Azure Container Registry para el área de trabajo" border="true":::

    __CLI de Azure__

    Si ha [instalado la extensión de Machine Learning para la CLI de Azure](reference-azure-machine-learning-cli.md), puede usar el comando `az ml workspace show` para mostrar la información del área de trabajo.

    ```azurecli-interactive
    az ml workspace show -w yourworkspacename -g resourcegroupname --query 'containerRegistry'
    ```

    Este comando devuelve un valor similar a `"/subscriptions/{GUID}/resourceGroups/{resourcegroupname}/providers/Microsoft.ContainerRegistry/registries/{ACRname}"`. La última parte de la cadena es el nombre de Azure Container Registry para el área de trabajo.

1. Limite el acceso a la red virtual mediante los pasos descritos en [Conexión privada a Azure Container Registry](../container-registry/container-registry-private-link.md). Al agregar la red virtual, seleccione la red virtual y la subred para los recursos de Azure Machine Learning.

1. Configure la instancia de ACR del área de trabajo para [permitir el acceso de los servicios de confianza](../container-registry/allow-access-trusted-services.md).

1. Creará un clúster de proceso de Azure Machine Learning. Esto se usa para compilar imágenes de Docker cuando ACR está detrás de una red virtual. Para obtener más información, vea [Creación de un clúster de proceso](how-to-create-attach-compute-cluster.md).

1. Use el SDK de Python de Azure Machine Learning para configurar el área de trabajo a fin de compilar imágenes de Docker mediante la instancia de proceso. En el fragmento de código siguiente se muestra cómo actualizar el área de trabajo para establecer un proceso de compilación. Reemplace `mycomputecluster` por el nombre del clúster que se va a usar:

    ```python
    from azureml.core import Workspace
    # Load workspace from an existing config file
    ws = Workspace.from_config()
    # Update the workspace to use an existing compute cluster
    ws.update(image_build_compute = 'mycomputecluster')
    # To switch back to using ACR to build (if ACR is not in the VNet):
    # ws.update(image_build_compute = '')
    ```

    > [!IMPORTANT]
    > La cuenta de almacenamiento, el clúster de proceso y Azure Container Registry deben estar en la misma subred de la red virtual.
    
    Para más información, vea la referencia del método [update()](/python/api/azureml-core/azureml.core.workspace.workspace#update-friendly-name-none--description-none--tags-none--image-build-compute-none--enable-data-actions-none-).

> [!TIP]
> Si ACR está detrás de una red virtual, también puede [deshabilitar el acceso público](../container-registry/container-registry-access-selected-networks.md#disable-public-network-access) a la misma.

## <a name="datastores-and-datasets"></a>Almacenes de datos y conjuntos de datos
En la tabla siguiente se muestra una lista de los servicios para los que debe omitir la validación:

| Servicio | ¿Omitir la validación necesaria? |
| ----- |:-----:|
| Azure Blob Storage | Sí |
| Recurso compartido de archivos de Azure | Sí |
| Azure Data Lake Store Gen1 | No |
| Azure Data Lake Store Gen2 | No |
| Azure SQL Database | Sí |
| PostgreSql | Sí |

> [!NOTE]
> En Azure Data Lake Store Gen1 y Azure Data Lake Store Gen2 se omite la validación de manera predeterminada, por lo que no tiene que hacer nada.

En el ejemplo de código siguiente se crea un almacén de datos de blobs de Azure y se establece `skip_validation=True`.

```python
blob_datastore = Datastore.register_azure_blob_container(workspace=ws,  

                                                         datastore_name=blob_datastore_name,  

                                                         container_name=container_name,  

                                                         account_name=account_name, 

                                                         account_key=account_key, 

                                                         skip_validation=True ) // Set skip_validation to true
```

### <a name="use-datasets"></a>Uso de conjuntos de datos

La sintaxis para omitir la validación de conjuntos de datos es similar para los siguientes tipos de datos:
- Archivo delimitado
- JSON 
- Parquet
- SQL
- Archivo

En el código siguiente se crea un conjunto de datos JSON y se establece `validate=False`.

```python
json_ds = Dataset.Tabular.from_json_lines_files(path=datastore_paths, 

validate=False) 
```

## <a name="securely-connect-to-your-workspace"></a>Conexión segura al área de trabajo

[!INCLUDE [machine-learning-connect-secure-workspace](../../includes/machine-learning-connect-secure-workspace.md)]

## <a name="workspace-diagnostics"></a>Diagnóstico del área de trabajo

[!INCLUDE [machine-learning-workspace-diagnostics](../../includes/machine-learning-workspace-diagnostics.md)]

## <a name="next-steps"></a>Pasos siguientes

Este artículo forma parte de una serie sobre la protección de un flujo de trabajo de Azure Machine Learning. Consulte los demás artículos de esta serie:

* [Información general sobre redes virtuales](how-to-network-security-overview.md)
* [Protección del entorno de entrenamiento](how-to-secure-training-vnet.md)
* [Protección del entorno de inferencia](how-to-secure-inferencing-vnet.md)
* [Habilitación de la función de Studio](how-to-enable-studio-virtual-network.md)
* [Uso de un DNS personalizado](how-to-custom-dns.md)
* [Uso de un firewall](how-to-access-azureml-behind-firewall.md)
