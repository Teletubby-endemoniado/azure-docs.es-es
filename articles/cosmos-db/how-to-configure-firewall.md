---
title: Configure an IP firewall for your Azure Cosmos DB account (Configuración de un firewall de dirección IP para la cuenta de Azure Cosmos DB)
description: Aprenda a configurar directivas de control de acceso por IP para la compatibilidad con el firewall en cuentas de Azure Cosmos.
ms.service: cosmos-db
ms.topic: how-to
ms.date: 03/03/2021
ms.author: thweiss
author: ThomasWeiss
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 61e836fcb07045ce25df02ba48823ac5a2d70762
ms.sourcegitcommit: d2875bdbcf1bbd7c06834f0e71d9b98cea7c6652
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129859311"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Configuración del firewall de IP en Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Para proteger los datos almacenados en su cuenta, Azure Cosmos DB admite un modelo de autorización basado en secretos que usa código de autentificación de mensajes basado en hash (HMAC). Además, Azure Cosmos DB admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Este modelo es muy parecido a las reglas de firewall de un sistema de base de datos tradicional y proporciona un nivel de seguridad adicional a la cuenta. Con los firewalls, puede configurar su cuenta de Azure Cosmos para que sea accesible solo desde un conjunto aprobado de máquinas o servicios en la nube. Para acceder a los datos almacenados en la base de datos de Azure Cosmos DB desde estos conjuntos aprobados de máquinas y servicios, seguirá siendo necesario que el autor de la llamada presente un token de autorización válido.

## <a name="ip-access-control"></a><a id="ip-access-control-overview"></a> Control de acceso de IP

De forma predeterminada, la cuenta de Azure Cosmos DB es accesible desde Internet siempre y cuando la solicitud vaya acompañada de un token de autorización válido. Para configurar el control de acceso basado en directivas IP, el usuario debe proporcionar el conjunto de direcciones IP o intervalos de direcciones IP en formato CIDR (Enrutamiento de interdominios sin clases) para incluirlos en la lista de direcciones IP permitidas de cliente para acceder a una cuenta de Azure Cosmos dada. Una vez que se aplica esta configuración, todas las solicitudes que se originan en máquinas que están fuera de esta lista de permitidos reciben la respuesta 403 (Prohibido). Cuando se usa el firewall de IP, se recomienda permitir que Azure Portal acceda a su cuenta. Este acceso es necesario para permitir el uso del explorador de datos, y para recuperar las métricas de la cuenta que se muestran en Azure Portal. Cuando se usa el explorador de datos, además de permitir que Azure Portal acceda a su cuenta, también deberá actualizar la configuración del firewall para agregar la dirección IP actual a las reglas de firewall. Tenga en cuenta que los cambios en el firewall pueden tardar hasta 15 minutos en propagarse, y que el firewall puede presentar un comportamiento incoherente durante este período.

Puede combinar el firewall basado en IP con el control de acceso de red virtual y subred. Al combinarlos, puede limitar el acceso a cualquier origen que tenga una dirección IP pública o desde una subred específica dentro de la red virtual. Para más información sobre el control de acceso de red virtual y subred, consulte [Acceso a los recursos de Azure Cosmos DB desde redes virtuales](./how-to-configure-vnet-service-endpoint.md).

En resumen, siempre se necesita el token de autorización para acceder a una cuenta de Azure Cosmos. Si el firewall de IP y la lista de control de acceso (ACL) de red virtual no se han configurado, se puede acceder a la cuenta de Azure Cosmos con el token de autorización. Una vez que el firewall de IP o las listas ACL de red virtual, o ambos, están configurados en la cuenta de Azure Cosmos, solo las solicitudes que se originan en los orígenes especificados (y con el token de autorización) obtienen respuestas válidas. 

Puede proteger los datos almacenados en su cuenta de Azure Cosmos DB mediante el uso de firewalls de dirección IP. Azure Cosmos DB admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Puede establecer un firewall de dirección IP en la cuenta de Azure Cosmos DB mediante una de las maneras siguientes:

* Desde Azure Portal
* Mediante declaración al usar una plantilla de Azure Resource Manager
* Mediante programación a través de la CLI de Azure o Azure PowerShell al actualizar la propiedad **ipRangeFilter**

## <a name="configure-an-ip-firewall-by-using-the-azure-portal"></a><a id="configure-ip-policy"></a> Configuración de un firewall de dirección IP mediante Azure Portal

Para establecer la directiva de control de acceso de IP en Azure Portal, vaya a la página de la cuenta de Azure Cosmos DB y seleccione **Firewall y redes virtuales** en el menú de navegación. Cambie el valor **Permitir acceso desde** por **Redes seleccionadas** y, a continuación, seleccione **Guardar**.

