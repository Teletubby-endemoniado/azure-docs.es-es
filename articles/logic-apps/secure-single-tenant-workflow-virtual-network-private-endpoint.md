---
title: Protección del tráfico entre flujos de trabajo de un solo inquilino y redes virtuales
description: Proteja el tráfico entre redes virtuales, cuentas de almacenamiento y flujos de trabajo de inquilino único en Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: how-to
ms.date: 08/31/2021
ms.openlocfilehash: 500ec9c14dea994b528528389ed03d2b561c1a56
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130241827"
---
# <a name="secure-traffic-between-virtual-networks-and-single-tenant-workflows-in-azure-logic-apps-using-private-endpoints"></a>Protección del tráfico entre redes virtuales y flujos de trabajo de inquilino único en Azure Logic Apps mediante puntos de conexión privados

Para comunicarse de forma segura y privada entre el flujo de trabajo de la aplicación lógica y una red virtual, puede configurar *puntos de conexión privados* para el tráfico entrante y usar la integración de red virtual para el tráfico saliente.

Un punto de conexión privado es una interfaz de red que se conecta de forma privada y segura a un servicio con la tecnología de Azure Private Link. Este servicio puede ser un servicio de Azure, como Azure Logic Apps, Azure Storage, Azure Cosmos DB, SQL o su propio servicio de Private Link. El punto de conexión privado usa una dirección IP privada de la red virtual para incorporar el servicio de manera eficaz a la red virtual.

En este artículo se muestra cómo configurar el acceso mediante puntos de conexión privados para el tráfico entrante, el tráfico saliente y la conexión a cuentas de almacenamiento.

Para más información, revise la siguiente documentación:

- [¿Qué es un punto de conexión privado de Azure?](../private-link/private-endpoint-overview.md)

- [¿Qué es Azure Private Link?](../private-link/private-link-overview.md)

- [¿Qué es un flujo de trabajo de aplicación lógica de inquilino único de Azure Logic Apps?](single-tenant-overview-compare.md)

## <a name="prerequisites"></a>Requisitos previos

Debe tener una red virtual de Azure nueva o existente que incluya una subred sin ninguna delegación. Esta subred se usa para implementar y asignar direcciones IP privadas de la red virtual.

Para más información, revise la siguiente documentación:

- [Inicio rápido: Creación de una red virtual mediante el Portal de Azure](../virtual-network/quick-create-portal.md)

- [¿Qué es la delegación de subred?](../virtual-network/subnet-delegation-overview.md)

- [Adición o eliminación de una delegación de subred](../virtual-network/manage-subnet-delegation.md)

<a name="set-up-inbound"></a>

## <a name="set-up-inbound-traffic-through-private-endpoints"></a>Configuración del tráfico entrante mediante puntos de conexión privados

Para proteger el tráfico entrante al flujo de trabajo, complete estos pasos generales:

1. Inicie el flujo de trabajo con un desencadenador integrado que pueda recibir y controlar las solicitudes entrantes, como el desencadenador Request o el desencadenador HTTP + Webhook. Este desencadenador configura el flujo de trabajo con un punto de conexión al que se puede llamar.

1. Agregue un punto de conexión privado a la red virtual.

1. Realice llamadas de prueba para comprobar el acceso al punto de conexión. Para llamar al flujo de trabajo de la aplicación lógica después de configurar este punto de conexión, debe estar conectado a la red virtual.

### <a name="prerequisites-for-inbound-traffic-through-private-endpoints"></a>Requisitos previos para el tráfico entrante mediante puntos de conexión privados

