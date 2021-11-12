---
title: Hospedaje de varios sitios en Azure Application Gateway
description: Este artículo proporciona información general de la compatibilidad multisitio de Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 08/31/2021
ms.author: azhussai
ms.topic: conceptual
ms.openlocfilehash: 5feb112a9d1c9b7eb229c65f8bcce3845a3f23ad
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131844988"
---
# <a name="application-gateway-multiple-site-hosting"></a>Hospedaje de varios sitios de Application Gateway

El hospedaje de varios sitios permite configurar más de una aplicación web en el mismo puerto de puertas de enlace de aplicaciones mediante agentes de escucha de acceso público. Permite configurar una topología más eficaz para las implementaciones al agregar hasta 100 sitios web a una puerta de enlace de aplicaciones. Cada sitio web se puede dirigir a su propio grupo de back-end. Por ejemplo, tres dominios, contoso.com, fabrikam.com y adatum.com, señalan a la dirección IP de la puerta de enlace de aplicaciones. Crearía tres clientes de escucha multisitio y configuraría cada uno con la configuración respectiva de protocolo y puerto. 

También puede definir nombres de host con el carácter comodín en un cliente de escucha de varios sitios y hasta cinco nombres de host por cliente de escucha. Para obtener más información, consulte los [nombres de host comodín en el cliente de escucha](#wildcard-host-names-in-listener-preview).

:::image type="content" source="./media/multiple-site-overview/multisite.png" alt-text="Instancia de Application Gateway multisitio":::

> [!IMPORTANT]
> La reglas se procesan en el orden en que aparecen en el portal para la SKU v1. Para la SKU v2, use la [prioridad de las reglas](#request-routing-rules-evaluation-order) para especificar el orden de procesamiento. Es muy recomendable configurar a los agentes de escucha multisitio antes de configurar un agente de escucha básico.  De esta forma se asegura de que el tráfico se enruta al back-end adecuado. Si un agente de escucha básico aparece en primer lugar y coincide con una solicitud entrante, lo procesa ese agente de escucha.

Las solicitudes para `http://contoso.com` se enrutan a ContosoServerPool y para `http://fabrikam.com` se enrutan a FabrikamServerPool.

De forma similar, puede hospedar varios subdominios del mismo dominio principal en la misma implementación de la puerta de enlace de aplicaciones. Por ejemplo, puede hospedar `http://blog.contoso.com` y `http://app.contoso.com` en la misma implementación de puerta de enlace de aplicaciones.

## <a name="request-routing-rules-evaluation-order"></a>Orden de evaluación de las reglas de enrutamiento de solicitudes

Cuando use clientes de escucha de varios sitios, para asegurarse de que el tráfico del cliente se enruta al back-end preciso, es importante que las reglas de enrutamiento de solicitudes se presenten en el orden correcto.
Por ejemplo, si tiene 2 clientes de escucha con nombres de host asociados como `*.contoso.com` y `shop.contoso.com` respectivamente, el cliente de escucha con el nombre de host `shop.contoso.com` tendría que procesarse antes que el cliente de escucha con `*.contoso.com`. Si el cliente de escucha con `*.contoso.com` se procesa primero, el cliente de escucha más específico `shop.contoso.com` no recibiría tráfico de clientes.

Esta ordenación se puede establecer proporcionando un valor de campo "Prioridad" a las reglas de enrutamiento de solicitudes asociadas a los clientes de escucha. Puede especificar un valor entero de 1 a 20000, siendo 1 la prioridad más alta y 20000 la prioridad más baja. En caso de que el tráfico de cliente entrante coincida con varios clientes de escucha, se usará la regla de enrutamiento de solicitudes con la prioridad más alta para atender la solicitud. Cada regla de enrutamiento de solicitudes debe tener un valor de prioridad único.

El campo de prioridad solo afecta al orden de evaluación de una regla de enrutamiento de solicitudes; esto no cambiará el orden de evaluación de las reglas basadas en rutas de acceso dentro de una regla de enrutamiento de solicitudes `PathBasedRouting`.

>[!NOTE]
>Esta característica solo está disponible actualmente a través de [Azure PowerShell](tutorial-multiple-sites-powershell.md#add-priority-to-routing-rules) y la [CLI de Azure](tutorial-multiple-sites-cli.md#add-priority-to-routing-rules). Próximamente se agregará la compatibilidad con el portal.

>[!NOTE]
>Si desea usar la prioridad de regla, tendrá que especificar valores de campo de prioridad de regla para todas las reglas de enrutamiento de solicitudes existentes. Una vez que el campo de prioridad de la regla esté en uso, cualquier nueva regla de enrutamiento que se cree también tendría que tener un valor de campo de prioridad de regla como parte de su configuración.

## <a name="wildcard-host-names-in-listener-preview"></a>Nombres de host con el carácter comodín en los clientes de escucha (versión preliminar)

Application Gateway permite el enrutamiento basado en host mediante un cliente de escucha HTTP(S) de varios sitios. Ahora, puede usar caracteres comodín como el asterisco (*) y el signo de interrogación (?) en el nombre del host y hasta 5 nombres de host por cliente de escucha HTTPS(S) de varios sitios. Por ejemplo, `*.contoso.com`.

Con un carácter comodín en el nombre del host, puede hacer coincidir varios nombres de host en un único cliente de escucha. Por ejemplo, `*.contoso.com` puede coincidir con `ecom.contoso.com`, `b2b.contoso.com` y `customer1.b2b.contoso.com` y así sucesivamente. Con una matriz de nombres de host, puede configurar más de un nombre de host para un cliente de escucha, para enrutar las solicitudes a un grupo de back-end. Por ejemplo, un cliente de escucha puede contener `contoso.com, fabrikam.com`, lo que aceptará las solicitudes para ambos nombres de host.

:::image type="content" source="./media/multiple-site-overview/wildcard-listener-diag.png" alt-text="Cliente de escucha comodín":::

>[!NOTE]
> Esta característica está en versión preliminar y solo está disponible para las SKU Standard_v2 y WAF_v2 de Application Gateway. Para obtener más información sobre las versiones preliminares, consulte los [términos de uso aquí](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En [Azure PowerShell](tutorial-multiple-sites-powershell.md), debe usar `-HostNames` en lugar de `-HostName`. Con los nombres de host, puede mencionar un máximo de cinco nombres de host como valores separados por comas y usar caracteres comodín. Por ejemplo: `-HostNames "*.contoso.com","*.fabrikam.com"`

En la [CLI de Azure](tutorial-multiple-sites-cli.md), debe usar `--host-names` en lugar de `--host-name`. Con los nombres de host, puede mencionar un máximo de cinco nombres de host como valores separados por comas y usar caracteres comodín. Por ejemplo: `--host-names "*.contoso.com,*.fabrikam.com"`

En Azure Portal, en el cliente de escucha de varios sitios, debe elegir el host de tipo **varios o comodín** para mencionar hasta cinco nombres de host con caracteres comodín permitidos.

:::image type="content" source="./media/multiple-site-overview/wildcard-listener-example.png" alt-text="IU de cliente de escucha comodín":::

### <a name="allowed-characters-in-the-host-names-field"></a>Caracteres permitidos en el campo de nombres de host

* `(A-Z,a-z,0-9)` -caracteres alfanuméricos
* `-` -guion o menos
* `.` -punto como delimitador
* `*` -puede coincidir con varios caracteres del intervalo permitido
* `?` -puede coincidir con un único carácter del intervalo permitido

### <a name="conditions-for-using-wildcard-characters-and-multiple-host-names-in-a-listener"></a>Condiciones para usar caracteres comodín y varios nombres de host en un cliente de escucha

* Solo puede mencionar hasta cinco nombres de host en un único cliente de escucha.
* Los asteriscos `*` solo se pueden mencionar una vez en un componente de nombre de estilo de dominio o nombre de host. Por ejemplo, componente1 *.componente2*.componente3. `(*.contoso-*.com)` es válido.
* Solo puede haber hasta dos asteriscos `*` en un nombre de host. Por ejemplo, `*.contoso.*` es válido y `*.contoso.*.*.com` no es válido.
* Solo puede haber un máximo de 4 caracteres comodín en un nombre de host. Por ejemplo, `????.contoso.com`, `w??.contoso*.edu.*` son válidos, pero `????.contoso.*` no es válido.
* El uso del asterisco `*` y el signo de interrogación `?` juntos en un componente de un nombre de host (`*?`, `?*` o `**`) no es válido. Por ejemplo, `*?.contoso.com` and `**.contoso.com` no son válidos.

### <a name="considerations-and-limitations-of-using-wildcard-or-multiple-host-names-in-a-listener"></a>Consideraciones y limitaciones del uso de caracteres comodín o varios nombres de host en un cliente de escucha

* [La terminación SSL y SSL de un extremo a otro](ssl-overview.md) requieren que configure el protocolo como HTTPS y cargue un certificado que se usará en la configuración del cliente de escucha. Si se trata de un cliente de escucha de varios sitios, también puede especificar el nombre de host, que normalmente es el CN del certificado SSL. Cuando se especifican varios nombres de host en el cliente de escucha o se usan caracteres comodín, se debe tener en cuenta lo siguiente:
    * Si es un nombre de host comodín como *.contoso.com, debe cargar un certificado comodín con CN como *.contoso.com
    * Si se mencionan varios nombres de host en el mismo cliente de escucha, debe cargar un certificado SAN (nombres alternativos del firmante) con CN que coincidan con los nombres de host mencionados.
* No se puede usar una expresión regular para mencionar el nombre de host. Solo se pueden usar caracteres comodín como el asterisco (*) y el signo de interrogación (?) para formar el patrón de nombre de host.
* Para la comprobación de estado del back-end, no se pueden asociar varios [sondeos personalizados](application-gateway-probe-overview.md) por configuración HTTP. En su lugar, puede sondear uno de los sitios web en el back-end o usar "127.0.0.1" para sondear el localhost del servidor backend. Sin embargo, cuando se usan caracteres comodín o varios nombres de host en un cliente de escucha, las solicitudes de todos los patrones de dominio especificados se enrutarán al grupo de back-end en función del tipo de regla (básica o basada en la ruta de acceso).
* Las propiedades "hostname" toman una cadena como entrada, en las que solo puede mencionar un nombre de dominio sin caracteres comodín y "hostnames" toma una matriz de cadenas como entrada, en la puede mencionar hasta 5 nombres de dominio con caracteres comodín. Sin embargo, no se pueden usar a la vez ambas propiedades.
* No se puede crear una regla de [redireccionamiento](redirect-overview.md) con un cliente de escucha de destino que use varios nombres de host o caracteres comodín.

Consulte [Creación de varios sitios con Azure PowerShell](tutorial-multiple-sites-powershell.md) o [con la CLI de Azure](tutorial-multiple-sites-cli.md) para ver la guía paso a paso sobre cómo configurar nombres de host con caracteres comodín en un cliente de escucha de varios sitios.



## <a name="host-headers-and-server-name-indication-sni"></a>Encabezados de host e Indicación de nombre de servidor (SNI)

Existen tres mecanismos comunes para habilitar el hospedaje de varios sitios en la misma infraestructura.

1. Hospede varias aplicaciones web, cada una en una dirección IP única.
2. Use el nombre de host para hospedar varias aplicaciones web en la misma dirección IP.
3. Use puertos distintos para hospedar varias aplicaciones web en la misma dirección IP.

En la actualidad, Application Gateway admite una dirección IP pública única en la que escucha el tráfico. Por lo tanto, actualmente no se permite tener varias aplicaciones, cada una con su propia dirección IP.

Application Gateway permite que haya varias aplicaciones y cada una escuche en un puerto diferente. Sin embargo, este escenario requiere que las aplicaciones acepten el tráfico en puertos no estándar.

Application Gateway se basa en los encabezados de host HTTP 1.1 para hospedar más de un sitio web en la misma dirección IP pública y en el mismo puerto. Los sitios que se hospedan en la puerta de enlace de aplicaciones también pueden admitir la descarga TLS con la extensión TLS de Indicación de nombre de servidor (SNI). Este escenario significa que el explorador cliente y la granja de servidores web back-end deben admitir la extensión TLS y HTTP/1.1, como se define en RFC 6066.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a configurar el hospedaje de varios sitios en Application Gateway
* [Uso de Azure Portal](create-multiple-sites-portal.md)
* [Uso de Azure PowerShell](tutorial-multiple-sites-powershell.md)
* [Uso de la CLI de Azure](tutorial-multiple-sites-cli.md)

Puede consultar la [plantilla de Resource Manager con hospedaje de múltiples sitios](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.network/application-gateway-multihosting) para ver una implementación completa basada en una plantilla.
