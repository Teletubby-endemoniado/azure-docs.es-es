---
title: ¿Qué es el riesgo? Azure AD Identity Protection
description: Explicación del riesgo en Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/28/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01d2e07e10c22a8a01c32391c43367d3e548a758
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132714435"
---
# <a name="what-is-risk"></a>¿Qué es el riesgo?

Las detecciones de riesgo en Azure AD Identity Protection incluyen todas las acciones sospechosas identificadas, relacionadas con las cuentas de usuario en el directorio. Las detecciones de riesgo (vinculadas tanto al usuario como al inicio de sesión) contribuyen a la puntuación de riesgo general del usuario que se encuentra en el informe Usuarios de riesgo.

Identity Protection proporciona a las organizaciones el acceso a recursos eficaces, para ver y responder rápidamente a estas acciones sospechosas. 

![Información general sobre seguridad que muestra usuarios e inicios de sesión de riesgo](./media/concept-identity-protection-risks/identity-protection-security-overview.png)

> [!NOTE]
> Identity Protection genera detecciones de riesgo solo cuando se usan las credenciales correctas. El hecho de que se usen credenciales incorrectas en un inicio de sesión no pone en peligro las credenciales.

## <a name="risk-types-and-detection"></a>Tipos de riesgo y detección

El riesgo se puede detectar a nivel de **Usuario** y de **Inicio de sesión**; además, existen dos tipos de detección o cálculo: **Tiempo real** y **Sin conexión**.

Es posible que las detecciones en tiempo real no se muestren en los informes durante un tiempo comprendido entre cinco y diez minutos. Es posible que las detecciones sin conexión no se muestren en los informes durante 48 horas.

> [!NOTE] 
> Nuestro sistema puede detectar que el evento de riesgo que ha contribuido a la puntuación de riesgo del usuario de riesgo era un falso positivo o que se ha corregido el riesgo del usuario con la aplicación de directivas, como al completar una solicitud de MFA o un cambio de contraseña seguro. Por lo tanto, el sistema descartará el estado de riesgo y se mostrará un detalle de riesgo de "La IA confirmó que el inicio de sesión es seguro" y dejará de contribuir al riesgo del usuario. 

### <a name="user-linked-detections"></a>Detecciones vinculadas al usuario

Se puede detectar una actividad de riesgo para un usuario que no está vinculada a un inicio de sesión malintencionado específico, sino al propio usuario. 

Estos riesgos se calculan sin conexión, usando orígenes de inteligencia sobre amenazas internas y externas de Microsoft, incluidos los investigadores de seguridad, los profesionales de la aplicación de la ley, los equipos de seguridad de Microsoft y otros orígenes de confianza.

