---
title: Solución de problemas de conectividad de puntos de conexión privados de Azure
description: Instrucciones paso a paso para diagnosticar la conectividad de puntos de conexión privados
services: private-endpoint
documentationcenter: na
author: rdhillon
manager: narayan
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: rdhillon
ms.openlocfilehash: 77295f2d47f0645b8d4610af3834f0b8a64ec7a0
ms.sourcegitcommit: 4abfec23f50a164ab4dd9db446eb778b61e22578
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130064464"
---
# <a name="troubleshoot-azure-private-endpoint-connectivity-problems"></a>Solución de problemas de conectividad de puntos de conexión privados de Azure

En este artículo se proporcionan instrucciones detalladas para validar y diagnosticar la configuración de la conectividad de puntos de conexión privados de Azure.

Un punto de conexión privado de Azure es una interfaz de red que le conecta de forma privada y segura a un servicio de punto de conexión privado. Esta solución le ayuda a proteger sus cargas de trabajo en Azure al proporcionar conectividad privada a los recursos del servicio de Azure desde la red virtual. De este modo, los servicios se incorporan a la red virtual mediante esta solución.

Estos son los escenarios de conectividad que están disponibles con los puntos de conexión privados:

- Red virtual de la misma región
- Redes virtuales emparejadas regionalmente
- Redes virtuales emparejadas globalmente
- Cliente local a través de VPN o circuitos de Azure ExpressRoute

## <a name="diagnose-connectivity-problems"></a>Diagnóstico de problemas de conectividad 

Revise estos pasos para asegurarse de que todas las configuraciones habituales son las esperadas a fin de resolver los problemas de conectividad con la configuración del punto de conexión privado.

1. Para revisar la configuración del punto de conexión privado, examine el recurso.

    a. Vaya al [Centro de Private Link](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/overview).

      ![Centro de Private Link](./media/private-endpoint-tsg/private-link-center.png)

    b. En el panel izquierdo, seleccione **Puntos de conexión privados**.
    
      ![Puntos de conexión privados](./media/private-endpoint-tsg/private-endpoints.png)

    c. Filtre y seleccione el punto de conexión privado que quiera diagnosticar.

    d. Revise la red virtual y la información de DNS.
     - Compruebe que el estado de la conexión es **Aprobado**.
     - Asegúrese de que la máquina virtual tenga conectividad con la red virtual que hospeda los puntos de conexión privados.
     - Compruebe que la información de FQDN (copia) y la dirección IP privada están asignadas.
    
       ![Configuración de red virtual y DNS](./media/private-endpoint-tsg/vnet-dns-configuration.png)
    
1. Use [Azure Monitor](../azure-monitor/overview.md) para ver si los datos fluyen.

    a. En el recurso de punto de conexión privado, seleccione **Supervisión**.
     - Seleccione **Bytes de entrada** o **Bytes de salida**. 
     - Vea si se transmiten los datos al intentar conectarse al punto de conexión privado. Se considera normal un retraso de aproximadamente 10 minutos.
    
       ![Comprobación de la telemetría del punto de conexión privado](./media/private-endpoint-tsg/private-endpoint-monitor.png)

