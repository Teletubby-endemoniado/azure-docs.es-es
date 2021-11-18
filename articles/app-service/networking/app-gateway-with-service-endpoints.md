---
title: 'Integración de Application Gateway: Azure App Service | Microsoft Docs'
description: En este artículo se describe cómo se integra Application Gateway con Azure App Service.
services: app-service
documentationcenter: ''
author: madsd
manager: ccompy
editor: ''
ms.assetid: 073eb49c-efa1-4760-9f0c-1fecd5c251cc
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/04/2021
ms.author: madsd
ms.custom: seodec18
ms.openlocfilehash: dda9b5a55255ca98ea6890caa5581a4096246645
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132714967"
---
# <a name="application-gateway-integration"></a>Integración de Application Gateway
Hay tres variaciones de App Service que requieren una configuración ligeramente diferente de la integración con Azure Application Gateway. Por ejemplo, la versión normal de App Service, también conocida como "multiinquilino", el equilibrador de carga interno (ILB) y el ASE externo. En este artículo se explica cómo configurarlo con App Service (multiinquilino) mediante el punto de conexión de servicio para proteger el tráfico. En el artículo también se tratarán las consideraciones sobre el uso del punto de conexión privado y la integración con ILB y ASE externo. Por último, el artículo tiene consideraciones sobre el sitio de SCM/Kudu.

## <a name="integration-with-app-service-multi-tenant"></a>Integración con App Service (multiinquilino)
App Service (multiinquilino) tiene un punto de conexión público accesible desde Internet. Con los [puntos de conexión de servicio](../../virtual-network/virtual-network-service-endpoints-overview.md) puede permitir el tráfico solo desde una subred específica en una instancia de Azure Virtual Network y bloquear todo lo demás. En el siguiente escenario, usaremos esta funcionalidad para garantizar que una instancia de App Service solo pueda recibir tráfico de una instancia específica de Application Gateway.

:::image type="content" source="./media/app-gateway-with-service-endpoints/service-endpoints-appgw.png" alt-text="En el diagrama se muestra el flujo de Internet a una instancia de Application Gateway en una red virtual de Azure y el flujo desde allí a través de un icono de firewall a instancias de aplicaciones de App Service.":::

Esta configuración tiene dos partes, además, crearemos las instancias de App Service y Application Gateway. La primera parte consiste en habilitar los puntos de conexión de servicio en la subred de Virtual Network donde se implementó Application Gateway. Los puntos de conexión de servicio garantizarán que todo el tráfico de red que sale de la subred hacia App Service se etiquete con el identificador de subred específico. La segunda parte consiste en establecer una restricción de acceso de la aplicación web específica para garantizar que solo se permita el tráfico etiquetado con este identificador de subred específico. Puede establecer la configuración con distintas herramientas en función de sus preferencias.