Además de la [configuración de red virtual de los requisitos previos generales](#prerequisites), debe tener un flujo de trabajo de aplicación lógica basado en inquilino único nuevo o existente que comience con un desencadenador integrado que pueda recibir solicitudes.

Por ejemplo, el desencadenador Request crea un punto de conexión en el flujo de trabajo que puede recibir y controlar las solicitudes entrantes de otros autores de llamadas, incluidos los flujos de trabajo. Este punto de conexión proporciona una dirección URL que puede usar para llamar al flujo de trabajo y desencadenarlo. En este ejemplo, los pasos continúan con el desencadenador Request.

Para más información, revise la siguiente documentación:

- [Creación de flujos de trabajo de aplicación lógica de inquilino único de Azure Logic Apps](create-single-tenant-workflows-azure-portal.md)

- [Recepción y respuesta de solicitudes HTTP entrantes con Azure Logic Apps](../connectors/connectors-native-reqres.md)

### <a name="create-the-workflow"></a>Crear el flujo de trabajo

1. Si aún no lo ha hecho, cree una aplicación lógica basada en inquilino único y un flujo de trabajo en blanco.

1. Una vez que se abra el diseñador, agregue el desencadenador Request como primer paso del flujo de trabajo.

   > [!NOTE]
   > Solo se puede llamar a desencadenadores Request y desencadenadores Webhook desde dentro de la red virtual. Las acciones y desencadenadores de webhook de API administradas no funcionarán porque requieren un punto de conexión público para recibir llamadas. 

1. En función de los requisitos del escenario, agregue otras acciones que quiera ejecutar en el flujo de trabajo.

1. Cuando haya terminado, guarde el flujo de trabajo.

Para más información, consulte [Creación de flujos de trabajo de aplicación lógica de inquilino único de Azure Logic Apps](create-single-tenant-workflows-azure-portal.md).

#### <a name="copy-the-endpoint-url"></a>Copia de la dirección URL del punto de conexión

1. En el menú del flujo de trabajo, seleccione **Información general**.

1. En la página **Información general**, copie y guarde la **dirección URL del flujo de trabajo** para su uso posterior.

   Para desencadenar el flujo de trabajo, llame o envíe una solicitud a esta dirección URL.

1. Asegúrese de que la dirección URL funciona; para ello, llame a o envíe una solicitud a la dirección URL. Puede usar la herramienta que desee para enviar la solicitud; por ejemplo, Postman.

### <a name="set-up-private-endpoint-connection"></a>Configuración de la conexión del punto de conexión privado

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Redes**.

1. En la página **Redes**, en **Conexiones de punto de conexión privado**, seleccione **Configurar las conexiones de punto de conexión privado**.

1. En la página **Conexiones de punto de conexión privado**, seleccione **Agregar**.

1. En el panel **Agregar punto de conexión privado** que se abre, proporcione la información solicitada sobre el punto de conexión.

   Para más información, consulte [Propiedades del punto de conexión privado](../private-link/private-endpoint-overview.md#private-endpoint-properties).

1. Después de que Azure aprovisione correctamente el punto de conexión privado, vuelva a intentar llamar a la dirección URL del flujo de trabajo.

   Esta vez, se produce un error `403 Forbidden` esperado, lo que significa que el punto de conexión privado está configurado y funciona correctamente.

1. Para asegurarse de que la conexión funciona correctamente, cree una máquina virtual en la misma red virtual que tiene el punto de conexión privado e intente llamar al flujo de trabajo de la aplicación lógica.

### <a name="considerations-for-inbound-traffic-through-private-endpoints"></a>Consideraciones para el tráfico entrante mediante puntos de conexión privados

- Si se accede desde fuera de la red virtual, la vista de supervisión no puede acceder a las entradas y salidas de desencadenadores y acciones.

- La implementación desde Visual Studio Code o la CLI de Azure solo funciona desde dentro de la red virtual. Puede usar el Centro de implementación para vincular la aplicación lógica a un repositorio de GitHub. A continuación, puede usar la infraestructura de Azure para compilar e implementar el código.

  Para que la integración de GitHub funcione, quite la configuración `WEBSITE_RUN_FROM_PACKAGE` de la aplicación lógica o establezca el valor en `0`.

- La habilitación de Private Link no afecta al tráfico saliente, que sigue fluyendo por la infraestructura de App Service.

<a name="set-up-outbound"></a>

## <a name="set-up-outbound-traffic-through-private-endpoints"></a>Configuración del tráfico saliente mediante puntos de conexión privados

Para proteger el tráfico saliente de la aplicación lógica, puede integrar la aplicación lógica con una red virtual. De manera predeterminada, el tráfico saliente de la aplicación lógica solo se ve afectado por los grupos de seguridad de red (NSG) y las rutas definidas por el usuario (UDR) cuando se dirige a una dirección privada, como `10.0.0.0/8`, `172.16.0.0/12` y `192.168.0.0/16`.

Si usa su propio servidor de nombres de dominio (DNS) con la red virtual, establezca la configuración de la aplicación `WEBSITE_DNS_SERVER` del recurso de aplicación lógica en la dirección IP del DNS. Si tiene un DNS secundario, agregue otra configuración de aplicación denominada `WEBSITE_DNS_ALT_SERVER` y establezca el valor también en la dirección IP del DNS. Además, actualice los registros DNS para que remitan los puntos de conexión privados a la dirección IP interna. Los puntos de conexión privados funcionan mediante el envío de la búsqueda de DNS a la dirección privada, no a la dirección pública del recurso específico. Para más información, vea [Puntos de conexión privados: Integración de su aplicación con una instancia de Azure Virtual Network](../app-service/overview-vnet-integration.md#private-endpoints).

> [!IMPORTANT]
> Para que el entorno de ejecución de Azure Logic Apps funcione, debe tener una conexión ininterrumpida con el almacenamiento de back-end. Para que los conectores administrados hospedados en Azure funcionen, debe tener una conexión ininterrumpida con el servicio de API administrada.

### <a name="considerations-for-outbound-traffic-through-private-endpoints"></a>Consideraciones sobre el tráfico saliente mediante puntos de conexión privados

La configuración de la integración de red virtual solo afecta al tráfico saliente. Para proteger el tráfico entrante, que sigue utilizando el punto de conexión compartido de App Service, vea [Configuración del tráfico entrante mediante puntos de conexión privados](#set-up-inbound).

Para más información, revise la siguiente documentación:

- [Integración de su aplicación con una instancia de Azure Virtual Network](../app-service/overview-vnet-integration.md)

- [Grupos de seguridad de red](../virtual-network/network-security-groups-overview.md)

- [Enrutamiento del tráfico de redes virtuales](../virtual-network/virtual-networks-udr-overview.md)

## <a name="connect-to-storage-account-with-private-endpoints"></a>Conexión a la cuenta de almacenamiento con puntos de conexión privados

Puede restringir el acceso a la cuenta de almacenamiento para que solo se puedan conectar los recursos de una red virtual. Azure Storage admite la adición de puntos de conexión privados a la cuenta de almacenamiento. Los flujos de trabajo de la aplicación lógica pueden usar estos puntos de conexión para comunicarse con la cuenta de almacenamiento. Para más información, vea [Uso de puntos de conexión privados para Azure Storage](../storage/common/storage-private-endpoints.md).

> [!NOTE]
> Los pasos siguientes requieren habilitar temporalmente el acceso público en la cuenta de almacenamiento. Si no puede habilitar el acceso público debido a las directivas de su organización, puede implementar la aplicación lógica con una cuenta de almacenamiento privada. Sin embargo, debe usar una plantilla de Azure Resource Manager (plantilla de ARM) para la implementación. Para obtener una plantilla de ARM de ejemplo, vea [Implementación de una aplicación lógica mediante una cuenta de almacenamiento protegida con puntos de conexión privados](https://github.com/VeeraMS/LogicApp-deployment-with-Secure-Storage).

1. Cree distintos puntos de conexión privados para cada uno de los servicios Table, Queue, Blob y File Storage.

1. Habilite el acceso público temporal en la cuenta de almacenamiento al implementar la aplicación lógica.

   1. En [Azure Portal](https://portal.azure.com), abra su recurso de cuenta de almacenamiento.

   1. En el menú del recurso de cuenta de almacenamiento, en **Seguridad y redes**, seleccione **Redes**.

   1. En el panel **Redes** de la pestaña **Firewalls y redes virtuales**, en **Permitir acceso desde**, seleccione **Todas las redes**.

1. Implemente el recurso de la aplicación lógica mediante Azure Portal o Visual Studio Code.

1. Una vez finalizada la implementación, habilite la integración entre la aplicación lógica y los puntos de conexión privados en la red virtual o subred que se conecta a la cuenta de almacenamiento.

   1. En [Azure Portal](https://portal.azure.com), abra el recurso de aplicación lógica.

   1. En el menú del recurso de aplicación lógica, en **Configuración**, seleccione **Redes**.

   1. Configure las conexiones necesarias entre la aplicación lógica y las direcciones IP de los puntos de conexión privados.

   1. Para acceder a los datos de flujo de trabajo de la aplicación lógica a través de la red virtual, en la configuración del recurso de la aplicación lógica, establezca la configuración `WEBSITE_CONTENTOVERVNET` en `1`.

   Si usa su propio servidor de nombres de dominio (DNS) con la red virtual, establezca la configuración de la aplicación `WEBSITE_DNS_SERVER` del recurso de aplicación lógica en la dirección IP del DNS. Si tiene un DNS secundario, agregue otra configuración de aplicación denominada `WEBSITE_DNS_ALT_SERVER` y establezca el valor también en la dirección IP del DNS. Además, actualice los registros DNS para que remitan los puntos de conexión privados a la dirección IP interna. Los puntos de conexión privados funcionan mediante el envío de la búsqueda de DNS a la dirección privada, no a la dirección pública del recurso específico. Para más información, vea [Puntos de conexión privados: Integración de su aplicación con una instancia de Azure Virtual Network](../app-service/overview-vnet-integration.md#private-endpoints).

1. Después de aplicar esta configuración de aplicación, puede eliminar el acceso público de la cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

- [Logic Apps en cualquier ubicación: posibilidades de redes con Logic Apps (inquilino único)](https://techcommunity.microsoft.com/t5/integrations-on-azure/logic-apps-anywhere-networking-possibilities-with-logic-app/ba-p/2105047)