---
title: Integración de Microsoft 365 Defender en Microsoft Sentinel | Microsoft Docs
description: Obtenga información sobre cómo el uso de Microsoft 365 Defender junto con Microsoft Sentinel le permite usar Microsoft Sentinel como cola de incidentes universales y aplicar sin problemas los puntos fuertes de Microsoft 365 Defender para ayudar a investigar los incidentes de seguridad de Microsoft 365. Además, aprenda a ingerir datos avanzados de búsqueda de componentes de Defender en Microsoft Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 34b39bcac7f852bedcfd1b03d399f5ef4987e0be
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132713163"
---
# <a name="microsoft-365-defender-integration-with-microsoft-sentinel"></a>Integración de Microsoft 365 Defender en Microsoft Azure Sentinel

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

> [!IMPORTANT]
> El conector Microsoft 365 Defender está actualmente en **VERSIÓN PRELIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

> [!IMPORTANT]
>
> **Microsoft 365 Defender** se conocía antes como **Microsoft Threat Protection** o **MTP**.
>
> **Microsoft Defender para punto de conexión** se conocía antes como **Protección contra amenazas avanzada de Microsoft Defender** o **MDATP**.
>
> **Microsoft Defender para Office 365** se conocía anteriormente como **Protección contra amenazas avanzada de Office 365**.
>
> Es posible que vea que los nombres antiguos todavía están en uso durante un tiempo.

## <a name="incident-integration"></a>Integración de incidentes

La integración de incidentes de [Microsoft 365 Defender](/microsoft-365/security/mtp/microsoft-threat-protection) de Microsoft Sentinel le permite transmitir todos los incidentes de Microsoft 365 Defender a Microsoft Sentinel y mantenerlos sincronizados entre ambos portales. Los incidentes de Microsoft 365 Defender (anteriormente conocido como Protección contra amenazas de Microsoft o MTP) incluyen todas las alertas, entidades e información pertinente asociadas, lo que le proporciona un contexto suficiente para realizar la evaluación de prioridades y una investigación preliminar en Microsoft Sentinel. Una vez en Sentinel, los incidentes permanecerán sincronizados de manera bidireccional con Microsoft 365 Defender, lo que le permite aprovechar las ventajas de ambos portales en su investigación de los incidentes.

Esta integración da a los incidentes de seguridad de Microsoft 365 visibilidad para que puedan administrarse desde Microsoft Sentinel, como parte de la cola de incidentes principales en toda la organización, por lo que puede ver y correlacionar los incidentes de Microsoft 365 con los de todos los demás sistemas en la nube y locales. Al mismo tiempo, permite aprovechar las ventajas y las funcionalidades únicas de Microsoft 365 Defender para las investigaciones detalladas y brinda una experiencia específica de Microsoft 365 en el ecosistema de Microsoft 365. Microsoft 365 Defender enriquece y agrupa las alertas de varios productos de Microsoft 365, lo que reduce el tamaño de la cola de incidentes de SOC y reduce el tiempo de resolución. Los servicios de componentes que forman parte de la pila de Microsoft 365 Defender son:

- **Microsoft Defender para punto de conexión** (anteriormente Microsoft Defender ATP)
- **Microsoft Defender for Identity** (anteriormente Azure ATP)
- **Microsoft Defender para Office 365** (anteriormente ATP de Office 365)
- **Microsoft Defender for Cloud Apps**

Además de recopilar alertas de estos componentes, Microsoft 365 Defender genera alertas propias. Crea incidentes a partir de todas estas alertas y los envía a Microsoft Sentinel.

### <a name="common-use-cases-and-scenarios"></a>Casos de uso y escenarios comunes

- Conexión de un solo clic de incidentes de Microsoft 365 Defender, incluidas todas las alertas y entidades de los componentes de Microsoft 365 Defender, en Microsoft Sentinel.

- Sincronización bidireccional entre incidentes de Sentinel y Microsoft 365 Defender en el estado, el propietario y el motivo de cierre.

- Aplicación de las funcionalidades de agrupación y enriquecimiento de alertas de Microsoft 365 Defender en Microsoft Sentinel, lo que reduce el tiempo de resolución.

- Vínculo profundo en contexto entre un incidente de Microsoft Sentinel y su incidente paralelo de Microsoft 365 Defender, para facilitar las investigaciones en ambos portales.

### <a name="connecting-to-microsoft-365-defender"></a>Conexión a Microsoft 365 Defender

Una vez que haya habilitado el conector de datos de Microsoft 365 Defender para [recopilar incidentes y alertas](connect-microsoft-365-defender.md), los incidentes de Microsoft 365 Defender aparecerán en la cola de incidentes de Microsoft Sentinel, con **Microsoft 365 Defender** en el campo **Nombre de producto**, poco después de que se generen en Microsoft 365 Defender.
- Pueden pasar hasta 10 minutos entre el momento en que se genere un incidente en Microsoft 365 Defender y el momento en que aparezca en Microsoft Sentinel.

- Los incidentes se ingerirán y se sincronizarán sin costo adicional.

Una vez conectada la integración de Microsoft 365 Defender, todos los conectores de alertas de componentes (Defender para punto de conexión, Defender for Identity, Defender para Office 365, Defender for Cloud Apps) se conectarán automáticamente en segundo plano si no lo estaban ya. Si se adquirieron licencias de componentes después de la conexión a Microsoft 365 Defender, las alertas y los incidentes del nuevo producto seguirán fluyendo hacia Microsoft Sentinel sin ningún cargo ni configuración adicionales.

