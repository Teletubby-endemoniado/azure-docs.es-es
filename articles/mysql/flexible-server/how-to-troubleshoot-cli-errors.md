---
title: Solución de errores de la CLI del servidor flexible de Azure Database for MySQL
description: En este tema se proporcionan instrucciones para solucionar problemas comunes con la CLI de Azure cuando se usa el servidor flexible de MySQL.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 08/24/2021
ms.openlocfilehash: eae5f1fe6a44cedafb00e10c9995905b993ca7b9
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123439992"
---
# <a name="troubleshoot-azure-database-for-mysql-flexible-server-cli-errors"></a>Solución de errores de la CLI del servidor flexible de Azure Database for MySQL
[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

Esta documentación le ayudará a solucionar problemas comunes con la CLI de Azure cuando se usa el servidor flexible de MySQL.

## <a name="command-not-found"></a>No se encontró el comando

 Si recibe un error que dice que un comando **está mal escrito o que el sistema no lo reconoce**, esto puede significar que la versión de la CLI de la máquina cliente no está actualizada. Ejecute ```az upgrade``` para actualizar a la versión más reciente. La actualización de la versión de la CLI puede ayudar a resolver problemas con las incompatibilidades de un comando debido a cambios en la API.

 
## <a name="debug-deployment-failures"></a>Depuración de errores de implementación 
Actualmente, la CLI de Azure no admite activar el registro de depuración, pero sí puede recuperarlo mediante los pasos siguientes.

>[!NOTE]
> - Reemplace ```examplegroup``` y ```exampledeployment``` por el grupo de recursos y el nombre de implementación correctos para el servidor de bases de datos. 
> - Puede ver el nombre de implementación en la página de implementaciones del grupo de recursos. Consulte [Búsqueda del nombre de implementación](../../azure-resource-manager/templates/deployment-history.md?tabs=azure-portal).

1. Muestre las implementaciones del grupo de recursos para identificar la implementación de MySQL Server. 
    ```azurecli

        az deployment operation group list \
          --resource-group examplegroup \
          --name exampledeployment
    ```

2. Obtenga el contenido de la solicitud de la implementación de MySQL Server. 
    ```azurecli

        az deployment operation group list \
          --name exampledeployment \
          -g examplegroup \
          --query [].properties.request
    ```
3. Examine el contenido de la respuesta. 
    ```azurecli
    az deployment operation group list \
      --name exampledeployment \
      -g examplegroup \
      --query [].properties.response
    ```

## <a name="error-codes"></a>Códigos de error

| Código de error | Mitigación |
| ---------- | ---------- | 
|MissingSubscriptionRegistration|Registre la suscripción con el proveedor de recursos. Ejecute el comando ```az provider register --namespace Microsoft.DBMySQL``` para resolver el problema.|
|InternalServerError| Intente ver los registros de actividad del servidor para ver si hay más información. Ejecute el comando ```az monitor activity-log list --correlation-id <enter correlation-id>```. Puede probar el mismo comando de la CLI después de unos minutos. Si los problemas persisten, [informe de ello](https://github.com/Azure/azure-cli/issues) o póngase en contacto con el soporte técnico de Microsoft.|
|ResourceNotFound| No se encuentra el recurso al que se hace referencia.  Puede comprobar las propiedades del recurso o si este se ha eliminado, o comprobar si el recurso es de otra suscripción. |
|LocationNotAvailableForResourceType| - Compruebe la disponibilidad del servidor flexible de Azure Database for MySQL en las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=mysql). <br>- Compruebe si los tipos de recursos de Azure DB for MySQL están registrados en su suscripción. |
|ResourceGroupBeingDeleted| Se está eliminando el grupo de recursos. Espere a que la eliminación finalice.|
|PasswordTooLong| La contraseña proporcionada es demasiado larga. Debe tener entre 8 y 128 caracteres. La contraseña debe contener caracteres de tres de las siguientes categorías: Letras del alfabeto inglés mayúsculas y minúsculas, números (0 a 9) y caracteres no alfanuméricos (!, $, #, %, etc.).|
|PasswordNotComplex| La contraseña proporcionada no es lo suficientemente compleja.  Debe tener entre 8 y 128 caracteres. La contraseña debe contener caracteres de tres de las siguientes categorías: Letras del alfabeto inglés mayúsculas y minúsculas, números (0 a 9) y caracteres no alfanuméricos (!, $, #, %, etc.).|
|PasswordTooShort| Debe tener entre 8 y 128 caracteres. La contraseña debe contener caracteres de tres de las siguientes categorías: Letras del alfabeto inglés mayúsculas y minúsculas, números (0 a 9) y caracteres no alfanuméricos (!, $, #, %, etc.).|
|SubscriptionNotFound| No se encontró la suscripción solicitada. Ejecute ```az account list all``` para ver todas las suscripciones actuales.|
|InvalidParameterValue| Se ha dado un valor no válido a un parámetro. Consulte los [documentos de referencia de la CLI](/cli/azure/mysql/flexible-server?view=azure-cli-latest&preserve-view=true) para ver cuáles son los valores correctos admitidos para los argumentos.|
|InvalidLocation| Se ha especificado una ubicación no válida. Compruebe la disponibilidad del servidor flexible de Azure Database for MySQL en las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=mysql). |
|InvalidServerName| Se ha identificado un nombre de servidor no válido. Compruebe el nombre de servidor. Ejecute el comando [az mysql flexible-server list](/cli/azure/mysql/flexible-server?view=azure-cli-latest&preserve-view=true#az_mysql_flexible_server_list) para ver toda la lista de servidores flexibles disponibles. |
|InvalidResourceIdSegment| Se ha identificado un error de sintaxis en la plantilla de Azure Resource Manager. Use una herramienta de formateador JSON para validar el JSON e identificar el error de sintaxis.|
|InvalidUserName| Escriba un nombre de usuario válido. El nombre de inicio de sesión del administrador no puede ser azure_superuser, azure_pg_admin, admin, administrator, root, guest ni public. No puede empezar por pg_.|
|BlockedUserName| El nombre de inicio de sesión del administrador no puede ser azure_superuser, azure_pg_admin, admin, administrator, root, guest ni public. No puede empezar por pg_. Evite usar estos patrones en el nombre del administrador.|

## <a name="next-steps"></a>Pasos siguientes

- Si sigue experimentando problemas, [notifíquelo](https://github.com/Azure/azure-cli/issues). 
- Si tiene alguna pregunta, visite nuestra página de Stack Overflow: https://aka.ms/azcli/questions. 
- Háganos saber cómo vamos con esta breve encuesta https://aka.ms/azureclihats. 
