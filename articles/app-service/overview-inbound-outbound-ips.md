---
title: Direcciones IP de entrada y salida
description: Aprenda cómo se usan las direcciones IP de entrada y salida en App Service, cuándo cambian y cómo encontrar las direcciones de su aplicación.
ms.topic: article
ms.date: 08/25/2020
ms.custom: seodec18, devx-track-azurepowershell
ms.openlocfilehash: f8354d52c43d786e3506cf6fcee0907f9f365ab6
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124836324"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Direcciones IP de entrada y salida en Azure App Service

[Azure App Service](overview.md) es un servicio multiinquilino, excepto en los [entornos de App Service](environment/intro.md). Las aplicaciones que no están en un entorno de App Service (no están en el [nivel aislado](https://azure.microsoft.com/pricing/details/app-service/)) comparten la infraestructura de red con otras aplicaciones. Como consecuencia, las direcciones IP de entrada y salida de una aplicación pueden ser diferentes y pueden cambiar incluso en determinadas situaciones.

Los [entornos de App Service](environment/intro.md) usan infraestructuras de red dedicadas, así que las aplicaciones que se ejecutan en un entorno de App Service obtienen direcciones IP estáticas dedicadas tanto para las conexiones de entrada como de salida.

## <a name="how-ip-addresses-work-in-app-service"></a>Funcionamiento de las direcciones IP en App Service

Una aplicación de App Service se ejecuta en un plan de App Service, y los planes de App Service se implementan en una de las unidades de implementación de la infraestructura de Azure (llamada internamente "espacio web"). Cada unidad de implementación tiene asignadas hasta cinco direcciones IP virtuales, lo que incluye una dirección IP de entrada pública y un conjunto de [direcciones IP de salida](#find-outbound-ips). Todos los planes de App Service en la misma unidad de implementación, así como las instancias de aplicación que se ejecutan en ellas, comparten el mismo conjunto de direcciones IP virtuales. En el caso de un App Service Environment (un plan de App Service en el [nivel Aislado](https://azure.microsoft.com/pricing/details/app-service/)), el plan de App Service es la propia unidad de implementación, por lo que las direcciones IP virtuales se dedican esta como resultado.

Dado que no se permite cambiar un plan de App Service entre unidades de implementación, las direcciones IP virtuales asignadas a la aplicación suelen ser las mismas, aunque hay excepciones.

## <a name="when-inbound-ip-changes"></a>Cuando cambia la dirección IP de entrada

Sin importar el número de instancias escaladas horizontalmente, cada aplicación tiene una única dirección IP de entrada. La dirección IP de entrada puede cambiar al realizar una de las siguientes acciones:

- Eliminar una aplicación y volver a crearla en otro grupo de recursos (puede cambiar la unidad de implementación).
- Eliminar la última aplicación de un grupo de recursos _y_ región y volver a crearla (puede cambiar la unidad de implementación).
- Eliminar un enlace TLS/SSL basado en IP, como durante la renovación de un certificado (consulte [Renovación de certificados](configure-ssl-certificate.md#renew-an-expiring-certificate)).

## <a name="find-the-inbound-ip"></a>Buscar la dirección IP de entrada

Solo ejecute el siguiente comando en un terminal local:

```bash
nslookup <app-name>.azurewebsites.net
```

## <a name="get-a-static-inbound-ip"></a>Obtención de una dirección IP de entrada estática

En ocasiones, podría desear una dirección IP estática dedicada para la aplicación. Para obtener una dirección IP de entrada estática, debe [proteger un dominio personalizado](configure-ssl-bindings.md#secure-a-custom-domain). Si realmente no necesita la funcionalidad TLS para proteger la aplicación, puede cargar incluso un certificado autofirmado para este enlace. En un enlace TLS basado en IP, el certificado se enlaza a la dirección IP propiamente dicha, de modo que App Service aprovisiona una dirección IP estática para conseguir que esto ocurra. 

## <a name="when-outbound-ips-change"></a>Cuando cambian las direcciones IP de salida

Sin importar el número de instancias escaladas horizontalmente, cada aplicación tiene un número establecido de direcciones IP de salida en cualquier momento dado. Las conexiones de salida desde la aplicación de App Service, como a una base de datos back-end, usan una de las direcciones IP de salida como dirección IP de origen. La dirección IP que se va a usar se selecciona aleatoriamente en tiempo de ejecución, por lo que el servicio de back-end debe abrir su firewall para todas las direcciones IP de salida de la aplicación.

El conjunto de direcciones IP de salida de la aplicación cambia cuando realiza una de las acciones siguientes:

- Eliminar una aplicación y volver a crearla en otro grupo de recursos (puede cambiar la unidad de implementación).
- Eliminar la última aplicación de un grupo de recursos _y_ región y volver a crearla (puede cambiar la unidad de implementación).
- Escale la aplicación entre los niveles inferiores (**Básica**, **Estándar** y **Premium**) y el nivel **PremiumV2** y el nivel **PremiumV3** (las direcciones IP se pueden agregar o quitar del conjunto).

Puede encontrar el conjunto de todas las posibles direcciones IP de salida que puede utilizar la aplicación, independientemente de los planes de tarifa, buscando la propiedad `possibleOutboundIpAddresses` o en el campo **Direcciones IP salientes adicionales** de la hoja **Propiedades** de Azure Portal. Consulte [Búsqueda de las direcciones IP de salida](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Búsqueda de las direcciones IP de salida

Para buscar las direcciones IP de salida que usa actualmente su aplicación en Azure Portal, haga clic en **Propiedades** en el panel de navegación izquierdo de la aplicación. Se enumeran en el campo **Direcciones IP de salida**.

Puede encontrar la misma información si ejecuta el comando siguiente en [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Para buscar _todas_ las posibles direcciones IP de salida para la aplicación, con independencia de los planes de tarifa, haga clic en **Propiedades** en el panel de navegación izquierdo de la aplicación. Se enumeran en el campo **Direcciones IP salientes adicionales**.

Puede encontrar la misma información si ejecuta el comando siguiente en [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="get-a-static-outbound-ip"></a>Obtención de una dirección IP estática de salida
Puede controlar la dirección IP del tráfico de salida desde la aplicación mediante la integración de una red virtual regional junto con una instancia de NAT Gateway, a fin de dirigir el tráfico a través de una dirección IP pública estática. La [integración de una red virtual regional](./web-sites-integrate-with-vnet.md) está disponible en los planes de App Service **Estándar**, **Premium**, **PremiumV2** y **PremiumV3**. Para más información sobre esta configuración, consulte [Integración de Virtual Network NAT Gateway](./networking/nat-gateway-integration.md).

## <a name="next-steps"></a>Pasos siguientes

Aprenda a restringir el tráfico entrante por dirección IP de origen.

> [!div class="nextstepaction"]
> [Restricciones de IP estática](app-service-ip-restrictions.md)