| Detección de riesgos | Descripción |
| --- | --- |
| Credenciales con fugas | Este tipo de detección de riesgo indica que se han filtrado las credenciales válidas del usuario. Cuando los cibercriminales llegan a poner en peligro las contraseñas válidas de usuarios legítimos, es frecuente que las compartan. Normalmente lo hacen publicándolas en la Web oscura, los sitios de pegado, o bien mediante el intercambio o la venta de esas credenciales en el mercado negro. Cuando el servicio de credenciales filtradas de Microsoft adquiere las credenciales de usuario de la Web oscura, los sitios de pegado u otros orígenes, se comparan con las credenciales válidas actuales de los usuarios de Azure AD para encontrar coincidencias válidas. Para obtener más información sobre las credenciales filtradas, consulte [Preguntas frecuentes](#common-questions). |
| Inteligencia de Azure AD sobre amenazas | Este tipo de detección de riesgo indica una actividad de usuario poco común para el usuario en cuestión o coherente con patrones de ataque conocidos basados en orígenes de inteligencia sobre amenazas internas y externas de Microsoft. |

### <a name="sign-in-risk"></a>Riesgo de inicio de sesión

Un riesgo de inicio de sesión representa la probabilidad de que el propietario de la identidad no haya autorizado una solicitud de autenticación determinada. 

Estos riesgos se pueden calcular en tiempo real o sin conexión, usando orígenes de inteligencia sobre amenazas internas y externas de Microsoft, incluidos los investigadores de seguridad, los profesionales de la aplicación de la ley, los equipos de seguridad de Microsoft y otros orígenes de confianza.

| Detección de riesgos | Tipo de detección | Descripción |
| --- | --- | --- |
| Dirección IP anónima | Tiempo real | Este tipo de detección de riesgos indica inicios de sesión desde una dirección IP anónima (por ejemplo, el explorador Tor o redes VPN anónimas). Estas direcciones IP normalmente las usan actores que quieren ocultar su telemetría de inicio de sesión (dirección IP, ubicación, dispositivo, etc.) con fines potencialmente malintencionados. |
| Viaje atípico | Sin conexión | Este tipo de detección de riesgos identifica dos inicios de sesión procedentes de ubicaciones geográficamente distantes, donde al menos una de las ubicaciones puede también ser inusual para el usuario, según su comportamiento anterior. Entre otros factores, este algoritmo de aprendizaje automático tiene en cuenta el tiempo entre los dos inicios de sesión y el tiempo que habría necesitado el usuario para viajar de la primera ubicación a la segunda, lo que indica que otro usuario utiliza las mismas credenciales. <br><br> Este algoritmo omite "falsos positivos" obvios que contribuyen a una condición de viaje imposible, como las VPN y las ubicaciones que usan con regularidad otros usuarios de la organización. El sistema tiene un período de aprendizaje inicial de 14 días o 10 inicios de sesión, lo que ocurra primero, durante el cual aprende el comportamiento de inicio de sesión del nuevo usuario. |
| Token anómalo | Sin conexión | Esta detección indica que hay características anómalas en el token, como una duración inusual del token o un token que se reproduce desde una ubicación desconocida. Esta detección abarca los tokens de sesión y los tokens de actualización. |
| Anomalía del emisor de tokens | Sin conexión |Esta detección de riesgo indica que el emisor del token SAML asociado puede estar en peligro. Las notificaciones incluidas en el token son inusuales o coinciden con patrones de atacante conocidos. |
| Dirección IP vinculada al malware | Sin conexión | Este tipo de detección de riesgos indica inicios de sesión desde direcciones IP infectadas con malware, que se sabe que se comunican activamente con un servidor bot. Esta detección se determina mediante la correlación de direcciones IP del dispositivo del usuario con direcciones IP que han estado en contacto con un servidor bot mientras este último estaba activo. <br><br> **[Esta detección ha quedado en desuso](../fundamentals/whats-new.md#planned-deprecation---malware-linked-ip-address-detection-in-identity-protection)** . Identity Protection ya no generará nuevas detecciones de "Dirección IP vinculada a malware". Los clientes que actualmente tienen detecciones de "Dirección IP vinculada a malware" en su inquilino podrán verlas, corregirlas o descartarlas hasta que se alcance el tiempo de retención de detección de 90 días.|
| Explorador sospechoso | Sin conexión | La detección sospechosa del explorador indica un comportamiento anómalo basado en la actividad de inicio de sesión sospechosa en varios inquilinos de distintos países en el mismo explorador. |
| Propiedades de inicio de sesión desconocidas | Tiempo real | Este tipo de detección de riesgos tiene en cuenta el historial de inicio de sesión anterior (dirección IP, latitud/longitud y ASN) para determinar inicios de sesión anómalos. El sistema almacena información acerca de las ubicaciones anteriores que ha utilizado un usuario y considera estas ubicaciones "conocidas". La detección de riesgos se desencadena cuando el inicio de sesión se produce desde una ubicación que no está en la lista de ubicaciones conocidas. Los usuarios recién creados estarán en "modo de aprendizaje" durante un tiempo, en el que las detecciones de riesgo de las propiedades de inicio de sesión desconocidas estarán desactivadas mientras nuestros algoritmos aprenden el comportamiento del usuario. La duración del modo de aprendizaje es dinámica y depende de cuánto tiempo tarde el algoritmo en recopilar información suficiente sobre los patrones de inicio de sesión del usuario. La duración mínima es de cinco días. Un usuario puede volver al modo de aprendizaje tras un largo período de inactividad. El sistema también ignora los inicios de sesión desde dispositivos conocidos y ubicaciones geográficamente cercanas a una ubicación conocida. <br><br> También se ejecuta esta detección para una autenticación básica o para protocolos heredados. Dado que estos protocolos no tienen propiedades modernas como el identificador de cliente, hay una telemetría limitada para reducir los falsos positivos. Se recomienda que los clientes realicen la migración a la autenticación moderna. <br><br> Se pueden detectar propiedades de inicio de sesión desconocidas en inicios de sesión interactivos y no interactivos. Cuando esta detección se realiza en inicios de sesión no interactivos, merece mayor supervisión debido al riesgo de ataques de reproducción de tokens.  |
| Vulneración de identidad de usuario confirmada por el administrador | Sin conexión | Esta detección indica que un administrador ha seleccionado "Confirmar vulneración de la identidad del usuario" en la interfaz de usuario de Usuarios de riesgo o mediante riskyUsers API. Para ver qué administrador ha confirmado este usuario comprometido, compruebe el historial de riesgos del usuario (a través de la interfaz de usuario o la API). |
| Dirección IP malintencionada | Sin conexión | Esta detección indica el inicio de sesión desde una dirección IP malintencionada. Una dirección IP se considera malintencionada si se recibe una alta tasa de errores debidos a credenciales no válidas desde la dirección IP u otros orígenes de reputación de IP. |
| Reglas de manipulación sospechosa de la bandeja de entrada | Sin conexión | Esta detección se debe a [Microsoft Defender for Cloud Apps](/cloud-app-security/anomaly-detection-policy#suspicious-inbox-manipulation-rules). Esta detección genera un perfil del entorno y activa alertas cuando se establecen reglas sospechosas que eliminan o mueven mensajes o carpetas en la bandeja de entrada de un usuario. Esta detección puede indicar que la cuenta del usuario está en peligro, que los mensajes se están ocultando intencionadamente y que el buzón se está usando para distribuir correo no deseado o malware en su organización. |
| Difusión de contraseña | Sin conexión | Un ataque de difusión de contraseñas es aquel por el que se ataca a varios nombres de usuario mediante contraseñas comunes, en un único ataque por la fuerza bruta, para obtener acceso no autorizado. Esta detección de riesgo se desencadena cuando se realiza un ataque de difusión de contraseñas. |
| Viaje imposible | Sin conexión | Esta detección se debe a [Microsoft Defender for Cloud Apps](/cloud-app-security/anomaly-detection-policy#impossible-travel). Esta detección identifica dos actividades de usuario (en una o varias sesiones) que se originan desde ubicaciones geográficamente distantes dentro de un período de tiempo menor que el tiempo que habría tardado el usuario para viajar de la primera ubicación a la segunda, lo que indica que otro usuario está usando las mismas credenciales. |
| Nuevo país | Sin conexión | Esta detección se debe a [Microsoft Defender for Cloud Apps](/cloud-app-security/anomaly-detection-policy#activity-from-infrequent-country). Esta detección tiene en cuenta las ubicaciones de actividad anteriores para determinar las ubicaciones nuevas e infrecuentes. El motor de detección de anomalías almacena información sobre las ubicaciones anteriores utilizadas por los usuarios de la organización. |
| Actividad desde una dirección IP anónima | Sin conexión | Esta detección se debe a [Microsoft Defender for Cloud Apps](/cloud-app-security/anomaly-detection-policy#activity-from-anonymous-ip-addresses). Esta detección identifica que los usuarios estaban activos desde una dirección IP que se ha identificado como una dirección IP de proxy anónima. |
| Reenvío sospechoso desde la bandeja de entrada | Sin conexión | Esta detección se debe a [Microsoft Defender for Cloud Apps](/cloud-app-security/anomaly-detection-policy#suspicious-inbox-forwarding). Esta detección busca reglas de reenvío de correo electrónico sospechosas, por ejemplo, si un usuario creó una regla de bandeja de entrada que reenvía una copia de todos los correos electrónicos a una dirección externa. |
| Inteligencia de Azure AD sobre amenazas | Sin conexión | Este tipo de detección de riesgo indica una actividad de inicio de sesión poco común para el usuario en cuestión o que es coherente con patrones de ataque conocidos basados en orígenes de inteligencia sobre amenazas internas y externas de Microsoft. |

### <a name="other-risk-detections"></a>Otras detecciones de riesgo

| Detección de riesgos | Tipo de detección | Descripción |
| --- | --- | --- |
| Riesgo adicional detectado | En tiempo real o sin conexión | Esta detección indica que se descubrió una de las detecciones prémium anteriores. Dado que las detecciones premium solo son visibles para los clientes de Azure AD Premium P2, se denominan "Riesgo adicional detectado" para los clientes sin licencias de Azure AD Premium P2. |

## <a name="common-questions"></a>Preguntas frecuentes

### <a name="risk-levels"></a>Niveles de riesgo

Identity Protection clasifica el riesgo en tres niveles: bajo, medio y alto. Al configurar [directivas de Identity Protection personalizadas](./concept-identity-protection-policies.md#custom-conditional-access-policy), también puede configurarla para que se desencadene en el nivel **Sin riesgo**. Sin riesgo significa que no hay ninguna indicación activa de que la identidad del usuario se ha visto en peligro.

Aunque Microsoft no proporciona detalles específicos sobre cómo se calcula el riesgo, podemos decir que cada nivel ofrece una mayor certeza de que el usuario o el inicio de sesión están en peligro. Por ejemplo, cosas como un caso de propiedades de inicio de sesión desconocidas para un usuario podría no ser tan amenazante como la filtración de credenciales para otro usuario.

### <a name="password-hash-synchronization"></a>Sincronización de hash de contraseña

Las detecciones de riesgos, como las credenciales filtradas, requieren la presencia de elementos hash de contraseña para que se produzca la detección. Para obtener más información sobre la sincronización de hash de contraseñas, consulte [Implementación de la sincronización de hash de contraseñas con la sincronización de Azure AD Connect](../hybrid/how-to-connect-password-hash-synchronization.md).

### <a name="why-are-there-risk-detections-generated-for-disabled-user-accounts"></a>¿Por qué se generan detecciones de riesgo para cuentas de usuario deshabilitadas?
          
Las cuentas de usuario deshabilitadas se pueden volver a habilitar. Si las credenciales de una cuenta deshabilitada están en peligro y la cuenta se vuelve a habilitar, los actores no autorizados podrían usar esas credenciales para obtener acceso. Por este motivo, Identity Protection genera detecciones de riesgos para actividades sospechosas en cuentas de usuario deshabilitadas para alertar a los clientes sobre posibles riesgos para la cuenta. Si una cuenta ya no está en uso y no se habilitará de nuevo, los clientes deben considerar la posibilidad de eliminarla para evitar el riesgo. No se generan detecciones de riesgo para las cuentas eliminadas.

### <a name="leaked-credentials"></a>Credenciales con fugas

#### <a name="where-does-microsoft-find-leaked-credentials"></a>¿Dónde busca Microsoft las credenciales filtradas?

Microsoft busca las credenciales filtradas en varios lugares, por ejemplo, en:

- Los sitios de pegado públicos, como pastebin.com y paste.ca, en los que los infiltrados normalmente publican este material. Esta ubicación es la parada obligada para la mayoría de los infiltrados en su búsqueda de credenciales robadas.
- Organismos de autoridad judicial.
- Otros grupos de Microsoft que investigan la web oscura.

#### <a name="why-arent-i-seeing-any-leaked-credentials"></a>¿Por qué no veo ninguna credencial filtrada?

Las credenciales filtradas se procesan siempre que Microsoft encuentra un nuevo lote disponible públicamente. Debido a la naturaleza confidencial, las credenciales filtradas se eliminan al poco tiempo de procesarse. Solo se procesarán en el inquilino las nuevas credenciales filtradas que se encuentren después de habilitar la sincronización de hash de contraseñas (PHS). No se realiza la comprobación con los pares de credenciales encontrados anteriormente. 

#### <a name="i-havent-seen-any-leaked-credential-risk-events-for-quite-some-time"></a>No he encontrado ningún evento de riesgo de credenciales filtradas durante bastante tiempo.

Si no ha detectado ningún evento de riesgo de credenciales filtradas, se debe a los siguientes motivos:

- No tiene PHS habilitada para el inquilino.
- Microsoft no ha encontrado ningún par de credenciales filtradas que coincida con sus usuarios.

#### <a name="how-often-does-microsoft-process-new-credentials"></a>¿Con qué frecuencia Microsoft procesa nuevas credenciales?

Las credenciales se procesan inmediatamente después de que se han encontrado, normalmente varios lotes al día.

### <a name="locations"></a>Ubicaciones

La ubicación en las detecciones de riesgo viene determinada por la búsqueda de direcciones IP.

## <a name="next-steps"></a>Pasos siguientes

- [Directivas disponibles para mitigar los riesgos](concept-identity-protection-policies.md)
- [Información general sobre seguridad](concept-identity-protection-security-overview.md)
