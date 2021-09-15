---
title: 'Funciones de plantillas: recursos'
description: Describe las funciones para usar en una plantilla de Azure Resource Manager (plantilla de ARM) para recuperar valores sobre recursos.
ms.topic: conceptual
ms.date: 08/31/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 889ff813a72c3ebdc1cca9fa83e7c1dfde367eb6
ms.sourcegitcommit: 43dbb8a39d0febdd4aea3e8bfb41fa4700df3409
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123450276"
---
# <a name="resource-functions-for-arm-templates"></a>Funciones de recursos para plantillas de ARM

Resource Manager ofrece las siguientes funciones para obtener valores de recursos en la plantilla de Azure Resource Manager (plantilla de ARM):

* [extensionResourceId](#extensionresourceid)
* [list*](#list)
* [pickZones](#pickzones)
* [providers (en desuso)](#providers)
* [reference](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [suscripción](#subscription)
* [subscriptionResourceId](#subscriptionresourceid)
* [tenantResourceId](#tenantresourceid)

Para obtener valores de parámetro, variables o la implementación actual, consulte [Funciones con valores de implementación](template-functions-deployment.md).

## <a name="extensionresourceid"></a>extensionResourceId

`extensionResourceId(baseResourceId, resourceType, resourceName1, [resourceName2], ...)`

Devuelve el identificador de recurso de un [recurso de extensión](../management/extension-resource-types.md), que es un tipo de recurso que se aplica a otro recurso para agregarlo a sus capacidades.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| baseResourceId |Sí |string |El identificador de recurso para el recurso al que se aplica el recurso de extensión. |
| resourceType |Sí |string |Tipo de recurso de extensión, incluido el espacio de nombres del proveedor de recursos. |
| resourceName1 |Sí |string |Nombre del recurso de extensión. |
| resourceName2 |No |string |Segmento con el nombre del siguiente segmento, si es necesario. |

Siga agregando nombres de recursos como parámetros cuando el tipo de recurso incluya más segmentos.

### <a name="return-value"></a>Valor devuelto

El formato básico del identificador de recurso devuelto por esta función es:

```json
{scope}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

El segmento de ámbito varía según el recurso base que se está ampliando. Por ejemplo, el identificador de una suscripción tiene segmentos diferentes a los del identificador de un grupo de recursos.

Cuando el recurso de extensión se aplica a un **recurso**, el identificador de recurso se devuelve en el formato siguiente:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{baseResourceProviderNamespace}/{baseResourceType}/{baseResourceName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Cuando el recurso de extensión se aplica a un **grupo de recursos**, el formato devuelto es:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

En la sección siguiente se muestra un ejemplo del uso de esta función con un grupo de recursos.

Cuando el recurso de extensión se aplica a una **suscripción**, el formato devuelto es:

```json
/subscriptions/{subscriptionId}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

Cuando el recurso de extensión se aplica a un **grupo de administración**, el formato devuelto es:

```json
/providers/Microsoft.Management/managementGroups/{managementGroupName}/providers/{extensionResourceProviderNamespace}/{extensionResourceType}/{extensionResourceName}
```

En la sección siguiente se muestra un ejemplo del uso de esta función con un grupo de administración.

### <a name="extensionresourceid-example"></a>Ejemplo de extensionResourceId

En el ejemplo siguiente se devuelve el identificador de recurso para el bloqueo de un grupo de recursos.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "lockName": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "lockResourceId": {
      "type": "string",
      "value": "[extensionResourceId(resourceGroup().Id , 'Microsoft.Authorization/locks', parameters('lockName'))]"
    }
  }
}
```

Una definición de directiva personalizada implementada en un grupo de administración se implementa como recurso de extensión. Para crear y asignar una directiva, implemente la siguiente plantilla en un grupo de administración.

:::code language="json" source="~/quickstart-templates/managementgroup-deployments/mg-policy/azuredeploy.json":::

Las definiciones de directivas integradas son recursos del nivel de inquilino. Para obtener un ejemplo de implementación de una definición de directiva integrada, consulte [tenantResourceId](#tenantresourceid).

<a id="listkeys"></a>
<a id="list"></a>

## <a name="list"></a>lista*

`list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)`

La sintaxis de esta función varía según el nombre de las operaciones de la lista. Cada implementación devuelve valores para el tipo de recurso que admite una operación de la lista. El nombre de la operación debe empezar por `list` y tener un sufijo. Algunos usos habituales son `list`, `listKeys`, `listKeyValue` y `listSecrets`.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| resourceName o resourceIdentifier |Sí |string |Identificador único para el recurso. |
| apiVersion |Sí |string |Versión de API de estado en tiempo de ejecución de un recurso. Por lo general, en el formato, **aaaa-mm-dd**. |
| functionValues |No |object | Un objeto que tiene valores para la función. Proporcione este objeto solo para las funciones que admiten la recepción de un objeto con valores de parámetro, como **listAccountSas** en una cuenta de almacenamiento. En este artículo se muestra un ejemplo de cómo pasar los valores de funciones. |

### <a name="valid-uses"></a>Usos válidos

Las funciones de lista se pueden usar en las propiedades de una definición de recursos. No use una función de lista que exponga información confidencial en la sección de salidas de una plantilla. Los valores de salida se almacenan en el historial de implementaciones y un usuario malintencionado podría recuperarlos.

Cuando se usa con la [iteración de la propiedad](copy-properties.md), puede usar las funciones de lista para `input` porque la expresión se asigna a la propiedad de recurso. No se pueden utilizar con `count` porque debe determinarse el recuento antes de resolver la función de lista.

### <a name="implementations"></a>Implementaciones

Los usos posibles de la lista* se muestran en la tabla siguiente.

| Tipo de recurso | Nombre de función |
| ------------- | ------------- |
| Microsoft.Addons/supportProviders | listsupportplaninfo |
| Microsoft.AnalysisServices/servers | [listGatewayStatus](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft.ApiManagement/service/authorizationServers | [listSecrets](/rest/api/apimanagement/2020-06-01-preview/authorization-server/list-secrets) |
| Microsoft.ApiManagement/service/gateways | [listKeys](/rest/api/apimanagement/2020-06-01-preview/gateway/list-keys) |
| Microsoft.ApiManagement/service/identityProviders | [listSecrets](/rest/api/apimanagement/2020-06-01-preview/identity-provider/list-secrets) |
| Microsoft.ApiManagement/service/namedValues | [listValue](/rest/api/apimanagement/2020-06-01-preview/named-value/list-value) |
| Microsoft.ApiManagement/service/openidConnectProviders | [listSecrets](/rest/api/apimanagement/2020-06-01-preview/openid-connect-provider/list-secrets) |
| Microsoft.ApiManagement/service/subscriptions | [listSecrets](/rest/api/apimanagement/2020-06-01-preview/subscription/list-secrets) |
| Microsoft.AppConfiguration/configurationStores | [ListKeys](/rest/api/appconfiguration/configurationstores/listkeys) |
| Microsoft.AppPlatform/Spring | [listTestKeys](/rest/api/azurespringcloud/services/listtestkeys) |
| Microsoft.Automation/automationAccounts | [listKeys](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchAccounts | [listkeys](/rest/api/batchmanagement/batchaccount/getkeys) |
| Microsoft.BatchAI/workspaces/experiments/jobs | listoutputfiles |
| Microsoft.Blockchain/blockchainMembers | [listApiKeys](/rest/api/blockchain/2019-06-01-preview/blockchainmembers/listapikeys) |
| Microsoft.Blockchain/blockchainMembers/transactionNodes | [listApiKeys](/rest/api/blockchain/2019-06-01-preview/transactionnodes/listapikeys) |
| Microsoft.BotService/botServices/channels | [listChannelWithKeys](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/botservice/resource-manager/Microsoft.BotService/stable/2020-06-02/botservice.json#L553) |
| Microsoft.Cache/redis | [listKeys](/rest/api/redis/redis/listkeys) |
| Microsoft.CognitiveServices/accounts | [listKeys](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft.ContainerRegistry/registries | [listBuildSourceUploadUrl](/rest/api/containerregistry/registries%20(tasks)/get-build-source-upload-url) |
| Microsoft.ContainerRegistry/registries | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft.ContainerRegistry/registries | [listUsages](/rest/api/containerregistry/registries/listusages) |
| Microsoft.ContainerRegistry/registries/agentpools | listQueueStatus |
| Microsoft.ContainerRegistry/registries/buildTasks | listSourceRepositoryProperties |
| Microsoft.ContainerRegistry/registries/buildTasks/steps | listBuildArguments |
| Microsoft.ContainerRegistry/registries/taskruns | listDetails |
| Microsoft.ContainerRegistry/registries/webhooks | [listEvents](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft.ContainerRegistry/registries/runs | [listLogSasUrl](/rest/api/containerregistry/runs/getlogsasurl) |
| Microsoft.ContainerRegistry/registries/tasks | [listDetails](/rest/api/containerregistry/tasks/getdetails) |
| Microsoft.ContainerService/managedClusters | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterMonitoringUserCredential](/rest/api/aks/managedclusters/listclustermonitoringusercredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft.ContainerService/managedClusters/accessProfiles | [listCredential](/rest/api/aks/managedclusters/getaccessprofile) |
| Microsoft.DataBox/jobs | listCredentials |
| Microsoft.DataFactory/datafactories/gateways | listauthkeys |
| Microsoft.DataFactory/factories/integrationruntimes | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers | [listSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft.DataShare/accounts/shares | [listSynchronizations](/rest/api/datashare/2020-09-01/shares/listsynchronizations) |
| Microsoft.DataShare/accounts/shareSubscriptions | [listSourceShareSynchronizationSettings](/rest/api/datashare/2020-09-01/sharesubscriptions/listsourcesharesynchronizationsettings) |
| Microsoft.DataShare/accounts/shareSubscriptions | [listSynchronizationDetails](/rest/api/datashare/2020-09-01/sharesubscriptions/listsynchronizationdetails) |
| Microsoft.DataShare/accounts/shareSubscriptions | [listSynchronizations](/rest/api/datashare/2020-09-01/sharesubscriptions/listsynchronizations) |
| Microsoft.Devices/iotHubs | [listkeys](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft.Devices/iotHubs/iotHubKeys | [listkeys](/rest/api/iothub/iothubresource/getkeysforkeyname) |
| Microsoft.Devices/provisioningServices/keys | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft.Devices/provisioningServices | [listkeys](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft.DevTestLab/labs | [ListVhds](/rest/api/dtl/labs/listvhds) |
| Microsoft.DevTestLab/labs/schedules | [ListApplicable](/rest/api/dtl/schedules/listapplicable) |
| Microsoft.DevTestLab/labs/users/serviceFabrics | [ListApplicableSchedules](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft.DevTestLab/labs/virtualMachines | [ListApplicableSchedules](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| Microsoft.DocumentDB/databaseAccounts | [listConnectionStrings](/rest/api/cosmos-db-resource-provider/2021-04-15/database-accounts/list-connection-strings) |
| Microsoft.DocumentDB/databaseAccounts | [listKeys](/rest/api/cosmos-db-resource-provider/2021-04-15/database-accounts/list-keys) |
| Microsoft.DocumentDB/databaseAccounts/notebookWorkspaces | [listConnectionInfo](/rest/api/cosmos-db-resource-provider/2021-04-15/notebook-workspaces/list-connection-info) |
| Microsoft.DomainRegistration | [listDomainRecommendations](/rest/api/appservice/domains/listrecommendations) |
| Microsoft.DomainRegistration/topLevelDomains | [listAgreements](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft.EventGrid/domains | [listKeys](/rest/api/eventgrid/version2020-06-01/domains/listsharedaccesskeys) |
| Microsoft.EventGrid/topics | [listKeys](/rest/api/eventgrid/version2020-06-01/topics/listsharedaccesskeys) |
| Microsoft.EventHub/namespaces/authorizationRules | [listkeys](/rest/api/eventhub) |
| Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/eventhub) |
| Microsoft.EventHub/namespaces/eventhubs/authorizationRules | [listkeys](/rest/api/eventhub) |
| Microsoft.ImportExport/jobs | [listBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft.Kusto/Clusters/Databases | [ListPrincipals](/rest/api/azurerekusto/databases/listprincipals) |
| Microsoft.LabServices/users | [ListEnvironments](/rest/api/labservices/globalusers/listenvironments) |
| Microsoft.LabServices/users | [ListLabs](/rest/api/labservices/globalusers/listlabs) |
| Microsoft.Logic/integrationAccounts/agreements | [listContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/assemblies | [listContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft.Logic/integrationAccounts/maps | [listContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/partners | [listContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/schemas | [listContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft.Logic/workflows | [listCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft.Logic/workflows | [listSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft.Logic/workflows/runs/actions | [listExpressionTraces](/rest/api/logic/workflowrunactions/listexpressiontraces) |
| Microsoft.Logic/workflows/runs/actions/repetitions | [listExpressionTraces](/rest/api/logic/workflowrunactionrepetitions/listexpressiontraces) |
| Microsoft.Logic/workflows/triggers | [listCallbackUrl](/rest/api/logic/workflowtriggers/listcallbackurl) |
| Microsoft.Logic/workflows/versions/triggers | [listCallbackUrl](/rest/api/logic/workflowversions/listcallbackurl) |
| Microsoft.MachineLearning/webServices | [listkeys](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft.MachineLearning/Workspaces | listworkspacekeys |
| Microsoft.MachineLearningServices/workspaces/computes | [listKeys](/rest/api/azureml/compute/list-keys) |
| Microsoft.MachineLearningServices/workspaces/computes | [listNodes](/rest/api/azureml/compute/list-nodes) |
| Microsoft.MachineLearningServices/workspaces | [listKeys](/rest/api/azureml/workspaces/list-keys) |
| Microsoft.Maps/accounts | [listKeys](/rest/api/maps-management/accounts/listkeys) |
| Microsoft.Media/mediaservices/assets | [listContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft.Media/mediaservices/assets | [listStreamingLocators](/rest/api/media/assets/liststreaminglocators) |
| Microsoft.Media/mediaservices/streamingLocators | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft.Media/mediaservices/streamingLocators | [listPaths](/rest/api/media/streaminglocators/listpaths) |
| Microsoft.Network/applicationSecurityGroups | listIpConfigurations |
| Microsoft.NotificationHubs/Namespaces/authorizationRules | [listkeys](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules | [listkeys](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft.OperationalInsights/workspaces | [list](/rest/api/loganalytics/workspaces/list) |
| Microsoft.OperationalInsights/workspaces | listKeys |
| Microsoft.PolicyInsights/remediations | [listDeployments](/rest/api/policy/remediations/listdeploymentsatresourcegroup) |
| Microsoft.RedHatOpenShift/openShiftClusters | [listCredentials](/rest/api/openshift/openshiftclusters/listcredentials) |
| Microsoft.Relay/namespaces/authorizationRules | [listkeys](/rest/api/relay/namespaces/listkeys) |
| Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules | listkeys |
| Microsoft.Relay/namespaces/HybridConnections/authorizationRules | [listkeys](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft.Relay/namespaces/WcfRelays/authorizationRules | [listkeys](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft.Search/searchServices | [listAdminKeys](/rest/api/searchmanagement/2021-04-01-preview/admin-keys/get) |
| Microsoft.Search/searchServices | [listQueryKeys](/rest/api/searchmanagement/2021-04-01-preview/query-keys/list-by-search-service) |
| Microsoft.ServiceBus/namespaces/authorizationRules | [listkeys](/rest/api/servicebus/stable/namespaces-authorization-rules/list-keys) |
| Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys](/rest/api/servicebus/stable/disasterrecoveryconfigs/listkeys) |
| Microsoft.ServiceBus/namespaces/queues/authorizationRules | [listkeys](/rest/api/servicebus/stable/queues-authorization-rules/list-keys) |
| Microsoft.ServiceBus/namespaces/topics/authorizationRules | [listkeys](/rest/api/servicebus/stable/topics%20%E2%80%93%20authorization%20rules/list-keys) |
| Microsoft.SignalRService/SignalR | [listkeys](/rest/api/signalr/signalr/listkeys) |
| Microsoft.Storage/storageAccounts | [listAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft.Storage/storageAccounts | [listkeys](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft.Storage/storageAccounts | [listServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft.StorSimple/managers/devices | [listFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft.StorSimple/managers/devices | [listFailoverTargets](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft.StorSimple/managers | [listActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft.StorSimple/managers | [listPublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft.Synapse/workspaces/integrationRuntimes | [listAuthKeys](/rest/api/synapse/integrationruntimeauthkeys/list) |
| Microsoft.Web/connectionGateways | ListStatus |
| microsoft.web/connections | listconsentlinks |
| Microsoft.Web/customApis | listWsdlInterfaces |
| microsoft.web/locations | listwsdlinterfaces |
| microsoft.web/apimanagementaccounts/apis/connections | listconnectionkeys |
| microsoft.web/apimanagementaccounts/apis/connections | `listSecrets` |
| microsoft.web/sites/backups | [list](/rest/api/appservice/webapps/listbackups) |
| Microsoft.Web/sites/config | [list](/rest/api/appservice/webapps/listconfigurations) |
| microsoft.web/sites/functions | [listkeys](/rest/api/appservice/webapps/listfunctionkeys)
| microsoft.web/sites/functions | [listSecrets](/rest/api/appservice/webapps/listfunctionsecrets) |
| microsoft.web/sites/hybridconnectionnamespaces/relays | [listkeys](/rest/api/appservice/appserviceplans/listhybridconnectionkeys) |
| microsoft.web/sites | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| microsoft.web/sites/slots/functions | [listSecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |
| microsoft.web/sites/slots/backups | [list](/rest/api/appservice/webapps/listbackupsslot) |
| Microsoft.Web/sites/slots/config | [list](/rest/api/appservice/webapps/listconfigurationsslot) |
| microsoft.web/sites/slots/functions | [listSecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Para determinar qué tipos de recursos tienen una operación de lista, dispone de las siguientes opciones:

* Vea las [operaciones de API de REST](/rest/api/) para un proveedor de recursos y busque operaciones List. Por ejemplo, las cuentas de almacenamiento tienen una [operación listKeys](/rest/api/storagerp/storageaccounts).
* Utilice el cmdlet de PowerShell [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation). En el ejemplo siguiente se obtienen todas las operaciones List para cuentas de almacenamiento:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```

* Use el siguiente comando de la CLI de Azure para filtrar solo las operaciones de la lista:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Valor devuelto

El objeto devuelto varía según la función de la lista que use. Por ejemplo, listKeys para una cuenta de almacenamiento devuelve el siguiente formato:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Otras funciones List tienen diferentes formatos de retorno. Para ver el formato de una función, inclúyalo en la sección de resultados tal y como se muestra en la plantilla de ejemplo.

### <a name="remarks"></a>Observaciones

Especifique el recursos con el nombre del recurso o la [función resourceId](#resourceid). Al usar una función de la lista en la misma plantilla que implementa el recurso al que se hace referencia, use el nombre del recurso.

Si usa una función `list` con un recurso que se implementa de forma condicional, se puede evaluar la función incluso si el recurso no está implementado. Se genera un error si la función `list` hace referencia a un recurso que no existe. Use la función `if` para asegurarse de que la función se evalúa solo cuando se implementa el recurso. Consulte la función [if function](template-functions-logical.md#if) para una plantilla de ejemplo que use if y list con un recurso implementado de forma condicional.

### <a name="list-example"></a>Ejemplo de lista

En el ejemplo siguiente se usa listKeys al establecer un valor para los [scripts de implementación](deployment-script-template.md).

```json
"storageAccountSettings": {
  "storageAccountName": "[variables('storageAccountName')]",
  "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
}
```

En el ejemplo siguiente se muestra una función de lista que toma un parámetro. En este caso, la función se **listAccountSas**. Pase un objeto para la hora de expiración. Dicha hora debe ser futura.

```json
"parameters": {
  "accountSasProperties": {
    "type": "object",
    "defaultValue": {
      "signedServices": "b",
      "signedPermission": "r",
      "signedExpiry": "2020-08-20T11:00:00Z",
      "signedResourceTypes": "s"
    }
  }
},
...
"sasToken": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties')).accountSasToken]"
```

## <a name="pickzones"></a>pickZones

`pickZones(providerNamespace, resourceType, location, [numberOfZones], [offset])`

Determina si un tipo de recurso admite zonas para la ubicación o región especificada.  Esta función solo admite recursos de zona; los servicios con redundancia de zona devolverán una matriz vacía.  Para obtener más información, consulte [Servicios compatibles con Availability Zones](../../availability-zones/az-region.md).  Para usar la función pickZones con servicios con redundancia de zona, consulte los siguientes ejemplos.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| providerNamespace | Sí | string | Espacio de nombres del proveedor de recursos para que el tipo de recurso compruebe la compatibilidad de la zona. |
| resourceType | Sí | string | El tipo de recurso para comprobar la compatibilidad de la zona. |
| ubicación | Sí | string | Región en la que se va a comprobar la compatibilidad de la zona. |
| numberOfZones | No | integer | Número de zonas lógicas que se van a devolver. El valor predeterminado es 1. El número debe ser un entero positivo de 1 a 3.  Use 1 para los recursos de una sola zona. En el caso de los recursos de varias zonas, el valor debe ser menor o igual que el número de zonas admitidas. |
| offset | No | integer | Desplazamiento de la zona lógica inicial. La función devuelve un error si el desplazamiento más el valor de numberOfZones supera el número de zonas admitidas. |

### <a name="return-value"></a>Valor devuelto

Matriz con las zonas admitidas. Cuando se usan los valores predeterminados para offset y numberOfZones, un tipo de recurso y una región que admiten zonas devuelven la siguiente matriz:

```json
[
    "1"
]
```

Cuando el parámetro `numberOfZones` se establece en 3, devuelve lo siguiente:

```json
[
    "1",
    "2",
    "3"
]
```

Si el tipo de recurso o la región no admiten zonas, se devuelve una matriz vacía.  También se devuelve una matriz vacía para los servicios con redundancia de zona.

```json
[
]
```

### <a name="remarks"></a>Comentarios

Hay distintas categorías para Azure Availability Zones: zonal y con redundancia de zona.  La función pickZones se puede usar para devolver un número de zona de disponibilidad o números para un recurso de zona.  Eb el caso de servicios con redundancia de zona (ZRS), la función devolverá una matriz vacía.  Normalmente, los recursos zonales se pueden identificar mediante el uso de una propiedad `zones` en el encabezado de recurso.  Los servicios con redundancia de zona tienen diferentes maneras de identificar y usar zonas de disponibilidad por recurso. Consulte la documentación de un servicio específico para determinar la categoría de compatibilidad con zonas de disponibilidad.  Para obtener más información, consulte [Servicios compatibles con Availability Zones](../../availability-zones/az-region.md).

Para determinar si una determinada región o ubicación de Azure admite zonas de disponibilidad, llame a la función pickZones() con un tipo de recurso zonal, por ejemplo, `Microsoft.Storage/storageAccounts`.  Si la respuesta no está vacía, la región admite zonas de disponibilidad.

### <a name="pickzones-example"></a>Ejemplo de pickZones

En la plantilla siguiente se muestran tres resultados para usar la función pickZones.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "functions": [],
  "variables": {},
  "resources": [],
  "outputs": {
    "supported": {
      "type": "array",
      "value": "[pickZones('Microsoft.Compute', 'virtualMachines', 'westus2')]"
    },
    "notSupportedRegion": {
      "type": "array",
      "value": "[pickZones('Microsoft.Compute', 'virtualMachines', 'northcentralus')]"
    },
    "notSupportedType": {
      "type": "array",
      "value": "[pickZones('Microsoft.Cdn', 'profiles', 'westus2')]"
    }
  }
}
```

La salida de los ejemplos anteriores devuelve tres matrices.

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| admitido | array | [ "1" ] |
| notSupportedRegion | array | [] |
| notSupportedType | array | [] |

Puede usar la respuesta de pickZones para determinar si se debe proporcionar un valor null para las zonas o asignar máquinas virtuales a diferentes zonas. En el ejemplo siguiente se establece un valor para la zona en función de la disponibilidad de las zonas.

```json
"zones": {
  "value": "[if(not(empty(pickZones('Microsoft.Compute', 'virtualMachines', 'westus2'))), string(add(mod(copyIndex(),3),1)), json('null'))]"
},
```

En el siguiente ejemplo se muestra cómo usar la función pickZones con el fin de habilitar la redundancia de zona para Cosmos DB.

```json
"resources": [
  {
    "type": "Microsoft.DocumentDB/databaseAccounts",
    "apiVersion": "2021-04-15",
    "name": "[variables('accountName_var')]",
    "location": "[parameters('location')]",
    "kind": "GlobalDocumentDB",
    "properties": {
      "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
      "locations": [
      {
        "locationName": "[parameters('primaryRegion')]",
        "failoverPriority": 0,
        "isZoneRedundant": "[if(empty(pickZones('Microsoft.Storage', 'storageAccounts', parameters('primaryRegion'))), bool('false'), bool('true'))]",
      },
      {
        "locationName": "[parameters('secondaryRegion')]",
        "failoverPriority": 1,
        "isZoneRedundant": "[if(empty(pickZones('Microsoft.Storage', 'storageAccounts', parameters('secondaryRegion'))), bool('false'), bool('true'))]",
      }
    ],
      "databaseAccountOfferType": "Standard",
      "enableAutomaticFailover": "[parameters('automaticFailover')]"
    }
  }
]
```

## <a name="providers"></a>providers

**La función providers está en desuso.** Por tanto, no se recomienda utilizarla. Si usó esta función para obtener una versión de la API para el proveedor de recursos, se recomienda proporcionar una versión de API específica en la plantilla. El uso de una versión de API devuelta dinámicamente puede interrumpir la plantilla si las propiedades cambian entre versiones.

## <a name="reference"></a>reference

`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Devuelve un objeto que representa el estado de tiempo de ejecución de un recurso.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| resourceName o resourceIdentifier |Sí |string |Nombre o identificador único de un recurso. Al hacer referencia a un recurso en la plantilla actual, proporcione solo el nombre del recurso como parámetro. Al hacer referencia a un recurso implementado anteriormente o cuando el nombre del recurso sea ambiguo, proporcione el identificador del recurso. |
| apiVersion |No |string |Versión de la API del recurso especificado. **Este parámetro es necesario no está aprovisionado dentro de la misma plantilla**. Por lo general, en el formato, **aaaa-mm-dd**. Para conocer las versiones válidas de la API para su recurso, consulte la [referencia de la plantilla](/azure/templates/). |
| 'Full' |No |string |Valor que especifica si se devuelve el objeto de recurso completo. Si no se especifica `'Full'`, se devuelve solo el objeto de propiedades del recurso. El objeto completo incluye valores como el identificador de recurso y la ubicación. |

### <a name="return-value"></a>Valor devuelto

Cada tipo de recurso devuelve propiedades diferentes para la función de referencia. La función no devuelve un solo formato predefinido. Además, el valor devuelto difiere en función del valor del argumento `'Full'`. Para ver las propiedades de un tipo de recurso, devuelva el objeto en la sección de resultados tal como se muestra en el ejemplo.

### <a name="remarks"></a>Observaciones

La función de referencia recupera el estado del entorno de ejecución de un recurso previamente implementado o de un recurso implementado en la plantilla actual. En este artículo se muestran ejemplos de ambos escenarios.

Normalmente, se usa la función de `reference` para devolver un valor determinado de un objeto, como el identificador URI del punto de conexión de blob o el nombre de dominio completo.

```json
"outputs": {
  "BlobUri": {
    "type": "string",
    "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))).primaryEndpoints.blob]"
  },
  "FQDN": {
    "type": "string",
    "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', parameters('ipAddressName'))).dnsSettings.fqdn]"
  }
}
```

Utilice `'Full'` cuando necesite valores de recurso que no forman parte del esquema de propiedades. Por ejemplo, para establecer directivas de acceso del almacén de claves, obtenga las propiedades de identidad para una máquina virtual.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "apiVersion": "2019-09-01",
  "name": "vaultName",
  "properties": {
    "tenantId": "[subscription().tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(resourceId('Microsoft.Compute/virtualMachines', variables('vmName')), '2019-03-01', 'Full').identity.tenantId]",
        "objectId": "[reference(resourceId('Microsoft.Compute/virtualMachines', variables('vmName')), '2019-03-01', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

### <a name="valid-uses"></a>Usos válidos

La función de referencia solo se puede utilizar en las propiedades de una definición de recursos y en la sección de salidas de una plantilla o implementación. Cuando se usa con la [iteración de la propiedad](copy-properties.md), puede usar la función reference para `input` porque la expresión se asigna a la propiedad de recurso.

No se puede utilizar la función reference para establecer el valor de la propiedad `count` en un bucle de copia. Puede usarla para establecer otras propiedades en el bucle. La referencia está bloqueada para la propiedad count, porque esa propiedad se debe determinar antes de que se resuelva la función reference.

Para usar la función `reference` o cualquier función `list*` en la sección de salidas de una plantilla anidada, debe establecer `expressionEvaluationOptions` para usar la evaluación de [ámbito interno](linked-templates.md#expression-evaluation-scope-in-nested-templates) o una plantilla vinculada en lugar de una anidada.

Si usa una función `reference` con un recurso que se implementa de forma condicional, se puede evaluar la función incluso si el recurso no está implementado.  Se genera un error si la función `reference` hace referencia a un recurso que no existe. Use la función `if` para asegurarse de que la función se evalúa solo cuando se implementa el recurso. Consulte la función [if](template-functions-logical.md#if) para una plantilla de ejemplo que use if y reference con un recurso implementado de forma condicional.

### <a name="implicit-dependency"></a>Dependencia implícita

Mediante el uso de la función de referencia, se declara implícitamente que un recurso depende de otro recurso si el recurso al que se hace referencia se aprovisiona en la misma plantilla y se hace referencia a él por su nombre (y no por el identificador del recurso). No tiene que usar también la propiedad dependsOn. La función no se evalúa hasta que el recurso al que se hace referencia haya completado la implementación.

### <a name="resource-name-or-identifier"></a>Nombre de recurso o identificador

Al hacer referencia a un recurso que esté implementado en la misma plantilla, especifique el nombre del recurso.

```json
"value": "[reference(parameters('storageAccountName'))]"
```

Al hacer referencia a un recurso que no esté implementado en la misma plantilla, especifique el identificador del recurso y `apiVersion`.

```json
"value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
```

Para evitar la ambigüedad sobre cuál es el recurso al que hace referencia, puede proporcionar un nombre de recurso completo.

```json
"value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', parameters('ipAddressName')))]"
```

Al construir una referencia completa a un recurso, el orden para combinar los segmentos a partir del tipo y el nombre no es simplemente una concatenación de los dos. En su lugar, después del espacio de nombres, use una secuencia de pares *tipo/nombre* de menos a más específico:

`{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]`

Por ejemplo:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` es correcto `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` no es correcto

Para simplificar la creación de cualquier identificador de recurso, use las funciones `resourceId()` descritas en este documento, en lugar de la función `concat()`.

### <a name="get-managed-identity"></a>Obtención de una identidad administrada

[Las identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md) son [tipos de recursos de extensión](../management/extension-resource-types.md) que se crean implícitamente para algunos recursos. Dado que la identidad administrada no se define explícitamente en la plantilla, debe hacer referencia al recurso al que se aplica la identidad. Utilice `Full` para obtener todas las propiedades, incluida la identidad creada implícitamente.

El patrón es:

`"[reference(resourceId(<resource-provider-namespace>, <resource-name>), <API-version>, 'Full').Identity.propertyName]"`

Por ejemplo, para obtener el identificador de la entidad de seguridad de una identidad administrada que se aplica a una máquina virtual, use:

```json
"[reference(resourceId('Microsoft.Compute/virtualMachines', variables('vmName')),'2019-12-01', 'Full').identity.principalId]",
```

O bien, para obtener el identificador de inquilino de una identidad administrada que se aplica a un conjunto de escalado de máquinas virtuales, use:

```json
"[reference(resourceId('Microsoft.Compute/virtualMachineScaleSets',  variables('vmNodeType0Name')), 2019-12-01, 'Full').Identity.tenantId]"
```

### <a name="reference-example"></a>Ejemplo de referencia

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) siguiente se implementa un recurso y se hace referencia a ese recurso.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
```

El ejemplo anterior devuelve los dos objetos. El objeto de propiedades está en el formato siguiente:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

El objeto completo está en el formato siguiente:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) siguiente se hace referencia a una cuenta de almacenamiento que no se implementa en esta plantilla. La cuenta de almacenamiento ya existe dentro de la misma suscripción.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageResourceGroup": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    }
  },
  "resources": [],
  "outputs": {
    "ExistingStorage": {
      "type": "object",
      "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]"
    }
  }
}
```

## <a name="resourcegroup"></a>resourceGroup

`resourceGroup()`

Devuelve un objeto que representa el grupo de recursos actual.

### <a name="return-value"></a>Valor devuelto

El objeto devuelto está en el formato siguiente:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "{resourceGroupLocation}",
  "managedBy": "{identifier-of-managing-resource}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

La propiedad **managedBy** solo se devuelve para los grupos de recursos que contienen recursos administrados por otro servicio. En los casos de Managed Applications, Databricks y AKS, el valor de la propiedad es el identificador del recurso que realiza la administración.

### <a name="remarks"></a>Observaciones

La función `resourceGroup()` no se puede usar en una plantilla que está [implementada en el nivel de suscripción](deploy-to-subscription.md). Solo puede usarse en las plantillas que se implementan en un grupo de recursos. Puede usar la `resourceGroup()`función en una [plantilla vinculada o anidada (con ámbito interno)](linked-templates.md) que tenga como destino un grupo de recursos, incluso cuando la plantilla primaria se implementa en la suscripción. En ese escenario, la plantilla vinculada o anidada se implementa en el nivel de grupo de recursos. Para más información sobre cómo establecer como destino un grupo de recursos en una implementación de nivel de suscripción, consulte [Implementar recursos de Azure en más de una suscripción o un grupo de recursos](./deploy-to-resource-group.md).

Un uso común de la función resourceGroup es crear recursos en la misma ubicación que el grupo de recursos. En el ejemplo siguiente se utiliza la ubicación del grupo de recursos para un valor de parámetro predeterminado.

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
}
```

También puede usar la función `resourceGroup` para aplicar etiquetas del grupo de recursos a un recurso. Para más información, consulte [Aplicación de etiquetas de un grupo de recursos](../management/tag-resources.md#apply-tags-from-resource-group).

Al usar plantillas anidadas para implementar en varios grupos de recursos, puede especificar el ámbito para evaluar la función `resourceGroup`. Para más información, consulte [Implementación de recursos de Azure en varias suscripciones y grupos de recursos](./deploy-to-resource-group.md).

### <a name="resource-group-example"></a>Ejemplo de grupo de recursos

La [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) siguiente devuelve las propiedades del grupo de recursos.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [],
  "outputs": {
    "resourceGroupOutput": {
      "type" : "object",
      "value": "[resourceGroup()]"
    }
  }
}
```

El ejemplo anterior devuelve un objeto en el formato siguiente:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Esta [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceGroupName.json) genera una propiedad de grupo de recursos específica.

## <a name="resourceid"></a>resourceId

`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2], ...)`

Devuelve el identificador único de un recurso. Utilice esta función cuando el nombre del recurso sea ambiguo o no esté aprovisionado dentro de la misma plantilla. El formato del identificador devuelto varía en función de si la implementación se produce en el ámbito de un grupo de recursos, una suscripción, un grupo de administración o un inquilino.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| subscriptionId |No |Cadena (en formato de GUID) |El valor predeterminado es la suscripción actual. Especifique este valor cuando necesite recuperar un recurso en otra suscripción. Proporcione solo este valor al realizar la implementación en el ámbito de un grupo de recursos o suscripción. |
| resourceGroupName |No |string |El valor predeterminado es el grupo de recursos actual. Especifique este valor cuando necesite recuperar un recurso en otro grupo de recursos. Proporcione solo este valor al realizar la implementación en el ámbito de un grupo de recursos. |
| resourceType |Sí |string |Tipo de recurso, incluido el espacio de nombres del proveedor de recursos. |
| resourceName1 |Sí |string |Nombre del recurso. |
| resourceName2 |No |string |Segmento con el nombre del siguiente segmento, si es necesario. |

Siga agregando nombres de recursos como parámetros cuando el tipo de recurso incluya más segmentos.

### <a name="return-value"></a>Valor devuelto

Cuando la plantilla se implementa en el ámbito de un grupo de recursos, el identificador de recurso se devuelve en el formato siguiente:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Puede usar la función resourceId para otros ámbitos de implementación, pero cambia el formato del id.

Si usa resourceId durante la implementación en una suscripción, el id. de recurso se devuelve en el formato siguiente:

```json
/subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Si usa resourceId durante la implementación en un grupo de administración o inquilino, el id. de recurso se devuelve en el formato siguiente:

```json
/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Para evitar confusiones, no se recomienda usar `resourceId` al trabajar con recursos implementados en la suscripción, el grupo de administración o el inquilino. En su lugar, use la función de id. que se diseñó para el ámbito.

En el caso de los [recursos del nivel de suscripción](deploy-to-subscription.md), use la función [subscriptionResourceId()](#subscriptionresourceid).

En el caso de los [recursos del nivel de grupo de administración](deploy-to-management-group.md), use la función [extensionResourceId](#extensionresourceid) para hacer referencia a un recurso que se implementa como extensión de un grupo de administración. Por ejemplo, las definiciones de directivas personalizadas que se implementan en un grupo de administración son extensiones del grupo de administración. Use la función [tenantResourceId](#tenantresourceid) para hacer referencia a los recursos que se implementan en el inquilino, pero están disponibles en el grupo de administración. Por ejemplo, las definiciones de directivas integradas se implementan como recursos de nivel de inquilino.

En el caso de los [recursos de nivel de inquilino](deploy-to-tenant.md), use la función [tenantResourceId](#tenantresourceid). Use tenantResourceId para las definiciones de directiva integradas, ya que se implementan en el nivel de inquilino.

### <a name="remarks"></a>Observaciones

El número de parámetros que proporcione varía en función de si el recurso es primario o secundario, y de si está en la misma suscripción o grupo de recursos.

Para obtener el Id. de un recurso primario de la misma suscripción y grupo de recursos, proporcione el tipo y el nombre del recurso.

```json
"[resourceId('Microsoft.ServiceBus/namespaces', 'namespace1')]"
```

Para obtener el Id. de un recurso secundario, preste atención al número de segmentos en el tipo de recurso. Proporcione un nombre de recurso para cada segmento del tipo de recurso. El nombre del segmento corresponde al recurso que existe para esa parte de la jerarquía.

```json
"[resourceId('Microsoft.ServiceBus/namespaces/queues/authorizationRules', 'namespace1', 'queue1', 'auth1')]"
```

Para obtener el Id. de un recurso de la misma suscripción en un grupo de recursos distinto, proporcione el nombre del grupo de recursos.

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts', 'examplestorage')]"
```

Para obtener el Id. de un recurso con una suscripción y grupo de recursos distintos, proporcione el Id. de suscripción y el nombre del grupo de recursos.

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

A menudo, necesitará utilizar esta función cuando se usa una cuenta de almacenamiento o red virtual en un grupo de recursos alternativo. En el ejemplo siguiente se muestra cómo un recurso de un grupo de recursos externos se puede utilizar fácilmente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string"
    },
    "virtualNetworkName": {
      "type": "string"
    },
    "virtualNetworkResourceGroup": {
      "type": "string"
    },
    "subnet1Name": {
      "type": "string"
    },
    "nicName": {
      "type": "string"
    }
  },
  "variables": {
    "subnet1Ref": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnet1Name'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Network/networkInterfaces",
      "apiVersion": "2015-05-01-preview",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnet1Ref')]"
              }
            }
          }
        ]
      }
    }
  ]
}
```

### <a name="resource-id-example"></a>Ejemplo de identificador de recursos

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) siguiente se devuelve el identificador de recursos de la cuenta de almacenamiento en el grupo de recursos:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [],
  "outputs": {
    "sameRGOutput": {
      "type": "string",
      "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
    },
    "differentRGOutput": {
      "type": "string",
      "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
    },
    "differentSubOutput": {
      "type": "string",
      "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
    },
    "nestedResourceOutput": {
      "type": "string",
      "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]"
    }
  }
}
```

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| sameRGOutput | String | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | String | /subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | String | /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | String | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