![Captura de pantalla que muestra cómo abrir la página Firewall en Azure Portal](./media/how-to-configure-firewall/azure-portal-firewall.png)

Cuando el control de acceso de IP se activa, Azure Portal proporciona la capacidad para especificar direcciones IP, intervalos de direcciones IP y modificadores. Los modificadores permiten el acceso a otros servicios de Azure y a Azure Portal. En las secciones siguientes se proporcionan detalles acerca de estos modificadores.

> [!NOTE]
> Después de habilitar una directiva de control de acceso de dirección IP para su cuenta de Azure Cosmos DB, se rechazan todas las solicitudes a la cuenta de Azure Cosmos DB desde máquinas fuera de la lista permitida de rangos de direcciones IP. La exploración de los recursos de Azure Cosmos DB desde el portal también se bloquea para garantizar la integridad del control de acceso.

### <a name="allow-requests-from-the-azure-portal"></a>Permitir las solicitudes desde Azure Portal

Cuando se habilita una directiva de control de acceso de IP mediante programación, debe agregar la dirección IP correspondiente a la propiedad **ipRangeFilter** de Azure Portal para mantener el acceso. Las direcciones IP del portal son las siguientes:

|Region|Dirección IP|
|------|----------|
|Alemania|51.4.229.218|
|China|139.217.8.252|
|US Gov|52.244.48.71|
|Todas las demás regiones|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Para habilitar las solicitudes para acceder a Azure Portal, seleccione la opción **Permitir el acceso desde Azure Portal**, tal como se muestra en la captura de pantalla siguiente:

