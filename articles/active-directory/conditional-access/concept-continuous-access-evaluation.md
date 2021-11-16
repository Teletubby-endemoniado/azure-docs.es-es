---
title: Evaluación continua de acceso en Azure AD
description: Respuesta más rápida a los cambios en el estado del usuario con la evaluación continua de acceso en Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/21/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: jlu
ms.custom: has-adal-ref
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4de2ed185b3a4421a06ac9b3c88df68821b61adf
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132307998"
---
# <a name="continuous-access-evaluation"></a>Evaluación continua de acceso

La expiración y actualización de los tokens es un mecanismo estándar del sector. Cuando una aplicación cliente como Outlook se conecta a un servicio como Exchange Online, las solicitudes de API se autorizan mediante tokens de acceso de OAuth 2.0. De manera predeterminada, los tokens de acceso son válidos durante una hora; cuando expiran, se redirige al cliente a Azure AD para actualizarlos. Ese período de actualización proporciona una oportunidad para volver a evaluar las directivas de acceso de los usuarios. Por ejemplo: podemos optar por no actualizar el token debido a una directiva de acceso condicional o porque el usuario se ha deshabilitado en el directorio. 

Los clientes han expresado su preocupación por el retraso entre el momento en que cambian las condiciones de un usuario y el momento en que se aplican los cambios de directiva. Azure AD ha experimentado con el enfoque directo de duraciones de tokens reducidas, pero hemos descubierto que pueden degradar las experiencias y la confiabilidad de los usuarios y no eliminan los riesgos.

La respuesta oportuna a las infracciones de las directivas o a los problemas de seguridad requiere realmente una "conversación" entre el emisor del token (Azure AD) y el usuario de confianza (la aplicación). Esta conversación bidireccional nos proporciona dos funcionalidades importantes. El usuario de confianza puede ver cuándo cambian las propiedades, como la ubicación de red, e indicárselo al emisor del token. También proporciona al emisor del token una manera de indicar al usuario de confianza que deje de respetar los tokens de un usuario determinado debido a que la cuenta esté en peligro, se haya deshabilitado u otros problemas. El mecanismo para esta conversación es la evaluación continua de acceso (CAE). El objetivo es que la respuesta sea casi en tiempo real, pero se puede observar una latencia de hasta 15 minutos debido al tiempo de propagación de los eventos.

La implementación inicial de la evaluación continua de acceso se centra en Exchange, Teams y SharePoint Online.

Para preparar las aplicaciones para el uso de CAE, consulte [Uso de las API habilitadas para la evaluación continua de acceso en las aplicaciones](../develop/app-resilience-continuous-access-evaluation.md).

### <a name="key-benefits"></a>Ventajas principales

- Terminación del usuario y cambio o restablecimiento de la contraseña: la revocación de la sesión de usuario se aplicará casi en tiempo real.
- Cambio de ubicación de red: las directivas de ubicación del acceso condicional se aplicarán casi en tiempo real.
- La exportación de tokens a una máquina fuera de una red de confianza se puede evitar con las directivas de ubicación del acceso condicional.

## <a name="scenarios"></a>Escenarios 

Hay dos escenarios que conforman la evaluación continua de acceso: la evaluación de eventos críticos y la evaluación de directivas de acceso condicional.

### <a name="critical-event-evaluation"></a>Evaluación de eventos críticos

La evaluación continua de acceso se implementa mediante la habilitación de servicios, como Exchange Online, SharePoint Online y Teams, para suscribirse a eventos críticos de Azure AD. A continuación, estos eventos se pueden evaluar y aplicar casi en tiempo real. La evaluación de los eventos críticos no se basa en las directivas de acceso condicional, por lo que está disponible en cualquier inquilino. Actualmente se evalúan los siguientes eventos:

- La cuenta de usuario se ha eliminado o deshabilitado.
- La contraseña de un usuario ha cambiado o se ha restablecido.
- Se habilita la autenticación multifactor para el usuario.
- El administrador revoca explícitamente todos los tokens de actualización de un usuario.
- Riesgo de usuario elevado detectado por Azure AD Identity Protection

Este proceso habilita el escenario en el que los usuarios pierden el acceso a los archivos, el correo electrónico, el calendario o las tareas de SharePoint Online de la organización y Teams desde las aplicaciones cliente de Microsoft 365 en cuestión de minutos después de un evento crítico. 

