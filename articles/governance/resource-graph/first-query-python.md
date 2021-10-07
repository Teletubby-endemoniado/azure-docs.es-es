---
title: 'Inicio rápido: La primera consulta de Python'
description: En este inicio rápido, dará los pasos necesarios para habilitar la biblioteca de Resource Graph para Python y ejecutará la primera consulta.
ms.date: 10/01/2021
ms.topic: quickstart
ms.custom:
- devx-track-python
- mode-api
ms.openlocfilehash: 0dfe97eda8244eb0926709d3ed39a514fbb984e9
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129355464"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-python"></a>Inicio rápido: Ejecución de la primera consulta de Resource Graph con Python

El primer paso para usar Azure Resource Graph es comprobar que están instaladas las bibliotecas necesarias para Python. Este inicio rápido lo guiará a través del proceso de agregar las bibliotecas a la instalación de Python.

Cuando finalice, habrá agregado las bibliotecas a la instalación de Python y ejecutado la primera consulta de Resource Graph.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="add-the-resource-graph-library"></a>Adición de la biblioteca de Resource Graph

Para que Python pueda consultar Azure Resource Graph, se debe agregar la biblioteca. Esta biblioteca funciona siempre que se pueda usar Python, lo que incluye [bash en Windows 10](/windows/wsl/install-win10) o instalado localmente.

1. Compruebe que está instalada la versión más reciente de Python (al menos la **3.8**). Si aún no está instalada, descárguela en [Python.org](https://www.python.org/downloads/).

1. Compruebe que está instalada la versión más reciente de la CLI de Azure (al menos la **2.5.1**). Si aún no está instalada, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

   > [!NOTE]
   > La CLI de Azure es necesaria para que Python pueda usar la **autenticación basada en la CLI** en los ejemplos siguientes. Para información sobre otras opciones, consulte [Autenticación mediante las bibliotecas de administración de Python](/azure/developer/python/azure-sdk-authenticate).

1. Autentíquese mediante la CLI de Azure.

   ```azurecli
   az login
   ```

1. En el entorno de Python que elija, instale las bibliotecas necesarias para Azure Resource Graph:

   ```bash
   # Add the Resource Graph library for Python
   pip install azure-mgmt-resourcegraph

   # Add the Resources library for Python
   pip install azure-mgmt-resource

   # Add the CLI Core library for Python for authentication (development only!)
   pip install azure-cli-core

   # Add the Azure identity library for Python
   pip install azure.identity
   ```

   > [!NOTE]
   > Si Python está instalado en todos los usuarios, estos comando deben ejecutarse desde una consola con privilegios elevados.

1. Compruebe que se han instalado las bibliotecas. `azure-mgmt-resourcegraph` debe tener la versión **2.0.0** o superior; `azure-mgmt-resource` debe tener la versión **9.0.0** o superior; y `azure-cli-core` debe tener la versión **2.5.0**.

   ```bash
   # Check each installed library
   pip show azure-mgmt-resourcegraph azure-mgmt-resource azure-cli-core azure.identity
   ```

## <a name="run-your-first-resource-graph-query"></a>Ejecutar la primera consulta de Resource Graph

Con las bibliotecas de Python agregadas al entorno elegido, es el momento de probar una consulta simple de Resource Graph basada en suscripción. La consulta devolverá los cinco primeros recursos de Azure con el **nombre** y el **tipo de recurso** de cada recurso. Para realizar una consulta por [grupo de administración](../management-groups/overview.md), use el parámetro `management_groups` con `QueryRequest`.

1. Ejecute la primera consulta de Azure Resource Graph mediante las bibliotecas instaladas y el método `resources`:

   ```python
   # Import Azure Resource Graph library
   import azure.mgmt.resourcegraph as arg

   # Import specific methods and models from other libraries
   from azure.mgmt.resource import SubscriptionClient
   from azure.identity import AzureCliCredential

   # Wrap all the work in a function
   def getresources( strQuery ):
       # Get your credentials from Azure CLI (development only!) and get your subscription list
       credential = AzureCliCredential()
       subsClient = SubscriptionClient(credential)
       subsRaw = []
       for sub in subsClient.subscriptions.list():
           subsRaw.append(sub.as_dict())
       subsList = []
       for sub in subsRaw:
           subsList.append(sub.get('subscription_id'))

       # Create Azure Resource Graph client and set options
       argClient = arg.ResourceGraphClient(credential)
       argQueryOptions = arg.models.QueryRequestOptions(result_format="objectArray")

       # Create query
       argQuery = arg.models.QueryRequest(subscriptions=subsList, query=strQuery, options=argQueryOptions)

       # Run query
       argResults = argClient.resources(argQuery)

       # Show Python object
       print(argResults)

   getresources("Resources | project name, type | limit 5")
   ```

   > [!NOTE]
   > Como esta consulta de ejemplo no proporciona un modificador de ordenación como `order by`, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud.

1. Actualice la llamada a `getresources` y cambie la consulta a `order by` la propiedad **Nombre**:

   ```python
   getresources("Resources | project name, type | limit 5 | order by name asc")
   ```

   > [!NOTE]
   > Al igual que con la primera consulta, es probable que al ejecutar esta consulta varias veces se produzca un conjunto diferente de recursos por solicitud. El orden de los comandos de consulta es importante. En este ejemplo, el `order by` viene después del `limit`. Este orden de comandos limita primero los resultados de la consulta y, luego, los ordena.

1. Actualice la llamada a `getresources` y cambie la consulta a primero `order by` la propiedad **Nombre** y, luego, `limit` a los cinco primeros resultados:

   ```python
   getresources("Resources | project name, type | order by name asc | limit 5")
   ```

Cuando la consulta final se ejecuta varias veces, suponiendo que nada cambie en su entorno, los resultados devueltos serán coherentes y estarán ordenados por la propiedad **Nombre**, pero todavía limitados a los cinco primeros resultados.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea quitar las bibliotecas instaladas del entorno de Python, puede hacerlo mediante el comando siguiente:

```bash
# Remove the installed libraries from the Python environment
pip uninstall azure-mgmt-resourcegraph azure-mgmt-resource azure-cli-core azure.identity
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha agregado las bibliotecas de Resource Graph al entorno de Python y ha ejecutado la primera consulta. Para más información sobre el lenguaje de Resource Graph, vaya a la página de detalles del lenguaje de consulta.

> [!div class="nextstepaction"]
> [Obtener más información sobre el lenguaje de consulta](./concepts/query-language.md)