![Captura de pantalla que muestra cómo habilitar el acceso a Azure Portal](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Permitir las solicitudes de los centros de datos globales de Azure u otros orígenes dentro de Azure

Si tiene acceso a su cuenta de Azure Cosmos DB desde los servicios que no proporcionan un IP estático (por ejemplo, Azure Stream Analytics y Azure Functions), aún podrá usar el firewall de dirección IP para limitar el acceso. Para habilitar el acceso desde otros orígenes dentro de Azure, seleccione la opción **Aceptar conexiones desde centros de datos de Azure**, tal como se muestra en la captura de pantalla siguiente:

![Captura de pantalla que muestra cómo aceptar conexiones de los centros de datos de Azure](./media/how-to-configure-firewall/enable-azure-services.png)

Cuando se habilita esta opción, se agrega la dirección IP `0.0.0.0` a la lista de direcciones IP permitidas. La dirección IP `0.0.0.0` restringe las solicitudes a la cuenta de Azure Cosmos DB desde el intervalo de IP del centro de datos de Azure. Esta configuración no permite que ningún otro intervalo IP tenga acceso a la cuenta de Azure Cosmos DB.

> [!NOTE]
> Esta opción configura el firewall para permitir todas las solicitudes de Azure, incluidas las solicitudes de las suscripciones de otros clientes implementados en Azure. La lista de IP permitidos por esta opción es amplia, por lo que limita la eficacia de una directiva de firewall. Use esta opción solo si las solicitudes no se originan en IP estáticos ni subredes de redes virtuales. Si se elige esta opción, se permite automáticamente el acceso desde Azure Portal, ya que Azure Portal se implementa en Azure.

### <a name="requests-from-your-current-ip"></a>Solicitudes desde la dirección IP actual

Para simplificar el desarrollo, Azure Portal le ayuda a identificar y agregar la dirección IP de su equipo cliente a la lista de permitidos. Las aplicaciones que ejecutan su máquina, a continuación, pueden obtener acceso a su cuenta de Azure Cosmos DB.

El portal detecta automáticamente la dirección IP del cliente. Puede que sea la dirección IP cliente de la máquina, pero también la dirección IP de la puerta de enlace de red. Asegúrese de quitar esta dirección IP antes de llevar las cargas de trabajo a producción.

Para agregar su dirección IP actual a la lista de direcciones IP, seleccione **Agregar mi IP actual**. Después, seleccione **Guardar**.

:::image type="content" source="./media/how-to-configure-firewall/enable-current-ip.png" alt-text="Captura de pantalla que muestra cómo configurar un firewall para la dirección IP actual":::

### <a name="requests-from-cloud-services"></a>Solicitudes desde servicios en la nube

En Azure, los servicios en la nube son una forma muy común de hospedar la lógica de servicio de nivel intermedio mediante Azure Cosmos DB. Para permitir el acceso a su cuenta de Azure Cosmos DB desde un servicio en la nube, debe agregar la dirección IP pública del servicio en la nube a la lista de direcciones IP permitida asociada a dicha cuenta de Azure Cosmos DB mediante la [configuración de la directiva de control de acceso a la dirección IP](#configure-ip-policy). De esta forma se garantiza que todas las instancias de rol de servicios en la nube tengan acceso a la cuenta de Azure Cosmos DB.

Puede recuperar las direcciones IP de los servicios en la nube en Azure Portal, tal como se muestra en la siguiente captura de pantalla:

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses.png" alt-text="Captura de pantalla con la dirección IP pública de un servicio en la nube que se muestra en Azure Portal":::

Al escalar horizontalmente su servicio en la nube mediante la adición de instancias de rol, esas nuevas instancias tendrán acceso automáticamente a la cuenta de Azure Cosmos DB que forman parte del mismo servicio en la nube.

### <a name="requests-from-virtual-machines"></a>Solicitudes desde máquinas virtuales

También puede usar [máquinas virtuales](https://azure.microsoft.com/services/virtual-machines/) o [conjuntos de escalado de máquinas virtuales](../virtual-machine-scale-sets/overview.md) para hospedar servicios de nivel intermedio mediante Azure Cosmos DB. Para configurar la cuenta de Cosmos DB para que permita el acceso desde máquinas virtuales, debe configurar la dirección IP pública de la máquina virtual o el conjunto de escalado de máquinas virtuales como una de las direcciones IP permitidas para la cuenta de Azure Cosmos DB mediante la [configuración de la directiva de control de acceso IP](#configure-ip-policy).

Puede recuperar las direcciones IP para máquinas virtuales en Azure Portal, como se muestra en la captura de pantalla siguiente:

:::image type="content" source="./media/how-to-configure-firewall/public-ip-addresses-dns.png" alt-text="Captura de pantalla con una dirección IP pública de una máquina virtual que se muestra en Azure Portal":::

Al agregar instancias de máquina virtual al grupo, reciben automáticamente acceso a su cuenta de Azure Cosmos DB.

### <a name="requests-from-the-internet"></a>Solicitudes desde Internet

Cuando se obtiene acceso a la cuenta de Azure Cosmos DB desde un equipo de Internet, la dirección IP del cliente o el intervalo de direcciones IP de la máquina se debe agregar a la lista de direcciones IP permitida para dicha cuenta.

### <a name="add-outbound-rules-to-the-firewall"></a>Adición de reglas de salida al firewall

Para obtener acceso a la lista actual de intervalos IP de salida que se van a agregar a la configuración del firewall, consulte [Descarga de intervalos IP y etiquetas de servicio de Azure](https://www.microsoft.com/download/details.aspx?id=56519).

Para automatizar la lista, consulte [Uso de Service Tag Discovery API](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api).

## <a name="configure-an-ip-firewall-by-using-a-resource-manager-template"></a><a id="configure-ip-firewall-arm"></a>Configuración de un firewall de dirección IP mediante una plantilla de Resource Manager

Para configurar el control de acceso a su cuenta de Azure Cosmos DB, asegúrese de que la plantilla de Resource Manager especifica la propiedad **ipRules** con una matriz de intervalos IP permitidos. Si configura el Firewall de IP en una cuenta de Cosmos ya implementada, asegúrese de que la matriz `locations` coincida con la que está implementada actualmente. No se puede modificar simultáneamente la matriz `locations` y otras propiedades. Para más información y ejemplos sobre las plantillas de Azure Resource Manager para Azure Cosmos DB, consulte [Plantillas de Azure Resource Manager para Azure Cosmos DB](./templates-samples-sql.md).

> [!IMPORTANT]
> La propiedad **ipRules** se introdujo con la versión 2020-04-01 de la API. En su lugar, las versiones anteriores exponían una propiedad **ipRangeFilter**, que es una lista de direcciones IP separadas por comas.

En el ejemplo siguiente se muestra cómo la propiedad **ipRules** se expone en la versión 2020-04-01 o posterior de la API:

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2020-04-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRules": [
      {
        "ipAddressOrRange": "40.76.54.131"
      },
      {
        "ipAddressOrRange": "52.176.6.30"
      },
      {
        "ipAddressOrRange": "52.169.50.45"
      },
      {
        "ipAddressOrRange": "52.187.184.26"
      }
    ]
  }
}
```

Este es el mismo ejemplo para cualquier versión de la API anterior a 2020-04-01:

```json
{
  "type": "Microsoft.DocumentDB/databaseAccounts",
  "name": "[variables('accountName')]",
  "apiVersion": "2019-08-01",
  "location": "[parameters('location')]",
  "kind": "GlobalDocumentDB",
  "properties": {
    "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
    "locations": "[variables('locations')]",
    "databaseAccountOfferType": "Standard",
    "enableAutomaticFailover": "[parameters('automaticFailover')]",
    "ipRangeFilter":"40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
  }
}
```

## <a name="configure-an-ip-access-control-policy-by-using-the-azure-cli"></a><a id="configure-ip-firewall-cli"></a>Configuración de una directiva de control de acceso IP con la CLI de Azure

El comando siguiente muestra cómo crear una cuenta de Azure Cosmos DB con control de acceso por IP:

```azurecli-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
ipRangeFilter='192.168.221.17,183.240.196.255,40.76.54.131'

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='East US 2' failoverPriority=1 isZoneRedundant=False \
    --ip-range-filter $ipRangeFilter
```

## <a name="configure-an-ip-access-control-policy-by-using-powershell"></a><a id="configure-ip-firewall-ps"></a>Configuración de una directiva de control de acceso IP mediante PowerShell

En el script siguiente se muestra cómo crear una cuenta de Azure Cosmos DB con control de acceso IP:

```azurepowershell-interactive
# Create a Cosmos DB account with default values and IP Firewall enabled
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$ipRules = @("192.168.221.17","183.240.196.255","40.76.54.131")

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0; "isZoneRedundant"=False },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=False }
)

