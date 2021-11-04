---
title: Inicio de sesión sin contraseña de Azure Active Directory
description: Conozca las opciones para el inicio de sesión sin contraseña en Azure Active Directory con las claves de seguridad FIDO2 o la aplicación Microsoft Authenticator.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/28/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f9e12d8d112bd9c3d9ea44b29803bc475baa9e4
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131500267"
---
# <a name="passwordless-authentication-options-for-azure-active-directory"></a>Opciones de autenticación sin contraseña de Azure Active Directory

Características como la autenticación multifactor (MFA) son una excelente manera de proteger la organización, aunque los usuarios se frustran a menudo con la capa de seguridad adicional de tener que recordar sus contraseñas. Los métodos de autenticación sin contraseña resultan más cómodos, ya que la contraseña se quita y se reemplaza por algo que se tiene más algo que se es o se sabe.

| Authentication  | Algo que se tiene | Algo que se es o se sabe |
| --- | --- | --- |
| Inicio de sesión sin contraseña | Dispositivo, teléfono o clave de seguridad con Windows 10 | PIN o biométrica |

Cada organización tiene diferentes necesidades en cuanto a la autenticación. Microsoft Azure global y Azure Government ofrecen las siguientes tres opciones de autenticación sin contraseña que se integran con Azure Active Directory (Azure AD):

- Windows Hello para empresas
- Aplicación Microsoft Authenticator
- Claves de seguridad FIDO2

