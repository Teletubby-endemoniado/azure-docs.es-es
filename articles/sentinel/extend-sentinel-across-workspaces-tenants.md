---
title: Extensión de Microsoft Sentinel por áreas de trabajo e inquilinos | Microsoft Docs
description: Cómo usar Microsoft Sentinel para consultar datos y analizarlos entre áreas de trabajo e inquilinos.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: microsoft-sentinel
ms.subservice: microsoft-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 3b49f1bdbe14dd13417bfef0348e1fd53543d7ca
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132521251"
---
# <a name="extend-microsoft-sentinel-across-workspaces-and-tenants"></a>Extensión de Microsoft Azure Sentinel entre áreas de trabajo e inquilinos

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

## <a name="the-need-to-use-multiple-microsoft-sentinel-workspaces"></a>La necesidad de usar varias áreas de trabajo de Microsoft Sentinel

Microsoft Sentinel se basa en un área de trabajo de Log Analytics. Observará que el primer paso en la incorporación de Microsoft Sentinel es seleccionar el área de trabajo de Log Analytics que quiere usar para ese propósito.

Puede sacar el máximo partido de la experiencia de Microsoft Sentinel al usar una sola área de trabajo. Incluso en algunas circunstancias, puede que sea necesario tener varias áreas de trabajo. En la tabla siguiente se enumeran algunas de estas situaciones y, cuando sea posible, se indica cómo se puede cumplir el requisito con una sola área de trabajo:

| Requisito | Descripción | Formas de reducir el número de áreas de trabajo |
|-------------|-------------|--------------------------------|
| Soberanía y cumplimiento normativo | Un área de trabajo está ligada a una región específica. Si los datos se deben mantener en diferentes [zonas geográficas de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) para satisfacer los requisitos normativos, deben dividirse en áreas de trabajo independientes. |  |
| Propiedad de los datos | Los límites de la propiedad de los datos, por ejemplo de subsidiarias o empresas afiliadas, se delimitan mejor mediante áreas de trabajo independientes. |  |
| Varios inquilinos de Azure | Microsoft Sentinel admite la recopilación de datos de los recursos SaaS de Azure y Microsoft solo dentro de su propio límite de inquilino de Azure Active Directory (Azure AD). Por lo tanto, cada inquilino de Azure AD requiere un área de trabajo independiente. |  |
| Control de acceso a datos pormenorizado | Es posible que una organización tenga que permitir que grupos diferentes, dentro o fuera de la organización, accedan a algunos de los datos recopilados por Microsoft Sentinel. Por ejemplo:<br><ul><li>Acceso de los propietarios de recursos a los datos que pertenecen a sus recursos</li><li>Acceso de SOC regional o subsidiario a los datos relevantes para sus partes de la organización</li></ul> | Usar [Azure RBAC de recursos](resource-context-rbac.md) o [Azure RBAC de nivel de tabla](https://techcommunity.microsoft.com/t5/azure-sentinel/table-level-rbac-in-azure-sentinel/ba-p/965043) |
| Configuración de retención pormenorizada | Históricamente, la única manera de establecer diferentes períodos de retención para tipos de datos diferentes era con varias áreas de trabajo. Esto ya no es necesario en muchos casos, gracias a la introducción de la configuración de retención de nivel de tabla. | Usar la [configuración de retención de nivel de tabla](https://techcommunity.microsoft.com/t5/azure-sentinel/new-per-data-type-retention-is-now-available-for-azure-sentinel/ba-p/917316) o automatizar la [eliminación de datos](../azure-monitor/logs/personal-data-mgmt.md#how-to-export-and-delete-private-data) |
| Facturación dividida | Al colocar áreas de trabajo en suscripciones independientes, se pueden facturar a distintas entidades. | Informes de uso y cargos cruzados |
| Arquitectura heredada | El uso de varias áreas de trabajo puede provenir de un diseño histórico que tuvo en cuenta limitaciones o procedimientos recomendados que ya no son válidos. También podría ser una opción de diseño arbitraria que se puede modificar para adaptarse mejor a Microsoft Sentinel.<br><br>Algunos ejemplos son:<br><ul><li>Uso de un área de trabajo predeterminada por suscripción al implementar Microsoft Defender for Cloud.</li><li>Necesidad de la configuración de retención o el control de acceso pormenorizado, soluciones para las que son relativamente nuevos.</li></ul> | Volver a diseñar las áreas de trabajo |

### <a name="managed-security-service-provider-mssp"></a>Proveedor de servicios de seguridad administrada (MSSP)

Un caso de uso determinado que exige varias áreas de trabajo es un servicio de MSSP de Microsoft Sentinel. En este caso, muchos si no se aplican todos los requisitos anteriores, se recomienda crear varias áreas de trabajo en los inquilinos. El MSSP puede usar [Azure Lighthouse](../lighthouse/overview.md) para extender las funcionalidades entre áreas de trabajo de Microsoft Sentinel en los inquilinos.

## <a name="microsoft-sentinel-multiple-workspace-architecture"></a>Arquitectura de varias áreas de trabajo de Microsoft Sentinel

Como se indica en los requisitos anteriores, hay casos en los que varias áreas de trabajo de Microsoft Sentinel, potencialmente a través de los inquilinos de Azure Active Directory (Azure AD), se deben supervisar y administrar de forma centralizada mediante un solo SOC.

- Un servicio de MSSP de Microsoft Sentinel.

- Un SOC global que atiende a varias subsidiarias, cada una de las cuales tiene su propio SOC local.

- Un SOC que supervisa varios inquilinos de Azure AD dentro de una organización.

Para abordar este requisito, Microsoft Sentinel ofrece funcionalidades de varias áreas de trabajo que permiten la supervisión, configuración y administración centrales, lo que proporciona un único panel en todo lo que abarca el SOC, tal como se muestra en el diagrama siguiente.

:::image type="content" source="media/extend-sentinel-across-workspaces-tenants/cross-workspace-architecture.png" alt-text="Arquitectura entre áreas de trabajo":::

Este modelo ofrece importantes ventajas con respecto a un modelo totalmente centralizado en el que todos los datos se copian en una sola área de trabajo:

- Asignación de roles flexible al SOC global y local o al MSSP de sus clientes.

- Menos desafíos relacionados con la propiedad de los datos, la privacidad de los datos y el cumplimiento normativo.

- Cargos y latencia de red mínima.

- Incorporación y retirada fáciles de nuevas subsidiarias o clientes.

En las secciones siguientes, explicaremos cómo operar este modelo y, en particular, cómo:

- Supervisar de forma centralizada varias áreas de trabajo, potencialmente a través de los inquilinos, proporcionando el SOC con un único panel.

- Configurar y administrar de forma centralizada varias áreas de trabajo, potencialmente a través de los inquilinos, mediante la automatización.

## <a name="cross-workspace-monitoring"></a>Supervisión entre áreas de trabajo

### <a name="manage-incidents-on-multiple-workspaces"></a>Administración de incidentes en varias áreas de trabajo

Microsoft Sentinel admite una [vista de incidentes de varias áreas de trabajo](./multiple-workspace-view.md), lo que facilita la supervisión y administración centrales de incidentes en varias áreas de trabajo. La vista de incidentes centralizada permite administrar incidentes directamente o explorar en profundidad de forma transparente los detalles del incidente en el contexto del área de trabajo de origen.

### <a name="cross-workspace-querying"></a>Consultas entre áreas de trabajo

Microsoft Sentinel permite incluir [varias áreas de trabajo en una sola consulta](../azure-monitor/logs/cross-workspace-query.md), lo que permite buscar y correlacionar datos de varias áreas de trabajo en una sola consulta.

- Use la [expresión workspace()](../azure-monitor/logs/workspace-expression.md) para hacer referencia a una tabla de un área de trabajo diferente.

- Use el [operador unión](/azure/data-explorer/kusto/query/unionoperator?pivots=azuremonitor) junto con la expresión workspace() para aplicar una consulta en las tablas de varias áreas de trabajo.

Puede usar las [funciones](../azure-monitor/logs/functions.md) guardadas para simplificar las consultas entre áreas de trabajo. Por ejemplo, si una referencia a un área de trabajo es larga, puede que quiera guardar la expresión `workspace("customer-A's-hard-to-remember-workspace-name").SecurityEvent` como una función llamada `SecurityEventCustomerA`. Después, puede escribir consultas como `SecurityEventCustomerA | where ...`.

Una función también puede simplificar una unión de uso frecuente. Por ejemplo, puede guardar la siguiente expresión como una función llamada `unionSecurityEvent`:

`union workspace(“hard-to-remember-workspace-name-1”).SecurityEvent, workspace(“hard-to-remember-workspace-name-2”).SecurityEvent`

Después, puede escribir una consulta en ambas áreas de trabajo empezando por `unionSecurityEvent | where ...`.

#### <a name="cross-workspace-analytics-rules"></a>Reglas de análisis entre áreas de trabajo<a name="scheduled-alerts"></a>
<!-- Bookmark added for backward compatibility with old heading -->
Ahora se pueden incluir consultas entre áreas de trabajo en las reglas de análisis programadas. Puede usar reglas de análisis entre áreas de trabajo en un SOC central y entre inquilinos (con Azure Lighthouse) como en el caso de un MSSP, sujeto a las siguientes limitaciones:

- Se pueden incluir **hasta 20 áreas de trabajo** en una sola consulta.
- Microsoft Sentinel debe **implementarse en todas las áreas de trabajo** a las que se hace referencia en la consulta.
- Las alertas generadas por una regla de análisis entre áreas de trabajo y los incidentes creados a partir de ellas existen **únicamente en el área de trabajo donde se definió la regla**. No se mostrarán en ninguna de las demás áreas de trabajo a las que se hace referencia en la consulta.

Las alertas y los incidentes creados por reglas de análisis entre áreas de trabajo contendrán todas las entidades relacionadas, incluidas las de todas las áreas de trabajo a las que se hace referencia, así como el área de trabajo "principal" (donde se definió la regla). Esto permitirá a los analistas tener una visión completa de las alertas y los incidentes.

> [!NOTE]
> La consulta de varias áreas de trabajo en la misma consulta puede afectar al rendimiento y, por lo tanto, solo se recomienda cuando la lógica requiere esta funcionalidad.

#### <a name="cross-workspace-workbooks"></a>Uso de libros entre áreas de trabajo<a name="using-cross-workspace-workbooks"></a>
<!-- Bookmark added for backward compatibility with old heading -->
Los [libros](./overview.md#workbooks) proporcionan paneles y aplicaciones a Microsoft Sentinel. Cuando se trabaja con varias áreas de trabajo, proporcionan supervisión y acciones en áreas de trabajo.

Los libros pueden proporcionar consultas entre áreas de trabajo en uno de los tres métodos, cada uno de los cuales se adapta a distintos niveles de experiencia del usuario final:

| Método  | Descripción | ¿Cuándo se debe usar? |
|---------|-------------|--------------------|
| Escritura de consultas entre áreas de trabajo | El creador del libro puede escribir consultas entre áreas de trabajo (descritas anteriormente) en el libro. | Esta opción permite que los creadores del libro protejan al usuario completamente de la estructura del área de trabajo. |
| Incorporación de un selector de área de trabajo al libro | El creador del libro puede implementar un selector de área de trabajo como parte del libro, tal como se describe [aquí](https://techcommunity.microsoft.com/t5/azure-sentinel/making-your-azure-sentinel-workbooks-multi-tenant-or-multi/ba-p/1402357). | Esta opción proporciona al usuario control sobre las áreas de trabajo que se muestran en el libro, por medio de un cuadro desplegable fácil de usar. |
| Edición del libro de forma interactiva | Un usuario avanzado que modifica un libro existente puede editar las consultas que hay en él y seleccionar las áreas de trabajo de destino mediante el selector de área de trabajo en el editor. | Esta opción permite a un usuario avanzado modificar fácilmente los libros existentes para trabajar con varias áreas de trabajo. |
|

#### <a name="cross-workspace-hunting"></a>Búsqueda entre áreas de trabajo

Microsoft Sentinel proporciona ejemplos precargados de consultas diseñadas para que pueda empezar a trabajar y a familiarizarse con las tablas y el lenguaje de consulta. Estas consultas de búsqueda integradas están desarrolladas por investigadores de seguridad de Microsoft, y lo hacen de forma continua, agregando nuevas consultas y ajustando las consultas existentes para que sean un punto de entrada para buscar nuevas detecciones e identificar signos de intrusión que pueden haber pasado inadvertidos por las herramientas de seguridad.  

Las funcionalidades de búsqueda entre áreas de trabajo permiten que los buscadores de amenazas creen nuevas consultas de búsqueda o adapten las existentes, para abarcar varias áreas de trabajo, mediante el operador de unión y la expresión workspace(), como se mostró anteriormente.

## <a name="cross-workspace-management-using-automation"></a>Administración entre áreas de trabajo mediante automatización

Para configurar y administrar varias áreas de trabajo de Microsoft Sentinel, deberá automatizar el uso de la API de administración de Microsoft Sentinel. Para obtener más información sobre cómo automatizar la implementación de recursos de Microsoft Sentinel, incluidas reglas de alertas, consultas de búsqueda, libros y guías, consulte [Extensión de Microsoft Sentinel: API, integración y automatización de la administración](https://techcommunity.microsoft.com/t5/azure-sentinel/extending-azure-sentinel-apis-integration-and-management/ba-p/1116885).

Consulte también [Implementación y administración de Microsoft Sentinel como código](https://techcommunity.microsoft.com/t5/azure-sentinel/deploying-and-managing-azure-sentinel-as-code/ba-p/1131928) y [Combinación de Azure Lighthouse con las funcionalidades de DevOps de Microsoft Sentinel](https://techcommunity.microsoft.com/t5/azure-sentinel/combining-azure-lighthouse-with-sentinel-s-devops-capabilities/ba-p/1210966) para obtener una metodología consolidada con colaboración de la comunidad para administrar Microsoft Sentinel como código y para implementar y configurar recursos desde un repositorio de GitHub privado.

## <a name="managing-workspaces-across-tenants-using-azure-lighthouse"></a>Administración de áreas de trabajo en los inquilinos mediante Azure Lighthouse

Como se mencionó anteriormente, en muchos escenarios, las diferentes áreas de trabajo de Microsoft Sentinel pueden encontrarse en distintos inquilinos de Azure AD. Puede usar [Azure Lighthouse](../lighthouse/overview.md) para extender todas las actividades entre áreas de trabajo superando los límites del inquilino, lo que permite a los usuarios del inquilino de administración trabajar en áreas de trabajo de Microsoft Sentinel en todos los inquilinos. Una vez que Azure Lighthouse esté [incorporado](../lighthouse/how-to/onboard-customer.md), use el [directorio y el selector de suscripción](./multiple-tenants-service-providers.md#how-to-access-microsoft-sentinel-in-managed-tenants) en Azure Portal para seleccionar todas las suscripciones que contienen áreas de trabajo que quiere administrar, con el fin de asegurarse de que estén disponibles en los diferentes selectores del área de trabajo del portal.

Al usar Azure Lighthouse, se recomienda crear un grupo para cada rol de Microsoft Sentinel y delegar los permisos de cada inquilino en esos grupos.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido cómo se pueden ampliar las funcionalidades de Microsoft Sentinel en varias áreas de trabajo e inquilinos. Para obtener una guía práctica sobre la implementación de la arquitectura entre áreas de trabajo de Microsoft Sentinel, consulte los siguientes artículos:

- Obtenga información sobre cómo [trabajar con varios inquilinos](./multiple-tenants-service-providers.md) en Microsoft Sentinel mediante Azure Lighthouse.
- Obtenga información sobre cómo [ver y administrar incidentes en varias áreas de trabajo](./multiple-workspace-view.md) sin problemas.