> [!NOTE] 
> Teams y SharePoint Online no admiten eventos de riesgo de usuario.

### <a name="conditional-access-policy-evaluation-preview"></a>Evaluación de directivas de acceso condicional (versión preliminar)

Exchange Online, SharePoint Online, Teams y MS Graph pueden sincronizar las directivas de acceso condicional principales para su evaluación dentro del propio servicio.

Este proceso habilita el escenario en el que los usuarios pierden el acceso a los archivos, el correo electrónico, el calendario o las tareas de la organización desde las aplicaciones cliente de Microsoft 365 o SharePoint Online inmediatamente después de un cambio de ubicación de red.

> [!NOTE]
> No se admiten todas las combinaciones de proveedores de recursos y aplicaciones. Consulte la tabla siguiente. Office hace referencia a Word, Excel y PowerPoint.

| | Outlook Web | Outlook Win32 | Outlook iOS | Outlook Android | Outlook Mac |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **SharePoint Online** | Compatible | Compatible | Compatible | Compatible | Compatible |
| **Exchange Online** | Compatible | Compatible | Compatible | Compatible | Compatible |

| | Aplicaciones web de Office | Aplicaciones Win32 de Office | Office para iOS | Office para Android | Office para Mac |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **SharePoint Online** | No compatible | Compatible | Compatible | Compatible | Compatible |
| **Exchange Online** | No compatible | Compatible | Compatible | Compatible | Compatible |

| | OneDrive web | OneDrive Win32 | OneDrive iOS | OneDrive Android | OneDrive Mac |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **SharePoint Online** | Compatible | No compatible | Compatible | Compatible | No compatible |

| | Teams web | Teams Win32 | Teams iOS | Teams Android | Teams Mac |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Servicio Teams** | Compatible | Compatible | Compatible | Compatible | Compatible |
| **SharePoint Online** | Compatible | Compatible | Compatible | Compatible | Compatible |
| **Exchange Online** | Compatible | Compatible | Compatible | Compatible | Compatible |

## <a name="client-capabilities"></a>Funcionalidades del cliente

### <a name="client-side-claim-challenge"></a>Desafío de notificaciones del lado cliente

Antes de la evaluación continua de acceso, los clientes podían reproducir el token de acceso desde su memoria caché, siempre y cuando no hubiera expirado. Con CAE, se presenta un nuevo caso en el que un proveedor de recursos puede rechazar un token aunque no haya expirado. Para informar a los clientes de que omitan su memoria caché aunque los tokens almacenados en caché no hayan expirado, presentamos un mecanismo llamado **desafío de notificaciones** para indicar que se ha rechazado el token y que Azure AD debe emitir un nuevo token de acceso. CAE requiere una actualización de cliente para comprender el desafío de notificaciones. La versión más reciente de las siguientes aplicaciones es compatible con el desafío de notificaciones:

| | Web | Win32 | iOS | Android | Mac |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Outlook** | Compatible | Compatible | Compatible | Compatible | Compatible |
| **Teams** | Compatible | Compatible | Compatible | Compatible | Compatible |
| **Office** | No compatible | Compatible | Compatible | Compatible | Compatible |
| **OneDrive** | Compatible | Compatible | Compatible | Compatible | Compatible |

### <a name="token-lifetime"></a>Duración del token

Dado que el riesgo y la directiva se evalúan en tiempo real, los clientes que negocian sesiones habilitadas para la evaluación continua de acceso ya no confían en directivas estáticas de vigencia del token de acceso. Este cambio significa que la directiva de vigencia del token configurable no se respeta para los clientes que negocian sesiones habilitadas para CAE.

La vigencia del token se incrementa para tener una duración larga, hasta 28 horas, en las sesiones de CAE. La revocación se basa en los eventos críticos y en la evaluación de directivas, no solo en un período de tiempo arbitrario. Este cambio aumenta la estabilidad de las aplicaciones sin afectar a la postura de seguridad. 

Si no usa clientes habilitados para CAE, la duración predeterminada del token de acceso permanecerá en 1 hora. El valor predeterminado solo cambia si ha configurado la duración del token de acceso con la característica en versión preliminar [Vigencia de token configurable (CTL)](../develop/active-directory-configurable-token-lifetimes.md).

## <a name="example-flow-diagrams"></a>Diagramas de flujo de ejemplo