## <a name="using-azure-portal"></a>Uso de Azure Portal
Con Azure Portal, siga estos cuatro pasos para aprovisionar y realizar la configuración. Si ya tiene los recursos, puede saltarse los primeros pasos.
1. Cree una instancia de App Service con uno de los inicios rápidos de la documentación de App Service, por ejemplo, [Inicio rápido de .NET Core](../quickstart-dotnetcore.md).
2. Cree una instancia de Application Gateway mediante el [inicio rápido del portal](../../application-gateway/quick-create-portal.md), pero sáltese la sección para agregar destinos de back-end.
3. Configure [App Service como back-end en Application Gateway](../../application-gateway/configure-web-app-portal.md), pero sáltese la sección de restricción del acceso.
4. Por último, cree la [restricción de acceso usando puntos de conexión de servicio](../../app-service/app-service-ip-restrictions.md#set-a-service-endpoint-based-rule).

Ahora puede acceder a App Service a través de Application Gateway, pero si trata de obtener acceso a App Service directamente, debería recibir un error HTTP 403 que indica que el sitio web está detenido.

:::image type="content" source="./media/app-gateway-with-service-endpoints/website-403-forbidden.png" alt-text="Captura de pantalla que muestra el texto de un error 403: prohibido.":::

## <a name="using-azure-resource-manager-template"></a>Uso de la plantilla de Azure Resource Manager
La [plantilla de implementación de Resource Manager][template-app-gateway-app-service-complete] aprovisionará un escenario completo. El escenario consta de una instancia de App Service bloqueada con puntos de conexión de servicio y restricciones de acceso para recibir únicamente el tráfico de Application Gateway. La plantilla incluye muchos valores predeterminados inteligentes y versiones de reparación únicas agregadas a los nombres de recursos para que sea simple. Para invalidarlos, tendrá que clonar el repositorio o descargar la plantilla y editarla.

Para aplicar la plantilla, puede usar el botón Implementar en Azure, que se encuentra en la descripción de la plantilla, o bien usar la función de PowerShell o la CLI adecuadas.

## <a name="using-azure-command-line-interface"></a>Uso de la interfaz de la línea de comandos de Azure
El [ejemplo CLI de Azure](../../app-service/scripts/cli-integrate-app-service-with-application-gateway.md) aprovisionará una instancia de App Service bloqueada con puntos de conexión de servicio y restricciones de acceso para recibir únicamente el tráfico de Application Gateway. Si solo necesita aislar el tráfico a una instancia de App Service que ya exista desde otra instancia existente de Application Gateway, bastará con ejecutar el siguiente comando.

```azurecli-interactive
az webapp config access-restriction add --resource-group myRG --name myWebApp --rule-name AppGwSubnet --priority 200 --subnet mySubNetName --vnet-name myVnetName
```

En la configuración predeterminada, el comando garantizará que se establezca la configuración de los puntos de conexión de servicio en la subred y la restricción de acceso en App Service.

## <a name="considerations-when-using-private-endpoint"></a>Consideraciones sobre el uso de un punto de conexión privado

Como alternativa al punto de conexión de servicio, puede usar un punto de conexión privado para proteger el tráfico entre Application Gateway y App Service (multiinquilino). Deberá asegurarse de que Application Gateway puede resolver mediante DNS la dirección IP privada de las aplicaciones de App Service, o bien usar la dirección IP privada en el grupo de back-end e invalidar el nombre de host en la configuración de HTTP.

:::image type="content" source="./media/app-gateway-with-service-endpoints/private-endpoint-appgw.png" alt-text="En el diagrama se muestra el flujo de tráfico a una instancia de Application Gateway en Azure Virtual Network, y el flujo desde allí a través de un punto de conexión privado a instancias de aplicaciones de App Service.":::

## <a name="considerations-for-ilb-ase"></a>Consideraciones del ASE de ILB
El ASE de ILB no está expuesto a Internet y el tráfico entre la instancia y Application Gateway está ya aislado en la red virtual. La siguiente [guía paso a paso](../environment/integrate-with-application-gateway.md) configura un ASE de ILB y lo integra con Application Gateway mediante Azure Portal.

Si desea asegurarse de que solo el tráfico procedente de la subred de Application Gateway llegue al ASE, puede configurar un grupo de seguridad de red (NSG) que afecte a todas las aplicaciones web del ASE. En el caso del NSG, es posible especificar el intervalo de direcciones IP de la subred y, opcionalmente, los puertos (80/443). No invalide [las reglas de NSG necesarias](../environment/network-info.md#network-security-groups) para que ASE funcione correctamente.

Para aislar el tráfico en una aplicación web concreta, deberá usar restricciones de acceso basadas en IP, ya que los puntos de conexión de servicio no funcionarán con el ASE. La dirección IP debe ser la IP privada de la instancia de Application Gateway.

## <a name="considerations-for-external-ase"></a>Consideraciones del ASE externo
El ASE externo tiene un equilibrador de carga de acceso público como la versión de App Service multiinquilino. Los puntos de conexión de servicio no funcionan con el ASE y por eso tendrá que usar las restricciones de acceso basadas en IP mediante la dirección IP pública de la instancia de Application Gateway. Para crear un ASE externo mediante Azure Portal, puede seguir este [inicio rápido](../environment/create-external-ase.md)

[template-app-gateway-app-service-complete]: https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.web/web-app-with-app-gateway-v2/ "Plantilla de Azure Resource Manager para un escenario completo"

## <a name="considerations-for-kuduscm-site"></a>Consideraciones sobre el sitio de KUDU/SCM
El sitio de SCM, también conocido como "KUDU", es un sitio de administración que existe para cada aplicación web. No es posible invertir el proxy en el sitio SCM y lo más probable es que también desee bloquearlo en direcciones IP específicas o en una subred específica.

Si desea utilizar las mismas restricciones de acceso que el sitio principal, puede heredar la configuración mediante el siguiente comando.

```azurecli-interactive
az webapp config access-restriction set --resource-group myRG --name myWebApp --use-same-restrictions-for-scm-site
```

Si desea establecer restricciones de acceso concretas para el sitio de SCM, puede agregar restricciones de acceso mediante la marca --scm-site como se muestra a continuación.

```azurecli-interactive
az webapp config access-restriction add --resource-group myRG --name myWebApp --scm-site --rule-name KudoAccess --priority 200 --ip-address 208.130.0.0/16
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre App Service Environment, consulte la [documentación de App Service Environment](../environment/index.yml).

Para proteger aún más la aplicación web, puede encontrar información sobre el Firewall de aplicaciones web en Application Gateway en la [documentación del Firewall de aplicaciones web de Azure](../../web-application-firewall/ag/ag-overview.md).