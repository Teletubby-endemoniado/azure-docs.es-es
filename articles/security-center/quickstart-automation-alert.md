---
title: Creación de una automatización de seguridad para alertas de seguridad específicas mediante una plantilla de Azure Resource Manager
description: Obtenga información sobre cómo crear una automatización del servicio Microsoft Defender for Cloud con el fin de desencadenar una aplicación lógica mediante alertas específicas de Defender for Cloud, con ayuda de una plantilla de Azure Resource Manager (ARM).
services: azure-resource-manager
author: memildin
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: memildin
ms.date: 08/20/2020
ms.openlocfilehash: 18d5dcfa0559bbf94da14fdca975da568d011da2
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131472264"
---
# <a name="quickstart-create-an-automatic-response-to-a-specific-security-alert-using-an-arm-template"></a>Inicio rápido: Creación de una respuesta automática a una alerta de seguridad concreta mediante una plantilla de Resource Manager

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

En este inicio rápido se describe cómo usar una plantilla de ARM para crear una automatización del flujo de trabajo que desencadene una aplicación lógica cuando Microsoft Defender for Cloud reciba alertas de seguridad específicas.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure.](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2fquickstarts%2fmicrosoft.security%2fsecuritycenter-create-automation-for-alertnamecontains%2fazuredeploy.json)

## <a name="prerequisites"></a>Prerrequisitos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Para ver una lista de los roles y permisos necesarios para la característica de automatización del flujo de trabajo de Microsoft Defender for Cloud, consulte la información sobre la [automatización del flujo de trabajo](workflow-automation.md).

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/securitycenter-create-automation-for-alertnamecontains/).

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.security/securitycenter-create-automation-for-alertnamecontains/azuredeploy.json":::

### <a name="relevant-resources"></a>Recursos relevantes

- [**Microsoft.Security/automations**](/azure/templates/microsoft.security/automations): La automatización que desencadenará la aplicación lógica al recibir una alerta de Microsoft Defender for Cloud que contenga una cadena específica.
- [**Microsoft.Logic/workflows**](/azure/templates/microsoft.logic/workflows): Una aplicación lógica vacía que se puede desencadenar.

Para ver más plantillas de inicio rápido de Defender for Cloud, consulte estas [versiones facilitadas por la comunidad](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Security&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Implementación de la plantilla

- **PowerShell**:

  ```azurepowershell-interactive
  New-AzResourceGroup -Name <resource-group-name> -Location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.security/securitycenter-create-automation-for-alertnamecontains/azuredeploy.json
  ```

- **CLI**:

  ```azurecli-interactive
  az group create --name <resource-group-name> --location <resource-group-location> #use this command when you need to create a new resource group for your deployment
  az deployment group create --resource-group <my-resource-group> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.security/securitycenter-create-automation-for-alertnamecontains/azuredeploy.json
  ```

- **Portal**:

  [![Implementación en Azure.](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2fquickstarts%2fmicrosoft.security%2fsecuritycenter-create-automation-for-alertnamecontains%2fazuredeploy.json)

  Para más información acerca de esta opción de implementación, consulte [Uso de un botón de implementación para implementar plantillas desde el repositorio de GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md).

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Use Azure Portal para comprobar que la automatización de flujos de trabajo se ha implementado.

1. En [Azure Portal](https://portal.azure.com), abra **Defender for Cloud**.
1. En la barra de menús superior, seleccione el icono de filtro y, después, seleccione la suscripción específica en la que ha implementado la nueva automatización de flujos de trabajo.
1. En el menú de Defender for Cloud, abra la **automatización del flujo de trabajo** y busque la nueva automatización.
    :::image type="content" source="./media/quickstart-automation-alert/validating-template-run.png" alt-text="Lista de automatizaciones configuradas." lightbox="./media/quickstart-automation-alert/validating-template-run.png":::
    >[!TIP]
    > Si tiene muchas automatizaciones de flujos de trabajo en la suscripción, use la opción de **filtrar por nombre**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no la necesite, elimine la automatización de flujos de trabajo mediante Azure Portal.

1. En [Azure Portal](https://portal.azure.com), abra **Defender for Cloud**.
1. En la barra de menús superior, seleccione el icono de filtro y, después, seleccione la suscripción específica en la que ha implementado la nueva automatización de flujos de trabajo.
1. En el menú de Defender for Cloud, abra la **automatización del flujo de trabajo** y busque la automatización que se va a eliminar.
    :::image type="content" source="./media/quickstart-automation-alert/deleting-workflow-automation.png" alt-text="Pasos para quitar una automatización de flujo de trabajo." lightbox="./media/quickstart-automation-alert/deleting-workflow-automation.png":::
1. Active la casilla correspondiente al elemento que se va a eliminar.
1. En la barra de herramientas, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un tutorial paso a paso que le guíe en el proceso de creación de una plantilla, consulte:

> [!div class="nextstepaction"]
> [Tutorial: Creación e implementación de su primera plantilla de Resource Manager](../azure-resource-manager/templates/template-tutorial-create-first-template.md)