## <a name="subscription"></a>subscription

`subscription()`

Devuelve detalles sobre la suscripción para la implementación actual.

### <a name="return-value"></a>Valor devuelto

La función devuelve el siguiente formato:

```json
{
  "id": "/subscriptions/{subscription-id}",
  "subscriptionId": "{subscription-id}",
  "tenantId": "{tenant-id}",
  "displayName": "{name-of-subscription}"
}
```

### <a name="remarks"></a>Observaciones

Al usar plantillas anidadas para implementar en varias suscripciones, puede especificar el ámbito para evaluar la función de suscripción. Para más información, consulte [Implementación de recursos de Azure en varias suscripciones y grupos de recursos](./deploy-to-resource-group.md).

### <a name="subscription-example"></a>Ejemplo de suscripción

En la [plantilla de ejemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) siguiente se muestra la función de suscripción a la que se llama en la sección de salidas.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [],
  "outputs": {
    "subscriptionOutput": {
      "value": "[subscription()]",
      "type" : "object"
    }
  }
}
```

## <a name="subscriptionresourceid"></a>subscriptionResourceId

`subscriptionResourceId([subscriptionId], resourceType, resourceName1, [resourceName2], ...)`

Devuelve el identificador único de un recurso implementado en el nivel de suscripción.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| subscriptionId |No |Cadena (en formato de GUID) |El valor predeterminado es la suscripción actual. Especifique este valor cuando necesite recuperar un recurso en otra suscripción. |
| resourceType |Sí |string |Tipo de recurso, incluido el espacio de nombres del proveedor de recursos. |
| resourceName1 |Sí |string |Nombre del recurso. |
| resourceName2 |No |string |Segmento con el nombre del siguiente segmento, si es necesario. |

Siga agregando nombres de recursos como parámetros cuando el tipo de recurso incluya más segmentos.

### <a name="return-value"></a>Valor devuelto

El identificador se devuelve con el formato siguiente:

```json
/subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Observaciones

