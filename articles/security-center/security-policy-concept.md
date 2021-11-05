---
title: Descripción de las directivas de seguridad, las iniciativas y las recomendaciones en Microsoft Defender for Cloud
description: Información sobre las directivas de seguridad, las iniciativas y las recomendaciones en Microsoft Defender for Cloud
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 03/04/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 00d73987acd0e5137913872138d6f78ac18e9bdf
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131083980"
---
# <a name="what-are-security-policies-initiatives-and-recommendations"></a>¿Qué son las directivas de seguridad, las iniciativas y las recomendaciones?

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Microsoft Defender for Cloud aplica iniciativas de seguridad a las suscripciones. Estas iniciativas contienen una o varias directivas de seguridad. Cada una de esas directivas da como resultado una recomendación de seguridad para mejorar la posición de seguridad. En esta página se explica con detalle cada una de estas ideas.


## <a name="what-is-a-security-policy"></a>¿Qué es una directiva de seguridad?

Una definición de Azure Policy, creada en Azure Policy, es una regla sobre condiciones de seguridad específicas que quiere controlar. Las definiciones integradas incluyen elementos como el control del tipo de recursos que se pueden implementar o la aplicación del uso de etiquetas en todos los recursos. También puede crear su propia definición de directiva personalizada.

Para implementar estas definiciones de directiva (tanto integradas como personalizadas), será preciso asignarlas. Puede asignar cualquiera de estas directivas a través de Azure Portal, PowerShell o la CLI de Azure. Las directivas se pueden habilitar o deshabilitar desde Azure Policy.

Hay diferentes tipos de directivas en Azure Policy. Defender for Cloud utiliza principalmente directivas de "auditoría" que comprueban condiciones y configuraciones específicas y, luego, notifican el cumplimiento. También hay directivas de "aplicación" que se pueden usar para aplicar una configuración segura.

## <a name="what-is-a-security-initiative"></a>¿Qué es una iniciativa de seguridad?

Una iniciativa de Azure Policy es una colección de definiciones de Azure Policy, o reglas, agrupadas para satisfacer un objetivo o propósito concreto. Las iniciativas de Azure simplifican la administración de las directivas al agrupar conjuntos de directivas, de manera lógica, en un único elemento.

Una iniciativa de seguridad define la configuración deseada de las cargas de trabajo y le ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o corporativos.

Al igual que las directivas de seguridad, las iniciativas de Defender for Cloud también se crean en Azure Policy. Puede usar [Azure Policy](../governance/policy/overview.md) para administrar las directivas y crear iniciativas y asignarlas a varias suscripciones o a grupos de administración completos.