1.  Use la **solución de problemas de conexión de máquinas virtuales** de Azure Network Watcher.

    a. Seleccione la máquina virtual cliente.

    b. Seleccione **Solución de problemas de conexión** y, después, seleccione la pestaña **Conexiones salientes**.
    
      ![Network Watcher: prueba de las conexiones salientes](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c. Seleccione **Usar Network Watcher para el seguimiento detallado de la conexión**.
    
      ![Network Watcher: solución de problemas de conexión](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d. Seleccione **Test by FQDN** (Probar por FQDN).
     - Pegue el FQDN del recurso de punto de conexión privado.
     - Proporcione un puerto. Normalmente, se usa el puerto 443 para Azure Storage o Azure Cosmos DB y el 1336 para SQL.

    e. Seleccione **Probar** y valide los resultados de la prueba.
    
      ![Network Watcher: resultados de la prueba](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
1. La resolución de DNS de los resultados de la prueba debe tener la misma dirección IP privada asignada al punto de conexión privado.

    a. Si la configuración de DNS es incorrecta, siga estos pasos:
     - Si usa una zona privada: 
       - Asegúrese de que la red virtual de la máquina virtual cliente está asociada a la zona privada.
       - Compruebe que existe el registro de la zona DNS privada. Si no existe, créelo.
     - Si usa DNS personalizado:
       - Revise la configuración personalizada de DNS y compruebe que esta sea correcta.
       Para más información, consulte [Introducción al punto de conexión privado: Configuración de DNS](./private-endpoint-overview.md#dns-configuration).

    b. Si se produce un error de conectividad debido a grupos de seguridad de red o rutas definidas por el usuario:
     - Revise las reglas de salida del grupo de seguridad de red y cree las reglas de salida adecuadas para permitir el tráfico.
    
       ![Reglas de salida de NSG](./media/private-endpoint-tsg/nsg-outbound-rules.png)

1. La máquina virtual de origen debe tener la ruta a la dirección IP del punto de conexión privado del próximo salto como InterfaceEndpoints en las rutas efectivas de la NIC. 

    a. Si no puede ver la ruta del punto de conexión privado en la máquina virtual de origen, compruebe los siguientes aspectos: 
     - La máquina virtual de origen y el punto de conexión privado forman parte de la misma red virtual. En caso afirmativo, deberá ponerse en contacto con el servicio de soporte técnico. 
     - La máquina virtual de origen y el punto de conexión privado forman parte de diferentes redes virtuales que se emparejan directamente entre sí. En caso afirmativo, deberá ponerse en contacto con el servicio de soporte técnico.
     - La máquina virtual de origen y el punto de conexión privado forman parte de redes virtuales diferentes que no se emparejan directamente entre sí; en este caso, compruebe la conectividad IP entre las redes virtuales.

1. Si la conexión tiene resultados validados, el problema de conectividad puede estar relacionado con otros aspectos, como secretos, tokens o contraseñas en el nivel de aplicación.
   - En este caso, revise la configuración del recurso de Private Link asociado al punto de conexión privado. Para más información, consulte la [guía de solución de problemas de Azure Private Link](troubleshoot-private-link-connectivity.md).
   
1. Siempre es conveniente acotar el problema antes de generar la incidencia de soporte técnico. 

    a. Si el origen es local y tiene problemas para conectarse localmente a un punto de conexión privado en Azure, intente estas conexiones: 
      - A otra máquina virtual del entorno local; compruebe si tiene conectividad IP con la red virtual desde el entorno local. 
      - Desde una máquina virtual de la red virtual al punto de conexión privado.
      
    b. Si el origen es Azure y el punto de conexión privado está en otra red virtual, intente estas conexiones: 
      - Al punto de conexión privado desde un origen diferente. De este modo, puede aislar cualquier problema específico de la máquina virtual. 
      - A cualquier máquina virtual que forme parte de la misma red virtual del punto de conexión privado.  

1. Si el punto de conexión privado está vinculado a un [servicio Private Link](./troubleshoot-private-link-connectivity.md) que está vinculado a una instancia de Load Balancer, compruebe si el grupo de back-end está informando de que el estado es correcto. La corrección del estado de Load Balancer resolverá el problema con la conexión al punto de conexión privado.

    - Para ver un diagrama visual o una [vista de dependencia](../azure-monitor/insights/network-insights-overview.md#dependency-view) de los recursos, las métricas y la información relacionados, vaya a:
        - Azure Monitor
        - Redes
        - Puntos de conexión privados
        - Vista de dependencias 

![Supervisión de redes](https://user-images.githubusercontent.com/20302679/134994620-0660b9e2-e2a3-4233-8953-d3e49b93e2f2.png)

![DependencyView](https://user-images.githubusercontent.com/20302679/134994637-fb8b4a1a-81d5-4723-b1c3-d7bdc72162f3.png)

9. Póngase en contacto con el equipo de [soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) si el problema sigue sin resolverse y el problema de conectividad persiste.

## <a name="next-steps"></a>Pasos siguientes

 * [Creación de un punto de conexión privado en la subred actualizada (Azure Portal)](./create-private-endpoint-portal.md)
 * [Guía de solución de problemas de Azure Private Link](troubleshoot-private-link-connectivity.md)