Utilice esta función para obtener el identificador de recurso de los recursos que se [implementan en la suscripción](deploy-to-subscription.md) en lugar de en un grupo de recursos. El identificador devuelto difiere del valor devuelto por la función [resourceId](#resourceid) en que no incluye un valor de grupo de recursos.

### <a name="subscriptionresourceid-example"></a>Ejemplo de subscriptionResourceId

La siguiente plantilla asigna un rol integrado. Puede implementarlo en un grupo de recursos o en una suscripción. Usa la función subscriptionResourceId para obtener el id. de recurso para recursos integrados.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "defaultValue": "[newGuid()]",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[subscriptionResourceId('Microsoft.Authorization/roleDefinitions', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2018-09-01-preview",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]"
      }
    }
  ]
}
```

## <a name="tenantresourceid"></a>tenantResourceId

`tenantResourceId(resourceType, resourceName1, [resourceName2], ...)`

Devuelve el identificador único de un recurso implementado en el nivel de inquilino.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| resourceType |Sí |string |Tipo de recurso, incluido el espacio de nombres del proveedor de recursos. |
| resourceName1 |Sí |string |Nombre del recurso. |
| resourceName2 |No |string |Segmento con el nombre del siguiente segmento, si es necesario. |

Siga agregando nombres de recursos como parámetros cuando el tipo de recurso incluya más segmentos.

### <a name="return-value"></a>Valor devuelto

El identificador se devuelve con el formato siguiente:

```json
/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Observaciones

