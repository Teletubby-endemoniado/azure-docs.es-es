---
title: Red virtual administrada y puntos de conexión privados administrados
description: Obtenga información sobre la red virtual administrada y los puntos de conexión privados administrados en Azure Data Factory.
ms.author: lle
author: lrtoyou1223
ms.service: data-factory
ms.subservice: integration-runtime
ms.topic: conceptual
ms.custom: seo-lt-2019, references_regions, devx-track-azurepowershell
ms.date: 09/28/2021
ms.openlocfilehash: f9c07abdfe512c2564fdfe1595f16db8a6372a8b
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129230246"
---
# <a name="azure-data-factory-managed-virtual-network"></a>Red virtual administrada de Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este artículo se explica la red virtual administrada y los puntos de conexión privados administrados en Azure Data Factory.


## <a name="managed-virtual-network"></a>Red virtual administrada

Al crear una instancia de Azure Integration Runtime (IR) en la red virtual (VNET) administrada de Azure Data Factory, el entorno de ejecución de integración se aprovisionará con la red virtual administrada y aprovechará los puntos de conexión privados para conectarse de forma segura a almacenes de datos compatibles. 

La creación de una instancia de Azure IR dentro de una red virtual administrada garantiza que el proceso de integración de datos esté aislado y protegido. 

Ventajas del uso de una red virtual administrada:

- Con una red virtual administrada puede dejar que Azure Data Factory se ocupe de la pesada tarea de administrar la red virtual. No es necesario crear una subred para Azure Integration Runtime que podría usar muchas direcciones IP privadas de la red virtual y requeriría una planeación previa de la infraestructura de la red. 
- No se necesita un conocimiento profundo de las redes de Azure para realizar integraciones de datos de forma segura. En su lugar, empezar a utilizar ETL seguro es mucho más sencillo para los ingenieros de datos. 
- La red virtual administrada, junto con los puntos de conexión privados administrados, sirven de protección contra la filtración de datos. 

> [!IMPORTANT]
>Actualmente, la red virtual administrada solo se admite en la misma región que Azure Data Factory.

> [!Note]
>El entorno público de ejecución de integración de Azure existente no puede cambiar al entorno de ejecución de Azure en la red virtual administrada de Azure Data Factory y viceversa.
 

:::image type="content" source="./media/managed-vnet/managed-vnet-architecture-diagram.png" alt-text="Arquitectura de la red virtual de Azure Data Factory":::

## <a name="managed-private-endpoints"></a>Puntos de conexión privados administrados

Los puntos de conexión privados administrados se crean en la red virtual administrada de Azure Data Factory y establecen un vínculo privado a recursos de Azure. Azure Data Factory administra estos puntos de conexión privados en su nombre. 

:::image type="content" source="./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png" alt-text="Nuevo punto de conexión privado administrado":::

Azure Data Factory admite vínculos privados. El vínculo privado le permite acceder a servicios de Azure (PaaS) (como Azure Storage, Azure Cosmos DB, Azure Synapse Analytics).

Cuando se usa un vínculo privado, el tráfico entre los almacenes de datos y la red virtual administrada atraviesa completamente la red troncal de Microsoft. Private Link protege frente a los riesgos de la filtración de datos. El vínculo privado a un recurso se establece mediante la creación de un punto de conexión privado.

El punto de conexión privado usa una dirección IP privada en la red virtual administrada para incorporar el servicio de manera eficaz a la red virtual. Los puntos de conexión privados se asignan a un recurso específico de Azure, no a todo el servicio. Los clientes pueden limitar la conectividad a un recurso específico aprobado por su organización. Más información sobre [vínculos privados y puntos de conexión privados](../private-link/index.yml).

> [!NOTE]
> Se recomienda crear puntos de conexión privados administrados para conectarse a todos los orígenes de datos de Azure. 
 
> [!WARNING]
> Si un almacén de datos de PaaS (Blob, ADLS Gen2 o Azure Synapse Analytics) tiene un punto de conexión privado que ya se ha creado, aunque permita el acceso desde todas las redes, ADF solo podrá acceder a dicho almacén mediante un punto de conexión privado administrado. Si aún no existe un punto de conexión privado, debe crear uno en estos escenarios. 

Una conexión de punto de conexión privado se crea con estado "pendiente" cuando se crea un punto de conexión privado administrado en Azure Data Factory. Se inicia un flujo de trabajo de aprobación. El propietario del recurso de vínculo privado es responsable de aprobar o rechazar la conexión.

:::image type="content" source="./media/tutorial-copy-data-portal-private/manage-private-endpoint.png" alt-text="Administración de un punto de conexión privado":::