![Autenticación: Seguridad frente a comodidad](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello para empresas

Windows Hello para empresas resulta muy conveniente para los trabajadores de la información que tienen su propio equipo con Windows designado. La información biométrica y las credenciales de PIN están vinculadas directamente al equipo del usuario, lo que impide el acceso de cualquier persona que no sea el propietario. Con la integración de la infraestructura de clave pública (PKI) y la compatibilidad integrada con el inicio de sesión único (SSO), Windows Hello para empresas ofrece un método sencillo y práctico de acceder directamente a los recursos corporativos del entorno local y la nube.

![Ejemplo de inicio de sesión de un usuario con Windows Hello para empresas](./media/concept-authentication-passwordless/windows-hellow-sign-in.jpeg)

En los pasos siguientes se muestra cómo funciona el proceso de inicio de sesión con Azure AD:

![Diagrama que describe los pasos necesarios para el inicio de sesión de un usuario con Windows Hello para empresas](./media/concept-authentication-passwordless/windows-hello-flow.png)

1. Un usuario inicia sesión en Windows mediante gestos de PIN o de información biométrica. El gesto desbloquea la clave privada de Windows Hello para empresas y se envía al proveedor de compatibilidad para seguridad de la autenticación en la nube, conocido como el *proveedor de punto de acceso de nube*.
1. El proveedor de CloudAP solicita una clave nonce (un número arbitrario aleatorio que se puede usar una sola vez) de Azure AD.
1. Azure AD devuelve un valor nonce que es válido durante 5 minutos.
1. El proveedor de punto de acceso de nube firma el valor nonce con la clave privada del usuario y devuelve el valor nonce firmado a Azure AD.
1. Azure AD valida el valor nonce firmado con la clave pública del usuario registrada de forma segura en la firma del valor nonce. Después de validar la firma, Azure AD valida el valor nonce firmado devuelto. Tras validar el valor nonce, Azure AD crea un token de actualización principal (PRT) con la clave de sesión que se ha cifrado con la clave de transporte del dispositivo y lo devuelve al proveedor de punto de acceso de nube.
1. El proveedor de CloudAP recibe el PRT cifrado con la clave de sesión. Mediante la clave de transporte privada del dispositivo, el proveedor de punto de acceso de nube descifra la clave de sesión y la protege con el Módulo de plataforma segura (TPM) del dispositivo.
1. El proveedor de acceso de punto de nube devuelve una respuesta de autenticación correcta a Windows. Después, el usuario puede acceder a Windows, así como a las aplicaciones locales y en la nube sin necesidad de autenticarse de nuevo (SSO).

La [guía de planeamiento](/windows/security/identity-protection/hello-for-business/hello-planning-guide) de Windows Hello para empresas se puede usar para ayudarle a tomar decisiones sobre el tipo de implementación y las opciones que es necesario tener en cuenta.

## <a name="microsoft-authenticator-app"></a>Aplicación Microsoft Authenticator

También puede permitir que el teléfono del empleado se convierta en un método de autenticación sin contraseña. Es posible que ya esté usando la aplicación Microsoft Authenticator como una opción de autenticación multifactor cómoda sumada a una contraseña. También puede usar la aplicación Authenticator como una opción sin contraseña.

![Inicio de sesión en Microsoft Edge con la aplicación Microsoft Authenticator](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

La aplicación Authenticator convierte cualquier teléfono Android o iOS en una credencial segura sin contraseña. Los usuarios pueden iniciar sesión en cualquier plataforma o explorador con este proceso: reciben una notificación en su teléfono, comprueban que el número mostrado en la pantalla coincide con el de su teléfono y, a continuación, usan datos biométricos (reconocimiento táctil o facial) o el PIN para confirmarlo. Consulte [Descarga e instalación de la aplicación Microsoft Authenticator](https://support.microsoft.com/account-billing/download-and-install-the-microsoft-authenticator-app-351498fc-850a-45da-b7b6-27e523b8702a) para conocer los detalles de la instalación.

La autenticación sin contraseñas mediante la aplicación Authenticator sigue el mismo patrón básico que Windows Hello para empresas. Es un poco más complicado, ya que el usuario debe identificarse para que Azure AD pueda encontrar la versión de la aplicación Microsoft Authenticator que se está usando:

![Diagrama que describe los pasos necesarios para el inicio de sesión de un usuario con la aplicación Microsoft Authenticator](./media/concept-authentication-passwordless/authenticator-app-flow.png)

1. El usuario escribe su nombre de usuario.
1. Azure AD detecta que el usuario tiene una credencial segura e inicia el flujo de credencial segura.
1. Se envía una notificación a la aplicación mediante Apple Push Notification Service (APNS) en dispositivos iOS o por medio de Firebase Cloud Messaging (FCM) en dispositivos Android.
1. El usuario recibe la notificación push y abre la aplicación.
1. La aplicación llama a Azure AD y recibe un desafío de prueba de presencia y un valor nonce.
1. Para completar el desafío, el usuario escribe su información biométrica o su PIN para desbloquear la clave privada.
1. El valor nonce se firma con la clave privada y se envía a Azure AD.
1. Azure AD realiza la validación de la clave pública-privada y devuelve un token.

Para empezar a trabajar con el inicio de sesión sin contraseña, complete el procedimiento siguiente:

> [!div class="nextstepaction"]
> [Habilitar el inicio de sesión sin contraseña con la aplicación Authenticator](howto-authentication-passwordless-phone.md)

## <a name="fido2-security-keys"></a>Claves de seguridad FIDO2

FIDO (Fast IDentity Online) Alliance ayuda a promover los estándares de autenticación abiertos y a reducir el uso de contraseñas como forma de autenticación. FIDO2 es el estándar más reciente que incorpora el estándar de autenticación web (WebAuthn).

Las claves de seguridad FIDO2 son un método de autenticación sin contraseña basado en estándares que no permite la suplantación de identidad y que puede venir en cualquier factor de forma. Fast Identity Online (FIDO) es un estándar abierto para la autenticación sin contraseña. FIDO permite a los usuarios y a las organizaciones aprovechar el estándar para iniciar sesión en sus recursos sin un nombre de usuario o una contraseña mediante una clave de seguridad externa o una clave de plataforma integrada en un dispositivo.

Los usuarios pueden registrarse y luego seleccionar una llave de seguridad de FIDO2 en la interfaz de inicio de sesión como medio principal de autenticación. Estas llaves de seguridad de FIDO2 suelen ser dispositivos USB, pero también pueden usar Bluetooth o NFC. Con un dispositivo de hardware que controla la autenticación, se aumenta la seguridad de una cuenta, ya que no hay ninguna contraseña que pueda quedar expuesta ni adivinarse.

Las claves de seguridad FIDO2 se pueden usar para iniciar sesión en sus dispositivos Windows 10 unidos a Azure AD o Azure AD híbrido y lograr el inicio de sesión único en sus recursos de nube y locales. Los usuarios también pueden iniciar sesión en exploradores compatibles. Las claves de seguridad FIDO2 son una excelente opción para las empresas que son muy conscientes de la seguridad o tienen escenarios o empleados que no quieren o no pueden usar su teléfono como un segundo factor.

Disponemos de un documento de referencia sobre los [exploradores que admiten la autenticación FIDO2 con Azure AD](fido2-compatibility.md), así como procedimientos recomendados para los desarrolladores que desean [ofrecer compatibilidad con la autenticación FIDO2 en las aplicaciones que desarrollan](../develop/support-fido2-authentication.md).

![Inicio de sesión en Microsoft Edge con una clave de seguridad](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

El proceso siguiente se utiliza cuando un usuario inicia sesión con una clave de seguridad FIDO2:

![Diagrama que describe los pasos necesarios para el inicio de sesión de un usuario con una clave de seguridad FIDO2](./media/concept-authentication-passwordless/fido2-security-key-flow.png)

1. El usuario conecta la clave de seguridad FIDO2 en su equipo.
2. Windows detecta la llave de seguridad FIDO2.
3. Windows envía una solicitud de autenticación.
4. Azure AD devuelve un valor nonce.
5. El usuario realiza su gesto para desbloquear la clave privada almacenada en el enclave seguro de la llave de seguridad FIDO2.
6. La llave de seguridad FIDO2 firma el valor nonce con la clave privada.
7. La solicitud del token de actualización principal (PRT) con el valor nonce firmado se envía a Azure AD.
8. Azure AD comprueba el valor nonce firmado con la clave pública FIDO2.
9. Azure AD devuelve el PRT para permitir el acceso a los recursos locales.

### <a name="fido2-security-key-providers"></a>Proveedores de claves de seguridad FIDO2

Los siguientes proveedores ofrecen claves de seguridad FIDO2 o diferentes factores de forma que permiten iniciar sesión sin contraseña. Animamos a los clientes a evaluar las propiedades de seguridad de estas claves poniéndose en contacto con el proveedor y con FIDO Alliance.

| Proveedor                  |     Biométrica     | USB | NFC | BLE | Certificado FIPS | Contacto                                                                                             |
|---------------------------|:-----------------:|:---:|:---:|:---:|:--------------:|-----------------------------------------------------------------------------------------------------|
| AuthenTrend               | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://authentrend.com/about-us/#pg-35-3                                                           |
| Ensurity                  | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://www.ensurity.com/contact                                                                    |
| Excelsecu                 | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://www.excelsecu.com/productdetail/esecufido2secu.html                                         |
| Feitian                   | ![y]              | ![y]| ![y]| ![y]| ![y]           | https://shop.ftsafe.us/pages/microsoft                                                              |
| Fortinet                  | ![n]              | ![y]| ![n]| ![n]| ![n]           | https://www.fortinet.com/                                                                           |
| GoTrustID Inc.            | ![n]              | ![y]| ![y]| ![y]| ![n]           | https://www.gotrustid.com/idem-key                                                                  |
| HID                       | ![n]              | ![y]| ![y]| ![n]| ![n]           | https://www.hidglobal.com/contact-us                                                                |
| Hypersecu                 | ![n]              | ![y]| ![n]| ![n]| ![n]           | https://www.hypersecu.com/hyperfido                                                                 |
| IDmelon Technologies Inc. | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://www.idmelon.com/#idmelon                                                                    |
| Kensington                | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://www.kensington.com/solutions/product-category/why-biometrics/                               |
| KONA I                    | ![y]              | ![n]| ![y]| ![y]| ![n]           | https://konai.com/business/security/fido                                                            |
| NEOWAVE                   | ![n]              | ![y]| ![y]| ![n]| ![n]           | https://neowave.fr/en/products/fido-range/                                                          |
| Nymi                      | ![y]              | ![n]| ![y]| ![n]| ![n]           | https://www.nymi.com/nymi-band                                                                      | 
| Octatco                   | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://octatco.com/                                                                                |
| OneSpan Inc.              | ![n]              | ![y]| ![n]| ![y]| ![n]           | https://www.onespan.com/products/fido                                                               |
| Thales Group              | ![n]              | ![y]| ![y]| ![n]| ![n]           | https://cpl.thalesgroup.com/access-management/authenticators/fido-devices                           |
| Thetis                    | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://thetis.io/collections/fido2                                                                 |
| Token2 Switzerland        | ![y]              | ![y]| ![y]| ![n]| ![n]           | https://www.token2.swiss/shop/product/token2-t2f2-alu-fido2-u2f-and-totp-security-key               |
| TrustKey Solutions        | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://www.trustkeysolutions.com/security-keys/                                                    |
| VinCSS                    | ![n]              | ![y]| ![n]| ![n]| ![n]           | https://passwordless.vincss.net                                                                     |
| Yubico                    | ![y]              | ![y]| ![y]| ![n]| ![y]           | https://www.yubico.com/solutions/passwordless/                                                      |


<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png

> [!NOTE]
> Si adquiere y planea usar claves de seguridad basadas en NFC, necesita un lector NFC compatible para la clave de seguridad. El lector NFC no es un requisito o limitación de Azure. Consulte al proveedor de la clave de seguridad basada en NFC para obtener una lista de lectores de NFC admitidos.

Si es proveedor y desea obtener el dispositivo en esta lista de dispositivos compatibles, consulte nuestras instrucciones sobre cómo [convertirse en un proveedor de claves de seguridad FIDO2 compatible con Microsoft](concept-fido2-hardware-vendor.md).

Para empezar a usar las claves de seguridad FIDO2, realice el procedimiento siguiente:

> [!div class="nextstepaction"]
> [Habilitar el inicio de sesión con claves de seguridad FIDO2 sin contraseña](howto-authentication-passwordless-security-key.md)

## <a name="supported-scenarios"></a>Escenarios admitidos

Se aplican las siguientes consideraciones:

- Los administradores pueden habilitar métodos de autenticación sin contraseña para su inquilino.

- Los administradores pueden establecer como destino a todos los usuarios o seleccionar usuarios o grupos dentro de su inquilino para cada método.

- Los usuarios pueden registrar y administrar estos métodos de autenticación sin contraseña en el portal de la cuenta.

- Los usuarios pueden iniciar sesión con estos métodos de autenticación sin contraseña:
   - Aplicación Microsoft Authenticator: funciona en los escenarios donde se usa la autenticación de Azure AD, lo que incluye todos los exploradores, durante la configuración de Windows 10, y con aplicaciones móviles integradas en cualquier sistema operativo.
   - Claves de seguridad: funcionan en la pantalla de bloqueo de Windows 10 e Internet en exploradores compatibles como Microsoft Edge (tanto versiones heredadas como la nueva Edge).

- Los usuarios pueden usar credenciales sin contraseña para acceder a los recursos en los inquilinos donde son invitados, pero es posible que aún deban realizar la autenticación multifactor en ese inquilino de recursos. Para más información, consulte [Posibilidad de doble autenticación multifactor](../external-identities/current-limitations.md#possible-double-multi-factor-authentication).  

- Es posible que los usuarios no registren credenciales sin contraseña dentro de un inquilino en el que sean invitados, de la misma manera que no tienen una contraseña administrada en ese inquilino.  


## <a name="choose-a-passwordless-method"></a>Elección de un método sin contraseña

La elección entre estas tres opciones sin contraseña depende de los requisitos de seguridad, plataforma y aplicación de su empresa.

Estos son algunos de los factores que se deben tener en cuenta al elegir la tecnología sin contraseña de Microsoft:

||**Windows Hello para empresas**|**Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator**|**Llaves de seguridad FIDO2**|
|:-|:-|:-|:-|
|**Requisito previo**| Windows 10, versión 1809 o posterior<br>Azure Active Directory| Aplicación Microsoft Authenticator<br>Teléfono (dispositivos iOS y Android que ejecutan Android 6.0 o posterior)|Windows 10, versión 1903 o posterior<br>Azure Active Directory|
|**Modo**|Plataforma|Software|Hardware|
|**Sistemas y dispositivos**|PC con un módulo de plataforma segura (TPM) integrado<br>Reconocimiento de PIN e información biométrica |PIN y reconocimiento biométrico en el teléfono|Dispositivos de seguridad FIDO2 que son compatibles con Microsoft|
|**Experiencia del usuario**|Inicie sesión con un PIN o mediante reconocimiento biométrico (facial, iris o huella digital) con dispositivos Windows.<br>La autenticación de Windows Hello está vinculada al dispositivo; el usuario necesita el dispositivo y un componente de inicio de sesión, como un PIN o un factor biométrico, para acceder a los recursos corporativos.|Inicio de sesión con un teléfono móvil con la huella digital, el reconocimiento facial o del iris, o bien con un PIN.<br>Los usuarios inician sesión en su cuenta profesional o personal desde su PC o teléfono móvil.|Inicio de sesión con el dispositivo de seguridad FIDO2 (información biométrica, PIN y NFC)<br>El usuario puede acceder al dispositivo según los controles de la organización y autenticarse con un PIN, información biométrica mediante dispositivos como llaves de seguridad USB, y por medio de tarjetas inteligentes, llaves o dispositivos ponibles habilitados para NFC.|
|**Escenarios habilitados**| Experiencia sin contraseña con dispositivos Windows.<br>Se aplica a PC de trabajo dedicados con posibilidad de inicio de sesión único en aplicaciones y dispositivos.|Solución en cualquier parte sin contraseña con el teléfono móvil.<br>Se aplica para el acceso a aplicaciones de trabajo o personales en la web desde cualquier dispositivo.|Experiencia sin contraseña para los trabajadores con NFC, información biométrica y PIN.<br>Aplicable a PC compartidos y cuando un teléfono móvil no es una opción viable (por ejemplo, personal del departamento de soporte técnico, quiosco multimedia público o equipo de hospital).|

Use la tabla siguiente para elegir qué método será más adecuado para sus requisitos y usuarios.

|Persona|Escenario|Entorno|Tecnología sin contraseña|
|:-|:-|:-|:-|
|**Administrador**|Acceso seguro a un dispositivo para tareas de administración|Dispositivo Windows 10 asignado|Windows Hello para empresas, llave de seguridad FIDO2 o ambos|
|**Administrador**|Tareas de administración en dispositivos que no son Windows| Dispositivo móvil o que no sean Windows|Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator|
|**Trabajador de la información**|Trabajo de productividad|Dispositivo Windows 10 asignado|Windows Hello para empresas, llave de seguridad FIDO2 o ambos|
|**Trabajador de la información**|Trabajo de productividad| Dispositivo móvil o que no sean Windows|Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator|
|**Trabajador de primera línea**|Quioscos multimedia de una fábrica, planta, comercio o entrada de datos|Dispositivos Windows 10 compartidos|Llaves de seguridad FIDO2|

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar sin contraseñas en Azure AD, siga uno de los artículos de procedimientos:

* [Habilitar el inicio de sesión con clave de seguridad FIDO2 sin contraseña](howto-authentication-passwordless-security-key.md)
* [Habilitación del inicio de sesión sin contraseña mediante el teléfono con la aplicación Authenticator](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Vínculos externos

* [FIDO Alliance](https://fidoalliance.org/)
* [Especificación FIDO2 CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
