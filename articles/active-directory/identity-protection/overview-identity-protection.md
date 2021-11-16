---
title: ¿Qué es Azure Active Directory Identity Protection?
description: Detección, corrección, investigación y análisis de riesgos con Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 06/15/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sahandle
ms.custom: contperf-fy21q1
ms.collection: M365-identity-device-management
ms.openlocfilehash: b624194285dd5ba7e0a54b8a15d3885a845d64dc
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132283247"
---
# <a name="what-is-identity-protection"></a>¿Qué es Identity Protection?

Identity Protection es una herramienta que permite a las organizaciones realizar tres tareas clave:

- [Automatizar la detección y corrección de riesgos basados en la identidad](howto-identity-protection-configure-risk-policies.md).
- [Investigar los riesgos](howto-identity-protection-investigate-risk.md) con los datos del portal.
- [Exportar los datos de detección de riesgos al SIEM](../../sentinel/data-connectors-reference.md#azure-active-directory-identity-protection).

Identity Protection usa los aprendizajes que Microsoft ha adquirido de su puesto en organizaciones con Azure AD, el espacio de consumidor con cuentas de Microsoft y juegos con Xbox para proteger a los usuarios. Microsoft analiza 6,5 billones de señales al día para identificar y proteger a los clientes frente a amenazas.

Las señales que genera y con las que se alimenta Identity Protection se pueden insertar en herramientas como Acceso condicional para tomar decisiones de acceso o alimentar una herramienta de información de seguridad y administración de eventos (SIEM) para realizar una investigación más detallada en función de las directivas aplicadas.

## <a name="why-is-automation-important"></a>¿Por qué es importante la automatización?

En su [entrada de blog de octubre de 2018](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Eight-essentials-for-hybrid-identity-3-Securing-your-identity/ba-p/275843), Alex Weinert, que dirige al equipo de protección y seguridad de la identidad de Microsoft, explica por qué la automatización es tan importante cuando se trabaja con el volumen de eventos:

> Cada día, nuestros sistemas de aprendizaje automático y heurística proporcionan puntuaciones de riesgo para 18 mil millones de intentos de inicio de sesión para más de 800 millones de cuentas distintas, 300 millones de los cuales proceden claramente de adversarios (entidades como actores criminales o hackers).
>
> El año pasado, en Ignite, hablé sobre los tres ataques principales a nuestros sistemas de identidad. Este es el volumen reciente de estos ataques
>   
>   - **Reproducción de infracciones**: 4600 millones detectados en mayo de 2018
>   - **Difusión de contraseña**: 350 000 en abril de 2018
>   - **Suplantación de identidad (phishing)** : Es difícil de cuantificar con precisión, pero en marzo de 2018, detectamos 23 millones de eventos de riesgo, muchos de los cuales estaban relacionados con la suplantación de identidad (phishing)

## <a name="risk-detection-and-remediation"></a>Detección y corrección de riesgos

Identity Protection identifica riesgos de muchos tipos, entre los que se incluyen:

- Uso de una dirección IP anónima
- Viaje atípico
- Dirección IP vinculada al malware
- Propiedades de inicio de sesión desconocidas
- Credenciales con fugas
- Difusión de contraseña
- Y mucho más...

Puede encontrar más información sobre estos y otros riesgos, y cómo y cuándo se calculan, en el artículo [¿Qué es el riesgo?](concept-identity-protection-risks.md).

Las señales de riesgo pueden desencadenar esfuerzos de corrección, como exigir a los usuarios que usen Azure AD Multi-Factor Authentication, que restablezcan su contraseña mediante el autoservicio de restablecimiento de contraseña o que apliquen bloqueos hasta que un administrador tome medidas.

## <a name="risk-investigation"></a>Investigación del riesgo

Los administradores pueden revisar las detecciones y realizar acciones manuales sobre ellas si es necesario. Hay tres informes clave que los administradores usan para las investigaciones en Identity Protection:

- Usuarios de riesgo
- Inicios de sesión no seguros
- Detecciones de riesgo

Puede encontrar más información en el artículo [Cómo investigar los riesgos](howto-identity-protection-investigate-risk.md).

### <a name="risk-levels"></a>Niveles de riesgo

Identity Protection clasifica el riesgo en tres niveles: bajo, medio y alto. 

Aunque Microsoft no proporciona detalles específicos sobre cómo se calcula el riesgo, diremos que cada nivel aporta una mayor seguridad de que el usuario o el inicio de sesión están en peligro. Por ejemplo, cosas como un caso de propiedades de inicio de sesión desconocidas para un usuario podría no ser tan amenazante como la filtración de credenciales para otro usuario.

## <a name="exporting-risk-data"></a>Exportación de datos de riesgo

Los datos de Identity Protection se pueden exportar a otras herramientas para su archivo y posterior investigación y correlación. Las API basadas en Microsoft Graph permiten a las organizaciones recopilar estos datos para su posterior procesamiento en una herramienta como su SIEM. Puede encontrar información sobre cómo acceder a la API de Identity Protection en el artículo [Introducción a Azure Active Directory Identity Protection y Microsoft Graph](howto-identity-protection-graph-api.md)

Puede encontrar información sobre la integración de información de Identity Protection con Microsoft Azure Sentinel en el artículo [Conexión de datos de Azure AD Identity Protection](../../sentinel/data-connectors-reference.md#azure-active-directory-identity-protection).

Además, Las organizaciones pueden optar por almacenar datos durante períodos más largos. Para ello, modifican la configuración de diagnóstico de Azure AD a fin de enviar datos de RiskyUsers y UserRiskEvents a un área de trabajo de Log Analytics, archivar datos en una cuenta de almacenamiento, transmitir datos a un centro de eventos o enviar datos a una solución de asociado. Puede encontrar información detallada sobre cómo hacerlo en el artículo [Procedimiento de exportación de datos de riesgo](howto-export-risk-data.md).

## <a name="permissions"></a>Permisos

Identity Protection requiere que los usuarios tengan el rol Lector de seguridad, Operador de seguridad, Administrador de seguridad, Lector global o Administrador global para poder acceder.

| Role | Puede hacer | No se puede hacer |
| --- | --- | --- |
| Administrador global | Acceso completo a Identity Protection |   |
| Administrador de seguridad | Acceso completo a Identity Protection | Restablecer la contraseña de un usuario |
| Operador de seguridad | Ver todos los informes de Identity Protection y la hoja de información general <br><br> Descartar el riesgo del usuario, confirmar el inicio de sesión seguro, confirmar el compromiso | Configurar o cambiar directivas <br><br> Restablecer la contraseña de un usuario <br><br> Configurar alertas |
| Lector de seguridad | Ver todos los informes de Identity Protection y la hoja de información general | Configurar o cambiar directivas <br><br> Restablecer la contraseña de un usuario <br><br> Configurar alertas <br><br> Enviar comentarios sobre las detecciones |

Actualmente, el rol de operador de seguridad no puede acceder al informe de inicios de sesión de riesgo.

Los administradores de acceso condicional también pueden crear directivas que representen el riesgo de inicio de sesión como una condición. Obtenga más información en el artículo [Acceso condicional: Condiciones](../conditional-access/concept-conditional-access-conditions.md#sign-in-risk).

## <a name="license-requirements"></a>Requisitos de licencia

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

| Capacidad | Detalles  | Aplicaciones de Azure AD Free y Microsoft 365 | Azure AD Premium P1|Azure AD Premium P2 |
| --- | --- | --- | --- | --- |
| Directivas de riesgo | Directiva de riesgo de usuario (mediante Identity Protection)  | No | No |Sí | 
| Directivas de riesgo | Directiva de riesgo de inicio de sesión (mediante Identity Protection o acceso condicional)  | No |  No |Sí |
| Informes de seguridad | Información general |  No | No |Sí |
| Informes de seguridad | Usuarios de riesgo  | Información limitada. Solo se muestran los usuarios con riesgo medio y alto. No hay ningún cajón de detalles ni historial de riesgos. | Información limitada. Solo se muestran los usuarios con riesgo medio y alto. No hay ningún cajón de detalles ni historial de riesgos. | Acceso total|
| Informes de seguridad | Inicios de sesión no seguros  | Información limitada. No se muestran detalles del riesgo ni el nivel de riesgo. | Información limitada. No se muestran detalles del riesgo ni el nivel de riesgo. | Acceso total|
| Informes de seguridad | Detecciones de riesgo   | No | Información limitada. No hay ningún cajón de detalles.| Acceso total|
| Notificaciones | Alertas detectadas sobre usuarios en riesgo  | No | No |Sí |
| Notificaciones | Resumen semanal| No | No | Sí | 
| | Directiva de registro de MFA | No | No | Sí |

Puede encontrar más información sobre estos informes completos en el artículo [Procedimiento: investigar los riesgos](howto-identity-protection-investigate-risk.md#navigating-the-reports).

## <a name="next-steps"></a>Pasos siguientes

- [Información general sobre seguridad](concept-identity-protection-security-overview.md)

- [¿Qué es el riesgo?](concept-identity-protection-risks.md)

- [Directivas disponibles para mitigar los riesgos](concept-identity-protection-policies.md)
