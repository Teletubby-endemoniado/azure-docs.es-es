---
title: 'Inicio de sesión con clave de seguridad sin contraseña: Azure Active Directory'
description: Habilitación del inicio de sesión con clave de seguridad sin contraseña para Azure AD mediante claves de seguridad FIDO2
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/12/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 033bc34d82e497f7de7b63d8e69a606e9a9501ee
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132486804"
---
# <a name="enable-passwordless-security-key-sign-in"></a>Habilitación del inicio de sesión con clave de seguridad sin contraseña 

En el caso de las empresas que usan las contraseñas hoy en día y tienen un entorno de PC compartido, las claves de seguridad proporcionan una manera perfecta para que los trabajadores se autentiquen sin escribir un nombre de usuario o una contraseña. Las claves de seguridad proporcionan productividad mejorada para los trabajadores y tienen una mejor seguridad.

Este documento se centra en la habilitación de la autenticación sin contraseña basada en claves de seguridad. Al final de este artículo, será capaz de iniciar sesión en aplicaciones basadas en Web con su cuenta de Azure AD mediante una llave de seguridad FIDO2.

## <a name="requirements"></a>Requisitos

- [Azure AD Multi-Factor Authentication](howto-mfa-getstarted.md)
- Habilitar el [registro de información de seguridad combinado](concept-registration-mfa-sspr-combined.md)
- [Claves de seguridad FIDO2](concept-authentication-passwordless.md#fido2-security-keys) compatibles
- WebAuthN requiere Windows 10, versión 1903 o posterior**

Para usar claves de seguridad para iniciar sesión en servicios y aplicaciones web, debe tener un explorador que admita el protocolo WebAuthN. Entre ellas se incluyen Microsoft Edge, Chrome, Firefox y Safari.

## <a name="prepare-devices"></a>Preparación de dispositivos

En el caso de los dispositivos unidos a Azure AD, la mejor experiencia está disponible en Windows 10, versión 1903 o posterior.

Los dispositivos unidos a Azure AD híbrido deben ejecutar la versión 2004, o posterior, de Windows 10.

## <a name="enable-passwordless-authentication-method"></a>Habilitar métodos de autenticación sin contraseña

### <a name="enable-the-combined-registration-experience"></a>Habilitar la experiencia de registro combinado

Las características de registro de los métodos de autenticación sin contraseña se basan en la característica de registro combinado. Siga los pasos del artículo [Habilitar el registro de información de seguridad combinado](howto-registration-mfa-sspr-combined.md) para habilitar el registro combinado.

### <a name="enable-fido2-security-key-method"></a>Habilitar el método de llaves de seguridad FIDO2

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** > **Seguridad** > **Métodos de autenticación** > **Directiva de métodos de autenticación**.
1. En el método **Llave de seguridad FIDO2**, elija las opciones siguientes:
   1. **Habilitar**: Sí o No
   1. **Destino**: Todos los usuarios o Seleccionar usuarios
1. **Guarde** la configuración.

   >[!NOTE]
   >Si ve un error al intentar guardar, la causa podría deberse al número de usuarios o grupos que se agregan. Como solución alternativa, reemplace los usuarios y grupos que está intentando agregar con un único grupo, en la misma operación y, a continuación, haga clic en **Guardar** de nuevo.


### <a name="fido-security-key-optional-settings"></a>Configuración opcional de la clave de seguridad FIDO 

Hay algunas opciones de configuración opcionales para administrar las claves de seguridad por inquilino.  

![Captura de pantalla de las opciones de la clave de seguridad FIDO2](media/howto-authentication-passwordless-security-key/optional-settings.png) 

**General**

- **Permitir configuración de autoservicio** debe permanecer establecido en **Sí**. Si se establece en no, los usuarios no podrán registrar una clave FIDO en el portal MySecurityInfo, aunque la directiva de métodos de autenticación sí lo permita.  
- La opción **Exigir la atestación** establecida en **Sí** requiere que los metadatos de la clave de seguridad FIDO se publiquen y se verifiquen con FIDO Alliance Metadata Service y que también se apruebe el conjunto adicional de pruebas de validación de Microsoft. Para más información, vea [¿Qué es una clave de seguridad compatible con Microsoft?](/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key)

**Directiva de restricción de claves**

- **Exigir las restricciones de claves** debe establecerse en **Sí** exclusivamente si su organización solo quiere permitir o no determinadas claves de seguridad FIDO, que se identifican mediante sus AAGuids. Puede colaborar con el proveedor de claves de seguridad para determinar los AAGuids de sus dispositivos. Si la clave ya está registrada, también se puede encontrar el AAGUID consultando los detalles del método de autenticación de la clave por usuario. 


## <a name="disable-a-key"></a>Deshabilitar una clave 

Para quitar una clave FIDO2 asociada a una cuenta de usuario, elimine la clave del método de autenticación del usuario.

1. Inicie sesión en el portal de Azure AD y busque la cuenta de usuario de la que se va a quitar la clave FIDO.
1. Seleccione **Métodos de autenticación** >, haga clic con el botón derecho en **clave de seguridad FIDO2** y haga clic en **Eliminar**. 

    ![Visualización de los detalles del método de autenticación](media/howto-authentication-passwordless-deployment/security-key-view-details.png)

## <a name="security-key-authenticator-attestation-guid-aaguid"></a>GUID de atestación del autenticador (AAGUID) de claves de seguridad

La especificación FIDO2 requiere que cada proveedor de claves de seguridad proporcione un GUID de atestación del autenticador (AAGUID) durante la atestación. Un AAGUID es un identificador de 128 bits que indica el tipo de clave, como la marca y el modelo. 

>[!NOTE]
>El fabricante debe asegurarse de que el AAGUID es idéntico en todas las claves sustancialmente idénticas realizadas por ese fabricante y diferente (con alta probabilidad) de los AAGUID de todos los demás tipos de claves. Para asegurarse, se debe generar aleatoriamente el AAGUID para un tipo determinado de clave de seguridad. Para obtener más información, consulte la recomendación propuesta por el W3C para la [autenticación web: una API para acceder a credenciales de clave pública: nivel 2 (w3.org)](https://w3c.github.io/webauthn/).

Existen dos formas de obtener el AAGUID. Puede preguntar al proveedor de claves de seguridad o ver los detalles del método de autenticación de la clave por usuario.

![Visualización del AAGUID para la clave de seguridad](media/howto-authentication-passwordless-deployment/security-key-aaguid-details.png)

## <a name="user-registration-and-management-of-fido2-security-keys"></a>Registro de usuario y administración de claves de seguridad FIDO2

1. Vaya a [https://myprofile.microsoft.com](https://myprofile.microsoft.com).
1. Inicie sesión si aún no lo ha hecho.
1. Haga clic en **Información de seguridad**.
   1. Si el usuario ya tiene al menos un método de Azure AD Multi-Factor Authentication registrado, puede registrar de inmediato una clave de seguridad FIDO2.
   1. Si no tiene registrado al menos un método de Azure AD Multi-Factor Authentication, debe agregar uno.
1. Agregue una llave de seguridad FIDO2 al hacer clic en **Agregar método** y seleccionar **Clave de seguridad**.
1. Elija **Dispositivo USB** o **Dispositivo NFC**.
1. Tenga preparada la llave y seleccione **Siguiente**.
1. Aparece un cuadro donde se le pide al usuario que cree o escriba un PIN para la clave de seguridad y luego que realice el gesto necesario para la clave, ya sea biométrico o toque.
1. Se le devuelve al usuario la experiencia de registro combinado y se le pide que proporcione un nombre descriptivo para la llave, de modo que pueda identificar cuál es si tiene varios. Haga clic en **Next**.
1. Haga clic en **Listo** para finalizar el proceso.

## <a name="sign-in-with-passwordless-credential"></a>Iniciar sesión con credenciales sin contraseña

En el ejemplo siguiente, un usuario ya ha aprovisionado su clave de seguridad FIDO2. El usuario puede optar por iniciar sesión en Internet con su llave de seguridad FIDO2 dentro del explorador compatible en Windows 10, versión 1903 o posterior.

![Inicio de sesión de clave de seguridad en Microsoft Edge](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-edge-sign-in.png)

## <a name="troubleshooting-and-feedback"></a>Solución de problemas y comentarios

Si quiere compartir comentarios o detectar problemas con esta característica, compártalos mediante la aplicación Centro de opiniones sobre Windows. Para ello, realice los pasos siguientes:

1. Abra el **Centro de opiniones** y asegúrese de que ha iniciado sesión.
1. Envíe los comentarios bajo la categorización siguiente:
   - Categoría: Seguridad y privacidad
   - Subcategoría: FIDO
1. Para capturar registros, use la opción **Volver a crear mi problema**.

## <a name="known-issues"></a>Problemas conocidos

### <a name="security-key-provisioning"></a>Aprovisionamiento de claves de seguridad

El aprovisionamiento y desaprovisionamiento de administrador de claves de seguridad no está disponible.


### <a name="upn-changes"></a>Cambios de UPN

Si cambia el UPN de un usuario, ya no puede modificar las llaves de seguridad FIDO2 en consecuencia. La solución para un usuario con una clave de seguridad FIDO2 es iniciar sesión en MySecurityInfo, eliminar la clave anterior y agregar una nueva.

## <a name="next-steps"></a>Pasos siguientes

[Inicio de sesión de Windows 10 con llave de seguridad FIDO2](howto-authentication-passwordless-security-key-windows.md)

[Habilitación de la autenticación de FIDO2 en recursos locales](howto-authentication-passwordless-security-key-on-premises.md)

[Más información sobre el registro de dispositivos](../devices/overview.md)

[Más información sobre Azure AD Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)