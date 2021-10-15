---
title: Delegación restringida de Kerberos con Azure Active Directory
description: Guía de arquitectura para lograr la delegación restringida de Kerberos con Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c12ab8421b578a0c1bf60f5d901d5d9223fc685
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129231280"
---
# <a name="windows-authentication---kerberos-constrained-delegation-with-azure-active-directory"></a>Autenticación de Windows: Delegación restringida de Kerberos con Azure Active Directory

La delegación restringida de Kerberos (KCD) proporciona delegación restringida entre recursos y se basa en los nombres principales de servicio. Requiere que los administradores de dominio creen las delegaciones y se limita a un único dominio. La KCD basada en recursos se usa a menudo como forma de proporcionar la autenticación Kerberos para una aplicación web que tiene usuarios en varios dominios dentro de un bosque de Active Directory.

Application Proxy de Azure Active Directory puede proporcionar inicio de sesión único (SSO) y acceso remoto a las aplicaciones basadas en KCD que requieren un vale de Kerberos para el acceso y la delegación restringida de Kerberos.

SSO se habilita en las aplicaciones de KCD locales que usan Autenticación integrada de Windows (IWA) mediante la concesión del permiso a los conectores de Application Proxy para suplantar a los usuarios en Active Directory. El conector de Application Proxy usa este permiso para enviar y recibir tokens en nombre de los usuarios.

## <a name="use-when"></a>Cuándo se utiliza

Existe la necesidad de proporcionar acceso remoto, proteger con autenticación previa y proporcionar SSO a las aplicaciones IWA locales.

![Diagrama de la arquitectura](./media/authentication-patterns/kcd-auth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario**: Accede a la aplicación heredada atendida por Application Proxy.

* **Explorador web**: Componente con el que el usuario interactúa para acceder a la dirección URL externa de la aplicación.

* **Azure AD**: Autentica al usuario. 

* **Servicio Application Proxy**: Actúa como proxy inverso para enviar la solicitud del usuario a la aplicación local. Se basa en Azure AD. Application Proxy también puede aplicar directivas de acceso condicional.

* **Conector de Application Proxy**: Se instala de forma local en servidores Windows para proporcionar conectividad a la aplicación. Devuelve la respuesta a Azure AD. Realiza la negociación de KCD con Active Directory, suplantando al usuario para obtener un token de Kerberos para la aplicación.

* **Active Directory**: Envía el token de Kerberos para la aplicación al conector de Application Proxy.

* **Aplicaciones heredadas**: Aplicaciones que reciben solicitudes de usuario de Application Proxy. Las aplicaciones heredadas devuelven la respuesta al conector de Application Proxy.

## <a name="implement-windows-authentication-kcd-with-azure-ad"></a>Implementación de la autenticación de Windows (KCD) con Azure AD

* [Delegación restringida de Kerberos para el inicio de sesión único para las aplicaciones con Proxy de aplicación](../app-proxy/application-proxy-configure-single-sign-on-with-kcd.md) 

* [Adición de una aplicación local para el acceso remoto mediante Application Proxy en Azure Active Directory](../app-proxy/application-proxy-add-on-premises-application.md)