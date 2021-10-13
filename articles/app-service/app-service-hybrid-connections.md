---
title: Conexiones híbridas
description: Aprenda a crear y usar conexiones híbridas en Azure App Service para acceder a recursos de redes distintas.
author: madsd
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.topic: article
ms.date: 05/05/2021
ms.author: madsd
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: 6ebfa0cb7e65ce09178e3b468a1bf766a0379595
ms.sourcegitcommit: f29615c9b16e46f5c7fdcd498c7f1b22f626c985
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2021
ms.locfileid: "129425835"
---
# <a name="azure-app-service-hybrid-connections"></a>Hybrid Connections de Azure App Service

Conexiones híbridas es tanto un servicio de Azure como una característica de Azure App Service. Como servicio tiene un uso y unas funcionalidades que van más allá de aquellas que se usan en App Service. Para más información sobre Conexiones híbridas y su uso fuera de App Service, consulte [Protocolo de Conexiones híbridas de Azure Relay][HCService].

En App Service, las Conexiones híbridas se pueden usar para acceder a recursos de aplicaciones en cualquier red que pueda realizar llamadas salientes a Azure a través del puerto 443. Las conexiones híbridas proporcionan acceso desde la aplicación a un punto de conexión TCP y no habilitan ninguna nueva forma de acceder a la aplicación. Dado que se utiliza en App Service, cada conexión híbrida se correlaciona con una combinación única de host y puerto TCP. Esto permite a las aplicaciones acceder a los recursos de cualquier sistema operativo, siempre que se trate de un punto de conexión TCP. La característica de conexiones híbridas no sabe ni le importa lo que es el protocolo de aplicación ni a qué se accede. Simplemente ofrece acceso a la red.  

## <a name="how-it-works"></a>Funcionamiento ##
Las Conexiones híbridas requieren la implementación de un agente de retransmisión desde donde se pueda acceder al punto de conexión deseado y a Azure. El agente de retransmisión, el Administrador de conexiones híbridas (HCM), llama a Azure Relay a través del puerto 443. Desde el sitio de la aplicación web, la infraestructura de App Service también se conecta a Azure Relay en nombre de la aplicación. Mediante las conexiones unidas, la aplicación puede acceder al punto de conexión deseado. La conexión usa TLS 1.2 para la seguridad y las claves de firma de acceso compartido (SAS) para la autenticación y la autorización.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-connectiondiagram.png" alt-text="Diagrama del flujo de alto nivel de conexión híbrida":::

Cuando la aplicación realiza una solicitud DNS que coincide con un punto de conexión híbrida configurado, el tráfico TCP saliente se redirige por la conexión híbrida,  

> [!NOTE]
> lo que significa que debe intentar usar siempre un nombre DNS para la conexión híbrida. Cierto software cliente no lleva a cabo una búsqueda de DNS si el punto de conexión utiliza una dirección IP. 
>

### <a name="app-service-hybrid-connection-benefits"></a>Beneficios de usar las conexiones híbridas de App Service ###

El uso de la funcionalidad de conexiones híbridas reporta varios beneficios, entre los que se cuentan los siguientes:

- Las aplicaciones pueden acceder de forma segura a los servicios y sistemas locales.
- La característica no requiere ningún punto de conexión al que se pueda acceder a través de Internet.
- Se configura de forma rápida y sencilla. No se requieren puertas de enlace.
- Cada conexión híbrida coincide con una combinación única de host:puerto, lo que resulta útil para la seguridad.
- Normalmente no requiere vulnerabilidades de firewall, porque todas las conexiones salen a través de puertos web estándar.
- Dado que la característica está en el nivel de red, da igual el lenguaje que use la aplicación y la tecnología que emplee el punto de conexión.
- Se puede utilizar para proporcionar acceso en varias redes desde una única aplicación. 
- Se admite en GA para aplicaciones de Windows y de Linux. No es compatible con aplicaciones de contenedor de Windows.

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Cosas que no se pueden hacer con las conexiones híbridas ###

Las cosas que no se pueden hacer con las conexiones híbridas incluyen:

- Montaje de una unidad.
- Uso de UDP.
- Acceso a servicios basados en TCP que usen puertos dinámicos, como el modo pasivo FTP o el modo pasivo extendido.
- Compatibilidad con LDAP, porque puede requerir UDP.
- Compatibilidad con Active Directory, porque un trabajo de App Service no se puede unir a un dominio. 

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Incorporación y creación de Conexiones híbridas en la aplicación ##

Para crear una conexión híbrida, vaya a [Azure Portal][portal] y seleccione su aplicación. Seleccione **Redes** > **Configure los puntos de conexión de la conexión híbrida**. Aquí se pueden ver las conexiones híbridas que están configuradas para su aplicación.  

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-portal.png" alt-text="Captura de pantalla de la lista de conexiones híbridas":::

