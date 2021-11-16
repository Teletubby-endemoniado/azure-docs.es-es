---
title: ¿Qué es una puntuación segura de identidad? - Azure Active Directory
description: Aprenda a usar la puntuación de seguridad de la identidad para mejorar el nivel de seguridad del directorio.
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 06/02/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: guptashi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 322643d443aac7cb0ec1aac06b46535114c8d340
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132309517"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory"></a>¿Qué es la puntuación segura de identidad en Azure Active Directory?

¿Es seguro un inquilino de Azure AD? Si no sabe qué responder a esta pregunta, en este artículo se explica cómo le ayuda la puntuación segura de identidad a supervisar y mejorar el nivel de seguridad de la identidad.

## <a name="what-is-an-identity-secure-score"></a>¿Qué es una puntuación segura de identidad?

La puntuación segura de identidad es un porcentaje que funciona como indicador del grado de cumplimiento de las recomendaciones del procedimiento de Microsoft relativo a la seguridad. Cada acción de mejora de la puntuación segura de identidad se adapta a la configuración específica.  

![Puntuación segura](./media/identity-secure-score/identity-secure-score-overview.png)

La puntuación le ayuda a:

- Medir de forma objetiva su nivel de seguridad de la identidad
- Planear la realización de mejoras en la seguridad de la identidad
- Ver si las mejoras han logrado sus objetivos

Puede acceder tanto a la puntuación como a la información relacionada en el panel de la puntuación segura de identidad. En dicho panel, encontrará:

- Su puntuación segura de identidad
- Un grafo de comparación de su puntuación segura de identidad con otros inquilinos del mismo sector y tamaño similar
- Un grafo de tendencias que muestra cómo ha cambiado su puntuación segura de identidad con el tiempo
- Una lista de posibles mejoras

Si realiza las acciones de mejora, puede:

- Mejorar su nivel de seguridad y su puntuación
- Aprovechar las características disponibles para su organización como parte de sus inversiones en identidad

## <a name="how-do-i-get-my-secure-score"></a>¿Cómo se obtiene la puntuación segura?

La puntuación segura de identidad está disponible en todas las ediciones de Azure AD. Las organizaciones pueden acceder a su puntuación de seguridad de la identidad desde **Azure Portal** > **Azure Active Directory** > **Seguridad** > **Puntuación de seguridad de la identidad**.

## <a name="how-does-it-work"></a>¿Cómo funciona?

Cada 48 horas, Azure examina la configuración de seguridad y compara los valores con los procedimientos recomendados. En función del resultado de esta evaluación, se calcula una nueva puntuación para el directorio. Es posible que la configuración de seguridad no esté completamente alineada con la guía del procedimiento recomendado y que las acciones de mejora solo se cumplan parcialmente. En estos casos, solo se le premiará con una parte de la puntuación máxima disponible para el control.

Cada recomendación se mide según la configuración de Azure AD. Si usa productos de terceros para habilitar una recomendación del procedimiento recomendado, puede indicar esta configuración en las opciones de una acción de mejora. También tiene la opción de establecer que se ignoren las recomendaciones si no se aplican a su entorno. Las recomendaciones ignoradas no contribuyen al cálculo de la puntuación.

![Ignore la acción o márquela como cubierta por terceros](./media/identity-secure-score/identity-secure-score-ignore-or-third-party-reccomendations.png)

- **Dirección de destino**: reconoce que la acción de mejora es necesaria y pretende abordarla en algún momento en el futuro. Este estado también se aplica a las acciones que se detectan como parcialmente, pero que no se completan por completo.
- **Planeado:** existen planes concretos para completar la acción de mejora.
- **Riesgo aceptado:** la seguridad siempre debe estar equilibrada con la facilidad de uso y no todas las recomendaciones funcionarán para su entorno. En ese caso, puede optar por aceptar el riesgo, o el riesgo restante, y no promulgar la acción de mejora. No se le dará ningún punto, pero la acción ya no estará visible en la lista de acciones de mejora. Puede ver esta acción en el historial o deshacerla en cualquier momento.
- **Resuelto a través de terceros** y **Resuelto a través de mitigación alternativa**: la acción de mejora ya se ha abordado mediante una aplicación o software de terceros, o una herramienta interna. Obtendrá los puntos que valga la acción, por lo que la puntuación refleja mejor su postura general de seguridad. Si una herramienta interna o de terceros ya no cubre el control, puede elegir otro estado. Tenga en cuenta que Microsoft no tendrá visibilidad sobre la integridad de la implementación si la acción de mejora está marcada como cualquiera de estos estados.

## <a name="how-does-it-help-me"></a>¿Cómo me ayuda?

La puntuación segura le ayuda a:

