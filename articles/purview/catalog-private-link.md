---
title: Uso de puntos de conexión privados para el acceso seguro a Purview
description: En este artículo se proporciona información general de alto nivel sobre cómo usar un punto de conexión privado para la cuenta de Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/19/2021
ms.openlocfilehash: dba76c0b2d25c8bc962dd847e0d2ffdaa5bdd3eb
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130234234"
---
# <a name="use-private-endpoints-for-your-azure-purview-account"></a>Uso de puntos de conexión privados para la cuenta de Azure Purview

> [!IMPORTANT]
> Si creó un punto de conexión privado del _portal_ para su cuenta de Purview **antes del 27 de septiembre de 2021 a las 15:30 UTC**, deberá realizar las acciones necesarias como se detalla en [Reconfiguración de DNS para puntos de conexión privados del portal](#reconfigure-dns-for-portal-private-endpoints). **Estas acciones deben completarse antes del 12 de noviembre de 2021. Si no lo hace, los puntos de conexión privados del portal existentes dejarán de funcionar**.


En este artículo se describe cómo configurar los puntos de conexión privados para Azure Purview.

## <a name="conceptual-overview"></a>Información conceptual
Puede usar [puntos de conexión privados de Azure](../private-link/private-endpoint-overview.md) para sus cuentas de Azure Purview, para que los usuarios de una red virtual (VNet) puedan acceder de forma segura al catálogo a través de una instancia de Private Link. Un punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual para la cuenta de Purview. El tráfico de red entre los clientes de la red virtual y la cuenta de Purview atraviesa la red virtual y un vínculo privado de la red troncal de Microsoft. 

Puede implementar el punto de conexión privado de la _cuenta_ de Azure Purview para permitir solo las llamadas de cliente a Azure Purview que se originan desde la red privada.

Para conectarse a Azure Purview Studio mediante una conectividad de red privada, puede implementar el punto de conexión privado del _portal_.

Puede implementar puntos de conexión privados de _ingesta_ si necesita examinar los orígenes de datos de IaaS y PaaS de Azure en las redes virtuales de Azure y los orígenes de datos locales a través de una conexión privada. Este método garantiza el aislamiento de red de los metadatos que fluyen desde los orígenes de datos al Mapa de datos de Azure Purview.

:::image type="content" source="media/catalog-private-link/purview-private-link-overview.png" alt-text="Captura de pantalla que muestra Azure Purview con puntos de conexión privados."::: 

## <a name="prerequisites"></a>Requisitos previos

Antes de implementar puntos de conexión privados para la cuenta de Azure Purview, asegúrese de cumplir los siguientes requisitos previos:

1. Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
<br>
2. Una red virtual de Azure existente. Implementación de una nueva [red virtual de Azure](../virtual-network/quick-create-portal.md), si aún no tiene ninguna.
<br>

## <a name="azure-purview-private-endpoint-deployment-scenarios"></a>Escenarios de implementación de puntos de conexión privados de Azure Purview

Use la siguiente lista de comprobación recomendada para hacer la implementación de la cuenta de Azure Purview con puntos de conexión privados:

|Escenario  |Objetivos  |
|---------|---------|
|**Escenario 1** - [Conexión a Azure Purview y examen de los orígenes de datos de forma privada y segura](./catalog-private-link-end-to-end.md) |Debe restringir el acceso a la cuenta de Azure Purview solo a través de un punto de conexión privado, incluido el acceso a Azure Purview Studio, las API de Atlas y el examen de orígenes de datos en el entorno local y Azure detrás de una red virtual mediante el entorno de ejecución de integración autohospedado, lo que garantiza el aislamiento de red de un extremo a otro. (Implementación de puntos de conexión privados de _cuenta_, _portal_ e _ingesta_).   |
|**Escenario 2** - [Conexión de forma privada y segura a la cuenta de Purview](./catalog-private-link-account-portal.md)   | Debe habilitar el acceso a la cuenta de Azure Purview, incluido el acceso a _Azure Purview Studio_ y la API de Atlas mediante puntos de conexión privados. (Implementación de puntos de conexión privados de _cuenta_ y _portal_).   |

## <a name="support-matrix-for-scanning-data-sources-through-_ingestion_-private-endpoint"></a>Matriz de compatibilidad para examinar orígenes de datos mediante el punto de conexión privado de _ingesta_

En escenarios en los que se usa el punto de conexión privado de _ingesta_ en la cuenta de Azure Purview y el acceso público a los orígenes de datos está deshabilitado, Azure Purview puede examinar los siguientes orígenes de datos que están detrás de un punto de conexión privado:

|Origen de datos detrás de un punto de conexión privado  |Tipo de entorno de ejecución de integración  |Tipo de credencial  |
|---------|---------|---------|
|Azure Blob Storage | IR autohospedado | Entidad de servicio|
|Azure Blob Storage | IR autohospedado | Clave de cuenta|
|Azure Data Lake Storage Gen 2 | IR autohospedado| Entidad de servicio|
|Azure Data Lake Storage Gen 2 | IR autohospedado| Clave de cuenta|
|Azure SQL Database | IR autohospedado| Autenticación de SQL|
|Azure SQL Database | IR autohospedado| Entidad de servicio|
|Instancia administrada de Azure SQL | IR autohospedado| Autenticación de SQL|
|Azure Cosmos DB| IR autohospedado| Clave de cuenta|
|SQL Server | IR autohospedado| Autenticación de SQL|
|Azure Synapse Analytics | IR autohospedado| Entidad de servicio|
|Azure Synapse Analytics | IR autohospedado| Autenticación de SQL|

## <a name="reconfigure-dns-for-portal-private-endpoints"></a>Reconfiguración de DNS para puntos de conexión privados del portal

Si creó un punto de conexión privado del _portal_ para su cuenta de Purview **antes del 27 de septiembre de 2021 a las 15:30 UTC**, deberá realizar las acciones necesarias como se detalla en esta sección.

### <a name="review-your-current-dns-settings"></a>Revisión de la configuración de DNS actual

1. En Azure Portal, busque la cuenta de Purview. En el menú de la izquierda, haga clic en **Redes** y seleccione **Conexiones de punto de conexión privado**. Haga clic en cada punto de conexión privado de la lista y siga los pasos que se indican a continuación.

    :::image type="content" source="media/catalog-private-link/purview-pe-dns-updates-1.png" alt-text="Captura de pantalla que muestra el punto de conexión privado de Purview."lightbox="media/catalog-private-link/purview-pe-dns-updates-1.png":::

2. Si el recurso secundario de destino es el _portal_, revise la **configuración de DNS**; de lo contrario, vuelva al paso anterior y seleccione el siguiente punto de conexión privado hasta que haya revisado todos los puntos de conexión privados y haya validado todos los puntos de conexión privados asociados al portal.

    :::image type="content" source="media/catalog-private-link/purview-pe-dns-updates-2.png" alt-text="Captura de pantalla que muestra el punto de conexión privado de Purview del portal."lightbox="media/catalog-private-link/purview-pe-dns-updates-2.png":::

3. En la ventana **Configuración de DNS**, compruebe los siguientes valores:
   
    - Si hay registros en la sección **Registros DNS personalizados**, siga los pasos que se describen en [Escenarios de corrección 1](#scenario-1) y [Escenario de corrección 2](#scenario-2).
    
        :::image type="content" source="media/catalog-private-link/purview-pe-dns-updates-3.png" alt-text="Captura de pantalla que muestra la configuración de DNS personalizada del punto de conexión privado de Purview del portal."lightbox="media/catalog-private-link/purview-pe-dns-updates-3.png":::

    - Si hay registros en la sección **Nombre de configuración** y la zona DNS es `privatelink.purviewstudio.azure.com`, no se requieren acciones para este punto de conexión privado. Vuelva al **paso 1** y revise los restantes puntos de conexión privados del portal.
  
        :::image type="content" source="media/catalog-private-link/purview-pe-dns-updates-4.png" alt-text="Captura de pantalla que muestra el punto de conexión privado de Purview del portal con la nueva zona DNS."lightbox="media/catalog-private-link/purview-pe-dns-updates-4.png":::
    
    - Si hay registros en la sección **Nombre de configuración** y la zona DNS es `privatelink.purview.azure.com`, siga los pasos que se describen en [Escenario de corrección 3](#scenario-3).

        :::image type="content" source="media/catalog-private-link/purview-pe-dns-updates-5.png" alt-text="Captura de pantalla que muestra el punto de conexión privado de Purview del portal con la anterior zona DNS."lightbox="media/catalog-private-link/purview-pe-dns-updates-5.png":::

### <a name="remediation-scenarios"></a>Escenarios de corrección

#### <a name="scenario-1"></a>Escenario 1

Si **ha añadido directamente los registros A de DNS necesarios al sistema DNS o al archivo host de las máquinas**, **no se requiere ninguna acción**.
    
:::image type="content" source="media/catalog-private-link/purview-pe-dns-updates-host.png" alt-text="Captura de pantalla que muestra el archivo host con registros A."lightbox="media/catalog-private-link/purview-pe-dns-updates-host.png":::

#### <a name="scenario-2"></a>Escenario 2

Si **ha configurado servidores DNS locales**, **reenviadores DNS o resolución DNS personalizada**, revise la configuración de DNS y tome las acciones adecuadas:

1. Revise el servidor DNS. Si el registro DNS es `web.purview.azure.com` o el reenviador condicional es `purview.azure.com`, **no se requiere ninguna acción**. 

2. Si el registro DNS es `web.privatelink.purview.azure.com`, actualice el registro a `web.privatelink.purviewstudio.azure.com`.

3. Si el reenviador condicional es `privatelink.purview.azure.com`, NO QUITE la zona. Debe agregar un nuevo reenviador condicional a `privatelink.purviewstudio.azure.com`.

#### <a name="scenario-3"></a>Escenario 3

Si ha configurado la **integración de zonas DNS privadas de Azure para la cuenta de Purview**, siga estos pasos a fin de volver a implementar los puntos de conexión privados para volver a definir la configuración de DNS:

1. Implemente un nuevo punto de conexión privado del portal:
       
    1. Vaya a [Azure Portal](https://portal.azure.com) y, a continuación, haga clic en su cuenta de Azure Purview. En **Configuración**, seleccione **Redes** y luego **Conexiones de punto de conexión privado**.

        :::image type="content" source="media/catalog-private-link/purview-pe-reconfigure-portal.png" alt-text="Captura de pantalla que muestra cómo crear un punto de conexión privado del portal."lightbox="media/catalog-private-link/purview-pe-reconfigure-portal.png":::

    2. Seleccione **+ Punto de conexión privado** para crear uno.

    3. Rellene la información básica.

    4. En la pestaña **Recurso**, seleccione **Microsoft.Purview/account** como **Tipo de recurso**.

    5. En **Recurso**, seleccione la cuenta de Azure Purview y en **Subrecurso de destino**, seleccione **portal**.

    6. En la pestaña **Configuración**, seleccione la red virtual y, a continuación, seleccione la zona DNS privada de Azure para crear una nueva zona DNS de Azure.
            
        :::image type="content" source="media/catalog-private-link/purview-pe-reconfigure-portal-dns.png" alt-text="Captura de pantalla que muestra cómo crear un punto de conexión privado del portal y una configuración de DNS."lightbox="media/catalog-private-link/purview-pe-reconfigure-portal-dns.png":::

    7. Vaya a la página de resumen y seleccione **Crear** para crear el punto de conexión privado del portal.

2. Elimine el punto de conexión privado anterior del portal asociado a la cuenta de Purview. 

3. Asegúrese de que se cree una nueva zona DNS privada de Azure, `privatelink.purviewstudio.azure.com`, durante la implementación del punto de conexión privado del portal y de que exista un registro A (web) correspondiente en la zona DNS privada. 
    
4. Asegúrese de que puede cargar correctamente Azure Purview Studio. El nuevo enrutamiento DNS puede tardar unos minutos (10, aproximadamente) en surtir efecto después de volver a configurar el DNS. Puede esperar unos minutos e intentarlo de nuevo si no se carga inmediatamente.
    
5. Si se produce un error en la navegación, realice la acción nslookup web.purview.azure.com, que debe resolverse en una dirección IP privada asociada al punto de conexión privado del portal.
  
6. Repita los pasos del 1 al 3 señalados anteriormente para todos los puntos de conexión privados existentes del portal que tenga. 

### <a name="validation-steps"></a>Pasos de validación

1. Asegúrese de que puede cargar correctamente Azure Purview Studio. El nuevo enrutamiento DNS puede tardar unos minutos (10, aproximadamente) en surtir efecto después de volver a configurar el DNS. Puede esperar unos minutos e intentarlo de nuevo si no se carga inmediatamente.

2. Si se produce un error en la navegación, ejecute nslookup `web.purview.azure.com`, que debe resolverse en una dirección IP privada asociada al punto de conexión privado del portal.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes  

Para obtener preguntas más frecuentes relacionadas con las implementaciones de puntos de conexión privados en Azure Purview, consulte [Preguntas más frecuentes sobre los puntos de conexión privados de Azure Purview](./catalog-private-link-faqs.md).
 
## <a name="troubleshooting-guide"></a>Guía de solución de problemas 
Para solucionar problemas de configuración de puntos de conexión privados para cuentas de Purview, consulte [Solución de problemas de configuración de punto de conexión privado para cuentas de Purview](./catalog-private-link-troubleshoot.md).

## <a name="known-limitations"></a>Limitaciones conocidas
Para ver la lista de limitaciones actuales relacionadas con los puntos de conexión privados de Azure Purview, consulte [Limitaciones conocidas de los puntos de conexión privados de Azure Purview](./catalog-private-link-troubleshoot.md#known-limitations).

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de redes privadas de un extremo a otro](./catalog-private-link-end-to-end.md)
- [Implementación de redes privadas para Purview Studio](./catalog-private-link-account-portal.md)
