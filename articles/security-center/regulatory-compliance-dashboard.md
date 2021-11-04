---
title: 'Tutorial: Comprobaciones de cumplimiento normativo; Microsoft Defender for Cloud'
description: 'Tutorial: Aprenda a mejorar el cumplimiento normativo mediante Microsoft Defender for Cloud.'
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: tutorial
ms.date: 08/09/2021
ms.author: memildin
ms.openlocfilehash: 6051f0625408345b0e8665bb3ddcccf97e940367
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131452858"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Tutorial: Mejora del cumplimiento normativo

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Microsoft Defender for Cloud resulta de gran ayuda para simplificar el proceso necesario para cumplir los requisitos de cumplimiento normativo, para lo que se usa el **panel de cumplimiento normativo**. 

Defender for Cloud evalúa continuamente el entorno de nube híbrida para analizar los factores de riesgo con arreglo a los controles y los procedimientos recomendados establecidos en los estándares que haya aplicado a las suscripciones. El panel refleja el estado de cumplimiento con respecto a estos estándares. 

Cuando habilite Defender for Cloud en una suscripción de Azure, se asignará automáticamente a esta suscripción el estándar [Azure Security Benchmark](/security/benchmark/azure/introduction). Este punto de referencia ampliamente respetado está basado en los controles del [Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/azure/) y del [National Institute of Standards and Technology (NIST)](https://www.nist.gov/) y hace hincapié en la seguridad centrada en la nube.

En el panel de cumplimiento normativo se indica el estado de todas estas valoraciones realizadas en el entorno conforme a las normativas y los estándares elegidos. Al actuar sobre las recomendaciones y reducir los factores de riesgo en su entorno, su estado de cumplimiento normativo mejora.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Evaluar su cumplimiento normativo mediante el panel de cumplimiento normativo.
> * Mejorar su estado de cumplimiento normativo mediante la realización de acciones sobre las recomendaciones
> * Descargar informes PDF/CSV, así como informes de certificación del estado de cumplimiento.
> * Configurar alertas sobre los cambios en el estado de cumplimiento.
> * Exportar los datos de cumplimiento como una secuencia continua y como instantáneas semanales

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para recorrer las características descritas en este tutorial:

- [Habilite las características de seguridad mejoradas](defender-for-cloud-introduction.md). Puede habilitarlas de forma gratuita durante 30 días.
- Debe haber iniciado sesión con una cuenta que tenga acceso de lectura a los datos de cumplimiento de directivas (**Lector de seguridad** no es suficiente). El rol **Lector global** para la suscripción funcionará. Como mínimo, necesitará tener asignados los roles **Colaborador de directivas de recursos** y **Administrador de seguridad**.

## <a name="assess-your-regulatory-compliance"></a>Mejora del cumplimiento de reglamentaciones

El panel de cumplimiento normativo muestra los estándares normativos seleccionados, con todos sus requisitos. Los requisitos compatibles se asignarán a las evaluaciones de seguridad correspondientes. El estado de estas evaluaciones refleja el estado de cumplimiento con respecto a los estándares.

Utilice el panel de cumplimiento normativo para centrarse en las deficiencias relativas al cumplimiento de las normativas y estándares elegidos. Esta vista focalizada también le permite supervisar continuamente su puntuación de cumplimiento a lo largo del tiempo para entornos híbrido y de nube dinámicos.

1. En el menú de Defender for Cloud, seleccione **Cumplimiento normativo**.

    En la parte superior de la pantalla, hay un panel con información general sobre su estado de cumplimiento y el conjunto de normativas de cumplimiento admitidas. Allí se indica la puntuación global de cumplimiento y el número de valoraciones aprobadas y suspendidas asociadas a cada estándar.

    :::image type="content" source="./media/regulatory-compliance-dashboard/compliance-dashboard.png" alt-text="Panel de cumplimiento normativo." lightbox="./media/regulatory-compliance-dashboard/compliance-dashboard.png":::

1. Seleccione la pestaña de la norma que le interese (1). Verá las suscripciones en las que se aplica la norma (2) y la lista de todos los controles de esa norma (3). Para saber qué controles se pueden aplicar, puede ver los detalles de las evaluaciones superadas y no superadas asociadas a ese control (4) y el número de recursos afectados (5). Algunos controles aparecen atenuados, no tienen valoraciones de Defender for Cloud asociadas. Compruebe sus requisitos y evalúelos en el entorno. Es posible que algunos de ellos estén relacionados con los procesos y no sean técnicos.

    :::image type="content" source="./media/regulatory-compliance-dashboard/compliance-drilldown.png" alt-text="Exploración de los detalles de cumplimiento con una norma específica.":::

## <a name="improve-your-compliance-posture"></a>Mejora de su estado de cumplimiento de la norma

El panel contiene información que le ayudará a mejorar el cumplimiento normativo y le permitirá resolver las recomendaciones directamente en él.

1.  Seleccione cualquiera de las evaluaciones no superadas que aparecen en el panel para ver los detalles de dicha recomendación. Cada recomendación incluye un conjunto de pasos de corrección para resolver el problema.

1.  Seleccione un recurso concreto para ver más detalles y resolver la recomendación relacionada. <br>Por ejemplo, en el estándar **Azure CIS 1.1.0**, seleccione la recomendación **El cifrado de disco se debe aplicar en las máquinas virtuales**.

    :::image type="content" source="./media/regulatory-compliance-dashboard/sample-recommendation.png" alt-text="Al seleccionar una recomendación de una norma se va directamente a la página de detalles de la recomendación.":::

1. En este ejemplo, si selecciona **Realizar acción** en la página de detalles de la recomendación, accederá Azure Portal, a las páginas de la máquina virtual de Azure, donde podrá abrir la pestaña **Seguridad** y habilitar el cifrado:

    :::image type="content" source="./media/regulatory-compliance-dashboard/encrypting-vm-disks.png" alt-text="Botón Realizar acción en la página de detalles de la recomendación que conduce a las opciones de corrección.":::

    Para más información sobre cómo aplicar las recomendaciones, consulte [Implementación de recomendaciones de seguridad en Microsoft Defender for Cloud](review-security-recommendations.md).

1.  Una vez realizadas las acciones necesarias para resolver las recomendaciones, podrá ver el resultado en el informe del panel de cumplimiento, ya que la puntuación de cumplimiento mejora.

    > [!NOTE]
    > Las valoraciones se ejecutan aproximadamente cada 12 horas, por lo que el efecto sobre los datos de cumplimiento solo se constatará tras la ejecución siguiente de la valoración en cuestión.

## <a name="generate-compliance-status-reports-and-certificates"></a>Generación de informes y certificados de estado de cumplimiento.

- Para generar un informe PDF con un resumen de su estado de cumplimiento actual en relación con un estándar concreto, seleccione **Descargar informe**.

    El informe proporciona un resumen general sobre el estado de cumplimiento del estándar seleccionado en función de los datos de las evaluaciones de Defender for Cloud. El informe se organiza según los controles de ese estándar determinado. El informe se puede compartir con las partes interesadas competentes y puede proporcionar evidencia a los auditores internos y externos.

    :::image type="content" source="./media/regulatory-compliance-dashboard/download-report.png" alt-text="Use la barra de herramientas del panel de cumplimiento normativo de Defender for Cloud para descargar los informes de cumplimiento.":::

- Para descargar **informes de certificación** de Azure y Dynamics para los estándares aplicados a las suscripciones, use la opción **Informes de auditoría**. 

    :::image type="content" source="media/release-notes/audit-reports-regulatory-compliance-dashboard.png" alt-text="Use la barra de herramientas del panel de cumplimiento normativo de Defender for Cloud para descargar los informes de certificación de Azure y Dynamics.":::

    Seleccione la pestaña correspondiente a cada tipo de informe pertinente (PCI, SOC, ISO y otros) y usar filtros para buscar los informes específicos que necesita.

    :::image type="content" source="media/release-notes/audit-reports-list-regulatory-compliance-dashboard-ga.png" alt-text="Filtrado de la lista de informes de auditoría de Azure disponibles con pestañas y filtros.":::

    Por ejemplo, desde la pestaña PCI puede descargar un archivo ZIP que contenga un certificado firmado digitalmente que demuestra el cumplimiento normativo de Microsoft Azure, Dynamics 365 y otros servicios en línea conforme al marco ISO22301, además de la información necesaria para interpretar y presentar el certificado. 

    > [!NOTE]
    > Al descargar uno de estos informes de certificación, se le mostrará el siguiente aviso de privacidad:
    > 
    > _Al descargar este archivo, otorga su consentimiento a Microsoft para almacenar el usuario actual y las suscripciones seleccionadas en el momento de la descarga. Estos datos se usan para notificarle en caso de que se realicen cambios o actualizaciones en el informe de auditoría descargado. Microsoft y las empresas de auditoría usan estos datos para generar los informes o la certificación solo cuando la notificación es necesaria._

## <a name="configure-frequent-exports-of-your-compliance-status-data"></a>Configuración de exportaciones frecuentes de los datos de estado de cumplimiento

Si desea hacer un seguimiento del estado de cumplimiento con otras herramientas de supervisión de su entorno, Defender for Cloud dispone de un mecanismo de exportación que facilita esta tarea. Configure la **exportación continua** para enviar una selección de los datos a un centro de eventos de Azure o un área de trabajo de Log Analytics. Obtenga más información en [Exportación continua de datos de Defender for Cloud](continuous-export.md).

Utilice los datos de exportación continua con un centro de eventos de Azure o un área de trabajo de Log Analytics:

- Exporte todos los datos de cumplimiento normativo en un **flujo continuo**:

    :::image type="content" source="media/regulatory-compliance-dashboard/export-compliance-data-stream.png" alt-text="Exportación ininterrumpida de un flujo de datos de cumplimiento normativo." lightbox="media/regulatory-compliance-dashboard/export-compliance-data-stream.png":::

- Exporte **instantáneas semanales** de los datos de cumplimiento normativo:

    :::image type="content" source="media/regulatory-compliance-dashboard/export-compliance-data-snapshot.png" alt-text="Exportación ininterrumpida de una instantánea semanal de los datos de cumplimiento normativo." lightbox="media/regulatory-compliance-dashboard/export-compliance-data-snapshot.png":::

> [!TIP]
> También puede exportar manualmente informes relativos a un único momento en el tiempo directamente desde el panel de cumplimiento normativo. Genere estos informes **PDF/CSV** o los **informes de certificación de Azure y Dynamics** con las opciones de la barra de herramientas **Descargar informe** o **Informes de auditoría**. Consulte [Evaluación del cumplimiento normativo.](#assess-your-regulatory-compliance) 


## <a name="run-workflow-automations-when-there-are-changes-to-your-compliance"></a>Ejecución de automatizaciones de flujos de trabajo cuando se producen cambios en el cumplimiento

La característica de automatización de flujos de trabajo de Defender for Cloud puede desencadenar Logic Apps cada vez que una de las evaluaciones de cumplimiento normativo cambie el estado.

Por ejemplo, si quiere que Defender for Cloud envíe un correo electrónico a un usuario específico cuando no se supere una valoración de cumplimiento, primero tendrá que crear la aplicación lógica (mediante [Azure Logic Apps](../logic-apps/logic-apps-overview.md)) y después tendrá que configurar el desencadenador en una nueva automatización de flujos de trabajo, tal y como se explica en [Automatización de respuestas a desencadenadores de Defender for Cloud](workflow-automation.md).

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="Uso de los cambios en las evaluaciones de cumplimiento normativo para desencadenar la automatización de un flujo de trabajo." lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::




## <a name="faq---regulatory-compliance-dashboard"></a>Preguntas frecuentes sobre el panel de cumplimiento normativo

- [¿Qué estándares se admiten en el panel de cumplimiento?](#what-standards-are-supported-in-the-compliance-dashboard)
- [¿Por qué algunos controles aparecen atenuados?](#why-do-some-controls-appear-grayed-out)
- [¿Cómo se puede quitar del panel un estándar integrado, como PCI-DSS, ISO 27001 o SOC2 TSP?](#how-can-i-remove-a-built-in-standard-like-pci-dss-iso-27001-or-soc2-tsp-from-the-dashboard)
- [He realizado el cambio sugerido según la recomendación, pero no aparece reflejado en el panel](#i-made-the-suggested-changed-based-on-the-recommendation-yet-it-isnt-being-reflected-in-the-dashboard)
- [¿Qué permisos necesito para acceder al panel de cumplimiento?](#what-permissions-do-i-need-to-access-the-compliance-dashboard)
- [No se carga el panel de cumplimiento normativo](#the-regulatory-compliance-dashboard-isnt-loading-for-me)
- [¿Cómo puedo ver un informe de los controles superados y no superados por estándar en mi panel?](#how-can-i-view-a-report-of-passing-and-failing-controls-per-standard-in-my-dashboard)
- [¿Cómo puedo descargar un informe con datos de cumplimiento en un formato distinto de PDF?](#how-can-i-download-a-report-with-compliance-data-in-a-format-other-than-pdf)
- [¿Cómo puedo crear excepciones para algunas de las directivas en el panel de cumplimiento normativo?](#how-can-i-create-exceptions-for-some-of-the-policies-in-the-regulatory-compliance-dashboard)
- [¿Qué planes o licencias de Microsoft Defender necesito para usar el panel de cumplimiento normativo?](#what-microsoft-defender-plans-or-licenses-do-i-need-to-use-the-regulatory-compliance-dashboard)
- [¿Cómo sé qué punto de referencia o estándar usar?](#how-do-i-know-which-benchmark-or-standard-to-use)

### <a name="what-standards-are-supported-in-the-compliance-dashboard"></a>¿Qué estándares se admiten en el panel de cumplimiento?
De forma predeterminada, el panel de cumplimiento normativo muestra Azure Security Benchmark. Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. Puede obtener más información en [Introducción a las pruebas comparativas de seguridad de Azure](../security/benchmarks/introduction.md).

Para hacer un seguimiento del cumplimiento con cualquier otro estándar, deberá agregarlo explícitamente al panel.
 
Puede agregar otros estándares, como Azure CIS 1.3.0, NIST SP 800-53, NIST SP 800-171, SWIFT CSP CSCF-v2020, UK oficial y UK NHS, HIPAA, Canada Federal PBMM, ISO 27001, SOC2-TSP, and PCI-DSS 3.2.1.  

Se agregarán más estándares al panel y se incluirán en la información sobre cómo [personalizar el conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md).

### <a name="why-do-some-controls-appear-grayed-out"></a>¿Por qué algunos controles aparecen atenuados?
Cada estándar de cumplimiento incluido en el panel incluye una lista de los controles del estándar. Para saber qué controles se pueden aplicar, puede ver los detalles de las evaluaciones superadas y no superadas.

Algunos controles aparecen atenuados, no tienen valoraciones de Defender for Cloud asociadas. Algunos pueden estar relacionados con el procedimiento o el proceso y, por lo tanto, no se pueden comprobar mediante Defender for Cloud. Otros todavía no tienen ninguna directiva o evaluación automatizada implementada, pero la tendrán en el futuro. Y otros controles pueden ser responsabilidad de la plataforma, como se explica en [Responsabilidad compartida en la nube](../security/fundamentals/shared-responsibility.md). 

### <a name="how-can-i-remove-a-built-in-standard-like-pci-dss-iso-27001-or-soc2-tsp-from-the-dashboard"></a>¿Cómo se puede quitar del panel un estándar integrado, como PCI-DSS, ISO 27001 o SOC2 TSP? 
Para personalizar el panel de cumplimiento normativo y centrarse solo en los estándares aplicables, puede quitar cualquiera de los estándares normativos incluidos que no sean aplicables para su organización. Para quitar un estándar, siga las instrucciones de [Eliminación de un estándar del panel](update-regulatory-compliance-packages.md#remove-a-standard-from-your-dashboard).

### <a name="i-made-the-suggested-changed-based-on-the-recommendation-yet-it-isnt-being-reflected-in-the-dashboard"></a>He realizado el cambio sugerido según la recomendación, pero no aparece reflejado en el panel
Después de tomar medidas para resolver las recomendaciones, espere 12 horas para ver los cambios en los datos de cumplimiento. Las evaluaciones se ejecutan aproximadamente cada 12 horas, por lo que este es el tiempo que tardará en verse el efecto en los datos de cumplimiento.
 
### <a name="what-permissions-do-i-need-to-access-the-compliance-dashboard"></a>¿Qué permisos necesito para acceder al panel de cumplimiento?
Para ver los datos de cumplimiento, debe tener también al menos acceso de **lectura** a los datos de cumplimiento de directivas; por lo tanto, el acceso de lector de seguridad solo no será suficiente. Si es lector global de la suscripción, también será suficiente.

El conjunto mínimo de roles para acceder al panel y administrar los estándares es **Colaborador de la directiva de recursos** y **Administrador de seguridad**.


### <a name="the-regulatory-compliance-dashboard-isnt-loading-for-me"></a>No se carga el panel de cumplimiento normativo
Para usar el panel de cumplimiento normativo, Microsoft Defender for Cloud debe tener Defender habilitado en el nivel de suscripción. Si el panel no se carga correctamente, pruebe los pasos siguientes:

1. Borre la caché del explorador.
1. Pruebe con otro explorador.
1. Intente abrir el panel desde otra ubicación de red.


### <a name="how-can-i-view-a-report-of-passing-and-failing-controls-per-standard-in-my-dashboard"></a>¿Cómo puedo ver un informe de los controles superados y no superados por estándar en mi panel?
En el panel principal, puede ver un informe de los controles superados y no superados de (1) los 4 principales estándares de cumplimiento con el resultado más bajo en el panel. Para ver todo el estado de todos los controles superados y no superados, seleccione (2) **Mostrar todo *x*** (donde x es el número de estándares de los que está realizando el seguimiento). Un plano de contexto muestra el estado de cumplimiento de cada uno de los estándares de los que se realiza el seguimiento.

:::image type="content" source="media/regulatory-compliance-dashboard/summaries-of-compliance-standards.png" alt-text="Sección de resumen del panel de cumplimiento normativo.":::


### <a name="how-can-i-download-a-report-with-compliance-data-in-a-format-other-than-pdf"></a>¿Cómo puedo descargar un informe con datos de cumplimiento en un formato distinto de PDF?
Al seleccionar **Descargar Informe**, puede seleccionar el estándar y el formato (PDF o CSV). El informe resultante reflejará el conjunto actual de suscripciones que ha seleccionado en el filtro del portal.

- El informe PDF muestra el estado resumido del estándar seleccionado.
- El informe CSV proporciona resultados detallados por recurso, ya que los relaciona con las directivas asociadas a cada control.

Actualmente, no se admite la descarga del informe de una directiva personalizada, solo se pueden descargar los estándares normativos proporcionados.


### <a name="how-can-i-create-exceptions-for-some-of-the-policies-in-the-regulatory-compliance-dashboard"></a>¿Cómo puedo crear excepciones para algunas de las directivas en el panel de cumplimiento normativo?
En el caso de las directivas integradas en Defender for Cloud y que se incluyen en la puntuación de seguridad, puede crear exenciones para uno o más recursos directamente en el portal, tal como se explica en [Exención de recursos y recomendaciones de la puntuación de seguridad](exempt-resource.md).

En el caso de otras directivas, puede crear una exención directamente en la propia directiva, siguiendo las instrucciones de [Estructura de exención de Azure Policy](../governance/policy/concepts/exemption-structure.md).


### <a name="what-microsoft-defender-plans-or-licenses-do-i-need-to-use-the-regulatory-compliance-dashboard"></a>¿Qué planes o licencias de Microsoft Defender necesito para usar el panel de cumplimiento normativo?
Si tiene *cualquiera* de los planes de Microsoft Defender habilitados en *cualquiera* de los recursos de Azure, puede acceder al panel de cumplimiento normativo de Defender for Cloud y a todos sus datos.


### <a name="how-do-i-know-which-benchmark-or-standard-to-use"></a>¿Cómo sé qué punto de referencia o estándar usar?
[Azure Security Benchmark](/security/benchmark/azure/introduction) (ASB) es el conjunto canónico de recomendaciones de seguridad y procedimientos recomendados definidos por Microsoft, alineado con marcos de control de cumplimiento comunes, incluidos [Microsoft Azure Foundations Benchmark CIS](https://www.cisecurity.org/benchmark/azure/) y [NIST SP 800-53](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final). ASB es un completo punto de referencia diseñado para recomendar las funcionalidades de seguridad más actualizadas de una amplia gama de servicios de Azure. Se recomienda ASB a los clientes que quieran maximizar su posición de seguridad y alinear su estado de cumplimiento con los estándares del sector.

El [punto de referencia CIS](https://www.cisecurity.org/benchmark/azure/) está creado por una entidad independiente (Centro para la seguridad en Internet [CIS]) y contiene recomendaciones sobre un subconjunto de servicios principales de Azure. Trabajamos con CIS para intentar garantizar que sus recomendaciones estén actualizadas con las últimas mejoras de Azure, pero a veces se quedan atrás y quedan obsoletas. Sin embargo, a algunos clientes les gusta usar esta evaluación de terceros objetiva de CIS como línea base de seguridad inicial y principal.

Desde que se ha publicado, muchos clientes han elegido migrar a Azure Security Benchmark como sustituto de los puntos de referencia de CIS.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a utilizar el panel de cumplimiento normativo de Defender for Cloud para:

> [!div class="checklist"]
> * Vea y supervise su estado de cumplimiento con arreglo a los estándares y regulaciones que le interesen.
> * Mejorar el estado de cumplimiento mediante la resolución de las recomendaciones pertinentes y la observación de la mejora en la puntuación del cumplimiento.

El panel de cumplimiento normativo puede simplificar considerablemente el proceso de cumplimiento y reducir en gran medida el tiempo necesario para recopilar las pruebas de cumplimiento en un entorno de Azure, un entorno híbrido y un entorno con varias nubes.

Para más información, consulte los artículos relacionados:

- [Personalización del conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md): aprenda a seleccionar los estándares que aparecen en el panel de cumplimiento normativo. 
- [Administración de recomendaciones de seguridad en Defender for Cloud](review-security-recommendations.md): aprenda a usar las recomendaciones de Defender for Cloud como ayuda para la protección de los recursos de Azure.
