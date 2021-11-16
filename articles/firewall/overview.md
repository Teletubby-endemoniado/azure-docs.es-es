---
title: ¿Qué es Azure Firewall?
description: Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc, contperf-fy21q1
ms.date: 11/10/2021
ms.author: victorh
ms.openlocfilehash: 54f5de052c6fed4729a41e9f54c614480efc1e7e
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132309930"
---
# <a name="what-is-azure-firewall"></a>¿Qué es Azure Firewall?

<!--- ![ICSA certification](media/overview/icsa-cert-firewall-small.png) --->

Azure Firewall es un servicio de seguridad de firewall de red inteligente y nativo de la nube que le proporciona la mejor protección contra amenazas para las cargas de trabajo en la nube que se ejecutan en Azure. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad en la nube sin restricciones. Asimismo, proporciona la opción de realizar la inspección del tráfico de este a oeste y de norte a sur.

Azure Firewall se ofrece en dos SKU: Estándar y Premium.

## <a name="azure-firewall-standard"></a>Azure Firewall Estándar

   Azure Firewall Estándar proporciona fuentes de inteligencia sobre amenazas y filtrado L3-L7 directamente desde Microsoft Cyber Security. El filtrado basado en la inteligencia sobre amenazas puede enviar alertas y denegar el tráfico desde o hacia dominios y direcciones IP malintencionados, que sean conocidos y que se actualicen en tiempo real para protegerle frente a ataques nuevos y emergentes.

   ![Introducción al estándar de firewall](media/overview/firewall-standard.png)

Para obtener información sobre las características estándar del firewall, consulte las [características estándar de Azure Firewall](features.md).


## <a name="azure-firewall-premium"></a>Azure Firewall Prémium

   Azure Firewall Premium proporciona funcionalidades avanzadas que incluyen IDPS basados en firmas para permitir la detección rápida de ataques mediante la búsqueda de patrones específicos. Estos patrones pueden incluir secuencias de bytes en el tráfico de red o secuencias de instrucciones maliciosas conocidas y que use el malware. Existen más de 58 000 firmas en más de 50 categorías que se actualizan en tiempo real para protegerle contra vulnerabilidades nuevas y emergentes. Las categorías de vulnerabilidades de seguridad incluyen malware, suplantación de identidad, minería de monedas y ataques de troyanos.

   ![Información general del firewall Premium](media/overview/firewall-premium.png)


Para obtener más información sobre las características del firewall Premium, consulte [Características de Azure Firewall Premium](premium-features.md).


## <a name="azure-firewall-manager"></a>Azure Firewall Manager

Puede usar Azure Firewall Manager para administrar de forma centralizada las instancias de Azure Firewall en varias suscripciones. Firewall Manager aprovecha la directiva de firewall para aplicar un conjunto común de reglas de red o aplicaciones y la configuración a los firewalls del inquilino.
 
Firewall Manager admite firewalls en entornos de redes virtuales y redes de Virtual WAN (Centro virtual seguro). Los centros virtuales usan la solución de automatización de rutas de Virtual WAN para simplificar el enrutamiento del tráfico al firewall con unos pocos clics.

Para obtener más información sobre Azure Firewall Manager, consulte [Azure Firewall Manager](../firewall-manager/overview.md).

## <a name="pricing-and-sla"></a>Precios y contrato de nivel de servicio

