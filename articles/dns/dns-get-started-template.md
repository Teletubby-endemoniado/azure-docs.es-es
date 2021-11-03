---
title: 'Creación de un registro y una zona DNS de Azure: plantilla de Azure Resource Manager'
titleSuffix: Azure DNS
description: Obtenga información sobre cómo crear una zona y un registro DNS en Azure DNS. Este es un inicio rápido paso a paso para crear y administrar su primera zona DNS y su primer mediante una plantilla de Azure Resource Manager.
services: dns
author: duongau
ms.author: duau
ms.date: 6/2/2021
ms.topic: quickstart
ms.service: dns
ms.custom: devx-track-azurepowershell, subject-armqs, mode-arm
ms.openlocfilehash: dba996bcd4d2416a41ac6a7f1ad94804c0ecb839
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131033102"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-an-arm-template"></a>Creación de un registro y una zona DNS de Azure mediante una plantilla de Resource Manager

En este inicio rápido se describe cómo usar una plantilla de Azure Resource Manager para crear una zona DNS que contenga un registro `A`.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.network%2Fazure-dns-new-zone%2Fazuredeploy.json)

## <a name="prerequisites"></a>Prerrequisitos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/azure-dns-new-zone).

En este inicio rápido, va a crear una zona DNS única con el sufijo `azurequickstart.org`. En la zona también se colocará un registro `A` que apunte a dos direcciones IP.

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.network/azure-dns-new-zone/azuredeploy.json":::

En la plantilla se han definido dos recursos de Azure:

- [**Microsoft.Network/dnsZones**](/azure/templates/microsoft.network/dnsZones)
- [**Microsoft.Network/dnsZones/A**](/azure/templates/microsoft.network/dnsZones/A): Se usa para crear un registro `A` en la zona.

Para encontrar más plantillas relacionadas con Azure Traffic Manager, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Implementación de la plantilla

1. Seleccione **Try It** (Probarlo) en el bloque de código siguiente para abrir Azure Cloud Shell y siga las instrucciones para iniciar sesión en Azure.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.network/azure-dns-new-zone/azuredeploy.json"

    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

    Read-Host -Prompt "Press [ENTER] to continue ..."
    ```

    Espere hasta que vea el aviso de la consola.

1. Seleccione **Copiar** en el bloque de código anterior para copiar el script de PowerShell.

1. Haga clic con el botón derecho en el panel de consola del shell y, a continuación, seleccione **Pegar**.

1. Escriba los valores.

    La implementación de la plantilla crea una zona con un registro `A` que apunta a dos direcciones IP. El nombre del grupo de recursos es el nombre del proyecto con **rg** anexado.

    La plantilla tarda un par de minutos en implementarse. Al finalizar, la salida es parecida a esta:

    :::image type="content" source="./media/dns-getstarted-template/create-dns-zone-powershell-output.png" alt-text="Salida de la implementación de PowerShell de la plantilla de Resource Manager de la zona DNS de Azure":::

Azure PowerShell se usa para implementar la plantilla. Además de Azure PowerShell, también puede usar Azure Portal, la CLI de Azure y API REST. Para obtener información sobre otros métodos de implementación, consulte [Implementación de plantillas](../azure-resource-manager/templates/deploy-portal.md).

## <a name="validate-the-deployment"></a>Validación de la implementación

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Grupos de recursos** en el panel izquierdo.

1. Seleccione el grupo de recursos que creó en la sección anterior. El nombre del grupo de recursos predeterminado es el nombre del proyecto con **rg** anexado.

1. El grupo de recursos debe contener los recursos que se ven aquí:

    :::image type="content" source="./media/dns-getstarted-template/resource-group-dns-zone.png" alt-text="Grupo de recursos de implementación de zona DNS":::

1. Seleccione la zona DNS con el sufijo de `azurequickstart.org` para comprobar que la zona se crea correctamente con un registro `A` que hace referencia al valor de `1.2.3.4` y `1.2.3.5`.

    :::image type="content" source="./media/dns-getstarted-template/dns-zone-overview.png" alt-text="Implementación de zona DNS":::

1. Copie uno de los nombres de servidor del paso anterior.

1. Abra un símbolo del sistema y ejecute el comando siguiente:

   ```cmd
   nslookup www.<dns zone name> <name server name>
   ```

   Por ejemplo:

   ```cmd
   nslookup www.2lwynbseszpam.azurequickstart.org ns1-09.azure-dns.com.
   ```

   Debería ver algo parecido a la captura de pantalla siguiente:

    :::image type="content" source="./media/dns-getstarted-template/dns-zone-validation.png" alt-text="nslookup de zona DNS":::

El nombre de host `www.2lwynbseszpam.azurequickstart.org` resuelve en `1.2.3.4` y `1.2.3.5`, tal como lo ha configurado. Este resultado confirma que la resolución de nombres funciona correctamente.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con la zona DNS, elimine el grupo de recursos. Esta acción elimina la zona DNS y todos los recursos relacionados.

Para eliminar el grupo de recursos, llame al cmdlet `Remove-AzResourceGroup`:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado lo siguiente:

- Zona DNS
- Registro `A`

Ahora que ha creado su primera zona y su primer registro DNS mediante una plantilla de Resource Manager, puede crear registros para una aplicación web de un dominio personalizado.

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)