### <a name="user-revocation-event-flow"></a>Flujo de eventos de revocación de usuario

![Flujo de eventos de revocación de usuario](./media/concept-continuous-access-evaluation/user-revocation-event-flow.png)

1. Un cliente compatible con CAE presenta credenciales o un token de actualización a Azure AD para solicitar un token de acceso para algún recurso.
1. Se devuelve un token de acceso junto con otros artefactos al cliente.
1. El administrador [revoca explícitamente todos los tokens de actualización de un usuario](/powershell/module/azuread/revoke-azureaduserallrefreshtoken). Se enviará un evento de revocación al proveedor de recursos desde Azure AD.
1. Se presenta un token de acceso al proveedor de recursos. El proveedor de recursos evalúa la validez del token y comprueba si hay algún evento de revocación para el usuario. El proveedor de recursos utiliza esta información para decidir si se concede acceso al recurso.
1. En este caso, el proveedor de recursos deniega el acceso y envía un error 401 y un desafío de notificaciones al cliente.
1. El cliente compatible con CAE comprende el desafío de notificaciones 401+. Omite las cachés y vuelve al paso 1, enviando su token de actualización junto con el desafío de notificaciones de vuelta a Azure AD. A continuación, Azure AD volverá a evaluar todas las condiciones y solicitará al usuario que vuelva a autenticarse en este caso.

### <a name="user-condition-change-flow-preview"></a>Flujo del cambio de condición del usuario (versión preliminar)

En el ejemplo siguiente, un administrador de acceso condicional ha configurado una directiva de acceso condicional basada en la ubicación para permitir solo el acceso desde intervalos IP específicos:

![Flujo de eventos de condición del usuario](./media/concept-continuous-access-evaluation/user-condition-change-flow.png)

1. Un cliente compatible con CAE presenta credenciales o un token de actualización a Azure AD para solicitar un token de acceso para algún recurso.
1. Azure AD evalúa todas las directivas de acceso condicional para ver si el usuario y el cliente cumplen las condiciones.
1. Se devuelve un token de acceso junto con otros artefactos al cliente.
1. El usuario sale de un intervalo de direcciones IP permitido.
1. El cliente presenta un token de acceso al proveedor de recursos desde fuera de un intervalo de direcciones IP permitido.
1. El proveedor de recursos evalúa la validez del token y comprueba la directiva de ubicación sincronizada desde Azure AD.
1. En este caso, el proveedor de recursos deniega el acceso y envía un error 401 y un desafío de notificaciones al cliente. El cliente tiene un desafío porque no procede de un intervalo IP permitido.
1. El cliente compatible con CAE comprende el desafío de notificaciones 401+. Omite las cachés y vuelve al paso 1, enviando su token de actualización junto con el desafío de notificaciones de vuelta a Azure AD. Azure AD vuelve a evaluar todas las condiciones y denegará el acceso en este caso.

## <a name="enable-or-disable-cae-preview"></a>Habilitación o deshabilitación de CAE (versión preliminar)

La configuración de CAE se ha movido a la hoja Acceso condicional. Los nuevos clientes de CAE pueden acceder a CAE y alternar directamente mientras crean directivas de acceso condicional. Pero algunos clientes existentes tienen que realizar la migración para poder empezar a acceder a CAE mediante Acceso condicional.

#### <a name="migration"></a>Migración

Los clientes que han configurado los valores de CAE en Seguridad tienen que migrar esta configuración a una directiva de acceso condicional. Use los pasos siguientes para migrar la configuración de CAE a una directiva de acceso condicional.

:::image type="content" source="media/concept-continuous-access-evaluation/migrate-continuous-access-evaluation.png" alt-text="Vista del portal que muestra la opción de migrar la evaluación continua de acceso a una directiva de acceso condicional." lightbox="media/concept-continuous-access-evaluation/migrate-continuous-access-evaluation.png":::

1. Inicie sesión en **Azure Portal** como administrador de acceso condicional, administrador de seguridad o administrador global. 
1.  Vaya a **Azure Active Directory** > **Seguridad** > **Evaluación continua de acceso (versión preliminar)** . 
1.  A continuación, verá la opción de **Migrar** la directiva. Esta acción es la única a la que tendrá acceso en este momento.
1. Vaya a **Acceso condicional** y encontrará una nueva directiva denominada **Directiva de CA creada a partir de la configuración de CAE** con la configuración establecida. Los administradores pueden optar por personalizar esta directiva o crear la suya propia para reemplazarla.

