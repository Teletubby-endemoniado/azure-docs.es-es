---
title: Conexión a redes virtuales de Azure mediante un ISE
description: Creación de un entorno del servicio de integración (ISE) que acceda a las redes virtuales de Azure desde Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: conceptual
ms.date: 08/11/2021
ms.openlocfilehash: 318c4ba47caab7ce7f84f8bc43522d04c3eb40da
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128644873"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-using-an-integration-service-environment-ise"></a>Conexión a redes virtuales de Azure desde Azure Logic Apps mediante un entorno del servicio de integración (ISE)

Para escenarios donde sus cuentas de integración y las aplicaciones lógicas necesitan tener acceso a una [red virtual de Azure](../virtual-network/virtual-networks-overview.md), cree un [*entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE es un entorno que usa almacenamiento dedicado y otros recursos que existen de forma independiente del servicio Logic Apps multiinquilino "global". Esta separación también reduce los posibles efectos que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación. Un ISE también le proporciona sus propias direcciones IP estáticas. Estas direcciones IP son independientes de las direcciones IP estáticas que comparten las aplicaciones lógicas en el servicio multiinquilino público.

Cuando crea un ISE, Azure *inserta* ese ISE en la red virtual de Azure que, luego, implementa el servicio Logic Apps en la red virtual. Al crear aplicaciones lógicas o cuentas de integración, seleccionará el ISE como ubicación. Así, las aplicaciones lógicas y las cuentas de integración tienen acceso directo a recursos tales como las máquinas virtuales, los servidores, los sistemas y los servicios de la red virtual.

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> Para que las aplicaciones lógicas y las cuentas de integración funcionen juntas en un ISE, ambas deben usar *el mismo ISE* como ubicación.

Un ISE ha aumentado los límites siguientes:

* Duración de la ejecución
* Retención de almacenamiento
* Rendimiento
* Tiempos de espera de solicitud y respuesta HTTP
* Tamaños de mensaje
* Solicitudes de conector personalizado

Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md). Para más información sobre los ISE, consulte [Acceso a recursos de Azure Virtual Network desde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

En este artículo se muestra cómo realizar estas tareas mediante Azure Portal:

* Habilite el acceso para el ISE.
* Cree el ISE.
* Agregue capacidad adicional al ISE.

También puede crear un ISE mediante el [ejemplo de plantilla de inicio rápido de Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.logic/integration-service-environment) o la API REST de Logic Apps, incluida la configuración de claves administradas por el cliente:

* [Creación de un entorno del servicio de integración (ISE) mediante la API REST de Logic Apps](../logic-apps/create-integration-service-environment-rest-api.md)
* [Configuración de claves administradas por el cliente para cifrar datos en reposo en los ISE](../logic-apps/customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Requisitos previos

* Una cuenta y una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

  > [!IMPORTANT]
  > Las aplicaciones lógicas, los desencadenadores integrados, las acciones integradas y los conectores que se ejecutan en el ISE usan un plan de tarifa diferente al plan de tarifa basado en el consumo. Para saber cómo funcionan los precios y la facturación para los ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#ise-pricing). Para ver las tarifas de precios, consulte los [precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

* Una [red virtual de Azure](../virtual-network/virtual-networks-overview.md) que tiene cuatro subredes *vacías*, que son necesarias para crear e implementar recursos en el ISE y que estos componentes internos y ocultos usan:

  * Proceso de Logic Apps
  * App Service Environment interno (conectores)
  * API Management interno (conectores)
  * Redis interno para el almacenamiento en caché y el rendimiento
  
  Puede crear las subredes por adelantado o cuando cree el ISE para crearlas al mismo tiempo. Sin embargo, antes de crear las subredes, asegúrese de revisar los [requisitos de subred](#create-subnet).

  * Asegúrese de que la red virtual [habilita el acceso para el ISE](#enable-access) para que su ISE funcione correctamente y permanezca accesible.

  * Si usa una [aplicación virtual de red (NVA)](../virtual-network/virtual-networks-udr-overview.md#user-defined), asegúrese de no habilitar la terminación TLS/SSL ni cambiar el tráfico TLS/SSL saliente. Además, asegúrese de no habilitar la inspección del tráfico que se origina en la subred de la ISE. Para más información, consulte [Enrutamiento del tráfico de redes virtuales](../virtual-network/virtual-networks-udr-overview.md).

  * Si quiere usar servidores de Sistema de nombres de dominio (DNS) personalizados para su red virtual de Azure, [configure esos servidores siguiendo estos pasos](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implementar el ISE en su red virtual. Para más información acerca de la administración de la configuración del servidor DNS, consulte [Crear, cambiar o eliminar una red virtual](../virtual-network/manage-virtual-network.md#change-dns-servers).

    > [!NOTE]
    > Si cambia el servidor DNS o la configuración de este, debe reiniciar el ISE para que pueda recoger dichos cambios. Para obtener más información, consulte [Restablecimiento del ISE](../logic-apps/ise-manage-integration-service-environment.md#restart-ISE).

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>Habilitar el acceso para el ISE

Cuando se usa un ISE con una red virtual de Azure, un problema de configuración habitual es tener uno o varios puertos bloqueados. Los conectores que usa para crear conexiones entre su ISE y los sistemas de destino también pueden tener sus propios requisitos de puerto. Por ejemplo, si se comunica con un sistema FTP mediante el conector FTP, el puerto que usa en el sistema FTP debe estar disponible, por ejemplo, el puerto 21 para enviar comandos.

Para asegurarse de que el ISE sea accesible y de que las aplicaciones lógicas del ISE puedan comunicarse a través de cada subred de la red virtual, [abra los puertos descritos en esta tabla para cada subred](#network-ports-for-ise). Si alguno de los puertos necesarios no está disponible, el ISE no funcionará correctamente.

* Si tiene varias instancias de ISE que necesitan acceso a otros puntos de conexión con restricciones de IP, implemente [Azure Firewall](../firewall/overview.md) o una [aplicación virtual de red](../virtual-network/virtual-networks-overview.md#filter-network-traffic) en la red virtual y enrute el tráfico saliente a través de ese firewall o aplicación virtual de red. Después, puede [configurar una dirección IP única, de salida, pública, estática y predecible](connect-virtual-network-vnet-set-up-single-ip-address.md) que todas las instancias de ISE de la red virtual puedan usar para comunicarse con los sistemas de destino. De ese modo, no es necesario configurar aperturas adicionales del firewall en los sistemas de destino para cada ISE.

   > [!NOTE]
   > También puede usar este enfoque para un solo ISE si el escenario requiere limitar el número de direcciones IP que necesitan acceso. Considere si los costos adicionales del firewall o el dispositivo de red virtual tienen sentido para su escenario. Obtenga más información sobre los [precios de Azure Firewall](https://azure.microsoft.com/pricing/details/azure-firewall/).

* Si ha creado una nueva red virtual de Azure y subredes sin ninguna restricción, no es necesario configurar [grupos de seguridad de red (NSG)](../virtual-network/network-security-groups-overview.md#network-security-groups) en la red virtual para controlar el tráfico entre las subredes.

* En el caso de una red virtual existente, puede, *de forma opcional*, configurar [grupos de seguridad de red (NSG)](../virtual-network/network-security-groups-overview.md#network-security-groups) para [filtrar el tráfico de red entre subredes](../virtual-network/tutorial-filter-network-traffic.md). Si quiere usar esta ruta, o si ya usa NSG, [abra los puertos descritos en esta tabla](#network-ports-for-ise) para esos NSG.

  Al configurar las [reglas de seguridad de NSG](../virtual-network/network-security-groups-overview.md#security-rules), debe usar *ambos* protocolos, **TCP** y **UDP**, o bien seleccionar **Cualquiera** en su lugar para no tener que crear reglas independientes para cada protocolo. Las reglas de seguridad de NSG describen los puertos que debe abrir para las direcciones IP que necesitan acceder a esos puertos. Asegúrese de que todo firewall, enrutador u otro elemento que exista entre estos puntos de conexión también mantengan esos puertos accesibles para esas direcciones IP.

* Si configura la tunelización forzada a través del firewall para redirigir el tráfico enlazado a Internet, revise los [requisitos de la tunelización forzada](#forced-tunneling).

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>Puertos de red usados por el ISE

En esta tabla se describen los puertos que necesita el ISE para ser accesible y el propósito de esos puertos. Para ayudar a reducir la complejidad que conlleva la configuración de reglas de seguridad, la tabla usa [etiquetas de servicio](../virtual-network/service-tags-overview.md) que representan grupos de prefijos de direcciones IP de un servicio de Azure concreto. Cuando así se indica, *ISE interno* e *ISE externo* hacen referencia al [punto de conexión de acceso seleccionado durante la creación del ISE](connect-virtual-network-vnet-isolated-environment.md#create-environment). Para más información, consulte [Acceso al punto de conexión](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access).

> [!IMPORTANT]
> En todas las reglas, establezca los puertos de origen en `*`, ya que estos son efímeros.

#### <a name="inbound-security-rules"></a>Reglas de seguridad de entrada

| Propósito | Etiqueta de servicio de origen o direcciones IP | Puertos de origen | Etiqueta de servicio de destino o direcciones IP | Puertos de destino | Notas |
|---------|------------------------------------|--------------|-----------------------------------------|-------------------|-------|
| Comunicación entre subredes en una red virtual | Espacio de direcciones de la red virtual con subredes del ISE | * | Espacio de direcciones de la red virtual con subredes del ISE | * | Necesario para que el tráfico fluya *entre* las subredes de la red virtual. <p><p>**Importante**: Para que el tráfico fluya entre los *componentes* de cada subred, asegúrese de abrir todos los puertos de cada subred. |
| Ambos: <p>Comunicación con la aplicación lógica <p><p>Historial de ejecución de aplicación lógica| ISE interno: <br>**VirtualNetwork** <p><p>ISE externo: **Internet** o consulte **Notas** | * | **VirtualNetwork** | 443 | En lugar de usar la etiqueta de servicio **Internet**, puede especificar la dirección IP de origen de estos elementos: <p><p>- Equipo o servicio que llama a cualquier desencadenador de solicitud o webhook de la aplicación lógica <p>- Equipo o servicio desde el que se quiere acceder al historial de ejecución de la aplicación lógica <p><p>**Importante**: El cierre o bloqueo de este puerto evita las llamadas a aplicaciones lógicas con desencadenadores de solicitud o webhooks. También se evita que acceda a las entradas y salidas de cada paso del historial de ejecución. Pero no se evita que acceda al historial de ejecución de la aplicación lógica.|
| Diseñador de Logic Apps: propiedades dinámicas | **LogicAppsManagement** | * | **VirtualNetwork** | 454 | Las solicitudes proceden de las [direcciones IP entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) del punto de conexión de acceso de Logic Apps de esa región. <p><p>**Importante**: Si está trabajando con la nube de Azure Government, la etiqueta de servicio **LogicAppsManagement** no funcionará. En su lugar, debe proporcionar las [direcciones IP de entrada](../logic-apps/logic-apps-limits-and-config.md#azure-government-inbound) de Logic Apps para Azure Government. |
| Comprobación del estado de la red | **LogicApps** | * | **VirtualNetwork** | 454 | Las solicitudes proceden de las [direcciones IP entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) del punto de conexión de acceso de Logic Apps y las [direcciones IP salientes](../logic-apps/logic-apps-limits-and-config.md#outbound) de esa región. <p><p>**Importante**: Si está trabajando con la nube de Azure Government, la etiqueta de servicio **LogicApps** no funcionará. En su lugar, debe proporcionar las [direcciones IP de entrada](../logic-apps/logic-apps-limits-and-config.md#azure-government-inbound) y las [direcciones IP de salida](../logic-apps/logic-apps-limits-and-config.md#azure-government-outbound) de Logic Apps para Azure Government. |
| Implementación del conector | **AzureConnectors** | * | **VirtualNetwork** | 454 | Necesario para implementar y actualizar conectores. El cierre o bloqueo de este puerto da lugar a errores de las implementaciones de ISE y evita las actualizaciones y correcciones del conector. <p><p>**Importante**: Si está trabajando con la nube de Azure Government, la etiqueta de servicio **AzureConnectors** no funcionará. En su lugar, debe proporcionar las [direcciones IP de salida del conector administrado](../logic-apps/logic-apps-limits-and-config.md#azure-government-outbound) para Azure Government. |
| Dependencia de administración de App Service | **AppServiceManagement** | * | **VirtualNetwork** | 454, 455 ||
| Comunicación de Azure Traffic Manager | **AzureTrafficManager** | * | **VirtualNetwork** | ISE interno: 454 <p><p>ISE externo: 443 ||
| Ambos: <p>Implementación de la directiva de conectores <p>API Management: punto de conexión de administración | **APIManagement** | * | **VirtualNetwork** | 3443 | Para la implementación de la directiva de conectores, es necesario el acceso a los puertos para implementar y actualizar conectores. El cierre o bloqueo de este puerto da lugar a errores de las implementaciones de ISE y evita las actualizaciones y correcciones del conector. |
| Acceso a instancias de Azure Cache for Redis entre instancias de rol | **VirtualNetwork** | * | **VirtualNetwork** | 6379 - 6383, además consulte **Notas**| Para que ISE funcione con Azure Cache for Redis, debe abrir estos [puertos de entrada y salida descritos en las Preguntas frecuentes sobre Azure Cache for Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
|||||||

#### <a name="outbound-security-rules"></a>Reglas de seguridad de entrada

| Propósito | Etiqueta de servicio de origen o direcciones IP | Puertos de origen | Etiqueta de servicio de destino o direcciones IP | Puertos de destino | Notas |
|---------|------------------------------------|--------------|-----------------------------------------|-------------------|-------|
| Comunicación entre subredes en una red virtual | Espacio de direcciones de la red virtual con subredes del ISE | * | Espacio de direcciones de la red virtual con subredes del ISE | * | Necesario para que el tráfico fluya *entre* las subredes de la red virtual. <p><p>**Importante**: Para que el tráfico fluya entre los *componentes* de cada subred, asegúrese de abrir todos los puertos de cada subred. |
| Comunicación con la aplicación lógica | **VirtualNetwork** | * | Internet | 443, 80 | Esta regla es necesaria para la comprobación de certificados de Capa de sockets seguros (SSL). Esta comprobación es para varios sitios internos y externos, lo cual es el motivo por el que se requiere Internet como destino. |
| Comunicación con la aplicación lógica | **VirtualNetwork** | * | Varía según el destino | Varía según el destino | Los puertos de destino varían en función de los puntos de conexión de los servicios externos con los que la aplicación lógica necesita comunicarse. <p><p>Por ejemplo, el puerto de destino es el puerto 25 para un servicio SMTP, el puerto 22 para un servicio SFTP y así sucesivamente. |
| Azure Active Directory | **VirtualNetwork** | * | **AzureActiveDirectory** | 80, 443 ||
| Dependencia de Azure Storage | **VirtualNetwork** | * | **Storage** | 80, 443, 445 ||
| Administración de conexiones | **VirtualNetwork** | * | **AppService** | 443 ||
| Publicación de métricas y registros de diagnóstico | **VirtualNetwork** | * | **AzureMonitor** | 443 ||
| Dependencia de Azure SQL | **VirtualNetwork** | * | **SQL** | 1433 ||
| Azure Resource Health | **VirtualNetwork** | * | **AzureMonitor** | 1886 | Necesario para publicar el estado de mantenimiento en Resource Health. |
| Dependencia de la directiva de registro en el centro de eventos y agente de supervisión | **VirtualNetwork** | * | **EventHub** | 5672 ||
| Acceso a instancias de Azure Cache for Redis entre instancias de rol | **VirtualNetwork** | * | **VirtualNetwork** | 6379 - 6383, además consulte **Notas**| Para que ISE funcione con Azure Cache for Redis, debe abrir estos [puertos de entrada y salida descritos en las Preguntas frecuentes sobre Azure Cache for Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Resolución de nombres DNS | **VirtualNetwork** | * | Direcciones IP de los servidores de Sistema de nombres de dominio (DNS) personalizados de la red virtual. | 53 | Solo es necesario cuando se usan servidores DNS personalizados en la red virtual. |
|||||||

Además, debe agregar reglas de salida para [App Service Environment (ASE)](../app-service/environment/intro.md):

* Si usa Azure Firewall, debe configurar el firewall con la [etiqueta de nombre de dominio completo (FQDN)](../firewall/fqdn-tags.md#current-fqdn-tags) de App Service Environment (ASE), que permite el acceso de salida al tráfico de la plataforma de ASE.

* Si usa un dispositivo de firewall distinto de Azure Firewall, debe configurar el firewall con *todas* las reglas enumeradas en las [dependencias de integración del firewall](../app-service/environment/firewall-integration.md#dependencies) que son necesarias para App Service Environment.

<a name="forced-tunneling"></a>

#### <a name="forced-tunneling-requirements"></a>Requisitos de tunelización forzada

Si configura o usa la [tunelización forzada](../firewall/forced-tunneling.md) a través del firewall, debe permitir dependencias externas adicionales para su ISE. La tunelización forzada permite redirigir el tráfico enlazado a Internet a un próximo salto designado, como la red privada virtual (VPN), o a un dispositivo virtual, en lugar de a Internet, para que pueda inspeccionar y auditar el tráfico de red saliente.

Si no permite el acceso a estas dependencias, se produce un error en la implementación de ISE, y el ISE implementado deja de funcionar.

* Rutas definidas por el usuario

  Para evitar el enrutamiento asimétrico, debe definir una ruta para cada una de las direcciones IP que se enumeran a continuación con **Internet** como el próximo salto.

  * [Direcciones de entrada y salida de Logic Apps para la región del ISE](../logic-apps/logic-apps-limits-and-config.md#firewall-configuration-ip-addresses-and-service-tags)  
  * [Direcciones IP de Azure para conectores de la región del ISE que se encuentran disponibles en este archivo de descarga](https://www.microsoft.com/download/details.aspx?id=56519)
  * [Direcciones de administración de App Service Environment](../app-service/environment/management-addresses.md)  
  * [Direcciones de administración de Azure Traffic Manager](https://azuretrafficmanagerdata.blob.core.windows.net/probes/azure/probe-ip-ranges.json)
  * [Direcciones IP del plano de control de Azure API Management](../api-management/api-management-using-with-vnet.md#control-plane-ip-addresses)

* Puntos de conexión del servicio

  Debe habilitar los puntos de conexión de servicio para Azure SQL, Storage, Service Bus, KeyVault y Event Hubs porque no puede enviar tráfico a través de un firewall a estos servicios.

*  Otras dependencias de entrada y salida

   El firewall *debe* permitir las siguientes dependencias de entrada y salida:
   
   * [Dependencias de Azure App Service](../app-service/environment/firewall-integration.md#deploying-your-ase-behind-a-firewall)
   * [Dependencias de Azure Cache Service](../azure-cache-for-redis/cache-how-to-premium-vnet.md#what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-virtual-networks)
   * [Dependencias de Azure API Management](../api-management/api-management-using-with-vnet.md#network-configuration-issues)

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Crear el ISE

1. En [Azure Portal](https://portal.azure.com), en el cuadro de búsqueda principal de Azure, escriba `integration service environments` como filtro y seleccione **Entornos del servicio de integración**.

   ![Buscar y seleccionar "Entornos del servicio de integración"](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. En el panel **Entornos del servicio de integración**, seleccione **Agregar**.

   ![Seleccione "Agrear" para crear el entorno del servicio de integración.](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. Proporcione estos detalles para su entorno y, después, seleccione **Revisar y crear**, por ejemplo:

   ![Proporcionar detalles del entorno](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propiedad | Obligatorio | Value | Descripción |
   |----------|----------|-------|-------------|
   | **Suscripción** | Sí | <*Azure-subscription-name*> | La suscripción de Azure que se usará para el entorno. |
   | **Grupos de recursos** | Sí | <*nombre del grupo de recursos de Azure*> | Un grupo de recursos de Azure nuevo o existente donde quiera crear el entorno. |
   | **Nombre del entorno del servicio de integración** | Sí | <*nombre del entorno*> | El nombre de ISE, que solo puede contener letras, números, guiones (`-`), caracteres de subrayado (`_`) y puntos (`.`). |
   | **Ubicación** | Sí | <*región del centro de datos de Azure*> | La región del centro de datos de Azure donde se implementará el entorno. |
   | **SKU** | Sí | **Premium** o **Desarrollador (sin SLA)** | La SKU de ISE que se va a crear y a usar. Si quiere conocer las diferencias entre estas SKU, consulte las [SKU de ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). <p><p>**Importante**: Esta opción solo está disponible durante la creación del ISE y no se puede cambiar más adelante. |
   | **Capacidad adicional** | Premium: <br>Sí <p><p>Desarrollador: <br>No aplicable | Premium: <br>De 0 a 10 <p><p>Desarrollador: <br>No aplicable | Número de unidades de procesamiento adicionales que se utilizará para este recurso ISE. Para agregar capacidad después de crearla, consulte [Add ISE capacity](../logic-apps/ise-manage-integration-service-environment.md#add-capacity) (Agregar capacidad ISE). |
   | **Punto de conexión de acceso** | Sí | **Interno** o **externo** | Tipo de puntos de conexión de acceso que se usan para el ISE. Estos puntos de conexión determinan si los desencadenadores de solicitudes o de webhooks de las aplicaciones lógicas del ISE pueden recibir llamadas desde fuera de la red virtual o no. <p><p>Por ejemplo, si quiere usar los siguientes desencadenadores basados en webhook, asegúrese de seleccionar **Externos**: <p><p>- Azure DevOps <br>- Azure Event Grid <br>- Common Data Service <br>- Office 365 <br>- SAP (versión ISE) <p><p>La selección también afecta a la manera en que puede ver y acceder a las entradas y salidas en el historial de ejecuciones de la aplicación lógica. Para obtener más información, consulte [Acceso al punto de conexión del ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). <p><p>**Importante**: Puede seleccionar el punto de conexión de acceso solo durante la creación del ISE y no puede cambiar esta opción más adelante. |
   | **Red virtual** | Sí | <*Azure-virtual-network-name*> | La red virtual de Azure donde quiere insertar su entorno para que las aplicaciones lógicas de ese entorno puedan acceder a la red virtual. Si no tiene una red, [cree primero una red virtual de Azure](../virtual-network/quick-create-portal.md). <p><p>**Importante**: *Solo* puede realizar esta inserción cuando se crea el ISE. |
   | **Subredes** | Sí | <*subnet-resource-list*> | Un ISE necesita cuatro subredes *vacías*, que son necesarias para crear e implementar recursos en el ISE y que usan los componentes internos de Logic Apps, como los conectores y el almacenamiento en caché, para mejorar el rendimiento. <p>**Importante**: Asegúrese de [revisar los requisitos de las subredes antes de continuar con estos pasos para crearlas](#create-subnet). |
   |||||

   <a name="create-subnet"></a>

   **Creación de las subredes**

   Un ISE necesita cuatro subredes *vacías* para crear e implementar recursos en el ISE y que usan los componentes internos de Logic Apps, como los conectores y el almacenamiento en caché, para mejorar el rendimiento. *No se pueden cambiar* estas direcciones de subred una vez creado el entorno. Si crea e implementa el ISE mediante Azure Portal, asegúrese de no delegar estas subredes en ningún servicio de Azure. Sin embargo, si crea e implementa el ISE mediante la API REST, Azure PowerShell o una plantilla de Azure Resource Manager, debe [delegar](../virtual-network/manage-subnet-delegation.md) una subred vacía en `Microsoft.integrationServiceEnvironment`. Para más información, consulte el artículo [Adición de una delegación de subred](../virtual-network/manage-subnet-delegation.md).

   Cada subred tiene que cumplir estos requisitos:

   * Usa un nombre que comienza con un carácter alfabético o un guion bajo (sin números), y no tiene estos caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`.

   * Usa el [formato Enrutamiento de interdominios sin clases (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).
   
     > [!IMPORTANT]
     >
     > No use los siguientes espacios de direcciones IP para la red virtual o las subredes, ya que Azure Logic Apps no pueden resolverlos:<p>
     > 
     > * 0.0.0.0/8
     > * 100.64.0.0/10
     > * 127.0.0.0/8
     > * 168.63.129.16/32
     > * 169.254.169.254/32

   * Usa al menos `/27` en el espacio de direcciones, porque cada subred requiere 32 direcciones. Por ejemplo, `10.0.0.0/27` tiene 32 direcciones porque 2<sup>(32-27)</sup> es 2<sup>5</sup> o 32. Más direcciones no proporcionarán ventajas adicionales. Para obtener más información sobre cómo calcular las direcciones, consulte [bloques de CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si usa [ExpressRoute](../expressroute/expressroute-introduction.md), tiene que [crear una tabla de rutas](../virtual-network/manage-route-table.md) que tenga la siguiente ruta y vincular esa tabla con cada subred que utilice el ISE:

     **Nombre**: <*nombre de ruta*><br>
     **Prefijo de dirección**: 0.0.0.0/0<br>
     **Próximo salto**: Internet

   1. En la lista **Subredes**, seleccione **Administrar configuración de subred**.

      ![Administrar configuración de subred](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. En el panel **Subredes**, seleccione **Subred**.

      ![Agregar cuatro subredes vacías](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. En el panel **Agregar subred**, proporcione esta información.

      * **Name**: nombre de la subred.
      * **Intervalo de direcciones (bloque CIDR)** : intervalo de la subred la red virtual y en formato CIDR.

      ![Agregar detalles de la subred](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. Cuando finalice, seleccione **Aceptar**.

   1. Repita estos pasos para tres subredes más.

      > [!NOTE]
      > Si las subredes que intenta crear no son válidas, Azure Portal muestra un mensaje, pero no bloquea el progreso.

   Para obtener más información sobre la creación de subredes, consulte [Add a virtual network subnet](../virtual-network/virtual-network-manage-subnet.md) (Agregar una subred de red virtual).

1. Una vez que Azure valide correctamente la información del ISE, seleccione **Crear**, por ejemplo:

   ![Después de una validación correcta, seleccionar "Crear"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure comienza a implementar el entorno, que normalmente tarda menos de dos horas en completarse. En ocasiones, la implementación puede tardar hasta cuatro horas. Para comprobar el estado de implementación, en la barra de herramientas de Azure, seleccione el icono de notificaciones, que abre el panel de notificaciones.

   ![Comprobar el estado de implementación](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Si la implementación finaliza correctamente, Azure muestra esta notificación:

   ![Implementación correcta](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   De lo contrario, siga las instrucciones de Azure Portal para solucionar problemas de implementación.

   > [!NOTE]
   > Si se produce un error en la implementación o se elimina el ISE, Azure podría tardar hasta una hora, o posiblemente más en raras ocasiones, antes de liberar las subredes. Por lo tanto, es posible que tenga que esperar antes de poder volver a usar esas subredes en otro ISE.
   >
   > Si elimina su red virtual, Azure generalmente tarda hasta dos horas antes de liberar las subredes, pero esta operación puede llevar más tiempo. 
   > Cuando elimine redes virtuales, asegúrese de que no haya recursos conectados. 
   > Consulte [Eliminar red virtual](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Para ver el entorno, seleccione **Ir al recurso** si Azure no va automáticamente al entorno una vez finalizada la implementación.

1. Para un ISE que tenga acceso de punto de conexión *externo*, debe crear un grupo de seguridad de red (NSG), si aún no tiene uno. Debe agregar una regla de seguridad de entrada al grupo de seguridad de red para permitir el tráfico desde las direcciones IP de salida del conector administrado. Para configurar esta regla, siga estos pasos:

   1. En el menú del ISE, en **Configuración**, seleccione **Propiedades**.

   1. En **Direcciones IP salientes del conector**, copie los intervalos de direcciones IP públicas, que también aparecen en el artículo [Límites y configuración: direcciones IP de salida](../logic-apps/logic-apps-limits-and-config.md#outbound).

   1. Cree un grupo de seguridad de red, si aún no tiene uno.
   
   1. En función de la siguiente información, agregue una regla de seguridad de entrada para las direcciones IP públicas de salida que copió. Para más información, consulte el [Tutorial: Filtrado del tráfico de red con un grupo de seguridad de red mediante Azure Portal](../virtual-network/tutorial-filter-network-traffic.md#create-a-network-security-group).

      | Propósito | Etiqueta de servicio de origen o direcciones IP | Puertos de origen | Etiqueta de servicio de destino o direcciones IP | Puertos de destino | Notas |
      |---------|------------------------------------|--------------|-----------------------------------------|-------------------|-------|
      | Permitir el tráfico que procede de las direcciones IP de salida del conector | <*connector-public-outbound-IP-addresses*> | * | Espacio de direcciones de la red virtual con subredes del ISE | * | |
      |||||||

1. Para comprobar el estado de la red del ISE, consulte [Administración del entorno del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md#check-network-health).

   > [!CAUTION]
   > Si la red del ISE es incorrecta, la instancia interna de App Service Environment (ASE) que usa el ISE también puede ser incorrecta. Si la instancia de ASE es incorrecta durante más de siete días, esta se suspende. Para resolver este estado, compruebe la configuración de la red virtual. Resuelva los problemas que encuentre y reinicie el ISE. De lo contrario, pasados 90 días se elimina la instancia de ASE suspendida y el ISE deja de ser utilizable. Por lo tanto, asegúrese de mantener correctamente el ISE para permitir el tráfico necesario.
   > 
   > Para más información, consulte los temas siguientes:
   >
   > * [Introducción a diagnósticos de Azure App Service](../app-service/overview-diagnostics.md)
   > * [Registro de mensajes para Azure App Service Environment](../app-service/environment/using-an-ase.md#logging)

1. Para empezar a crear aplicaciones lógicas y otros artefactos en el ISE, consulte [Incorporación de recursos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md).

   > [!IMPORTANT]
   > Después de crear el ISE, podrá usar los conectores ISE administrados, pero recuerde que no aparecen automáticamente en el selector de conectores del Diseñador de aplicaciones lógicas. Para poder usar estos conectores ISE, tiene que [agregarlos e implementarlos manualmente en el ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment) para que aparezcan en el Diseñador de aplicaciones lógicas.

## <a name="next-steps"></a>Pasos siguientes

* [Incorporación de recursos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Administración de entornos del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
