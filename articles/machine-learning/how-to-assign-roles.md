---
title: Administración de roles del área de trabajo
titleSuffix: Azure Machine Learning
description: Conozca cómo obtener acceso a un área de trabajo de Azure Machine Learning mediante el control de acceso basado en roles de Azure (Azure RBAC).
services: machine-learning
ms.service: machine-learning
ms.subservice: enterprise-readiness
ms.topic: how-to
ms.reviewer: Blackmist
ms.author: johwu
author: johnwu0604
ms.date: 10/21/2021
ms.custom: how-to, seodec18, devx-track-azurecli, contperf-fy21q2
ms.openlocfilehash: 7bd1ffcdbe139f9ea0659801c0119b1e6ba6e750
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131559758"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Administración del acceso a un área de trabajo de Azure Machine Learning

En este artículo, obtendrá información sobre cómo administrar el acceso (autorización) a un área de trabajo de Azure Machine Learning. El [control de acceso basado en roles de Azure (Azure RBAC)](../role-based-access-control/overview.md) se usa para administrar el acceso a los recursos de Azure, como la capacidad de crear nuevos recursos o usar los existentes. Se asignan roles específicos a los usuarios de Azure Active Directory (Azure AD) que les conceden acceso a los recursos. Azure proporciona tanto roles integrados como la capacidad de crear roles personalizados.

> [!TIP]
> Aunque este artículo se centra en Azure Machine Learning, los servicios individuales en los que se basa Azure Machine Learning proporcionan su propia configuración de RBAC. Por ejemplo, con la información de este artículo, podrá configurar quién puede enviar solicitudes de puntuación a un modelo implementado como un servicio web en Azure Kubernetes Service. Pero Azure Kubernetes Service proporciona su propio conjunto de roles de Azure. Para obtener información específica de RBAC que podría resultar útil con Azure Machine Learning, consulte los siguientes vínculos:
>
> * [Control del acceso a los recursos de clúster de Azure Kubernetes](../aks/azure-ad-rbac.md)
> * [Uso de Azure RBAC para la autorización de Kubernetes](../aks/manage-azure-rbac.md)
> * [Uso de Azure RBAC para acceder a los datos de blob](../storage/blobs/assign-azure-role-data-access.md)

> [!WARNING]
> La aplicación de algunos roles puede limitar la funcionalidad de la interfaz de usuario en el Estudio de Azure Machine Learning para otros usuarios. Por ejemplo, si el rol de un usuario no tiene la capacidad de crear una instancia de proceso, la opción para crear una instancia de proceso no estará disponible en Estudio. Este comportamiento es el esperado y evita que el usuario intente realizar operaciones que devolverán un error de acceso denegado.

## <a name="default-roles"></a>Roles predeterminados

Las áreas de trabajo de Azure Machine Learning tienen cuatro roles integrados que se encuentran disponibles de manera predeterminada. Al agregar usuarios a un área de trabajo, se les puede asignar uno de los roles integrados que se describen a continuación.

| Role | Nivel de acceso |
| --- | --- |
| **Científico de datos de AzureML** | Puede realizar todas las acciones dentro de un área de trabajo de Azure Machine Learning, excepto crear o eliminar recursos de proceso, y modificar la propia área de trabajo. |
| **Lector** | Acciones de solo lectura en el área de trabajo. Los lectores pueden enumerar y ver los recursos (incluidas las credenciales del [almacén de datos](how-to-access-data.md)) de un área de trabajo, pero no pueden crear ni actualizar estos recursos. |
| **Colaborador** | Ver, crear, editar o eliminar (si procede) los recursos de un área de trabajo. Por ejemplo, los colaboradores pueden crear un experimento, crear o conectar un clúster de proceso, enviar una ejecución e implementar un servicio web. |
| **Propietario** | Acceso total al área de trabajo, incluida la posibilidad de ver, crear, editar o eliminar (si procede) los recursos de un área de trabajo. Además, puede cambiar las asignaciones de roles. |