- Medir de forma objetiva su nivel de seguridad de la identidad
- Planear la realización de mejoras en la seguridad de la identidad
- Ver si las mejoras han logrado sus objetivos

## <a name="what-you-should-know"></a>Qué debería saber

### <a name="who-can-use-the-identity-secure-score"></a>¿Quién puede usar la puntuación segura de identidad?

La puntuación segura de identidad pueden usarla los roles siguientes:

- Administrador global
- Administrador de seguridad
- Lectores de seguridad

### <a name="how-are-controls-scored"></a>¿Cómo se puntúan los controles?

Los controles se pueden puntuar de dos maneras. Algunos se puntúan en forma binaria: obtiene el 100 % de la puntuación si tiene la característica o la opción configurada según nuestra recomendación. Otras puntuaciones se calculan como un porcentaje de la configuración total. Por ejemplo, si la recomendación de mejora indica que obtendrá un 10,71 % como máximo si protege a todos los usuarios con MFA y solo tiene protegidos 5 usuarios de un total de 100, se le otorgaría una puntuación parcial de aproximadamente un 0,53 % (5 protegidos/100 totales * 10,71 % como máximo = 0,53 % de puntuación parcial).

### <a name="what-does-not-scored-mean"></a>¿Qué significa [Not Scored]?

Las acciones con la etiqueta [Not Scored] (Sin puntuación) son las que puede realizar en su organización, pero que no se puntúan porque no están conectadas en la herramienta (aún). Por consiguiente, puede mejorar aún más la seguridad, pero no obtendrá ninguna puntuación por esas acciones ahora mismo.

### <a name="how-often-is-my-score-updated"></a>¿Con qué frecuencia se actualiza la puntuación?

La puntuación se calcula una vez al día (aproximadamente a las 1:00 A.M., hora del Pacífico). Si realiza algún cambio en una acción medida, la puntuación se actualizará automáticamente al día siguiente. Cualquier cambio tarda hasta 48 horas en reflejarse en la puntuación.

### <a name="my-score-changed-how-do-i-figure-out-why"></a>Mi puntuación ha cambiado. ¿Cómo averiguo por qué?

Vaya al [portal de Microsoft 365 Defender](https://security.microsoft.com/), donde encontrará su puntuación de seguridad de Microsoft completa. Puede ver fácilmente todos los cambios de la puntuación segura mediante la revisión de los cambios detallados en la pestaña Historial.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>¿Mide la puntuación segura el riesgo de vulneración de la seguridad?

En pocas palabras, no. La puntuación segura no expresa una medida absoluta de la probabilidad de sufrir una vulneración de la seguridad. Expresa en qué medida ha adoptado características que pueden compensar el riesgo de vulneración de la seguridad. Ningún servicio puede garantizar que no va a sufrir una vulneración de la seguridad y la puntuación segura no se debe interpretar de ningún modo como una garantía.

### <a name="how-should-i-interpret-my-score"></a>¿Cómo debo interpretar la puntuación?

La puntuación mejora por configurar las características de seguridad recomendadas o por realizar tareas relacionadas con la seguridad (como la lectura de informes). Se puntúan algunas acciones por la finalización parcial, como por ejemplo, habilitar la autenticación multifactor (MFA) para los usuarios. Su puntuación segura representa directamente los servicios de seguridad de Microsoft que usa. Recuerde que debe existir un equilibrio entre seguridad y facilidad de uso. Todos los controles de seguridad tienen un componente de impacto en el usuario. Los controles que tienen un impacto bajo en el usuario deben tener poco o ningún efecto en las operaciones diarias de los usuarios.

Para ver el historial de puntuación, diríjase al [portal de Microsoft 365 Defender](https://security.microsoft.com/) y revise la puntuación de seguridad de Microsoft general. Puede revisar los cambios de su puntuación segura general haciendo clic en Vista de historial. Elija una fecha concreta para ver los controles que se han habilitado para ese día y los puntos que ha logrado por cada uno de ellos.

### <a name="how-does-the-identity-secure-score-relate-to-the-microsoft-365-secure-score"></a>¿Cómo se relaciona la puntuación de seguridad de la identidad con la puntuación de seguridad de Microsoft 365?

La [puntuación segura de Microsoft](/office365/securitycompliance/microsoft-secure-score) contiene cinco categorías distintas de control y puntuación:

- Identidad
- data
- Dispositivos
- Infraestructura
- Aplicaciones

La puntuación segura de identidad representa la parte de la identidad de la puntuación segura de Microsoft, lo que significa que las recomendaciones de la puntuación segura de identidad y la puntuación de identidad en Microsoft son idénticas.

## <a name="next-steps"></a>Pasos siguientes

[Obtener más información acerca de la puntuación segura de Microsoft](/office365/securitycompliance/microsoft-secure-score)