### <a name="microsoft-365-defender-incidents-and-microsoft-incident-creation-rules"></a>Incidentes de Microsoft 365 Defender y reglas de creación de incidentes de Microsoft

- Los incidentes generados por Microsoft 365 Defender, basados en alertas procedentes de productos de seguridad de Microsoft 365, se crean mediante la lógica personalizada de Microsoft 365 Defender.

- Las reglas de creación de incidentes de Microsoft en Microsoft Sentinel también crean incidentes a partir de las mismas alertas, mediante una lógica de Microsoft Sentinel personalizada (distinta).

- El uso conjunto de ambos mecanismos es totalmente compatible y se puede usar para facilitar la transición a la nueva lógica de creación de incidentes de Microsoft 365 Defender. Sin embargo, esto creará **incidentes duplicados** para las mismas alertas.

- Para evitar la creación de incidentes duplicados para las mismas alertas, se recomienda que los clientes desactiven todas las **reglas de creación de incidentes de Microsoft** para productos de Microsoft 365 (Defender para punto de conexión, Defender for Identity, Defender para Office 365 y Defender for Cloud Apps) al conectar Microsoft 365 Defender. Para ello, puede deshabilitar la creación de incidentes en la página del conector. Tenga en cuenta que, si lo hace, los filtros aplicados por las reglas de creación de incidentes no se aplicarán a la integración de incidentes de Microsoft 365 Defender.

    > [!NOTE]
    > Todos los tipos de alerta de Microsoft Defender for Cloud Apps están incorporados ahora en Microsoft 365 Defender.

### <a name="working-with-microsoft-365-defender-incidents-in-microsoft-sentinel-and-bi-directional-sync"></a>Trabajo con incidentes de Microsoft 365 Defender en Microsoft Sentinel y sincronización bidireccional

Los incidentes de Microsoft 365 Defender aparecerán en la cola de incidentes de Microsoft Sentinel con el nombre de producto **Microsoft 365 Defender**, y con detalles y funcionalidades similares a cualquier otro incidente de Sentinel. Cada incidente contiene un vínculo al incidente paralelo en el portal de Microsoft 365 Defender.

A medida que el incidente evolucione en Microsoft 365 Defender y se agreguen más alertas o entidades, el incidente de Microsoft Sentinel se actualizará en consecuencia.

Los cambios realizados en el estado, el motivo de cierre o la asignación de un incidente de Microsoft 365, ya sea en Microsoft 365 Defender o en Microsoft Sentinel, también se actualizarán en consecuencia en la cola de incidentes del otro. La sincronización se llevará a cabo en ambos portales inmediatamente después de que se aplique el cambio en el incidente, sin ningún retraso. Podría ser necesaria una actualización para ver los cambios más recientes.

En Microsoft 365 Defender, todas las alertas de un incidente se pueden transferir a otro, lo que da lugar a la combinación de los incidentes. Cuando esta combinación sucede, los incidentes de Microsoft Sentinel reflejarán los cambios. Un incidente contendrá todas las alertas de ambos incidentes originales, y el otro incidente se cerrará automáticamente y se le agregará la etiqueta "redirigido".

> [!NOTE]
> Los incidentes de Microsoft Sentinel pueden contener un máximo de 150 alertas. Los incidentes de Microsoft 365 Defender pueden tener más. Si un incidente de Microsoft 365 Defender con más de 150 alertas se sincroniza con Microsoft Sentinel, el incidente de Sentinel se mostrará con "150+" alertas y proporcionará un vínculo al incidente paralelo en Microsoft 365 Defender, donde podrá ver el conjunto completo de alertas.

## <a name="advanced-hunting-event-collection"></a>Colección de eventos de búsqueda avanzada

El nuevo conector Microsoft 365 Defender le permite transmitir eventos de **búsqueda avanzada**, un tipo de datos de eventos sin procesar, desde Microsoft 365 Defender y sus servicios de componentes hacia Microsoft Sentinel. Actualmente, puede recopilar eventos de [búsqueda avanzada](/microsoft-365/security/defender/advanced-hunting-overview) de Microsoft Defender para punto de conexión y, *desde octubre de 2021*, de Microsoft Defender para Office 365 y transmitirlos directamente a tablas creadas específicamente en el área de trabajo de Microsoft Sentinel. Estas tablas se basan en el mismo esquema que se usa en el portal de Microsoft 365 Defender, lo que le ofrece acceso pleno al conjunto completo de eventos de búsqueda avanzada y le permite hacer lo siguiente:

- Copiar fácilmente las consultas existentes de búsqueda avanzada de Microsoft Defender para punto de conexión u Office 365 en Microsoft Sentinel.

- Usar los registros de eventos sin procesar con el fin de proporcionar más información para las alertas, la búsqueda y la investigación, y poner en correlación estos eventos con los eventos de otros orígenes de datos en Microsoft Sentinel.

- Almacenar los registros con mayor retención, por encima de la retención predeterminada de 30 días de Microsoft Defender para punto de conexión u Office 365 o Microsoft 365 Defender. Para ello, configure la retención del área de trabajo o configure la retención por tabla en Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

En este documento, aprendió la manera de aprovechar el uso de Microsoft 365 Defender junto con Microsoft Sentinel mediante el conector Microsoft 365 Defender.

- Obtenga instrucciones para [habilitar el conector Microsoft 365 Defender](connect-microsoft-365-defender.md).
- Cree [alertas personalizadas](detect-threats-custom.md) o [investigue incidentes](investigate-cases.md).