Para agregar una conexión híbrida nueva, seleccione **[+] Agregar conexión híbrida**.  Verá una lista de las conexiones híbridas que ya haya creado. Para agregar una o varias de ellas en la aplicación, seleccione las que desee y, luego, seleccione **Agregar conexión híbrida seleccionada**.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-addhc.png" alt-text="Captura de pantalla del portal de conexiones híbridas":::

Si quiere crear una conexión híbrida, seleccione **Crear conexión híbrida nueva**. Especifique: 

- Nombre de la conexión híbrida.
- El nombre de host del punto de conexión.
- El puerto del punto de conexión.
- El espacio de nombres de Service Bus que desea usar.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-createhc.png" alt-text="Captura de pantalla del cuadro de diálogo Crear conexión híbrida nueva":::

Cada conexión híbrida está asociada a un espacio de nombres de Service Bus y cada uno de estos espacios se encuentra en una región de Azure. Es importante intentar usar un espacio de nombres de Service Bus que se encuentre en la misma región que la aplicación, para evitar una latencia de red excesiva.

Si desea quitar una conexión híbrida de una aplicación, haga clic con el botón derecho en ella y seleccione **Desconectar**.  

Cuando se agrega una conexión híbrida a la aplicación, puede seleccionarla para ver su información detallada. 

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-properties.png" alt-text="Captura de pantalla de los detalles de las conexiones híbridas":::

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Creación de una conexión híbrida en el portal de Azure Relay ###

Además de la experiencia del portal dentro de la aplicación, puede crear conexiones híbridas en el portal de Azure Relay. Para que App Service use una conexión híbrida, debe:

* Requerir autorización del cliente.
* Tener un elemento de metadatos llamado punto de conexión que contenga una combinación de host:puerto como valor.

## <a name="hybrid-connections-and-app-service-plans"></a>Conexiones híbridas y planes de App Service ##

Las conexiones híbridas de App Service solo están disponibles en las SKU de precios de nivel Básico, Estándar, Premium y Aislado. No hay límites asociados al plan de precios.  

| Plan de precios | Número de conexiones híbridas que se pueden usar en el plan |
|----|----|
| Básico | 5 por plan |
| Estándar | 25 por plan |
| Premium (v1-v3) | 220 por aplicación |
| Aislado (v1-v2) | 220 por aplicación |

La interfaz de usuario del plan de App Service muestra cuántas conexiones híbridas se usan y qué aplicaciones las usan.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-aspproperties.png" alt-text="Captura de pantalla de las propiedades del plan de App Service":::

Seleccione la conexión híbrida para ver detalles. Puede consultar toda la información que vio en la vista de la aplicación. También puede ver cuántas otras aplicaciones en el mismo plan usan esa conexión híbrida.

Hay un límite en el número de puntos de conexión de conexiones híbridas que se pueden usar en un plan de App Service. Sin embargo, cada conexión híbrida se puede usar en cualquier cantidad de aplicaciones de ese plan. Por ejemplo, una conexión híbrida única que se usa en cinco aplicaciones distintas en un plan de App Service cuenta como una conexión híbrida.

### <a name="pricing"></a>Precios ###

