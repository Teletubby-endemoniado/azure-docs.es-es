---
title: Integración de soluciones de seguridad en Azure Security Center | Microsoft Docs
description: Aprenda cómo Azure Security Center se integra con los asociados para mejorar la seguridad general de los recursos de Azure.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 12/10/2020
ms.author: memildin
ms.openlocfilehash: 66f2ca97ce20dc0aeb6b1fcecf691861b645284b
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129706698"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integración de soluciones de seguridad en Azure Security Center
Este documento le ayuda a administrar las soluciones de seguridad que ya está conectadas a Azure Security Center y a agregar otras nuevas.

## <a name="integrated-azure-security-solutions"></a>Soluciones de seguridad de Azure integradas
Security Center facilita la habilitación de soluciones de seguridad integradas en Azure. Dicha integración aporta las siguientes ventajas:

- **Implementación simplificada**: Security Center ofrece un aprovisionamiento optimizado de soluciones de asociados integradas. En el caso de soluciones antimalware y de evaluación de vulnerabilidades, Security Center puede aprovisionar el agente en las máquinas virtuales. En el caso de aplicaciones de firewall, Security Center puede ocuparse de gran parte de la configuración de red necesaria.
- **Detecciones integradas**: Los eventos de seguridad de las soluciones de asociados se recopilan, agregan y aparecen automáticamente como parte de las alertas e incidentes de Security Center. Estos eventos también se fusionan con las detecciones procedentes de otros orígenes para proporcionar funcionalidades avanzadas de detección de amenazas.
- **Supervisión y administración unificadas del mantenimiento**: Los clientes pueden usar eventos de mantenimiento integrados para supervisar todas las soluciones de asociados de un vistazo. La administración básica está disponible con un acceso sencillo a la configuración avanzada mediante la solución de asociado.

Actualmente, las soluciones de seguridad integradas incluyen la evaluación de vulnerabilidades por [Qualys](https://www.qualys.com/public-cloud/#azure) y [Rapid7](https://www.rapid7.com/products/insightvm/) y [Microsoft Azure Web Application Firewall en Azure Application Gateway](../web-application-firewall/ag/ag-overview.md).

> [!NOTE]
> Security Center no instala Log Analytics en aplicaciones virtuales de asociados, porque la mayoría de los proveedores de seguridad prohíben que se ejecuten agentes externos en su aplicación.

Para más información sobre la integración de las herramientas de examen de vulnerabilidades de Qualys, incluido un examen integrado disponible para los clientes de Azure Defender, consulte [Solución de evaluación de vulnerabilidades integrada en Azure Defender para Azure y máquinas híbridas](deploy-vulnerability-assessment-vm.md).

Security Center también ofrece análisis de vulnerabilidades para:

* SQL Database: consulte [Exploración de los informes de evaluación de vulnerabilidades en el panel de evaluación de vulnerabilidades](defender-for-sql-on-machines-vulnerability-assessment.md#explore-vulnerability-assessment-reports).
* Imágenes de Azure Container Registry: consulte [Uso de Azure Defender para registros de contenedor para examinar las imágenes en busca de vulnerabilidades](defender-for-container-registries-usage.md).

## <a name="how-security-solutions-are-integrated"></a>Integración de soluciones de seguridad
Las soluciones de seguridad de Azure que se implementan desde Security Center se conectan automáticamente. También se pueden conectar otros orígenes de datos de seguridad, como equipos que se ejecutan en el entorno local o en otras nubes.

:::image type="content" source="./media/security-center-partner-integration/security-solutions-page.png" alt-text="Integración de soluciones para asociados." lightbox="./media/security-center-partner-integration/security-solutions-page.png":::

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Administración de soluciones de seguridad de Azure integradas y otros orígenes de datos

1. En [Azure Portal](https://azure.microsoft.com/features/azure-portal/), abra **Security Center**.

1. En el menú de Security Center, seleccione **Soluciones de seguridad**.

En la página **Soluciones de seguridad**, puede ver el mantenimiento de las soluciones de seguridad integradas de Azure y ejecutar tareas de administración básicas.

### <a name="connected-solutions"></a>Soluciones conectadas

La sección **Soluciones conectadas** incluye soluciones de seguridad que están conectadas actualmente a Security Center. También muestra el estado de mantenimiento de cada solución.  

![Soluciones conectadas.](./media/security-center-partner-integration/connected-solutions.png)

El estado de una solución de asociado puede ser:

* **Correcto** (verde): no hay problemas de mantenimiento.
* **Incorrecto** (rojo): hay una incidencia de mantenimiento que requiere atención inmediata.
* **Creación de informes detenida** (naranja): la solución ha dejado de informar sobre su mantenimiento.
* **Sin notificar** (gris): la solución aún no ha notificado nada y no hay datos de mantenimiento disponibles. Es posible que el estado de una solución no se notifique si se conectó recientemente y todavía se está implementando.

> [!NOTE]
> Si los datos sobre el estado de mantenimiento no están disponibles, Security Center muestra la fecha y hora del último evento recibido para indicar si la solución está enviando notificaciones o no. Si no hay disponible ningún dato de mantenimiento y no se han recibido alertas en los últimos 14 días, Security Center indica que la solución es incorrecta o que no notifica nada.
>
>

Seleccione **Ver** para más información y opciones adicionales, entre las que se incluyen:

   - **Consola de soluciones**: se abre la experiencia de administración para esta solución.
   - **Vincular VM**: se abre la página Vincular aplicaciones. Aquí puede conectar recursos a la solución de asociados.
   - **Eliminar solución**
   - **Configuración**

   ![Detalle de solución de asociado.](./media/security-center-partner-integration/partner-solutions-detail.png)


### <a name="discovered-solutions"></a>Soluciones detectadas

Security Center detecta automáticamente las soluciones de seguridad que se ejecutan en Azure, pero que no están conectadas a Security Center y las muestra en la sección **Soluciones detectadas**. Estas soluciones incluyen las de Azure, como [Azure AD Identity Protection](../active-directory/identity-protection/overview-identity-protection.md) y soluciones de asociados.

> [!NOTE]
> Habilite **Azure Defender** en el nivel de suscripción para la característica de soluciones detectadas. Obtenga más información en [Inicio rápido: habilitación de Azure Defender](enable-azure-defender.md).

Seleccione **Conectar** en una solución para integrarla con Security Center y recibir alertas de seguridad.

### <a name="add-data-sources"></a>Agregar orígenes de datos

La sección **Agregar orígenes de datos** incluye otros orígenes de datos disponibles que se pueden conectar. Para obtener instrucciones acerca de cómo agregar datos desde cualquiera de estos orígenes, haga clic en **ADD**.

![Orígenes de datos.](./media/security-center-partner-integration/add-data-sources.png)



## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a integrar soluciones de asociados en Security Center. Para obtener información sobre cómo configurar una integración con Azure Sentinel o cualquier otro SIEM, consulte [Exportación continua de datos de Security Center](continuous-export.md).