Para obtener información de los precios de Azure Firewall, consulte [Precios de Azure Firewall](https://azure.microsoft.com/pricing/details/azure-firewall/).

Para obtener información del Acuerdo de Nivel de Servicio de Azure Firewall, consulte [SLA para Azure Firewall](https://azure.microsoft.com/support/legal/sla/azure-firewall/).

## <a name="whats-new"></a>Novedades

Para obtener información sobre las novedades de Azure Firewall, consulte [Actualizaciones de Azure](https://azure.microsoft.com/updates/?category=networking&query=Azure%20Firewall).


## <a name="known-issues"></a>Problemas conocidos

Azure Firewall presenta los siguientes problemas conocidos:

|Incidencia  |Descripción  |Mitigación  |
|---------|---------|---------|
|Las reglas de filtrado de red para protocolos que no son TCP/UDP (por ejemplo, ICMP) no funcionan con el tráfico enlazado a Internet|Las reglas de filtrado de red de protocolos que no son TCP/UDP no funcionan con la traducción SNAT a la dirección IP pública. Los protocolos que no son TCP/UDP no se admiten entre subredes de radio y redes virtuales.|Azure Firewall usa Standard Load Balancer, [que actualmente no admite SNAT para los protocolos IP](../load-balancer/load-balancer-overview.md). Se están examinando opciones para admitir este escenario en una versión futura.|
|Falta de compatibilidad entre PowerShell y CLI con ICMP|Azure PowerShell y la CLI no admiten ICMP como protocolo válido en las reglas de red.|Aun así se puede usar ICMP como protocolo a través del portal y la API REST. Estamos trabajando para agregar pronto ICMP a PowerShell y la CLI.|
|Las etiquetas FQDN requieren que se establezca una combinación protocolo: puerto|Las reglas de aplicaciones con las etiquetas FQDN requieren la definición de puerto: protocolo.|Puede usar **https** como valor de puerto: protocolo. Estamos trabajando para que este campo sea opcional cuando se usen etiquetas FQDN.|
|No se admite la posibilidad de mover un firewall a otro grupo de recursos o suscripción.|No se admite la posibilidad de mover un firewall a otro grupo de recursos o suscripción.|La compatibilidad con esta funcionalidad está en nuestro mapa de ruta. Para mover un firewall a otro un grupo de recursos o suscripción, debe eliminar la instancia actual y volver a crearla en el nuevo grupo de recursos o suscripción.|
|Las alertas de inteligencia sobre amenazas pueden estar enmascaradas|Reglas de red con destino 80/443 para las alertas de inteligencia sobre amenazas de las máscaras de filtrado de salida cuando la configuración está establecida en el modo de solo alerta.|Cree filtrado de salida para 80/443 mediante reglas de aplicaciones. También puede cambiar el modo de inteligencia sobre amenazas a **Alertar y denegar**.|
|DNAT de Azure Firewall no funciona para destinos de IP privadas|La compatibilidad con DNAT de Azure Firewall se limita a la entrada/salida de Internet. DNAT no funciona actualmente con destinos de IP privada. Por ejemplo, radio a radio.|Se trata de una limitación actual.|
|No se puede eliminar la primera configuración de IP pública|Cada IP pública de Azure Firewall se asigna a una *configuración de IP*.  La primera configuración de IP se asigna durante la implementación del firewall y, normalmente, también contiene una referencia a la subred del firewall (a menos que se configure explícitamente de forma diferente mediante una implementación de plantilla). No se puede eliminar esta configuración de IP porque podría anular la asignación del firewall. Aún podrá cambiar o eliminar la dirección IP pública asociada con esta configuración de IP si el firewall tiene al menos otra dirección IP pública disponible para su uso.|es así por diseño.|
|Availability Zones solo se puede configurar durante la implementación.|Availability Zones solo se puede configurar durante la implementación. No se puede configurar una vez implementado un firewall.|es así por diseño.|
|SNAT en conexiones entrantes|Además de DNAT, en las conexiones mediante la dirección IP pública del firewall (entrante) se aplica SNAT en una de las direcciones IP privadas del firewall. Este requisito hoy en día (también para aplicaciones virtuales de red activa/activa) es para garantizar el enrutamiento simétrico.|Para conservar el código fuente original para HTTP/S, considere el uso de encabezados [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For). Por ejemplo, use un servicio como [Azure Front Door](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) o [Azure Application Gateway](../application-gateway/rewrite-http-headers-url.md) delante del firewall. También puede agregar WAF como parte de Azure Front Door Service y la cadena al firewall.
|El filtrado por nombre de dominio completo de SQL se admite solo en modo de proxy (puerto 1433)|Para Azure SQL Database, Azure Synapse Analytics y Azure SQL Managed Instance:<br><br>El filtrado por nombre de dominio completo de SQL se admite solo en modo de proxy (puerto 1433).<br><br>Para IaaS de Azure SQL:<br><br>Si va a usar puertos que no son los estándar, puede especificar esos puertos en las reglas de la aplicación.|Para SQL en modo de redirección (el valor predeterminado si se conecta desde dentro de Azure), puede filtrar en su lugar el acceso mediante la etiqueta de servicio de SQL como parte de las reglas de red de Azure Firewall.
|El tráfico SMTP de salida del puerto TCP 25 está bloqueado|Azure Firewall bloquea los mensajes de correo electrónico salientes que se envían directamente a dominios externos (como `outlook.com` y `gmail.com`) en el puerto TCP 25. Este es el comportamiento predeterminado de la plataforma en Azure. |Use servicios de retransmisión SMTP autenticados, que normalmente se conectan a través del puerto TCP 587, pero también admiten otros puertos.  Para más información, consulte [Solución de problemas de conectividad SMTP saliente en Azure](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Actualmente, Azure Firewall puede comunicarse con IP públicas mediante el puerto TCP 25 de salida, pero no se garantiza que funcione y no se admite en todos los tipos de suscripción. En el caso de las IP privadas, como redes virtuales, VPN y Azure ExpressRoute, Azure Firewall admite una conexión saliente del puerto TCP 25.
|Agotamiento de puertos SNAT|Azure Firewall admite actualmente 1024 puertos por cada dirección IP pública por cada instancia de conjunto de escalado de máquinas virtuales de back-end. De manera predeterminada, hay dos instancias de conjunto de escalado de máquinas virtuales.|Se trata de una limitación de SLB y estamos buscando constantemente oportunidades para aumentar los límites. Mientras tanto, se recomienda configurar las implementaciones de Azure Firewall con un mínimo de cinco direcciones IP públicas para implementaciones susceptibles de agotamiento de SNAT. Esto aumenta cinco veces los puertos SNAT disponibles. Realice la asignación desde un prefijo de dirección IP para simplificar los permisos del flujo descendente.|
|DNAT no es compatible con la tunelización forzada habilitada|Los firewalls implementados con la tunelización forzada habilitada no admiten el acceso entrante desde Internet debido al enrutamiento asimétrico.|Esto es así por diseño. La ruta de acceso de retorno de las conexiones entrantes pasa por el firewall local, que no ha visto la conexión establecida.
|Es posible que el FTP pasivo saliente no funcione en firewalls con varias direcciones IP públicas, en función de la configuración del servidor FTP.|FTP pasivo establece distintas conexiones para los canales de control y datos. Cuando un firewall con varias direcciones IP públicas envía datos de salida, selecciona de manera aleatoria una de sus direcciones IP públicas como dirección IP de origen. FTP puede generar un error cuando los canales de control y de datos usan direcciones IP de origen diferentes, en función de la configuración del servidor FTP.|Está previsto una configuración de SNAT explícita. Entretanto, puede configurar el servidor FTP para que acepte los canales de control y de datos de distintas direcciones IP de origen (vea [un ejemplo de IIS](/iis/configuration/system.applicationhost/sites/sitedefaults/ftpserver/security/datachannelsecurity)). Otra opción es usar una única dirección IP en esta situación.|
|Es posible que el FTP pasivo entrante no funcione en función de la configuración del servidor FTP |FTP pasivo establece distintas conexiones para los canales de control y datos. En las conexiones entrantes de Azure Firewall se aplica SNAT a una de las direcciones IP privadas del firewall para garantizar el enrutamiento simétrico. FTP puede generar un error cuando los canales de control y de datos usan direcciones IP de origen diferentes, en función de la configuración del servidor FTP.|Se está investigando la conservación de la dirección IP de origen original. Entretanto, puede configurar el servidor FTP para que acepte los canales de control y de datos de distintas direcciones IP de origen.|
|FTP activo no funcionará cuando el cliente FTP deba acceder a un servidor FTP mediante Internet.|FTP activo usa un comando PORT del cliente FTP que indica al servidor FTP qué dirección IP y puerto usar para el canal de datos. Este comando PORT usa la dirección IP privada del cliente, que no se puede cambiar. El tráfico del lado cliente que recorre Azure Firewall será tráfico con NAT para las comunicaciones basadas en Internet, por lo que el servidor FTP considera el comando PORT como no válido.|Se trata de una limitación general del FTP activo cuando se usa con NAT del lado del cliente.|
|Falta una dimensión de protocolo en la métrica NetworkRuleHit|La métrica ApplicationRuleHit permite el filtrado basado en el protocolo, pero esta funcionalidad falta en la métrica NetworkRuleHit correspondiente.|Se está investigando una solución.|
|No se admiten las reglas NAT con puertos entre el 64000 y el 65535|Azure Firewall permite cualquier puerto en el rango 1-65535 en reglas de red y de aplicación; sin embargo, las reglas NAT solo admiten los puertos del rango 1-63999.|Se trata de una limitación actual.
|Las actualizaciones de configuración pueden tardar cinco minutos por término medio.|Una actualización de la configuración de Azure Firewall puede tardar entre tres y cinco minutos por término medio, y no se admiten actualizaciones en paralelo.|Se está investigando una solución.|
|Azure Firewall usa encabezados TLS SNI para filtrar el tráfico HTTPS y MSSQL|Si el software del explorador o servidor no admite la extensión Server Name Indicator (SNI), no será posible conectarse mediante Azure Firewall.|Si el software de explorador o servidor no admite SNI, es posible que pueda controlar la conexión mediante una regla de red, en lugar de una regla de aplicación. Consulte [Indicación de nombre de servidor](https://wikipedia.org/wiki/Server_Name_Indication) para conocer el software que admite SNI.|
|DNS personalizado (versión preliminar) no funciona con la tunelización forzada.|Si está habilitada la tunelización forzada, DNS personalizado no funciona.|Se está investigando una solución.|
|El inicio o la detención no funcionan con los firewalls configurados en modo de túnel forzado|El inicio o la detención no funcionan con Azure Firewall configurado en modo de túnel forzado. Si se intenta iniciar Azure Firewall con la tunelización forzada configurada, aparece el siguiente error:<br><br>*Set-AzFirewall: AzureFirewall FW-xx management IP configuration cannot be added to an existing firewall. Redeploy with a management IP configuration if you want to use forced tunneling support.<br>StatusCode: 400<br>ReasonPhrase: Solicitud incorrecta* (Set-AzFirewall: la configuración de la IP de administración de AzureFirewall FW-xx no se puede agregar a un firewall existente. Vuelva a realizar la implementación de desea usar el soporte técnico de la tunelización forzada.
StatusCode: 400
ReasonPhrase: solicitud incorrecta).|Bajo investigación.<br><br>Como solución alternativa, puede eliminar el firewall existente y crear otro con los mismos parámetros.|
|No se pueden agregar etiquetas de directiva de firewall mediante el portal ni mediante plantillas de Azure Resource Manager (ARM)|La directiva de Azure Firewall tiene una limitación de compatibilidad de revisión que impide agregar una etiqueta mediante Azure Portal ni mediante plantillas de ARM. Se genera el siguiente error: *No se pudieron guardar las etiquetas del recurso*.|Se está investigando una solución. O bien, puede usar el cmdlet de Azure PowerShell `Set-AzFirewallPolicy` para actualizar las etiquetas.|
|Actualmente, no se admite IPv6.|Si agrega una dirección IPv6 a una regla, se produce un error en el firewall.|Use solo direcciones IPv4. La compatibilidad con IPv6 está en proceso de investigación.|
|La actualización de varios grupos de IP produce un error de conflicto.|Cuando se actualizan dos o más grupos de IP conectados al mismo firewall, uno de los recursos entra en estado de error.|Es un problema conocido, una limitación. <br><br>Al actualizar un grupo de IP, se desencadena una actualización en todos los firewalls a los que está conectado el grupo de IP. Si se inicia una actualización en un segundo grupo de IP con el firewall aún en estado *Actualizando*, se produce un error en la actualización del grupo de IP.<br><br>Para evitar el error, los grupos de IP conectados al mismo firewall se deben actualizar de uno en uno. Deje tiempo suficiente entre las actualizaciones para que el firewall pueda salir del estado *Actualizando*.|
|No se admite la eliminación de elementos RuleCollectionGroup mediante plantillas de ARM.|No se admite la eliminación de elementos RuleCollectionGroup mediante plantillas de ARM y se produce un error.|No es un operación admitida.|
|La regla DNAT para permitir *cualquiera* (*) permitirá el tráfico SNAT.|Si una regla DNAT permite *cualquiera* (*) como dirección IP de origen, una regla de red implícita coincidirá con el tráfico de red virtual a red virtual, y siempre transmitirá el tráfico mediante SNAT.|Se trata de una limitación actual.|
|No se admite la adición de una regla DNAT a un centro virtual protegido con un proveedor de seguridad.|Esto da como resultado una ruta asincrónica para el tráfico DNAT de vuelta, que va al proveedor de seguridad.|No compatible.|
| Error al crear más de 2000 colecciones de reglas. | El número máximo de colecciones de reglas de NAT/aplicación o red es 2000 (límite de Resource Manager). | Se trata de una limitación actual. |
|No se puede ver el nombre de la regla de red en los registros de Azure Firewall|Los datos de registro de reglas de red de Azure Firewall no muestran el nombre de la regla para el tráfico de red.|Se está investigando una característica para admitir esta opción.|

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Creación de una instancia de Azure Firewall y una directiva de firewall: plantilla de Azure Resource Manager](../firewall-manager/quick-firewall-policy.md)
- [Inicio rápido: Implementación de Azure Firewall con Availability Zones: plantilla de Resource Manager](deploy-template.md)
- [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md)
- [Módulo de Learn: Introducción a Azure Firewall](/learn/modules/introduction-azure-firewall/)