Si el propietario aprueba la conexión, se establece el vínculo privado. De lo contrario, no se establece. En cualquier caso, el punto de conexión privado administrado se actualizará con el estado de la conexión.

:::image type="content" source="./media/tutorial-copy-data-portal-private/approve-private-endpoint.png" alt-text="Punto de conexión privado administrado aprobado":::

Solo un punto de conexión privado administrado en un estado aprobado puede enviar tráfico a un recurso de vínculo privado determinado.

## <a name="interactive-authoring"></a>Creación interactiva
Entre las funcionalidades de la creación interactiva se incluyen probar la conexión, examinar la lista de carpetas y la lista de tablas, obtener esquemas y obtener una vista previa de los datos. Puede habilitar la creación interactiva al crear o editar una instancia de Azure Integration Runtime que se encuentre en una red virtual administrada por ADF. El servicio de back-end asignará previamente el proceso para las funcionalidades de creación interactiva. De lo contrario, el proceso se asignará cada vez que se realice cualquier operación interactiva, lo que tardará más tiempo. El período de vida (TTL) para la creación interactiva es de 60 minutos, lo que significa que se deshabilitará automáticamente después de 60 minutos de la última operación de creación interactiva.

:::image type="content" source="./media/managed-vnet/interactive-authoring.png" alt-text="Creación interactiva":::

## <a name="activity-execution-time-using-managed-virtual-network"></a>Tiempo de ejecución de la actividad mediante una red virtual administrada
Por diseño, el entorno de ejecución de integración de Azure en la red virtual administrada tiene un tiempo en la cola más largo que el entorno de ejecución de Azure, ya que no se reserva un nodo de proceso por factoría de datos, por lo que hay una preparación antes de que se inicie cada actividad y se produce principalmente en la unión a una red virtual y no al entorno de ejecución de integración de Azure. En el caso de las actividades que no son de copia, incluida la actividad de canalización y la actividad externa, hay un período de vida (TTL) de 60 minutos al desencadenarlas por primera vez. Dentro de TTL, el tiempo de cola es más corto porque el nodo ya está preparado. 
> [!NOTE]
> La actividad de copia todavía no tiene compatibilidad con TTL.

> [!NOTE]
> No se admiten 2 DIU para actividad de copia en la red virtual administrada.

## <a name="create-managed-virtual-network-via-azure-powershell"></a>Creación de una red virtual administrada mediante Azure PowerShell
```powershell
$subscriptionId = ""
$resourceGroupName = ""
$factoryName = ""
$managedPrivateEndpointName = ""
$integrationRuntimeName = ""
$apiVersion = "2018-06-01"
$privateLinkResourceId = ""

$vnetResourceId = "subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/managedVirtualNetworks/default"
$privateEndpointResourceId = "subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/managedVirtualNetworks/default/managedprivateendpoints/${managedPrivateEndpointName}"
$integrationRuntimeResourceId = "subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/integrationRuntimes/${integrationRuntimeName}"

# Create managed Virtual Network resource
New-AzResource -ApiVersion "${apiVersion}" -ResourceId "${vnetResourceId}" -Properties @{}

# Create managed private endpoint resource
New-AzResource -ApiVersion "${apiVersion}" -ResourceId "${privateEndpointResourceId}" -Properties @{
        privateLinkResourceId = "${privateLinkResourceId}"
        groupId = "blob"
    }

# Create integration runtime resource enabled with VNET
New-AzResource -ApiVersion "${apiVersion}" -ResourceId "${integrationRuntimeResourceId}" -Properties @{
        type = "Managed"
        typeProperties = @{
            computeProperties = @{
                location = "AutoResolve"
                dataFlowProperties = @{
                    computeType = "General"
                    coreCount = 8
                    timeToLive = 0
                }
            }
        }
        managedVirtualNetwork = @{
            type = "ManagedVirtualNetworkReference"
            referenceName = "default"
        }
    }

```

## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos
### <a name="supported-data-sources"></a>Orígenes de datos compatibles
Los orígenes de datos siguientes tienen compatibilidad nativa con puntos de conexión privados y se pueden conectar a través de Private Link desde una red virtual administrada por ADF.
- Azure Blob Storage (sin incluir la cuenta de almacenamiento V1)
- Azure Cognitive Search
- API de SQL de Azure Cosmos DB
- Azure Data Lake Storage Gen2
- Azure Database for MariaDB
- Azure Database for MySQL
- Azure Database for PostgreSQL
- Azure Files (sin incluir la cuenta de almacenamiento V1)
- Azure Key Vault
- Azure Machine Learning
- Servicio Azure Private Link
- Azure Purview
- Azure SQL Database (sin incluir Azure SQL Managed Instance)
- Azure Synapse Analytics
- Azure Table Storage (sin incluir la cuenta de almacenamiento V1)

