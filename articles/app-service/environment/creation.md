---
title: Creación de una instancia de App Service Environment
description: Aprenda a crear una instancia de App Service Environment.
author: ccompy
ms.assetid: 7690d846-8da3-4692-8647-0bf5adfd862a
ms.topic: article
ms.date: 09/07/2021
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: fdf74a88a34f9d497bc23b9fed00c8fa55b6875a
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129236093"
---
# <a name="create-an-app-service-environment"></a>Creación de una instancia de App Service Environment
> [!NOTE]
> En este artículo se trata App Service Environment v3, que se usa con planes de App Service v2 aislados
> 


[App Service Environment (ASE)][Intro] es una implementación de inquilino único de Azure App Service que se ejecuta en una instancia de Azure Virtual Network (VNet). Para implementar Azure App Service Environment, será preciso usar una subred. Esta subred no se puede usar para ninguna otra cosa que no sea el ASE. 

## <a name="before-you-create-your-ase"></a>Antes de crear su ASE

Después de crear el ASE, no se puede cambiar:

- Location
- Subscription
- Resource group
- Instancia de Azure Virtual Network (red virtual) usada
- Subredes usadas
- Tamaño de la subred
- Nombre del ASE

La subred debe ser lo suficientemente grande como para contener el tamaño máximo al que se escalará Azure App Service Environment. Elija una subred lo suficientemente grande como para satisfacer sus necesidades de escalado máximo, ya que no se podrá cambiar una vez creada. El tamaño recomendado es /24 con 256 direcciones.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Hay dos cosas importantes que deben tenerse en cuenta antes de implementar el ASE. 

- Tipo de VIP
- tipo de implementación

Hay dos tipos de VIP diferentes: internas y externas. Con una VIP interna, se accede a las aplicaciones del ASE en una dirección de la subred del ASE y las aplicaciones no están en un DNS público. Durante la creación en el portal, hay una opción para crear una zona DNS privada de Azure para el ASE. Con una VIP externa, las aplicaciones estarán en una dirección pública accesible desde Internet y están en el DNS público. 

Existen tres tipos de implementación diferentes: 

- zona única 
- redundancia de zona
- grupo host

El ASE de zona única está disponible en todas las regiones en las que ASEv3 esté disponible. Cuando se tiene un ASE de zona única, se tiene una cargo de instancia mínima del plan App Service de una instancia de Windows Isolated v2. En cuanto tenga una o varias instancias, ese cargo desaparecerá. No es un cargo acumulativo. 

En un ASE con redundancia de zona, las aplicaciones se reparten entre tres zonas de la misma región. El ASE con redundancia de zona está disponible en un subconjunto de regiones compatibles con ASE limitadas principalmente por las regiones que admiten zonas de disponibilidad. Si tiene un ASE con redundancia de zona, el tamaño más pequeño del plan de App Service es de tres instancias. Esto garantiza que haya una instancia en cada zona de disponibilidad. Los planes de App Service se pueden escalar verticalmente a una o varias instancias a la vez. El escalado no tiene que estar en unidades de tres, pero la aplicación solo se equilibra entre todas las zonas de disponibilidad cuando el número total de instancias es múltiplo de tres. Un ASE con redundancia de zona tiene el triple de infraestructura y se crea con componentes con redundancia de zona para que, si incluso dos de las tres zonas quedan fuera de servicio por cualquier motivo, las cargas de trabajo permanezcan disponibles. Debido al aumento de las necesidades del sistema, el cargo mínimo por un ASE con redundancia de zona es de nueve instancias. Si tiene menos de nueve instancias totales del plan de App Service en su ASEv3, la diferencia se cobrará como Windows I1v2. Si tiene nueve o más instancias, no hay ningún cargo adicional por tener un ASE con redundancia de zona. Para más información sobre la redundancia de zona, lea [Regiones y zonas de disponibilidad][AZoverview].

