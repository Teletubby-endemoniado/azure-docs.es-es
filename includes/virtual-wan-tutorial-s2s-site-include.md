---
ms.author: cherylmc
author: cherylmc
ms.date: 08/17/2021
ms.service: virtual-wan
ms.topic: include
ms.openlocfilehash: 5f52297778ec6dc30e96aeac00cd8751b6b39582
ms.sourcegitcommit: d43193fce3838215b19a54e06a4c0db3eda65d45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2021
ms.locfileid: "122638246"
---
1. Vaya a su **Virtual WAN* -> Sitios VPN** para abrir la página **Sitios VPN**.
1. En la página **Sitios de VPN**, haga clic en **+Crear sitio**.
1. Dentro de la página **Crear un sitio VPN**, en la pestaña **Aspectos básicos** rellene los campos siguientes:

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/site-basics.png" alt-text="La captura de pantalla muestra la página Crear un sitio VPN con la pestaña Aspectos básicos abierta." lightbox="./media/virtual-wan-tutorial-site-include/site-basics.png":::

    * **Región**: anteriormente se conocía como "ubicación". Se trata de la ubicación en la que desea crear este recurso de sitio.
    * **Nombre**: el nombre que quiera usar para hacer referencia a su sitio local.
    * **Proveedor del dispositivo**: nombre del proveedor del dispositivo VPN (por ejemplo: Citrix, Cisco o Barracuda). Agregar el proveedor del dispositivo puede ayudar al equipo de Azure a comprender mejor su entorno para agregar posibilidades de optimización adicionales en el futuro, así como para ayudarle a solucionar problemas.
    * **Espacio de direcciones privadas**: es el espacio de direcciones IP que se encuentra en el sitio local. El tráfico destinado a este espacio de direcciones se enruta al sitio local. Esto es necesario cuando no se habilita BGP para el sitio.
    
      >[!NOTE]
      >Si edita el espacio de direcciones después de crear el sitio (por ejemplo, agrega un espacio de direcciones adicional), puede tardar entre 8 y 10 minutos en actualizar las rutas efectivas mientras se recrean los componentes.
      >
1. Seleccione **Vínculos** para agregar información sobre los vínculos físicos en la rama. Si tiene un dispositivo CPE de asociado de Virtual WAN, consulte con su asociado para ver si esta información se intercambia con Azure como parte de la configuración de carga de información de la sucursal desde sus sistemas.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/links.png" alt-text="La captura de pantalla muestra la página Crear un sitio VPN con la pestaña Vínculos abierta." lightbox="./media/virtual-wan-tutorial-site-include/links.png":::

   * **Nombre del vínculo**: el nombre que quiera proporcionar para el vínculo físico en el sitio VPN. Ejemplo: mylink1.
   * **Velocidad del vínculo**: esta es la velocidad del dispositivo VPN en la ubicación de la rama. Ejemplo: 50, que significa que 50 Mbps es la velocidad del dispositivo VPN en el sitio de la rama.
   * **Nombre del proveedor del vínculo**: nombre del vínculo físico en el sitio VPN. Ejemplo: ATT, Verizon.
   * **Dirección IP/nombre de dominio completo del vínculo**: dirección IP pública del dispositivo local que usa este vínculo. Opcionalmente, puede proporcionar la dirección IP privada del dispositivo VPN local que está detrás de ExpressRoute. Puede incluir también un nombre de dominio completo. Por ejemplo, *algo.contoso.com*. El nombre de dominio completo debe poder resolverse desde la puerta de enlace de VPN. Esto es posible si se puede acceder al servidor DNS que hospeda este nombre de dominio completo a través de Internet. La dirección IP tiene prioridad cuando se especifica la dirección IP y el nombre de dominio completo.

     >[!NOTE]
     >
     >* Admite una dirección IPv4 por cada nombre de dominio completo. Si el nombre de dominio completo se va a resolver en varias direcciones IP, la puerta de enlace de VPN recoge la primera dirección IP4 de la lista. En este momento, no se admiten direcciones IPv6.
     >
     >* VPN Gateway mantiene una caché DNS que se actualiza cada 5 minutos. La puerta de enlace intenta resolver los nombres de dominio completos solo para los túneles desconectados. Un restablecimiento de la puerta de enlace o un cambio de configuración también pueden desencadenar la resolución de un nombre de dominio completo.
     >
   * **Link Border Gateway Protocol** (Protocolo de puerta de enlace de borde del vínculo): la configuración de BGP en un vínculo de Virtual WAN es equivalente a la configuración de BGP en una VPN de puerta de enlace de red virtual de Azure. Su dirección del par BGP local no puede ser la misma que la dirección IP pública del dispositivo VPN, ni que el espacio de direcciones de red virtual del sitio VPN. Use otra dirección IP en el dispositivo VPN para la dirección IP del par BGP. Puede ser una dirección asignada a la interfaz de bucle invertido en el dispositivo. Especifique esta dirección en el sitio VPN correspondiente que representa la ubicación.  Para conocer los requisitos previos de BGP, consulte [Acerca de BGP con Azure VPN Gateway](../articles/vpn-gateway/vpn-gateway-bgp-overview.md). Siempre tiene la opción de editar una conexión de vínculo VPN para actualizar sus parámetros BGP (IP de emparejamiento en el vínculo y el número de espacio de direcciones).
1. Puede agregar o eliminar más vínculos. Se admiten cuatro vínculos por cada sitio VPN. Por ejemplo, si tiene cuatro ISP (proveedor de servicios de Internet) en la ubicación de la sucursal, puede crear cuatro vínculos, uno por cada ISP, y proporcionar la información de cada vínculo.
1. Una vez que haya terminado de rellenar los campos, seleccione **Revisar y crear** para verificarlo. Haga clic en **Crear** para crear el sitio.
1. En la página **Sitios VPN**, haga clic en **Hub association: Connected to this hub (Asociación del centro de conectividad: conectado a este centro de conectividad)** para borrar el filtro.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/connect.png" alt-text="La captura de pantalla muestra Conectarse a este centro." lightbox="./media/virtual-wan-tutorial-site-include/connect.png":::
1. Una vez que se haya borrado el filtro, puede ver el sitio.

   :::image type="content" source="./media/virtual-wan-tutorial-site-include/sites.png" alt-text="Captura de pantalla en la que se muestra el sitio.":::