Esta función se usa para obtener el identificador de recurso de un recurso que se implementa en el inquilino. El identificador devuelto difiere de los valores devueltos por otras funciones de identificador de recurso en que no incluye los valores de un grupo de recursos o una suscripción.

### <a name="tenantresourceid-example"></a>Ejemplo de tenantResourceId

Las definiciones de directivas integradas son recursos del nivel de inquilino. Para implementar una asignación de directiva que hace referencia a una definición de directiva integrada, use la función tenantResourceId.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string",
      "defaultValue": "0a914e76-4921-4c19-b460-a2d36003525a",
      "metadata": {
        "description": "Specifies the ID of the policy definition or policy set definition being assigned."
      }
    },
    "policyAssignmentName": {
      "type": "string",
      "defaultValue": "[guid(parameters('policyDefinitionID'), resourceGroup().name)]",
      "metadata": {
        "description": "Specifies the name of the policy assignment, can be used defined or an idempotent name as the defaultValue provides."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "name": "[parameters('policyAssignmentName')]",
      "apiVersion": "2019-09-01",
      "properties": {
        "scope": "[subscriptionResourceId('Microsoft.Resources/resourceGroups', resourceGroup().name)]",
        "policyDefinitionId": "[tenantResourceId('Microsoft.Authorization/policyDefinitions', parameters('policyDefinitionID'))]"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar una descripción de las secciones de una plantilla de Azure Resource Manager en [Nociones sobre la estructura y la sintaxis de las plantillas de Resource Manager](./syntax.md).
* Para combinar varias plantillas, consulte [Uso de plantillas vinculadas y anidadas al implementar recursos de Azure](linked-templates.md).
* Para iterar un número especificado de veces al crear un tipo de recurso, consulte [Iteración de recursos en las plantillas de Resource Manager](copy-resources.md).
* Para ver cómo implementar la plantilla que ha creado, consulte [Implementación de recursos con plantillas de Resource Manager y Azure PowerShell](deploy-powershell.md).
