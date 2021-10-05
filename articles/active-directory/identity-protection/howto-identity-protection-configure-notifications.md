---
title: Notificaciones de Azure Active Directory Identity Protection
description: Obtenga información sobre cómo contribuyen las notificaciones a sus actividades de investigación.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 09/23/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fea2ff8f44cd017d901e94e7458d968d5c82e9f
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128619825"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Notificaciones de Azure Active Directory Identity Protection

Azure AD Identity Protection envía dos tipos de correos electrónicos de notificación automatizados para ayudar a administrar el riesgo del usuario y las detecciones de riesgo:

- Correo electrónico de los usuarios en riesgo detectados
- Correo electrónico de resumen semanal

En este artículo se proporciona una introducción de ambos correos electrónicos de notificación.

No se admite el envío de correos electrónicos a los usuarios con roles asignados por grupos.

## <a name="users-at-risk-detected-email"></a>Correo electrónico de los usuarios en riesgo detectados

En respuesta a una cuenta detectada en riesgo, Azure AD Identity Protection genera una alerta de correo electrónico con el asunto **Usuarios en riesgo detectados**. El correo electrónico incluye un vínculo al informe **[Usuarios marcados en riesgo](./overview-identity-protection.md)** . Como práctica recomendada, debería investigar inmediatamente los usuarios en peligro.

La configuración de esta alerta permite especificar a qué nivel de riesgo del usuario desea que se genere la alerta. El correo electrónico se generará cuando el nivel de riesgo del usuario alcance lo que haya especificado. Por ejemplo, si establece la directiva para alertar sobre el riesgo de usuario medio y la puntuación del riesgo del usuario John pasa a riesgo medio debido a un riesgo de inicio de sesión en tiempo real, recibirá el correo electrónico de usuarios en riesgo detectados. Si el usuario tiene detecciones de riesgo posteriores que hacen que el cálculo de su nivel de riesgo sea el especificado (o superior), recibirá correos electrónicos de usuarios en riesgo detectados cuando se vuelva a calcular la puntuación del riesgo del usuario. Por ejemplo, si un usuario pasa a un riesgo medio el 1 de enero, recibirá una notificación por correo electrónico si la configuración está establecida para alertar sobre el riesgo medio. Si ese mismo usuario tiene otra detección de riesgos el 5 de enero que también es de riesgo medio y su puntuación de riesgo se vuelve a calcular y sigue siendo de nivel medio, recibirá otra notificación por correo electrónico. 

Pero solo se enviará una notificación por correo electrónico adicional si la detección de riesgos (que ha provocado el cambio en el nivel de riesgo del usuario) es más reciente que el envío del último correo electrónico. Por ejemplo, un usuario inicia sesión el 1 de enero a las 5:00 y no hay ningún riesgo en tiempo real (lo que significa que no se generará ningún correo electrónico debido a ese inicio de sesión). Diez minutos después, a las 5:10, el mismo usuario vuelve a iniciar sesión y tiene un riesgo alto en tiempo real, lo que hace que su nivel de riesgo cambie a alto y se envíe un correo electrónico. Después, a las 5:15, la puntuación de riesgo sin conexión para el inicio de sesión original de las 5:00 cambia a riesgo alto debido al procesamiento de riesgos sin conexión. No se enviará un correo electrónico adicional de usuario marcado como de riesgo, ya que el primer inicio de sesión ha sido anterior al segundo que ya ha desencadenado una notificación por correo electrónico.

Para evitar una sobrecarga de correos electrónicos, solo recibirá un correo electrónico en un período de cinco segundos. Esta demora significa que si varios usuarios pasan al nivel de riesgo especificado durante el mismo período de cinco segundos, se agregarán y se enviará un solo mensaje de correo electrónico para representar el cambio de nivel de riesgo de todos ellos.

Si su organización ha habilitado la corrección automática, como se describe en el artículo [Experiencias de usuario con Azure AD Identity Protection](concept-identity-protection-user-experience.md), existe la posibilidad de que el usuario pueda corregir el riesgo antes de que usted tenga la oportunidad de investigar. Para ver los usuarios de riesgo y los inicios de sesión de riesgo que se han corregido, puede agregar "Corregido" al filtro **Estado de riesgo** de en los informes **Usuarios de riesgo** o **Inicios de sesión de riesgo**.

![Correo electrónico de los usuarios en riesgo detectados](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Configuración de alertas detectadas sobre usuarios en riesgo

Como administrador, puede establecer:

- **El nivel de riesgo de usuario que desencadena la generación de este correo electrónico**: de forma predeterminada, el nivel de riesgo se establece en "Alto" riesgo.
- **Los destinatarios de este correo electrónico**: los usuarios con los roles de administrador de empresa, administrador de seguridad o lector de seguridad se agregan automáticamente a esta lista. Intentamos enviar correos electrónicos a los 20 primeros miembros de cada rol. Si un usuario está inscrito en PIM para subir a uno de estos roles previa petición, **solo recibirá mensajes de correo electrónico si se sube en el momento en que se envía el correo electrónico**.
   - Opcionalmente, puede **agregar correo electrónico personalizado aquí**. Los usuarios definidos deben tener los permisos adecuados para ver los informes vinculados en Azure Portal.

Configure el correo electrónico de los usuarios en riesgo en **Azure Portal** en **Azure Active Directory** > **Seguridad** > **Identity Protectionl** > **Alertas detectadas sobre usuarios en riesgo**.

## <a name="weekly-digest-email"></a>Correo electrónico de resumen semanal

El correo electrónico de resumen semanal contiene un sumario de nuevas detecciones de riesgo.  
Incluye:

- Se detectaron nuevos usuarios de riesgo
- Se detectaron nuevos inicios de sesión de riesgo (en tiempo real)
- Vínculos a los informes relacionados en Identity Protection

![Correo electrónico de resumen semanal](./media/howto-identity-protection-configure-notifications/weekly-digest-email.png)

Los usuarios con los roles de administrador de empresa, administrador de seguridad o lector de seguridad se agregan automáticamente a esta lista. Intentamos enviar correos electrónicos a los 20 primeros miembros de cada rol. Si un usuario está inscrito en PIM para subir a uno de estos roles previa petición, **solo recibirá mensajes de correo electrónico si se sube en el momento en que se envía el correo electrónico**.

### <a name="configure-weekly-digest-email"></a>Configuración de correo electrónico de resumen semanal

Como administrador, puede activar o desactivar el envío de un correo electrónico de resumen semanal y elegir a los usuarios asignados para recibir el correo electrónico.

Configure el correo electrónico de resumen semanal en **Azure Portal** bajo **Azure Active Directory** > **Seguridad** > **Identity Protection** > **Resumen semanal**.

## <a name="see-also"></a>Consulte también

- [Azure Active Directory Identity Protection](./overview-identity-protection.md)