Puede encontrar más información sobre la evaluación continua de acceso como control de sesión en la sección [Personalización de la evaluación continua de acceso](concept-conditional-access-session.md#customize-continuous-access-evaluation).

### <a name="strict-enforcement"></a>Cumplimiento estricto 

Con la configuración de CAE más reciente en Acceso condicional, el cumplimiento estricto es una nueva característica que permite mejorar la seguridad en función de dos factores: la variación de la dirección IP y la funcionalidad del cliente. Esta funcionalidad se puede habilitar al personalizar las opciones CAE para una directiva determinada. Al activar el cumplimiento estricto, CAE revocará el acceso al detectar cualquier instancia de [variación de dirección IP](#ip-address-variation) o de falta de [funcionalidad de cliente](#client-capabilities) de CAE.

> [!NOTE] 
> Solo debe habilitar el cumplimiento estricto después de asegurarse de que todas las aplicaciones cliente admiten CAE y que ha incluido todas las direcciones IP que ven Azure AD y los proveedores de recursos (como Exchange Online y Azure Resource Manager) en la directiva de ubicación bajo Acceso condicional. De lo contrario, podría resultar bloqueado.

## <a name="limitations"></a>Limitaciones

### <a name="group-membership-and-policy-update-effective-time"></a>Tiempo válido de actualización de las directivas y de la pertenencia a grupos

Los cambios realizados por los administradores en las directivas de acceso condicional y la pertenencia a grupos pueden tardar hasta un día en ser efectivos. El retraso procede de la replicación entre Azure AD y los proveedores de recursos, como Exchange Online y SharePoint Online. Se han realizado algunas optimizaciones de las actualizaciones de directivas que reducen el retraso a dos horas. Sin embargo, no se cubren aún todos los escenarios.  

Cuando los cambios de pertenencia a grupos o directivas de acceso condicional se deban aplicar inmediatamente a determinados usuarios, tiene dos opciones. 

- Ejecute el [comando de PowerShell revoke-azureaduserallrefreshtoken](/powershell/module/azuread/revoke-azureaduserallrefreshtoken) para revocar todos los tokens de actualización de un usuario especificado.
- Seleccione "Revocar sesión" en la página del perfil de usuario de Azure Portal para revocar la sesión del usuario y asegurarse de que las directivas actualizadas se aplicarán inmediatamente.

### <a name="ip-address-variation"></a>Variación de la dirección IP

El proveedor de identidades y los proveedores de recursos pueden ver diferentes direcciones IP. Esta falta de coincidencia puede ocurrir debido a:

- Implementaciones de proxy de red en la organización
- Configuraciones IPv4/IPv6 incorrectas entre el proveedor de identidades y el proveedor de recursos
 
Ejemplos:

- El proveedor de identidades ve una dirección IP del cliente.
- El proveedor de recursos ve otra dirección IP del cliente después de pasar por un servidor proxy.
- La dirección IP que ve el proveedor de identidades forma parte de un intervalo IP permitido en la directiva, pero la dirección IP que ve el proveedor de recursos no.

Para evitar bucles infinitos debido a estos escenarios, Azure AD emite un token de CAE de una hora y no aplicará el cambio de ubicación del cliente. En este caso, se mejora la seguridad en comparación con los tokens de una hora tradicionales, ya que se evalúan los [otros eventos](#critical-event-evaluation) además de los eventos de cambio de ubicación del cliente.

### <a name="supported-location-policies"></a>Directivas de ubicación admitidas

CAE solo tiene información sobre las [ubicaciones con nombre basadas en IP](../conditional-access/location-condition.md#ip-address-ranges). CAE no tiene información sobre otras condiciones de ubicación, como las [direcciones IP de confianza de MFA](../authentication/howto-mfa-mfasettings.md#trusted-ips) o las ubicaciones basadas en países. Cuando el usuario procede de una dirección IP de confianza de MFA, de ubicaciones de confianza que incluyen las direcciones IP de confianza de MFA o de la ubicación de país, CAE no se aplica después de que el usuario se mueva a otra ubicación. En esos casos, Azure AD emitirá un token de acceso de una hora sin comprobación de cumplimiento de IP instantánea. 

> [!IMPORTANT]
> Al configurar ubicaciones para la evaluación continua de acceso, use solo la [condición de ubicación de acceso condicional basado en IP](../conditional-access/location-condition.md) y configure todas las direcciones IP, **incluidas las IPv4 e IPv6**, que el proveedor de identidades y el proveedor de recursos podrán ver. No use condiciones de ubicación de país o la característica de direcciones IP de confianza que está disponible en la página de configuración del servicio Multi-Factor Authentication de Azure AD.

### <a name="office-and-web-account-manager-settings"></a>Configuración de Office y del Administrador de cuentas web

| Canal de actualización de Office | DisableADALatopWAMOverride | DisableAADWAM |
| --- | --- | --- |
| Canal semestral para empresas | Si se establece en Enabled o en 1, no se admite CAE. | Si se establece en Enabled o en 1, no se admite CAE. |
| Canal actual <br> or <br> Canal mensual para empresas | Se admite CAE para cualquier configuración. | Se admite CAE para cualquier configuración. |

Para obtener una explicación de los canales de actualización de Office, consulte [Información general sobre los canales de actualización de Aplicaciones de Microsoft 365](/deployoffice/overview-update-channels). La recomendación es que las organizaciones no deshabiliten el Administrador de cuentas web (WAM).

### <a name="coauthoring-in-office-apps"></a>Coautoría en aplicaciones de Office

Cuando varios usuarios colaboran en un documento al mismo tiempo, es posible que CAE no pueda revocar inmediatamente su acceso al documento en función de la revocación del usuario o los eventos de cambio de directiva. En este caso, el usuario pierde el acceso por completo después de: 

- Cerrar el documento
- Cerrar la aplicación de Office
- Después de un período de 10 horas

Para reducir este tiempo, un administrador de SharePoint puede reducir la duración máxima de las sesiones de coautoría para los documentos almacenados en SharePoint Online y OneDrive para la Empresa mediante la [configuración de una directiva de ubicación de red en SharePoint Online](/sharepoint/control-access-based-on-network-location). Una vez que se cambia esta configuración, la vigencia máxima de las sesiones de coautoría se reducirá a 15 minutos, y se puede ajustar más mediante el comando de PowerShell de SharePoint Online "[Set-SPOTenant –IPAddressWACTokenLifetime](/powershell/module/sharepoint-online/set-spotenant)".

### <a name="enable-after-a-user-is-disabled"></a>Habilitar después de deshabilitar un usuario

Si habilita un usuario justo después de deshabilitarlo, hay cierta latencia antes de que la cuenta se reconozca como habilitada en los servicios de Microsoft del flujo descendente.

- SharePoint online y Teams suelen tener un retraso de 15 minutos. 
- Exchange Online suele tener un retraso de 35 a 40 minutos. 

### <a name="push-notifications"></a>Notificaciones de inserción

Una directiva de direcciones IP no se evalúa antes de que se publiquen las notificaciones de inserción. Este escenario existe porque las notificaciones de inserción son de salida y no tienen una dirección IP asociada con la que evaluar. Si un usuario hace clic en esa notificación de inserción, por ejemplo, un correo electrónico en Outlook, las directivas de direcciones IP de CAE se siguen aplicando antes de que se pueda mostrar el correo electrónico. Las notificaciones de inserción muestran una vista previa del mensaje, que no está protegida por una directiva de direcciones IP. Todas las demás comprobaciones de CAE se realizan antes de que se envíe la notificación de inserción. Si se quita el acceso de un usuario o dispositivo, la aplicación del cumplimiento se produce dentro del período documentado. 

## <a name="faqs"></a>Preguntas más frecuentes

### <a name="how-will-cae-work-with-sign-in-frequency"></a>¿Cómo funcionará CAE con la frecuencia de inicio de sesión?

La frecuencia de inicio de sesión se respetará con o sin CAE.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de las API habilitadas para la evaluación continua de acceso en las aplicaciones](../develop/app-resilience-continuous-access-evaluation.md)
- [Desafíos de notificaciones, solicitudes de notificaciones y funcionalidades de cliente](../develop/claims-challenge.md)
- [Acceso condicional: Sesión](concept-conditional-access-session.md)
- [Supervisión y solución de problemas de evaluación continua de acceso](howto-continuous-access-evaluation-troubleshoot.md)