Además de haber un requisito de SKU del plan de App Service, hay un costo adicional por el uso de conexiones híbridas. Existe un cargo por cada cliente de escucha que use una conexión híbrida. El agente de escucha es el Administrador de conexiones híbridas. Si tiene cinco conexiones híbridas compatibles con dos Administradores de conexiones híbridas, esto sería un total de 10 agentes de escucha. Para más información, consulte [Precios de Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

La característica Conexiones híbridas requiere un agente de retransmisión en la red que hospeda el punto de conexión de la conexión híbrida. Dicho agente de retransmisión se denomina Hybrid Connection Manager (HCM). Para descargar HCM, desde la aplicación en [Azure Portal][portal], seleccione **Redes** > **Configure los puntos de conexión de la conexión híbrida**.  

Esta herramienta se ejecuta en Windows Server 2012 y versiones posteriores. El Administrador de conexiones híbridas se ejecuta como un servicio y se conecta para la salida a Azure Relay en el puerto 443.  

Después de instalar HCM, puede ejecutar HybridConnectionManagerUi.exe para usar la interfaz de usuario de la herramienta. Este archivo se encuentra en el directorio de instalación del Administrador de conexiones híbridas. En Windows 10, también puede simplemente buscar la *interfaz de usuario del Administrador de conexiones híbridas* en el cuadro de búsqueda.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcm.png" alt-text="Captura de pantalla de Administrador de conexiones híbridas":::

Cuando inicia la interfaz de usuario de HCM, lo primero que se ve es una tabla donde se muestran todas las conexiones híbridas configuradas con esta instancia de HCM. Si desea hacer algún cambio, primero debe autenticarse con Azure. 

Para agregar una o varias conexiones híbridas a HCM:

1. Inicie la interfaz de usuario de HCM.
2. Seleccione **Add a new Hybrid Connection** (Agregar conexión híbrida).
:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcmadd.png" alt-text="Captura de pantalla de configuración de conexiones híbridas nuevas":::

1. Inicie sesión con su cuenta de Azure para obtener las conexiones híbridas disponibles con sus suscripciones. HCM no sigue usando la cuenta de Azure después. 
1. Elija una suscripción.
1. Seleccione las conexiones híbridas que desea que retransmita el HCM.
:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcmadded.png" alt-text="Captura de pantalla de las conexiones híbridas":::

1. Seleccione **Guardar**.

Ahora puede ver las conexiones híbridas que agregó. También puede seleccionar la conexión híbrida configurada para ver detalles.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-hcmdetails.png" alt-text="Captura de pantalla de los detalles de la conexión híbrida":::

Para que HCM admita las conexiones híbridas con las que se ha configurado, necesita:

- Acceso de TCP a Azure a través del puerto 443.
- Acceso de TCP al punto de conexión de la conexión híbrida.
- La capacidad de realizar búsquedas de DNS en el host del punto de conexión y el espacio de nombres de Service Bus.

> [!NOTE]
> Azure Relay depende de Web Sockets para la conectividad. Esta funcionalidad solo está disponible en Windows Server 2012 o versiones posteriores. Por este motivo, HCM no se admite en versiones anteriores de Windows Server 2012.
>

### <a name="redundancy"></a>Redundancia ###

Cada HCM puede admitir varias conexiones híbridas. Además, varios HCM pueden admitir cualquier conexión híbrida dada. El comportamiento predeterminado es enrutar el tráfico a través de los HCM configurados en cualquier punto de conexión dado. Si desea alta disponibilidad en las conexiones híbridas de la red, ejecute varias instancias de HCM en máquinas independientes. El algoritmo de distribución de carga usado por el servicio Relay para distribuir el tráfico a los Administradores de conexiones híbridas es de asignación aleatoria. 

### <a name="manually-add-a-hybrid-connection"></a>Incorporación manual de una conexión híbrida ###

Para permitir que alguien de fuera de su suscripción hospede una instancia de HCM para una conexión híbrida determinada, comparta con dicho usuario la cadena de conexión de puerta de enlace de la conexión híbrida. Puede ver la cadena de conexión de la puerta de enlace en las propiedades de la conexión híbrida en [Azure Portal][portal]. Para usar dicha cadena, seleccione **Enter Manually** (Especificar manualmente) en el HCM y pegue la cadena de conexión de puerta de enlace.

:::image type="content" source="media/app-service-hybrid-connections/hybridconn-manual.png" alt-text="Incorporación manual de una conexión híbrida":::

### <a name="upgrade"></a>Actualizar ###

El Administrador de conexiones híbridas se actualiza periódicamente para solucionar problemas o proporcionar mejoras. Cuando se publique una actualización, se mostrará un elemento emergente en la interfaz de usuario del Administrador de conexiones híbridas. Al aplicar la actualización se aplicarán los cambios y se reiniciará el Administrador de conexiones híbridas. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Incorporación de una conexión híbrida a su aplicación mediante programación ##

La CLI de Azure es compatible con las conexiones híbridas. Los comandos proporcionados funcionan tanto a nivel de aplicación como del plan de App Service.  Los comandos a nivel de aplicación son:

```azurecli
az webapp hybrid-connection

Group
    az webapp hybrid-connection : Methods that list, add and remove hybrid-connections from webapps.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    add    : Add a hybrid-connection to a webapp.
    list   : List the hybrid-connections on a webapp.
    remove : Remove a hybrid-connection from a webapp.
```

Los comandos del plan de App Service permiten establecer qué clave usará una conexión híbrida determinada. Hay dos claves establecidas en cada conexión híbrida, una principal y una secundaria. Puede optar por usar la clave principal o secundaria con los siguientes comandos. Esto le permite cambiar las claves para cuando desee regenerarlas periódicamente. 

```azurecli
az appservice hybrid-connection --help

Group
    az appservice hybrid-connection : A method that sets the key a hybrid-connection uses.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    set-key : Set the key that all apps in an appservice plan use to connect to the hybrid-
                connections in that appservice plan.
```

## <a name="secure-your-hybrid-connections"></a>Proteja su Conexiones híbridas ##

Cualquier usuario que tenga permisos suficientes en la retransmisión de Azure Service Bus subyacente puede agregar una conexión híbrida existente a otros App Service Web Apps. Esto significa que si debe evitar que otros usuarios reutilicen esa misma conexión híbrida (por ejemplo, cuando el recurso de destino es un servicio que no tiene ninguna medida de seguridad adicional para evitar el acceso no autorizado), debe bloquear el acceso a la retransmisión de Azure Service Bus.

Cualquier persona con acceso `Reader` a la retransmisión puede _ver_ la conexión híbrida al intentar agregarla a su aplicación web en Azure Portal, pero no puede _agregarla_, ya que carece de los permisos necesarios para recuperar la cadena de conexión que se usa para establecer la conexión de retransmisión. Para agregar correctamente la conexión híbrida, deben tener el permiso `listKeys` (`Microsoft.Relay/namespaces/hybridConnections/authorizationRules/listKeys/action`). El rol `Contributor` o cualquier otro que incluya este permiso en la retransmisión permite a los usuarios usar la conexión híbrida y agregarla a sus propias aplicaciones web.

## <a name="manage-your-hybrid-connections"></a>Administración de las conexiones híbridas ##

Si necesita cambiar el host o el puerto del punto de conexión de una conexión híbrida, siga estos pasos:

1. Quite la conexión híbrida del Administrador de conexiones híbridas en el equipo local; para ello, seleccione la conexión y luego **Quitar** en la parte superior izquierda de la ventana de detalles de la conexión híbrida.
1. Desconecte la conexión híbrida de App Service; para ello, vaya a **Conexiones híbridas** en la página **Redes** de App Service.
1. Vaya a la retransmisión del punto de conexión que necesita actualizar y seleccione **Conexiones híbridas** en **Entidades** en el menú de navegación izquierdo.
1. Seleccione la conexión híbrida que quiere actualizar y seleccione **Propiedades** en **Configuración** en el menú de navegación izquierdo.
1. Realice los cambios y presione **Guardar cambios** en la parte superior.
1. Vuelva a la configuración de **Conexiones híbridas** de App Service y agregue la conexión híbrida de nuevo. Asegúrese de que el punto de conexión se actualiza según lo previsto. Si no ve la conexión híbrida en la lista, actualice transcurridos entre cinco y diez minutos.
1. Vuelva al Administrador de conexiones híbridas en el equipo local y agregue la conexión de nuevo.

## <a name="troubleshooting"></a>Solución de problemas ##

El estado "Conectado" significa que hay al menos una instancia de HCM configurada con esa conexión híbrida y es capaz de conectarse a Azure. Si el estado de la conexión híbrida no indica **Conectado**, esta no está configurada en ningún HCM con acceso a Azure. Cuando el HCM muestra **No conectado**, hay algunas cosas que deben comprobarse:

* ¿Tiene el host acceso de salida a Azure en el puerto 443? Puede probar desde el host de HCM mediante el comando *Test-NetConnection Destination -P Port* de PowerShell. 
* ¿Es posible que su HCM esté en mal estado? Pruebe a reiniciar el servicio local "Servicio de Administrador de conexiones híbridas de Azure".

* ¿Tiene instalado software en conflicto? El Administrador de conexiones híbridas no puede coexistir con el Administrador de conexiones híbridas de Biztalk o Service Bus para Windows Server. Por lo tanto, al instalar HCM, las versiones de estos paquetes deben quitarse primero.

Si el estado indica **Conectado**, pero la aplicación no puede comunicarse con su punto de conexión, entonces:

* Asegúrese de estar usando un nombre DNS en la conexión híbrida. Si usa una dirección IP, es posible que no se produzca la búsqueda de DNS de cliente necesaria. Si el cliente que se ejecuta en la aplicación web no realiza una búsqueda de DNS, la conexión híbrida no funciona.
* Compruebe que el nombre DNS que se usa en la conexión híbrida pueda resolverse desde el host de HCM. Compruebe la resolución mediante *nslookup EndpointDNSname*, donde EndpointDNSname es una coincidencia exacta con lo que se usa en la definición de la conexión híbrida.
* Pruebe el acceso desde el host de HCM al punto de conexión mediante el comando *Test-NetConnection EndpointDNSname -P Port* de PowerShell. Si no puede comunicarse con el punto de conexión desde el host de HCM, compruebe los firewalls entre los dos hosts, incluidos los firewalls basados en host en el host de destino.

En App Service, es posible invocar la herramienta de la línea de comandos **tcpping** desde la consola de herramientas avanzadas (Kudu). Esta herramienta puede indicarle si tiene acceso a un punto de conexión TCP, pero no si tiene acceso a un punto de conexión híbrida. Cuando usa la herramienta de la consola en un punto de conexión híbrida, simplemente confirma que usa una combinación de host:puerto.  

Si tiene un cliente de línea de comandos para el punto de conexión, puede probar la conectividad desde la consola de la aplicación. Por ejemplo, puede probar el acceso a los puntos de conexión de servidor web mediante cURL.

<!--Links-->
[HCService]: /azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: /azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
