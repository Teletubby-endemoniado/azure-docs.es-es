---
title: Direcciones IP del servicio Azure API Management | Microsoft Docs
description: Aprenda a recuperar las direcciones IP del servicio Azure API Management y sepa cuándo cambian.
services: api-management
documentationcenter: ''
author: dlepow
ms.service: api-management
ms.topic: article
ms.date: 04/13/2021
ms.author: apimpm
ms.custom: fasttrack-edit
ms.openlocfilehash: fe5f282150aae2103d20963416f390bf159c48ea
ms.sourcegitcommit: d2875bdbcf1bbd7c06834f0e71d9b98cea7c6652
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129856892"
---
# <a name="ip-addresses-of-azure-api-management"></a>Direcciones IP de Azure API Management

En este artículo se indica cómo recuperar las direcciones IP del servicio Azure API Management. Las direcciones IP pueden ser públicas o privadas si el servicio está en una red virtual.

Puede usar direcciones IP para crear reglas de firewall, filtrar el tráfico entrante a los servicios de back-end o restringir el tráfico saliente.

## <a name="ip-addresses-of-api-management-service"></a>Direcciones IP del servicio API Management

Cada instancia de servicio de API Management en el nivel de Desarrollador, Básico, Estándar o Premium tiene direcciones IP públicas, que solo son exclusivas de esa instancia de servicio (no se comparten con otros recursos). 

Puede recuperar las direcciones IP desde el panel de información general del recurso en Azure Portal.

![Direcciones IP de API Management](media/api-management-howto-ip-addresses/public-ip.png)

También puede capturarlas mediante programación con la siguiente llamada API:

```
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<service-name>?api-version=<api-version>
```

Las direcciones IP públicas formarán parte de la respuesta:

```json
{
  ...
  "properties": {
    ...
    "publicIPAddresses": [
      "13.77.143.53"
    ],
    ...
  }
  ...
}
```

En las [implementaciones en varias regiones](api-management-howto-deploy-multi-region.md), cada implementación regional tiene una dirección IP pública.

## <a name="ip-addresses-of-api-management-service-in-vnet"></a>Direcciones IP del servicio API Management en una red virtual

Si el servicio API Management se encuentra en una red virtual, tendrá dos tipos de direcciones IP: públicas y privadas.

Las direcciones IP públicas se usan para la comunicación interna en el puerto `3443`, para administrar la configuración (por ejemplo, mediante Azure Resource Manager). En la configuración de la red virtual externa, también se usan para el tráfico de API en tiempo de ejecución. 

Las direcciones IP virtuales privadas (VIP), disponibles **únicamente** en el [modo de red virtual interna](api-management-using-with-internal-vnet.md), se usan para conectarse desde dentro de la red a los puntos de conexión y puertas de enlace de API Management, al portal para desarrolladores y al plano de administración para el acceso directo a la API. Puede usarlas para configurar los registros DNS dentro de la red.

Verá direcciones de ambos tipos en Azure Portal y en la respuesta a la llamada API:

![API Management en direcciones IP de red virtual](media/api-management-howto-ip-addresses/vnet-ip.png)


```json
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<service-name>?api-version=<api-version>

{
  ...
  "properties": {
    ...
    "publicIPAddresses": [
      "13.85.20.170"
    ],
    "privateIPAddresses": [
      "192.168.1.5"
    ],
    ...
  },
  ...
}
```

API Management usa una dirección IP pública para conexiones fuera de la red virtual y una dirección IP privada para conexiones sin la red virtual. 

Al implementar API Management en la [configuración de red virtual interna](api-management-using-with-internal-vnet.md) y API Management se conecta a back-end privados (accesibles desde intranet), las direcciones IP internas de la subred se usan para el tráfico de la API en tiempo de ejecución. Cuando se envía una solicitud desde API Management a un back-end privado, será visible una dirección IP privada como origen de la solicitud. Por lo tanto, en esta configuración, si existe un requisito para restringir el tráfico entre API Management y un back-end interno, es mejor usar todo el prefijo de subred de API Management con una regla de IP y no solo la dirección IP privada asociada al recurso de API Management. 

Cuando se envía una solicitud desde API Management a un back-end de acceso público (accesible desde Internet), siempre será visible una dirección IP pública como origen de la solicitud.

## <a name="ip-addresses-of-consumption-tier-api-management-service"></a>Direcciones IP del nivel de consumo en el servicio API Management

Si el servicio API Management es un servicio con nivel de consumo, no tiene una dirección IP dedicada. El servicio con nivel de consumo se ejecuta en una infraestructura compartida y sin una dirección IP determinista. 

En lo que respecta a la restricción del tráfico, puede usar el intervalo de direcciones IP de los centros de datos de Azure. Consulte el [artículo sobre documentación de Azure Functions](../azure-functions/ip-addresses.md#data-center-outbound-ip-addresses) para conocer los pasos concretos.

## <a name="changes-to-the-ip-addresses"></a>Cambios en las direcciones IP

En los niveles Desarrollador, Básico, Estándar y Premium de API Management, las direcciones IP públicas (VIP) son estáticas durante la vigencia de un servicio, con las siguientes excepciones:

* El servicio se elimina y se vuelve a crear.
* La suscripción al servicio se [suspende](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) o se [advierte](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (por ejemplo, por falta de pago) y luego se reinstaura.
* Azure Virtual Network se agrega o se quita del servicio.
* El servicio API Management se cambia entre los modos de implementación de red virtual externa e interna.

En las [implementaciones en varias regiones](api-management-howto-deploy-multi-region.md), la dirección IP regional cambia si una región está vacía y, posteriormente, se restituye. La dirección IP regional también cambia al habilitar, agregar o quitar [zonas de disponibilidad](zone-redundancy.md).