> [!Note]
> Todavía puede acceder a todos los orígenes de datos admitidos por Data Factory a través de la red pública.

> [!NOTE]
> Dado que Azure SQL Managed Instance no admite por el momento puntos de conexión privados nativos, puede acceder a este servicio desde la red virtual administrada mediante el servicio Private Link y Load Balancer. Consulte [Tutorial: Acceso a SQL Managed Instance desde una VNET administrada de Data Factory mediante un punto de conexión privado](tutorial-managed-virtual-network-sql-managed-instance.md).

### <a name="on-premises-data-sources"></a>Orígenes de datos locales
Para acceder a orígenes de datos locales desde una red virtual administrada mediante un punto de conexión privado, consulte [Tutorial: Acceso a SQL Server local desde una red virtual administrada por Data Factory mediante un punto de conexión privado](tutorial-managed-virtual-network-on-premise-sql-server.md).

### <a name="azure-data-factory-managed-virtual-network-is-available-in-the-following-azure-regions"></a>La característica Managed Virtual Network de Azure Data Factory está disponible en las siguientes regiones de Azure:
- Este de Australia
- Sudeste de Australia
- Sur de Brasil
- Centro de Canadá
- Este de Canadá
- Centro de la India
- Centro de EE. UU.
- Este de China 2
- Norte de China 2
- Este de Asia
- Este de EE. UU.
- Este de EE. UU. 2
- Centro de Francia
- Centro-oeste de Alemania
- Japón Oriental
- Japón Occidental
- Centro de Corea del Sur
- Centro-Norte de EE. UU
- Norte de Europa
- Este de Noruega
- Norte de Sudáfrica
- Centro-sur de EE. UU.
- Sudeste de Asia
- Norte de Suiza
- Norte de Emiratos Árabes Unidos
- US Gov: Arizona
- US Gov Texas
- US Gov - Virginia
- Sur de Reino Unido
- Oeste de Reino Unido
- Centro-Oeste de EE. UU.
- Oeste de Europa
- Oeste de EE. UU.
- Oeste de EE. UU. 2


### <a name="outbound-communications-through-public-endpoint-from-adf-managed-virtual-network"></a>Comunicaciones salientes a través del punto de conexión público desde la red virtual administrada de ADF
- Se abren todos los puertos para las comunicaciones salientes.
- Azure Storage y Azure Data Lake Gen2 no pueden establecer una conexión a través del punto de conexión público desde la red virtual administrada de ADF.

### <a name="linked-service-creation-of-azure-key-vault"></a>Creación de un servicio vinculado para Azure Key Vault 
- Cuando se crea un servicio vinculado para Azure Key Vault, no existe ninguna referencia a Azure Integration Runtime. Por este motivo, no se puede generar un punto de conexión privado cuando se crea un servicio vinculado para Azure Key Vault. Sin embargo, si se crea un servicio vinculado para almacenes de datos que hace referencia a un servicio vinculado de Azure Key Vault y dicho servicio vinculado hace referencia a Azure Integration Runtime con la característica Managed Virtual Network habilitada, puede crear un punto de conexión privado para el servicio vinculado de Azure Key Vault durante la creación. 
- La operación **Prueba de conexión** del servicio vinculado de Azure Key Vault solo valida el formato de la dirección URL, pero no realiza ninguna operación de red.
- La columna **Using private endpoint** (Uso de punto de conexión privado) siempre se muestra en blanco, incluso si crea un punto de conexión privado para Azure Key Vault.

### <a name="linked-service-creation-of-azure-hdi"></a>Creación de un servicio vinculado de Azure HDI
- La columna **Using private endpoint** (Uso del punto de conexión privado) siempre aparece en blanco, aun cuando se haya creado un punto de conexión privado para HDI usando el servicio Private Link y un equilibrador de carga con reenvío de puertos.

:::image type="content" source="./media/managed-vnet/akv-pe.png" alt-text="Punto de conexión privado para AKV":::

## <a name="next-steps"></a>Pasos siguientes

- Tutorial: [Creación de una canalización de copia mediante la red virtual administrada y los puntos de conexión privados](tutorial-copy-data-portal-private.md) 
- Tutorial: [Creación de una canalización de flujo de datos de asignación mediante la red virtual administrada y los puntos de conexión privados](tutorial-data-flow-private.md)