# Make sure there are no spaces in the comma-delimited list of IP addresses or CIDR ranges.
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "ipRules"=$ipRules
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2020-04-01" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a><a id="troubleshoot-ip-firewall"></a>Solución de problemas con una directiva de control de acceso por IP

Puede solucionar problemas con una directiva de control de acceso por IP mediante las siguientes opciones:

### <a name="azure-portal"></a>Azure portal

Al habilitar una directiva de control de acceso por IP para su cuenta de Azure Cosmos DB, bloquea todas las solicitudes a la cuenta desde máquinas fuera de la lista permitida de rangos de direcciones IP. Para habilitar las operaciones en el plano de los datos del portal, como explorar los contenedores y consultar los documentos, debe permitir expresamente el acceso a Azure Portal mediante el panel **Firewall** del portal.

### <a name="sdks"></a>SDK

Cuando tenga acceso a recursos de Azure Cosmos DB mediante SDK desde máquinas que no están en la lista de permitidos, se devolverá una respuesta genérica **403 Prohibido** sin más detalles. Compruebe la lista de direcciones IP permitidas para su cuenta y asegúrese de que se aplique la configuración de directiva correcta a su cuenta de Azure Cosmos DB.

### <a name="source-ips-in-blocked-requests"></a>Direcciones IP de origen en solicitudes bloqueadas

Habilite el registro de diagnóstico en su cuenta de Azure Cosmos DB. Estos registros muestran cada solicitud y respuesta. Los mensajes relacionados con el firewall se registran con un código de retorno 403. Al filtrar estos mensajes, puede ver las direcciones IP de origen para las solicitudes bloqueadas. Consulte [Registro de diagnóstico de Azure Cosmos DB](./monitor-cosmos-db.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Solicitudes de una subred con un punto de conexión de servicio para la base de datos Azure Cosmos DB habilitada

Las solicitudes de una subred en una red virtual que tiene habilitado un punto de conexión de servicio para Azure Cosmos DB envían la identidad de la red virtual y la subred a las cuentas de Azure Cosmos DB. Estas solicitudes no tienen la dirección IP pública del origen, de modo que los filtros de IP las rechazan. Para permitir el acceso desde subredes específicas en redes virtuales, agregue una lista de control de acceso que se describe en [Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md). Las reglas de firewall pueden tardar hasta 15 minutos en aplicarse y el firewall puede presentar un comportamiento incoherente durante este período.

### <a name="private-ip-addresses-in-list-of-allowed-addresses"></a>Direcciones IP privadas en la lista de direcciones permitidas

Se producirá un error al crear o actualizar una cuenta de Azure Cosmos con una lista de direcciones permitidas que contienen direcciones IP privadas. Asegúrese de que no se ha especificado ninguna dirección IP privada en la lista.

## <a name="next-steps"></a>Pasos siguientes

Para configurar un punto de conexión de servicio de red virtual para la cuenta de Azure Cosmos DB, consulte los artículos siguientes:

* [Control de acceso a una red virtual y subred para la cuenta de Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md)
* [Configuración del acceso basado en red virtual y subred para la cuenta de Azure Cosmos DB](how-to-configure-vnet-service-endpoint.md)
