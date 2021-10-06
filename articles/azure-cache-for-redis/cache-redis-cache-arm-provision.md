---
title: Implementación de Azure Resource Manager mediante una plantilla de Azure Cache for Redis
description: Aprenda a usar una plantilla de Azure Resource Manager (plantilla de Resource Manager) para implementar un recurso de Azure Cache for Redis. Se suministran plantillas para escenarios frecuentes.
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.custom: subject-armqs, devx-track-azurepowershell
ms.date: 04/28/2021
ms.openlocfilehash: 1f284169387209f3783a9621419bf17ea8240619
ms.sourcegitcommit: c27f71f890ecba96b42d58604c556505897a34f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129537707"
---
# <a name="quickstart-create-an-azure-cache-for-redis-using-an-arm-template"></a>Inicio rápido: Creación de una instancia de Azure Cache for Redis mediante una plantilla de Resource Manager

Descubra cómo crear una plantilla de Azure Resource Manager (plantilla de Resource Manager) que implemente una instancia de Azure Cache for Redis. La memoria caché se puede usar con una cuenta de almacenamiento existente para mantener los datos de diagnóstico. Aprenderá a definir los recursos que se implementan y los parámetros que se especifican cuando se ejecuta la implementación. Puede usar esta plantilla para sus propias implementaciones o personalizarla para satisfacer sus necesidades. Actualmente, se comparten los ajustes de configuración de diagnóstico para todas las cachés de la misma región para una suscripción. Actualizar una caché en la región afectará a todas las demás cachés de la región.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.cache%2Fredis-cache%2Fazuredeploy.json)

## <a name="prerequisites"></a>Requisitos previos

* **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* **Una cuenta de almacenamiento**: Para crear una cuenta de almacenamiento, consulte [Creación de una cuenta de Azure Storage](../storage/common/storage-account-create.md?tabs=azure-portal). La cuenta de almacenamiento se utiliza con datos de diagnóstico.

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/redis-cache/).

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.cache/redis-cache/azuredeploy.json":::

Los recursos siguientes se definen en la plantilla:

* [Microsoft.Cache/Redis](/azure/templates/microsoft.cache/redis)
* [Microsoft.Insights/diagnosticSettings](/azure/templates/microsoft.insights/diagnosticsettings)

También hay plantillas de Resource Manager disponibles para el nuevo [nivel Premium](cache-overview.md#service-tiers).

* [Creación de una instancia de Azure Cache for Redis de nivel Prémium con agrupación en clústeres](https://azure.microsoft.com/resources/templates/redis-premium-cluster-diagnostics/)
* [Creación de una instancia de Azure Cache for Redis de nivel Prémium con persistencia de datos](https://azure.microsoft.com/resources/templates/redis-premium-persistence/)
* [Creación de una instancia de Redis Cache premium implementada en una red virtual](https://azure.microsoft.com/resources/templates/redis-premium-vnet/)

Para buscar las últimas plantillas, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/) y busque _Azure Cache for Redis_.

## <a name="deploy-the-template"></a>Implementación de la plantilla

1. Seleccione la imagen siguiente para iniciar sesión en Azure y abrir la plantilla.

    [![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.cache%2Fredis-cache%2Fazuredeploy.json)
1. Seleccione o escriba los siguientes valores:

    * **Suscripción**: seleccione la suscripción de Azure que se usa para crear el recurso compartido de datos y los restantes recursos.
    * **Grupo de recursos**: seleccione **Crear nuevo** para crear un grupo de recursos, o bien seleccione un grupo de recursos existente.
    * **Ubicación**: seleccione una ubicación para el grupo de recursos. La cuenta de almacenamiento y Redis Caché deben estar en la misma región. Redis Caché utiliza de forma predeterminada la misma ubicación que el grupo de recursos. Por lo tanto, especifique la misma ubicación que la de la cuenta de almacenamiento.
    * **Nombre de Redis Cache**: escriba un nombre para la introducir de Redis Caché.
    * **Cuenta de almacenamiento de diagnósticos existente**: escriba el identificador de recurso de una cuenta de almacenamiento. La sintaxis es `/subscriptions/&lt;SUBSCRIPTION ID>/resourceGroups/&lt;RESOURCE GROUP NAME>/providers/Microsoft.Storage/storageAccounts/&lt;STORAGE ACCOUNT NAME>`.

    Use el valor predeterminado en el resto de la configuración.
1. Seleccione **Acepto los términos y condiciones indicados anteriormente** y, después, **Comprar**.

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Abra la instancia de Redis Cache que ha creado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, elimine el grupo de recursos, que elimina los recursos que contiene.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprenderá a crear una plantilla de Azure Resource Manager que implementa una instancia de Azure Cache for Redis. Para obtener información sobre cómo crear una plantilla de Azure Resource Manager que implemente una aplicación web de Azure con una instancia de Azure Cache for Redis, consulte [Creación de una aplicación web y Azure Cache for Redis mediante una plantilla](./cache-web-app-arm-with-redis-cache-provision.md).