La iniciativa predeterminada asignada automáticamente a todas las suscripciones en Microsoft Defender for Cloud es Azure Security Benchmark. Este punto de referencia es el conjunto de directrices específico de Azure y creado por Microsoft relativo a los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. Esta prueba comparativa, que cuenta con amplísimo respaldo, se basa en los controles del [Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/azure/) y del [Instituto Nacional de Normas y Tecnología (NIST)](https://www.nist.gov/), con un enfoque en seguridad centrada en la nube. Mas información sobre [Azure Security Benchmark](/security/benchmark/azure/introduction).

Defender for Cloud le ofrece las siguientes opciones para trabajar con iniciativas y directivas de seguridad:

- **Ver y editar la iniciativa predeterminada integrada**: al habilitar Defender for Cloud, la iniciativa denominada "Azure Security Benchmark" se asigna automáticamente a todas las suscripciones registradas de Defender for Cloud. Para personalizar esta iniciativa, puede habilitar o deshabilitar directivas individuales en ella mediante la edición de los parámetros de una directiva. Consulte la lista de [directivas de seguridad integradas](./policy-reference.md) para comprender las opciones disponibles.

- **Agregar sus propias directivas personalizadas**: si quiere personalizar las iniciativas de seguridad que se aplican a su suscripción, puede hacerlo en Defender for Cloud. A continuación, recibirá recomendaciones si las máquinas no siguen las directivas que creó. Para instrucciones sobre la creación y asignación de directivas personalizadas, consulte [Uso de iniciativas y directivas de seguridad personalizadas](custom-security-policies.md).

- **Agregar estándares de cumplimiento normativo como iniciativas**: el panel de cumplimiento normativo de Defender for Cloud muestra el estado de todas las evaluaciones del entorno, en el contexto de una normativa o estándar determinado (por ejemplo, Azure CIS, NIST SP 800-53 R4 o SWIFT CSP CSCF-v2020). Para obtener más información, consulte [Mejora del cumplimiento normativo](regulatory-compliance-dashboard.md).

## <a name="what-is-a-security-recommendation"></a>¿Qué es una recomendación de seguridad?

Mediante las directivas, Defender for Cloud analiza periódicamente el estado de cumplimiento de los recursos para identificar posibles debilidades o errores de configuración de seguridad. Luego, proporciona recomendaciones sobre cómo corregir esos problemas. Las recomendaciones son el resultado de la evaluación de los recursos con respecto a las directivas pertinentes y la identificación de los recursos que no cumplen los requisitos definidos.

Defender for Cloud realiza sus recomendaciones de seguridad en función de las iniciativas elegidas. Cuando una directiva de una iniciativa se compara con los recursos y se encuentra que uno o varios no se ajustan al cumplimiento normativo, se presenta como una recomendación en Defender for Cloud.

Las recomendaciones son acciones que se deben llevar a cabo para proteger y consolidar los recursos. Cada recomendación proporciona la siguiente información:

- Descripción breve del problema
- Pasos de corrección que se deben llevar a cabo para implementar la recomendación.
- Recursos afectados.

En la práctica, funciona de la siguiente manera:

1. Azure Security Benchmark es una ***iniciativa*** que contiene requisitos.

    Por ejemplo, las cuentas de Azure Storage deben restringir el acceso a la red para reducir su superficie expuesta de ataques.

1. La iniciativa incluye varias ***directivas***, cada una con un requisito de un tipo de recurso específico. Estas directivas aplican los requisitos en la iniciativa. 

    Para seguir con el ejemplo, el requisito de almacenamiento se aplica con la directiva "Las cuentas de almacenamiento deben restringir el acceso a la red mediante el uso de reglas de red virtual".

1. Microsoft Defender for Cloud evalúa constantemente las suscripciones conectadas. Si detecta un recurso que no cumple una directiva, muestra una ***recomendación*** para corregir esa situación y endurecer la seguridad de los recursos que no cumplan los requisitos de seguridad.

    Por ejemplo, si una cuenta de Azure Storage de alguna de sus suscripciones protegidas no está protegida con reglas de red virtual, aparecerá la recomendación para endurecer la seguridad de esos recursos. 

Por lo tanto, (1) una iniciativa incluye (2) directivas que generan (3) recomendaciones específicas del entorno.

## <a name="viewing-the-relationship-between-a-recommendation-and-a-policy"></a>Visualización de la relación entre una recomendación y una directiva

Como se mencionó anteriormente, las recomendaciones integradas de Defender for Cloud se basan en Azure Security Benchmark. Casi todas las recomendaciones tienen una directiva subyacente derivada de un requisito en la prueba comparativa.

Cuando se revisan los detalles de una recomendación, a menudo resulta útil poder ver la directiva subyacente. Para cada recomendación compatible con una directiva, use el vínculo **View policy definition** (Ver definición de directiva) de la página de detalles de la recomendación para ir directamente a la entrada Azure Policy de la directiva pertinente:

:::image type="content" source="media/release-notes/view-policy-definition.png" alt-text="Vínculo a la página de Azure Policy de la directiva específica que admite una recomendación.":::

Use este vínculo para ver la definición de directiva y revisar la lógica de evaluación. 

Si va a revisar la lista de recomendaciones de la [guía de referencia de recomendaciones de seguridad](recommendations-reference.md), también verá vínculos a las páginas de definición de directiva:

:::image type="content" source="media/release-notes/view-policy-definition-from-documentation.png" alt-text="Acceso a la página de Azure Policy de una directiva específica directamente desde la página de referencia de recomendaciones de Microsoft Defender for Cloud":::


## <a name="next-steps"></a>Pasos siguientes

En esta página se explica, en líneas generales, los conceptos básicos y las relaciones entre las directivas, las iniciativas y las recomendaciones. Para obtener información relacionada, consulte:

- [Creación de iniciativas personalizadas](custom-security-policies.md)
- [Deshabilitación de las directivas de seguridad para desactivar las recomendaciones](tutorial-security-policy.md#disable-security-policies-and-disable-recommendations)
- [Obtenga información sobre cómo editar una directiva de seguridad en Azure Policy](../governance/policy/tutorials/create-and-manage.md).
