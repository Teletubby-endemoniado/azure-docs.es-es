---
title: Conexiones del indexador a través de un punto de conexión privado
titleSuffix: Azure Cognitive Search
description: Configure las conexiones del indexador para acceder a través de un punto de conexión privado al contenido de recursos de Azure protegidos.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/13/2021
ms.openlocfilehash: 79bb517faffdda7e9d7ddef45e7b52f5e81dc201
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128589689"
---
# <a name="make-indexer-connections-through-a-private-endpoint"></a>Establecimiento de conexiones del indexador a través de un punto de conexión privado

> [!NOTE]
> Puede usar el [enfoque de servicio de Microsoft de confianza](../storage/common/storage-network-security.md#trusted-microsoft-services) para omitir las restricciones de red virtual o IP en una cuenta de almacenamiento. También puede habilitar el servicio de búsqueda para acceder a los datos de la cuenta de almacenamiento. Para ello, consulte [Acceso a los datos de las cuentas de almacenamiento de forma segura mediante una excepción de servicio de confianza](search-indexer-howto-access-trusted-service-exception.md).
> 
> Sin embargo, cuando se usa este enfoque, la comunicación entre Azure Cognitive Search y su cuenta de almacenamiento se produce mediante la dirección IP pública de la cuenta de almacenamiento, a través de la red troncal de Microsoft segura.

Muchos recursos de Azure, como las cuentas de Azure Storage, pueden configurarse para que acepten las conexiones de una lista de redes virtuales y rechacen las conexiones externas cuyo origen sea una red pública. Si usa un indexador para indexar datos en Azure Cognitive Search y el origen de datos se encuentra en una red privada, puede crear una [conexión de punto de conexión privado](../private-link/private-endpoint-overview.md) de salida para acceder a los datos.

El método de conexión de este indexador está sujeto a los dos requisitos siguientes:

+ El recurso de Azure que proporciona contenido o código debe registrarse previamente en el servicio [Azure Private Link](https://azure.microsoft.com/services/private-link/).

+ El servicio Azure Cognitive Search debe estar en el nivel Básico o superior. Algunas características no están disponibles en el nivel Gratis. Además, si el indexador tiene un conjunto de aptitudes, el nivel debe ser Estándar 2 (S2) o superior. Para más información, consulte los [límites del servicio](search-limits-quotas-capacity.md#shared-private-link-resource-limits).

## <a name="shared-private-link-resources-management-apis"></a>API de administración de recursos de vínculo privado compartido

Los puntos de conexión privados de recursos protegidos que se crean a través de las API de Azure Cognitive Search se conocen como *recursos de vínculo privado compartido*. Eso se debe a que "comparte" el acceso a un recurso, como una cuenta de almacenamiento, que se ha integrado en el [servicio Azure Private Link](https://azure.microsoft.com/services/private-link/).

A través de su API REST de administración, Azure Cognitive Search proporciona una operación [CreateOrUpdate](/rest/api/searchmanagement/2021-04-01-preview/shared-private-link-resources/create-or-update) que puede usar para configurar el acceso desde un indexador de Azure Cognitive Search.

Puede crear conexiones de punto de conexión privado solo a algunos recursos mediante la versión preliminar de Search Management API (versión *2020-08-01-preview* o posterior), que se designa como *versión preliminar previa* en la tabla siguiente. Los recursos sin una designación *versión preliminar* se pueden crear con la versión de la API en versión preliminar o disponible con carácter general (*2020-08-01* o posterior).

En la siguiente tabla se enumeran los recursos de Azure para los que puede crear puntos de conexión privados de salida desde Azure Cognitive Search. Para crear un recurso compartido de vínculo privado, escriba los valores de **Id. de grupo** exactamente como están escritos en la API. Los valores distinguen mayúsculas de minúsculas.

| Recurso de Azure | Identificador de grupo |
| --- | --- |
| Azure Storage: Blob | `blob`|
| Azure Storage: Data Lake Storage Gen2 | `dfs` y `blob` |
| Azure Storage: Tablas | `table`|
| Azure Cosmos DB: SQL API | `Sql`|
| Azure SQL Database | `sqlServer`|
| Azure Database for MySQL (versión preliminar) | `mysqlServer`|
| Azure Key Vault | `vault` |
| Azure Functions (versión preliminar) | `sites` |

También puede consultar los recursos de Azure para los que se admiten conexiones de punto de conexión privado de salida mediante la [lista de API compatibles](/rest/api/searchmanagement/2021-04-01-preview/private-link-resources/list-supported).

En el resto de este artículo, se usa una combinación de Azure Portal, o la [CLI de Azure](/cli/azure/), si lo prefiere, y [Postman](https://www.postman.com/), o cualquier otro cliente HTTP como [curl](https://curl.se/), si lo prefiere, para demostrar las llamadas API de REST.

> [!NOTE]
> Para crear una conexión de punto de conexión privado a Azure Data Lake Storage Gen2, debe crear dos puntos de conexión privados. Un punto de conexión privado con el valor de groupID de "dfs" y otro punto de conexión privado con el valor de groupID de "blob".

## <a name="set-up-indexer-connection-through-private-endpoint"></a>Configuración de una conexión del indexador a través de un punto de conexión privado

Use las siguientes instrucciones para configurar una conexión de indexador a través de un punto de conexión privado a un recurso seguro de Azure.

Los ejemplos de este artículo se basan en los siguientes supuestos:
* El nombre del servicio de búsqueda es _contoso-search_, que existe en el grupo de recursos _contoso_ de una suscripción con el identificador _00000000-0000-0000-0000-000000000000_. 
* El identificador de recurso de este servicio de búsqueda es _/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search_.

### <a name="step-1-secure-your-azure-resource"></a>Paso 1: Protección del recurso de Azure

Los pasos para restringir el acceso varían según el recurso. En los siguientes escenarios se muestran tres de los tipos más comunes de recursos.

- Escenario 1: origen de datos

    A continuación se muestra un ejemplo de cómo configurar una cuenta de almacenamiento de Azure. Si selecciona esta opción y deja la página vacía, significa que no se admite tráfico de redes virtuales.

    ![Captura de pantalla del panel "Firewalls y redes virtuales" del almacenamiento de Azure, en la que se muestra la opción para permitir el acceso a las redes seleccionadas](media\search-indexer-howto-secure-access\storage-firewall-noaccess.png)

- Escenario 2: Azure Key Vault

    A continuación se muestra un ejemplo de cómo configurar Azure Key Vault.
 
    ![Captura de pantalla del panel "Firewalls y redes virtuales" de Azure Key Vault, en la que se muestra la opción para permitir el acceso a las redes seleccionadas](media\search-indexer-howto-secure-access\key-vault-firewall-noaccess.png)
    
- Escenario 3: Azure Functions

    No se necesitan cambios en la configuración de red para Azure Functions. En los siguientes pasos, al crear el punto de conexión privado compartido, la función solo permitirá automáticamente el acceso a través de un vínculo privado, después de crear un punto de conexión privado compartido a la función.

### <a name="step-2-create-a-shared-private-link-resource-to-the-azure-resource"></a>Paso 2: Creación de un recurso de vínculo privado compartido al recurso de Azure

En la siguiente sección se describe cómo crear un recurso de vínculo privado compartido usando Azure Portal o la CLI de Azure.

#### <a name="option-1-portal"></a>Opción 1: Azure Portal

> [!NOTE]
> Azure Portal solo admite la creación de un punto de conexión privado compartido usando valores de Id. de grupo que sean de disponibilidad general. Para MySQL y Azure Functions, use los pasos de la CLI de Azure, que se describen a continuación en la opción 2.

Para solicitar a Azure Cognitive Search que cree una conexión de punto de conexión privado saliente, en la hoja "Acceso privado compartido" haga clic en "Agregar acceso privado compartido". En la hoja que se abre a la derecha, puede elegir "Conectarse a un recurso de Azure en mi directorio" o "Conectarse a un recurso de Azure por Id. de recurso o alias".

Si usa la primera opción (recomendada), la hoja le ayudará a elegir el recurso de Azure adecuado y rellenará automáticamente otras propiedades, como el Id. de grupo y el tipo del recurso.

   ![Captura de pantalla del panel "Add Shared Private Access" (Agregar acceso privado compartido), en la que se muestra una experiencia guiada para crear un recurso de vínculo privado compartido. ](media\search-indexer-howto-secure-access\new-shared-private-link-resource.png)

Si usa la segunda opción, puede escribir manualmente el Id. de recurso de Azure y elegir el Id. de grupo correspondiente. Los Ids. de grupo se muestran al principio de este artículo.

![Captura de pantalla del panel "Add Shared Private Access" (Agregar acceso privado compartido), en la que se muestra una experiencia manual para crear un recurso de vínculo privado compartido. ](media\search-indexer-howto-secure-access\new-shared-private-link-resource-manual.png)

#### <a name="option-2-azure-cli"></a>Opción 2: CLI de Azure

También puede realizar la siguiente llamada de API con la [CLI de Azure](/cli/azure/). Use la versión 2020-08-01-preview de la API si usa un Id. de grupo que se encuentra en versión preliminar. Por ejemplo, los Ids. de grupo *sites* y *mysqlServer* están en versión preliminar y requieren que use la API de versión preliminar.

```dotnetcli
az rest --method put --uri https://management.azure.com/subscriptions/<search service subscription ID>/resourceGroups/<search service resource group name>/providers/Microsoft.Search/searchServices/<search service name>/sharedPrivateLinkResources/<shared private endpoint name>?api-version=2020-08-01 --body @create-pe.json
```

A continuación se muestra un ejemplo del contenido del archivo *create-pe.json*:

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve"
      }
}
```

Se devuelve la respuesta `202 Accepted` en caso de éxito. El proceso de creación de un punto de conexión privado de salida es una operación de larga duración (asincrónica), Implica la implementación de los siguientes recursos:

+ Un punto de conexión privado asignado con una dirección IP privada con un estado `"Pending"`. La dirección IP privada se obtiene del espacio de direcciones que se asigna a la red virtual del entorno de ejecución del indexador privado específico del servicio de búsqueda. Tras la aprobación del punto de conexión privado, cualquier comunicación desde Azure Cognitive Search al recurso de Azure se origina desde la dirección IP privada y un canal de vínculo privado seguro.

+ Una zona DNS privada para el tipo de recurso, que se basa en el valor de `groupId`. Mediante la implementación de este recurso se asegura de que todas las búsquedas DNS en el recurso privado utilizan la dirección IP asociada con el punto de conexión privado.

Asegúrese de especificar el valor de `groupId` correcto para el tipo de recurso para el que crea el punto de conexión privado. Cualquier discrepancia producirá un mensaje de respuesta incorrecta.

### <a name="step-3-check-the-status-of-the-private-endpoint-creation"></a>Paso 3: Comprobación del estado de la creación del punto de conexión privado

En este paso, confirmará que el estado de aprovisionamiento del recurso cambia a "Correcto".

#### <a name="option-1-portal"></a>Opción 1: Azure Portal

> [!NOTE]
> El estado de aprovisionamiento estará visible en Azure Portal para los Ids. de grupo y de disponibilidad general que se encuentran en versión preliminar.

Azure Portal le mostrará el estado del punto de conexión privado compartido. En el siguiente ejemplo el estado es "Actualizando".

![Captura de pantalla del panel "Add Shared Private Access" (Agregar acceso privado compartido), en la que se muestra la creación de recursos en curso. ](media\search-indexer-howto-secure-access\new-shared-private-link-resource-progress.png)

Una vez creado correctamente el recurso, recibirá una notificación del portal, y el estado de aprovisionamiento del recurso cambiará a "Correcto".

![Captura de pantalla del panel "Add Shared Private Access" (Agregar acceso privado compartido), en la que se muestra la creación de recursos completada. ](media\search-indexer-howto-secure-access\new-shared-private-link-resource-success.png)

#### <a name="option-2-azure-cli"></a>Opción 2: CLI de Azure

La llamada `PUT` para crear el punto de conexión privado compartido devuelve un valor de encabezado `Azure-AsyncOperation` similar al siguiente:

`"Azure-AsyncOperation": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

Puede sondear el estado realizando una consulta manual del valor `Azure-AsyncOperationHeader`.

```dotnetcli
az rest --method get --uri https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01
```

### <a name="step-4-approve-the-private-endpoint-connection"></a>Paso 4: Aprobación de la conexión de punto de conexión privado

> [!NOTE]
> En esta sección se usa Azure Portal para recorrer el flujo de aprobación de un punto de conexión privado al recurso de Azure al que va a conectarse. Como alternativa, puede usar la [API REST](/rest/api/storagerp/privateendpointconnections) que está disponible a través del proveedor de recursos de almacenamiento.
>
> Otros proveedores, como Azure Cosmos DB o Azure SQL Server, ofrecen API de proveedor de recursos de almacenamiento similares para administrar conexiones de punto de conexión privado.

1. En Azure Portal, vaya al recurso de Azure con el que quiera establecer conexión y seleccione la pestaña **Redes**. A continuación, vaya a la sección en la que se enumeran las conexiones de punto de conexión privado. A continuación se muestra un ejemplo de una cuenta de almacenamiento. Una vez que la operación asincrónica se ha realizado correctamente, debe haber una solicitud de una conexión de punto de conexión privado con el mensaje de solicitud de la llamada API anterior.

   ![Captura de pantalla de Azure Portal en la que se muestra el panel "Conexiones de punto de conexión privado".](media\search-indexer-howto-secure-access\storage-privateendpoint-approval.png)

1. Seleccione el punto de conexión privado que Azure Cognitive Search ha creado. En la columna **Punto de conexión privado**, identifique la conexión del punto de conexión privado con el nombre que se especifica en la API anterior, seleccione **Aprobar** y escriba un mensaje adecuado. El contenido del mensaje no es significativo.

   Asegúrese de que la conexión del punto de conexión privado aparece como se muestra en la siguiente captura de pantalla. El estado puede tardar entre uno y dos minutos en actualizarse en el portal.

   ![Captura de pantalla de Azure Portal en la que se muestra un estado "Approved" el panel "Conexiones de punto de conexión privado".](media\search-indexer-howto-secure-access\storage-privateendpoint-after-approval.png)

Cuando se aprueba la solicitud de conexión de punto de conexión privado, el tráfico *puede* fluir a través del punto de conexión privado. Después de que se aprueba el punto de conexión privado, Azure Cognitive Search crea las asignaciones de zona DNS necesarias en la zona DNS que se crea para él.

### <a name="step-5-query-the-status-of-the-shared-private-link-resource"></a>Paso 5: Consulta del estado del recurso de vínculo privado compartido

Para confirmar que el recurso de vínculo privado compartido se ha actualizado después de la aprobación, vuelva a visitar la hoja "Shared Private Access" (Acceso privado compartido) del servicio Search en Azure Portal y compruebe el "Estado de conexión".

   ![Captura de pantalla de Azure Portal, que muestra un recurso de vínculo privado compartido como "Aprobado".](media\search-indexer-howto-secure-access\new-shared-private-link-resource-approved.png)

Como alternativa, también puede obtener el "Estado de conexión" mediante la [API GET](/rest/api/searchmanagement/2021-04-01-preview/shared-private-link-resources/get).

```dotnetcli
az rest --method get --uri https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01
```

Esto devolvería un elemento JSON, donde el estado de conexión se mostraría como "estado" debajo de la sección "propiedades". A continuación se muestra un ejemplo de una cuenta de almacenamiento.

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve",
        "status": "Approved",
        "resourceRegion": null,
        "provisioningState": "Succeeded"
      }
}

```

Si la opción "Estado de aprovisionamiento" (`properties.provisioningState`) del recurso es `Succeeded` y la opción "Estado de conexión" (`properties.status`) es `Approved`, significa que el recurso de vínculo privado compartido es funcional y el indexador se puede configurar para que se comunique a través del punto de conexión privado.

### <a name="step-6-configure-the-indexer-to-run-in-the-private-environment"></a>Paso 6: Configuración del indexador para que se ejecute en el entorno privado

> [!NOTE]
> Este paso se puede realizar antes de que se apruebe la conexión del punto de conexión privado. Hasta ese momento, cualquier indexador que intente comunicarse con un recurso seguro (como la cuenta de almacenamiento) terminará con un estado de error transitorio. Los nuevos indexadores no se crearán. En cuanto se apruebe la conexión del punto de conexión privado, los indexadores pueden acceder a la cuenta de almacenamiento privada.

En los siguientes pasos se muestra cómo configurar el indexador para que se ejecute en el entorno privado usando la API de REST. También puede establecer el entorno de ejecución mediante el editor de JSON en Azure Portal.

1. Cree la definición de origen de datos, el índice y el conjunto de aptitudes (si usa uno) como lo haría normalmente. No hay propiedades en ninguna de estas definiciones que varíen al usar un punto de conexión privado compartido.

1. [Cree un indexador](/rest/api/searchservice/create-indexer) que apunte al origen de datos, el índice y el conjunto de aptitudes que creó en el paso anterior. Además, fuerce a que el indexador se ejecute en el entorno de ejecución privado; para ello, establezca la propiedad de configuración `executionEnvironment` del indexador en `private`.

    ```json
    {
        "name": "indexer",
        "dataSourceName": "blob-datasource",
        "targetIndexName": "index",
        "parameters": {
            "configuration": {
                "executionEnvironment": "private"
            }
        },
        "fieldMappings": []
    }
    ```

    A continuación se muestra un ejemplo de la solicitud en Postman.
    
    ![Captura de pantalla que muestra la creación de un indexador en la interfaz de usuario de Postman.](media\search-indexer-howto-secure-access\create-indexer.png)    

Una vez que el indexador se cree correctamente, debería conectarse al recurso de Azure a través de la conexión de punto de conexión privado. Puede supervisar el estado del indexador. Para ello, se utiliza [Indexer Status API](/rest/api/searchservice/get-indexer-status).

> [!NOTE]
> Si ya tiene indexadores, puede actualizarlos a través de la [API PUT](/rest/api/searchservice/create-indexer). Para ello, establezca `executionEnvironment` en `private` o use el editor de JSON en Azure Portal.

## <a name="troubleshooting"></a>Solución de problemas

+ Si se produce un error al crear un indexador con el mensaje "Las credenciales del origen de datos no son válidas", significa que el estado de la conexión del punto de conexión privado aún no es *Approved* o que la conexión no es funcional. Para solucionar el problema: 
  + Obtenga el estado del recurso de vínculo privado compartido mediante [GET API](/rest/api/searchmanagement/2021-04-01-preview/shared-private-link-resources/get). Si el estado es *Approved*, compruebe el valor `properties.provisioningState` del recurso. Si el estado es `Incomplete`, significa que no se pudieron configurar algunas de las dependencias subyacentes del recurso. Una nueva emisión de la solicitud de `PUT` para volver a crear el recurso de vínculo privado compartido debería corregir el problema. Es posible que haya que volver a aprobarla. Vuelva a comprobar el estado del recurso para asegurarse de que se ha corregido el problema.

+ Si crea el indexador sin establecer su propiedad `executionEnvironment`, el proceso podría finalizar correctamente, pero su historial mostrará que sus ejecuciones no son correctas. Para solucionar el problema:
  + [Actualice el indexador](/rest/api/searchservice/update-indexer) para especificar el entorno de ejecución.

+ Si ha creado el indexador sin establecer la propiedad `executionEnvironment` y se ejecuta correctamente, significa que Azure Cognitive Search ha decidido que su entorno de ejecución es el entorno *privado* específico del servicio de búsqueda. Esto puede cambiar en función de los recursos que haya consumido el indexador y la carga del servicio de búsqueda, entre otros factores, y puede producir un error más adelante. Para solucionar el problema:
  + Se recomienda encarecidamente establecer la propiedad `executionEnvironment` en `private` para asegurarse de que no aparecerán errores en el futuro.

+ Si está viendo la página de redes del origen de datos en Azure Portal y selecciona un punto de conexión privado que creó para que el servicio Azure Cognitive Search acceda a este origen de datos, es posible que reciba un error *Sin acceso*. Se espera que esto sea así. Puede cambiar el estado de la solicitud de conexión en la página del portal del servicio de destino, pero para administrar aún más el recurso de vínculo privado compartido, debe ver el recurso de vínculo privado compartido en la página de red del servicio Search en Azure Portal.

[Cuotas y límites](search-limits-quotas-capacity.md) determinan el número de recursos de vínculo privado compartido que se pueden crear y que pueden depender de la SKU del servicio de búsqueda.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los puntos de conexión privados:

+ [Solución de problemas relacionados con los recursos compartidos de Private Link](troubleshoot-shared-private-link-resources.md)
+ [¿Qué son los puntos de conexión privados?](../private-link/private-endpoint-overview.md)
+ [Configuraciones de DNS necesarias para los puntos de conexión privados](../private-link/private-endpoint-dns.md)