> [!IMPORTANT]
> El acceso de los roles puede tener un ámbito de varios niveles en Azure. Por ejemplo, un usuario con acceso de propietario a un área de trabajo podría no tener acceso de propietario al grupo de recursos que contiene el área de trabajo. Para obtener más información, consulte [Cómo funciona Azure RBAC](../role-based-access-control/overview.md#how-azure-rbac-works).


## <a name="manage-workspace-access"></a>Administración del acceso al área de trabajo

Si es propietario de un área de trabajo, puede agregar y eliminar roles del área de trabajo. También puede asignar roles a los usuarios. Use los siguientes vínculos para descubrir cómo administrar el acceso:
- [Interfaz de usuario de Azure Portal](../role-based-access-control/role-assignments-portal.md)
- [PowerShell](../role-based-access-control/role-assignments-powershell.md)
- [CLI de Azure](../role-based-access-control/role-assignments-cli.md)
- [REST API](../role-based-access-control/role-assignments-rest.md)
- [Plantillas del Administrador de recursos de Azure](../role-based-access-control/role-assignments-template.md)

## <a name="create-custom-role"></a>Creación de un rol personalizado

Si los roles integrados no son suficientes, puede crear roles personalizados. Los roles personalizados pueden tener permisos para leer, escribir, eliminar y procesar recursos de ese área de trabajo. Puede hacer que el rol esté disponible en un nivel de área de trabajo específico, un nivel de grupo de recursos específico o un nivel de suscripción específico.

> [!NOTE]
> Debe ser un propietario del recurso en ese nivel para crear roles personalizados en ese recurso.

Para crear un rol personalizado, primero debe construir un archivo JSON de definición de rol que especifica el permiso y el ámbito del rol. En el ejemplo siguiente se define un rol personalizado llamado "Data Scientist Custom" (Personalizado para científico de datos) con ámbito en un nivel de área de trabajo específica:

`data_scientist_custom_role.json` :
```json
{
    "Name": "Data Scientist Custom",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

> [!TIP]
> Puede cambiar el campo `AssignableScopes` para establecer el ámbito de este rol personalizado en el nivel de suscripción, el nivel de grupo de recursos o un nivel de área de trabajo específica.
> El rol personalizado anterior es solo un ejemplo, consulte algunos [roles personalizados sugeridos para el servicio Azure Machine Learning Service](#customroles).

Este rol personalizado puede hacer todo en el área de trabajo, excepto las siguientes acciones:

- No puede crear o actualizar un recurso de proceso.
- No puede eliminar un recurso de proceso.
- No puede agregar, eliminar o modificar asignaciones de roles.
- No puede eliminar el área de trabajo.

Para implementar este rol personalizado, use el siguiente comando de la CLI de Azure:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Después de la implementación, este rol está disponible en el área de trabajo especificada. Ahora puede agregar y asignar este rol en Azure Portal.

Para más información sobre los roles personalizados, consulte [Roles personalizados de Azure](../role-based-access-control/custom-roles.md). 

### <a name="azure-machine-learning-operations"></a>Operaciones de Azure Machine Learning

Para más información sobre las operaciones (acciones y no acciones) que se pueden usar con roles personalizados, consulte [Operaciones del proveedor de recursos](../role-based-access-control/resource-provider-operations.md#microsoftmachinelearningservices). También puede usar el siguiente comando de la CLI de Azure para enumerar las operaciones:

```azurecli-interactive
az provider operation show –n Microsoft.MachineLearningServices
```

## <a name="list-custom-roles"></a>Lista de roles personalizados

Ejecute el siguiente comando en la CLI de Azure:

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

Para ver la definición de roles de un rol personalizado específico, use el siguiente comando de la CLI de Azure. Tenga en cuenta que `<role-name>` debe tener el mismo formato devuelto por el comando anterior:

```azurecli-interactive
az role definition list -n <role-name> --subscription <sub-id>
```

## <a name="update-a-custom-role"></a>Actualización de un rol personalizado

Ejecute el siguiente comando en la CLI de Azure:

```azurecli-interactive
az role definition update --role-definition update_def.json --subscription <sub-id>
```

Debe tener permisos en todo el ámbito de la nueva definición de roles. Por ejemplo, si este nuevo rol tiene un ámbito entre tres suscripciones, debe tener permisos en las tres suscripciones. 

> [!NOTE]
> Las actualizaciones de roles pueden tardar entre 15 minutos y una hora en aplicarse a todas las asignaciones de roles de ese ámbito.

## <a name="use-azure-resource-manager-templates-for-repeatability"></a>Uso de plantillas de Azure Resource Manager para repetibilidad

Si prevé que tendrá que volver a crear asignaciones de roles complejas, una plantilla Azure Resource Manager puede ser una gran ayuda. En la plantilla [machine-learning-dependencies-role-assignment template](https://github.com/Azure/azure-quickstart-templates/tree/master//quickstarts/microsoft.machinelearningservices/machine-learning-dependencies-role-assignment), se muestra cómo se pueden especificar las asignaciones de roles en el código fuente para su reutilización. 

## <a name="common-scenarios"></a>Escenarios frecuentes

En la tabla siguiente se proporciona un resumen de las actividades de Azure Machine Learning y los permisos necesarios para realizarlas en el menor ámbito. Por ejemplo, si una actividad se puede realizar con un ámbito de área de trabajo (columna 4), cualquier el ámbito superior con ese permiso también funcionará automáticamente.

> [!IMPORTANT]
> Todas las rutas de acceso de esta tabla que comienzan por `/` son **rutas de acceso relativas** a `Microsoft.MachineLearningServices/`:

| Actividad | Ámbito a nivel de supervisión | Ámbito a nivel de grupo de recursos | Ámbito a nivel de área de trabajo |
| ----- | ----- | ----- | ----- |
| Crear área de trabajo nueva | No se requiere | Propietario o colaborador | N/A (se convierte en Propietario o hereda el rol del ámbito superior después de la creación) |
| Solicitud de cuota de Amlcompute de nivel de suscripción o establecimiento de cuota de nivel de área de trabajo | Propietario, colaborador o rol personalizado </br>que permite `/locations/updateQuotas/action`</br> en el ámbito de suscripción | No autorizado | No autorizado |
| Crear un clúster de proceso | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `/workspaces/computes/write` |
| Crear una instancia de proceso | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `/workspaces/computes/write` |
| Envío de cualquier tipo de ejecución | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `"/workspaces/*/read", "/workspaces/environments/write", "/workspaces/experiments/runs/write", "/workspaces/metadata/artifacts/write", "/workspaces/metadata/snapshots/write", "/workspaces/environments/build/action", "/workspaces/experiments/runs/submit/action", "/workspaces/environments/readSecrets/action"` |
| Publicación de canalizaciones y puntos de conexión | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `"/workspaces/endpoints/pipelines/*", "/workspaces/pipelinedrafts/*", "/workspaces/modules/*"` |
| Implementación de un modelo registrado en un recurso AKS/ACI | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `"/workspaces/services/aks/write", "/workspaces/services/aci/write"` |
| Puntuación con un punto de conexión de AKS implementado | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permite: `"/workspaces/services/aks/score/action", "/workspaces/services/aks/listkeys/action"` (cuando no se usa la autenticación de Azure Active Directory) o `"/workspaces/read"` (cuando se usa la autenticación por tokens) |
| Acceso al almacenamiento mediante cuadernos interactivos | No se requiere | No se requiere | Propietario, colaborador o rol personalizado que permita: `"/workspaces/computes/read", "/workspaces/notebooks/samples/read", "/workspaces/notebooks/storage/*", "/workspaces/listStorageAccountKeys/action"` |
| Creación de un nuevo rol personalizado | Propietario, colaborador o rol personalizado que permita `Microsoft.Authorization/roleDefinitions/write`. | No se requiere | Propietario, colaborador o rol personalizado que permita: `/workspaces/computes/write` |

> [!TIP]
> Si recibe un error al intentar crear un área de trabajo por primera vez, asegúrese de que su rol permita `Microsoft.MachineLearningServices/register/action`. Esta acción le permite registrar el proveedor de recursos de Azure Machine Learning con su suscripción de Azure.

### <a name="user-assigned-managed-identity-with-azure-ml-compute-cluster"></a>Identidad administrada asignada por el usuario con el clúster de proceso de Azure Machine Learning

Para asignar una identidad asignada por el usuario a un clúster de proceso de Azure Machine Learning, necesita permisos de escritura para crear el proceso y el [rol de operador de identidad administrada](../role-based-access-control/built-in-roles.md#managed-identity-operator). Para obtener más información sobre Azure RBAC con identidades administradas, consulte [Cómo administrar una identidad administrada asignada por el usuario](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

### <a name="mlflow-operations"></a>Operaciones de MLflow

Para realizar operaciones de MLflow con el área de trabajo Azure Machine Learning, use el rol personalizado en los siguientes ámbitos:

| Operación de MLflow | Ámbito |
| --- | --- |
| Enumerar todos los experimentos en el almacén de seguimiento del área de trabajo, obtener un experimento por identificador, obtener un experimento por nombre | `Microsoft.MachineLearningServices/workspaces/experiments/read` |
| Crear un experimento con un nombre, establecer una etiqueta en un experimento, restaurar un experimento marcado para su eliminación| `Microsoft.MachineLearningServices/workspaces/experiments/write` | 
| Eliminar un experimento | `Microsoft.MachineLearningServices/workspaces/experiments/delete` |
| Obtener una ejecución y datos y metadatos relacionados, obtener una lista de todos los valores de la métrica especificada para una ejecución determinada, enumerar los artefactos de una ejecución | `Microsoft.MachineLearningServices/workspaces/experiments/runs/read` |
| Crear una nueva ejecución dentro de un experimento, eliminar ejecuciones, restaurar ejecuciones eliminadas, registrar métricas en la ejecución actual, establecer etiquetas en una ejecución, eliminar etiquetas en una ejecución, registrar los parámetros (par clave-valor) que se usan para una ejecución, registrar un lote de métricas, parámetros y etiquetas para una ejecución, actualizar el estado de ejecución | `Microsoft.MachineLearningServices/workspaces/experiments/runs/write` |
| Obtener el modelo registrado por nombre, obtener una lista de todos los modelos registrados en el registro, buscar modelos registrados, modelos de versión más recientes para cada fase de solicitudes, obtener la versión de un modelo registrado, buscar versiones del modelo, obtener el URI donde se almacenan los artefactos de una versión del modelo, buscar ejecuciones por identificadores de experimento | `Microsoft.MachineLearningServices/workspaces/models/read` |
| Crear un nuevo modelo registrado, actualizar el nombre o la descripción de un modelo registrado, cambiar el nombre del modelo registrado existente, crear una versión del modelo, actualizar la descripción de una versión del modelo, pasar un modelo registrado a una de las fases | `Microsoft.MachineLearningServices/workspaces/models/write` |
| Eliminar un modelo registrado junto con todas sus versiones, eliminar versiones específicas de un modelo registrado | `Microsoft.MachineLearningServices/workspaces/models/delete` |

<a id="customroles"></a>

## <a name="example-custom-roles"></a>Roles personalizados de ejemplo

### <a name="data-scientist"></a>Científico de datos

Permite que un científico de datos realice todas las operaciones dentro de un área de trabajo **excepto**:

* Creación de proceso
* Implementación de modelos en un clúster de AKS de producción
* Implementación de un punto de conexión de canalización de producción

`data_scientist_custom_role.json` :
```json
{
    "Name": "Data Scientist Custom",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute or deploy production endpoints.",
    "Actions": [
        "Microsoft.MachineLearningServices/workspaces/*/read",
        "Microsoft.MachineLearningServices/workspaces/*/action",
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/*/write"
    ],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/delete",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*",
        "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/services/aks/write",
        "Microsoft.MachineLearningServices/workspaces/services/aks/delete",
        "Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>"
    ]
}
```

### <a name="data-scientist-restricted"></a>Restringido para el científico de datos

Una definición de roles más restringida sin caracteres comodín en las acciones permitidas. Puede realizar todas las operaciones dentro de un área de trabajo **excepto**:

* Creación de proceso
* Implementación de modelos en un clúster de AKS de producción
* Implementación de un punto de conexión de canalización de producción

`data_scientist_restricted_custom_role.json` :
```json
{
    "Name": "Data Scientist Restricted Custom",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute or deploy production endpoints",
    "Actions": [
        "Microsoft.MachineLearningServices/workspaces/*/read",
        "Microsoft.MachineLearningServices/workspaces/computes/start/action",
        "Microsoft.MachineLearningServices/workspaces/computes/stop/action",
        "Microsoft.MachineLearningServices/workspaces/computes/restart/action",
        "Microsoft.MachineLearningServices/workspaces/computes/applicationaccess/action",
        "Microsoft.MachineLearningServices/workspaces/notebooks/storage/read",
        "Microsoft.MachineLearningServices/workspaces/notebooks/storage/write",
        "Microsoft.MachineLearningServices/workspaces/notebooks/storage/delete",
        "Microsoft.MachineLearningServices/workspaces/notebooks/samples/read",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/write",
        "Microsoft.MachineLearningServices/workspaces/experiments/write",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/submit/action",
        "Microsoft.MachineLearningServices/workspaces/pipelinedrafts/write",
        "Microsoft.MachineLearningServices/workspaces/metadata/snapshots/write",
        "Microsoft.MachineLearningServices/workspaces/metadata/artifacts/write",
        "Microsoft.MachineLearningServices/workspaces/environments/write",
        "Microsoft.MachineLearningServices/workspaces/models/write",
        "Microsoft.MachineLearningServices/workspaces/modules/write",
        "Microsoft.MachineLearningServices/workspaces/datasets/registered/write", 
        "Microsoft.MachineLearningServices/workspaces/datasets/registered/delete",
        "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/write",
        "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/listNodes/action",
        "Microsoft.MachineLearningServices/workspaces/environments/build/action"
    ],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/computes/write",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/delete",
        "Microsoft.MachineLearningServices/workspaces/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/listKeys/action",
        "Microsoft.Authorization/*",
        "Microsoft.MachineLearningServices/workspaces/datasets/registered/profile/read",
        "Microsoft.MachineLearningServices/workspaces/datasets/registered/preview/read",
        "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/profile/read",
        "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/preview/read",
        "Microsoft.MachineLearningServices/workspaces/datasets/registered/schema/read",    
        "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/schema/read",
        "Microsoft.MachineLearningServices/workspaces/datastores/write",
        "Microsoft.MachineLearningServices/workspaces/datastores/delete"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>"
    ]
}
```
     
### <a name="mlflow-data-scientist"></a>Científico de datos de MLflow

permite que un científico de datos realice todas las operaciones admitidas en AzureML de MLflow **excepto**:

* Creación de proceso
* Implementación de modelos en un clúster de AKS de producción
* Implementación de un punto de conexión de canalización de producción

`mlflow_data_scientist_custom_role.json` :
```json
{
    "Name": "MLFlow Data Scientist Custom",
    "IsCustom": true,
    "Description": "Can perform azureml mlflow integrated functionalities that includes mlflow tracking, projects, model registry",
    "Actions": [
        "Microsoft.MachineLearningServices/workspaces/experiments/read",
        "Microsoft.MachineLearningServices/workspaces/experiments/write",
        "Microsoft.MachineLearningServices/workspaces/experiments/delete",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/read",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/write",
        "Microsoft.MachineLearningServices/workspaces/models/read",
        "Microsoft.MachineLearningServices/workspaces/models/write",
        "Microsoft.MachineLearningServices/workspaces/models/delete"
    ],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/delete",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*",
        "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/services/aks/write",
        "Microsoft.MachineLearningServices/workspaces/services/aks/delete",
        "Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>"
    ]
}
```   

### <a name="mlops"></a>MLOps

permite asignar un rol a una entidad de servicio y usarlo para automatizar las canalizaciones de MLOps. Por ejemplo, para enviar ejecuciones en una canalización ya publicada:

`mlops_custom_role.json` :
```json
{
    "Name": "MLOps Custom",
    "IsCustom": true,
    "Description": "Can run pipelines against a published pipeline endpoint",
    "Actions": [
        "Microsoft.MachineLearningServices/workspaces/read",
        "Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/read",
        "Microsoft.MachineLearningServices/workspaces/metadata/artifacts/read",
        "Microsoft.MachineLearningServices/workspaces/metadata/snapshots/read",
        "Microsoft.MachineLearningServices/workspaces/environments/read",    
        "Microsoft.MachineLearningServices/workspaces/metadata/secrets/read",
        "Microsoft.MachineLearningServices/workspaces/modules/read",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/read",
        "Microsoft.MachineLearningServices/workspaces/datasets/registered/read",
        "Microsoft.MachineLearningServices/workspaces/datastores/read",
        "Microsoft.MachineLearningServices/workspaces/environments/write",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/write",
        "Microsoft.MachineLearningServices/workspaces/metadata/artifacts/write",
        "Microsoft.MachineLearningServices/workspaces/metadata/snapshots/write",
        "Microsoft.MachineLearningServices/workspaces/environments/build/action",
        "Microsoft.MachineLearningServices/workspaces/experiments/runs/submit/action"
    ],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/computes/write",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/delete",
        "Microsoft.MachineLearningServices/workspaces/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/listKeys/action",
        "Microsoft.Authorization/*"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>"
    ]
}
```

### <a name="workspace-admin"></a>Administrador del área de trabajo

permite realizar todas las operaciones dentro del ámbito de un área de trabajo, **excepto**:

* Crear una nueva área de trabajo
* Asignación de cuotas de nivel de suscripción o de área de trabajo

El administrador del área de trabajo tampoco puede crear un nuevo rol. Solo puede asignar roles integrados o personalizados existentes dentro del ámbito de su área de trabajo:

`workspace_admin_custom_role.json` :
```json
{
    "Name": "Workspace Admin Custom",
    "IsCustom": true,
    "Description": "Can perform all operations except quota management and upgrades",
    "Actions": [
        "Microsoft.MachineLearningServices/workspaces/*/read",
        "Microsoft.MachineLearningServices/workspaces/*/action",
        "Microsoft.MachineLearningServices/workspaces/*/write",
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.Authorization/roleAssignments/*"
    ],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>"
    ]
}
```

<a name="labeler"></a>
### <a name="data-labeler"></a>Etiquetador de datos

Permite definir un rol con ámbito únicamente para etiquetar los datos:

`labeler_custom_role.json` :
```json
{
    "Name": "Labeler Custom",
    "IsCustom": true,
    "Description": "Can label data for Labeling",
    "Actions": [
        "Microsoft.MachineLearningServices/workspaces/read",
        "Microsoft.MachineLearningServices/workspaces/labeling/projects/read",
        "Microsoft.MachineLearningServices/workspaces/labeling/labels/write"
    ],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/labeling/projects/summary/read"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>"
    ]
}
```

### <a name="labeling-team-lead"></a>Etiquetado del responsable del equipo

Le permite revisar y rechazar el conjunto de datos etiquetado y ver la información de etiquetado. Además de esto, este rol también permite realizar el rol de un etiquetador.

`labeling_team_lead_custom_role.json` :
```json
{
    "properties": {
        "roleName": "Labeling Team Lead",
        "description": "Team lead for Labeling Projects",
        "assignableScopes": [
            "/subscriptions/<subscription_id>"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.MachineLearningServices/workspaces/read",
                    "Microsoft.MachineLearningServices/workspaces/labeling/labels/read",
                    "Microsoft.MachineLearningServices/workspaces/labeling/labels/write",
                    "Microsoft.MachineLearningServices/workspaces/labeling/labels/reject/action",
                    "Microsoft.MachineLearningServices/workspaces/labeling/projects/read",
                    "Microsoft.MachineLearningServices/workspaces/labeling/projects/summary/read"
                ],
                "notActions": [
                    "Microsoft.MachineLearningServices/workspaces/labeling/projects/write",
                    "Microsoft.MachineLearningServices/workspaces/labeling/projects/delete",
                    "Microsoft.MachineLearningServices/workspaces/labeling/export/action"
                ],
                "dataActions": [],
                "notDataActions": []
            }
        ]
    }
}
```

## <a name="troubleshooting"></a>Solución de problemas

Estos son algunos de los aspectos que debe tener en cuenta al usar el control de acceso basado en rol de Azure (RBAC de Azure):

- Al crear un recurso en Azure (por ejemplo, un área de trabajo), no es directamente el propietario del recurso. El rol se hereda del rol más alto del ámbito con el que está autorizado en esa suscripción. Por ejemplo, si es un administrador de red y tiene permisos para crear un área de trabajo de Machine Learning, se le asignará el rol de administrador de red en esa área de trabajo, y no el rol de propietario.

- Para realizar operaciones de cuota en un área de trabajo, necesita permisos de nivel de suscripción. Esto significa que la configuración de la cuota de nivel de suscripción o de nivel de área de trabajo para los recursos de proceso administrados solo puede producirse si tiene permisos de escritura en el ámbito de la suscripción.

- Cuando hay dos asignaciones de roles al mismo usuario de Azure Active Directory con secciones en conflicto de Actions/NotActions, las operaciones enumeradas en NotActions de un rol podrían no surtir efecto si también aparecen como acciones en otro rol. Para más información sobre cómo Azure analiza las asignaciones de roles, consulte [Cómo determina Azure RBAC si un usuario tiene acceso a un recurso](../role-based-access-control/overview.md#how-azure-rbac-determines-if-a-user-has-access-to-a-resource).

- Para implementar los recursos de proceso dentro de una red virtual, debe tener explícitamente permiso para las siguientes acciones:
    - `Microsoft.Network/virtualNetworks/*/read` en los recursos de red virtual.
    - `Microsoft.Network/virtualNetworks/subnets/join/action` en el recurso de subred.
    
    Para obtener más información sobre Azure RBAC con redes, consulte los [Roles integrados de redes](../role-based-access-control/built-in-roles.md#networking).

- A veces puede tardar hasta una hora para que las nuevas asignaciones de roles surtan efecto sobre los permisos almacenados en caché en la pila.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a la seguridad en la empresa](concept-enterprise-security.md)
- [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md)
- [Tutorial: Entrenamiento de modelos](tutorial-train-models-with-aml.md)
- [Operaciones del proveedor de recursos](../role-based-access-control/resource-provider-operations.md#microsoftmachinelearningservices)