En una implementación de grupo host, las aplicaciones se implementan en un grupo host dedicado. El grupo host dedicado no tiene redundancia de zona. La implementación de grupos host dedicados permite que el ASE se implemente en hardware dedicado. No hay un cargo mínimo por instancia con el uso de un ASE en un grupo host dedicado, pero sí hay que pagar por el grupo host cuando se aprovisiona el ASE. Además, paga una tarifa de plan de App Service con descuento a medida que crea sus planes y escala horizontalmente. Hay un número finito de núcleos disponibles con una implementación de host dedicada que son utilizados tanto por los planes de App Service como por los roles de infraestructura. Las implementaciones de host dedicadas del ASE no pueden llegar al número total de 200 instancias disponibles normalmente en un ASE. El número total de instancias posibles está relacionado con el número total de instancias del plan de App Service más el número de roles de infraestructura basados en la carga. 

## <a name="creating-an-ase-in-the-portal"></a>Creación de un ASE en el portal

1. Para crear una instancia de Azure App Service Environment, busque **App Service Environment v3** en Marketplace.  

2. Aspectos básicos:  Seleccione la suscripción, seleccione o cree el grupo de recursos y escriba el nombre del ASE.  Seleccione el tipo de IP virtual. Si selecciona Interno, la dirección de App Service Environment entrante será una dirección en la subred de App Service Environment. Si selecciona Externo, la dirección de ASE entrante será una dirección de Internet pública. El nombre del ASE también se usará como sufijo de dominio del ASE. Si el nombre de la instancia de App Service Environment es *contoso* y tiene una instancia de App Service Environment con IP virtual interna, el sufijo de dominio será *contoso.appserviceenvironment.net*. Si el nombre de la instancia de App Service Environment es *contoso* y tiene una IP virtual externa, el sufijo de dominio será *contoso.p.azurewebsites.net*. 

    ![Pestaña Aspectos básicos de creación de una instancia de App Service Environment](./media/creation/creation-basics.png)

3. Hospedaje: en la opción de implementación del grupo host, seleccione *Habilitado* o *Deshabilitado*. La implementación del grupo host se usa para seleccionar el hardware dedicado. Si selecciona Habilitado, App Service Environment se implementará en un hardware dedicado. Si la implementación se realiza en este tipo de hardware, se le cobra por todo el host dedicado durante la creación de App Service Environment y, luego, un precio reducido por las instancias del plan de App Service. 

    ![Selecciones de hospedaje de App Service Environment](./media/creation/creation-hosting.png)

4. Redes: seleccione o cree la red virtual y seleccione o cree la subred. Si va a crear un ASE de VIP interna, puede configurar las zonas privadas de Azure DNS para que apunten el sufijo de dominio al ASE. Los detalles sobre cómo configurar DNS manualmente se encuentran en la sección DNS de [Uso de App Service Environment][UsingASE].

    ![Selecciones de red de App Service Environment](./media/creation/creation-networking.png)

5. Revisión y creación; compruebe que la configuración sea correcta y seleccione Crear. App Service Environment puede tardar casi dos horas en crearse. 

Una vez completada la creación del ASE, puede seleccionarlo como ubicación al crear las aplicaciones. Para más información sobre la creación de aplicaciones en la nueva instancia de App Service Environment o sobre la administración de App Service Environment, consulte [Uso de App Service Environment][UsingASE].

## <a name="dedicated-hosts"></a>Hosts dedicados

El ASE se implementa normalmente en máquinas virtuales que se aprovisionan en un hipervisor multiinquilino. Si necesita implementarlo en sistemas dedicados, incluido el hardware, puede aprovisionar su ASE en hosts dedicados. Los hosts dedicados vienen en parejas para garantizar la redundancia. Las implementaciones del ASE basadas en hosts dedicados tienen un precio distinto del normal. Hay un cargo por el host dedicado y, luego, otro cargo por cada instancia del plan de App Service. Las implementaciones en grupos host no tienen redundancia de zona. Para realizar la implementación en hosts dedicados, seleccione **Habilitar** para la implementación de grupos host en la pestaña Hospedaje.

<!--Links-->
[Intro]: ./overview.md
[MakeASE]: ./creation.md
[ASENetwork]: ./networking.md
[UsingASE]: ./using.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/network-security-groups-overview.md
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../web-application-firewall/ag/ag-overview.md
[logalerts]: ../../azure-monitor/alerts/alerts-log.md
[AZoverview]: ../../availability-zones/az-overview.md
