---
title: 'Inicio rápido: Creación y configuración de una instancia de Route Server mediante Azure Portal'
description: En esta guía de inicio rápido aprenderá a crear y configurar una instancia de Route Server mediante Azure Portal.
services: route-server
author: duongau
ms.service: route-server
ms.topic: quickstart
ms.date: 09/08/2021
ms.author: duau
ms.openlocfilehash: 7dfded652c14ec6805b7fa48004cf5dfe8d72b5f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131061112"
---
# <a name="quickstart-create-and-configure-route-server-using-the-azure-portal"></a>Inicio rápido: Creación y configuración de una instancia de Route Server mediante Azure Portal

Este artículo le ayuda a configurar Azure Route Server para su emparejamiento con una aplicación virtual de red (NVA) en la red virtual mediante Azure Portal. Azure Route Server aprenderá las rutas de la NVA y las programará en las máquinas virtuales de la red virtual. Azure Route Server también anunciará las rutas de la red virtual a la NVA. Para más información, consulte [Azure Route Server](overview.md).

:::image type="content" source="media/quickstart-configure-route-server-portal/environment-diagram.png" alt-text="Diagrama del entorno de implementación de Route Server mediante Azure Portal." border="false":::

> [!IMPORTANT]
> Si tiene un Azure Route Server creado antes del 1 de septiembre y no tiene una dirección IP pública asociada, deberá volver a crear el Route Server para que pueda obtener una dirección IP con fines de administración.

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Consulte [Límites de servicio de Azure Route Server](route-server-faq.md#limitations).

## <a name="create-a-route-server"></a>Creación de una instancia de Route Server

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Inicie sesión en la cuenta de Azure y seleccione su suscripción.

Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.

### <a name="create-a-route-server"></a>Creación de una instancia de Route Server

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione **Servidor de rutas**.

1. Seleccione **+ Create new route server** (+ Crear un nuevo servidor de rutas).

    :::image type="content" source="./media/quickstart-configure-route-server-portal/route-server-landing-page.png" alt-text="Captura de pantalla de la página de aterrizaje de Route Server."::: 

1. En la página **Create a Route Server** (Crear un servidor de rutas), escriba o seleccione la información necesaria.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/create-route-server-page.png" alt-text="Captura de pantalla de la página para crear un servidor de rutas.":::     

    | Configuración | Value |
    |----------|-------|
    | Subscription | Seleccione la suscripción de Azure que desea usar para implementar el servidor de rutas. |
    | Grupo de recursos | Seleccione un grupo de recursos en el que crear el servidor de rutas. Si no tiene un grupo de recursos existente, puede crear uno. |
    | Nombre | Escriba un nombre para el servidor de rutas. |
    | Region | Seleccione la región en la que se creará el servidor de rutas. Seleccione la misma región de la red virtual que creó anteriormente para ver la red virtual en la lista desplegable. |
    | Virtual Network | Seleccione la red virtual en la que se creará el servidor de rutas. Puede crear una red virtual o usar una existente. Si usa una red virtual existente, asegúrese de que la red virtual existente tenga suficiente espacio para un mínimo de una subred /27 para cumplir con los requisitos de subred del servidor de rutas. Si no ve la red virtual en la lista desplegable, asegúrese de que ha seleccionado el grupo de recursos y la región correctos. |
    | Subnet | Una vez que cree o seleccione una red virtual, aparecerá el campo subred. Esta subred está dedicada únicamente a Route Server. Seleccione **Manage subnet configuration** (Administrar configuración de subred) y cree la subred de Azure Route Server. Seleccione **+ Subnet** (+ Subred) y cree una subred con las siguientes directrices:</br><br>- La subred debe tener el nombre *RouteServerSubnet*.</br><br>- La subred debe ser un mínimo de /27 o mayor.</br> |
    | Dirección IP pública | Cree un recurso de dirección IP pública estándar o seleccione uno existente para asignarlo a Route Server. Para garantizar la conectividad con el servicio back-end que administra la configuración de Route Server, se requiere una dirección IP pública. |

1. Seleccione **Review + create** (Revisar y crear), revise el resumen y, después, seleccione **Create** (Crear). 

    > [!NOTE]
    > La implementación del servidor de rutas tardará unos 20 minutos.

## <a name="set-up-peering-with-nva"></a>Configuración del emparejamiento con la NVA

La sección le ayudará a configurar el emparejamiento de BGP con la NVA.

1. Vaya a [Route Server](./overview.md) en Azure Portal y seleccione el servidor de rutas que desea configurar.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/select-route-server.png" alt-text="Captura de pantalla de la lista de servidores de rutas."::: 

1. Seleccione **Peers** (Emparejamientos) en *Settings* (Configuración) en el panel de navegación izquierdo. Seleccione **+ Add** (+ Agregar) para agregar un nuevo emparejamiento.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/peers-landing-page.png" alt-text="Captura de pantalla de la página de aterrizaje de emparejamientos."::: 

1. Especifique la siguiente información sobre el emparejamiento de NVA.

    :::image type="content" source="./media/quickstart-configure-route-server-portal/add-peer-page.png" alt-text="Captura de pantalla de la página para agregar un emparejamiento.":::

    | Configuración | Value |
    |----------|-------|
    | Nombre | Asigne un nombre al emparejamiento entre el servidor de rutas y la NVA. |
    | ASN |  Escriba el número de sistema autónomo (ASN) de la NVA. |
    | Dirección IPv4 | Escriba la dirección IP de la NVA con la que se comunicará Route Server para establecer BGP. |

1. Seleccione **Add** (Agregar) para agregar este emparejamiento.

## <a name="complete-the-configuration-on-the-nva"></a>Finalización de la configuración en la NVA

Necesitará el ASN y las direcciones IP de emparejamiento de Azure Route Server para completar la configuración en la NVA para establecer una sesión de BGP. Puede obtener esta información en la página de información general del servidor de rutas.

:::image type="content" source="./media/quickstart-configure-route-server-portal/route-server-overview.png" alt-text="Captura de pantalla de la página de información general del servidor de rutas.":::

## <a name="configure-route-exchange"></a>Configuración del intercambio de rutas

Si tiene una puerta de enlace de ExpressRoute y una instancia de VPN Gateway y quiere que intercambien rutas con el servidor de rutas, puede habilitar el intercambio de rutas.

1. Vaya a [Route Server](./overview.md) en Azure Portal y seleccione el servidor de rutas que desea configurar.

1. Seleccione **Configuration** (Configuración) en *Settings* (Configuración) en el panel de navegación izquierdo.

1. Seleccione **Enable** (Habilitar) para la opción **Branch-to-Branch** (De rama a rama) y, a continuación, seleccione **Save** (Guardar).

    :::image type="content" source="./media/quickstart-configure-route-server-portal/enable-route-exchange.png" alt-text="Captura de pantalla que muestra cómo habilitar el intercambio de rutas.":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita la instancia de Azure Route Server, seleccione **Delete** (Eliminar) en la página de información general para desaprovisionar el servidor de rutas.

:::image type="content" source="./media/quickstart-configure-route-server-portal/delete-route-server.png" alt-text="Captura de pantalla que muestra cómo eliminar el servidor de rutas.":::

## <a name="next-steps"></a>Pasos siguientes

Después de crear la instancia de Azure Route Server, continúe para obtener información sobre cómo interactúa Azure Route Server con las puertas de enlace de VPN y ExpressRoute: 

> [!div class="nextstepaction"]
> [Compatibilidad de Azure Route Server (versión preliminar) con ExpressRoute y VPN de Azure](expressroute-vpn-support.md)