---
title: 'Uso de Application Proxy para integrar aplicaciones locales con Defender for Cloud Apps: Azure Active Directory'
description: Configure una aplicación local en Azure Active Directory para trabajar con Microsoft Defender for Cloud Apps. Use Control de aplicaciones de acceso condicional de Defender for Cloud Apps para supervisar y controlar sesiones en tiempo real basadas en directivas de acceso condicional. Puede aplicar estas directivas a aplicaciones locales que usen el proxy de aplicación en Azure Active Directory (Azure AD).
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: how-to
ms.date: 04/28/2021
ms.author: kenwith
ms.reviewer: ashishj
ms.openlocfilehash: e392eb7158a25423e0302846b434cf4d576effbe
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132331041"
---
# <a name="configure-real-time-application-access-monitoring-with-microsoft-defender-for-cloud-apps-and-azure-active-directory"></a>Configuración de la supervisión del acceso a las aplicaciones en tiempo real con Microsoft Defender for Cloud Apps y Azure Active Directory
Configure una aplicación local en Azure Active Directory (Azure AD) para usar Microsoft Defender for Cloud Apps para la supervisión en tiempo real. Defender for Cloud Apps usa Control de aplicaciones de acceso condicional para supervisar y controlar sesiones en tiempo real basadas en directivas de acceso condicional. Puede aplicar estas directivas a aplicaciones locales que usen el proxy de aplicación en Azure Active Directory (Azure AD).

Estos son algunos ejemplos de los tipos de directivas que puede crear con Defender for Cloud Apps:

- Bloquear o proteger la descarga de documentos confidenciales en dispositivos no administrados.
- Supervisar cuándo los usuarios de alto riesgo inician sesión en aplicaciones y luego registrar sus acciones desde dentro de la sesión. Con esta información, puede analizar el comportamiento del usuario para determinar cómo se aplican las directivas de sesión.
- Usar certificados de cliente o de cumplimiento de dispositivos para bloquear el acceso a determinadas aplicaciones desde dispositivos no administrados.
- Restringir las sesiones de usuario desde redes no corporativas. Puede dar acceso restringido a los usuarios que acceden a una aplicación desde fuera de la red corporativa. Por ejemplo, este acceso restringido puede impedir que el usuario descargue los documentos confidenciales.

Para obtener más información, vea [Protección de aplicaciones con Control de aplicaciones de acceso condicional de Microsoft Cloud App Security](/cloud-app-security/proxy-intro-aad).

## <a name="requirements"></a>Requisitos

Licencia:

- Licencia de EMS E5, o
- Azure Active Directory Premium P1 y Defender for Cloud Apps independientes

Aplicación local:

- La aplicación en el entorno local debe usar la delegación limitada de Kerberos (KCD)

Configurar proxy de aplicación:

- Configure Azure AD para usar el proxy de aplicación, incluida la preparación del entorno y la instalación del conector del proxy de aplicación. Para obtener un tutorial, vea [Adición de una aplicación local para el acceso remoto mediante el proxy de aplicación en Azure Active Directory](../app-proxy/application-proxy-add-on-premises-application.md). 

## <a name="add-on-premises-application-to-azure-ad"></a>Adición de una aplicación local en Azure AD

Agregue una aplicación local en Azure AD. Para obtener instrucciones, vea [Adición de una aplicación local a Azure AD](../app-proxy/application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). Al agregar la aplicación, asegúrese de establecer las dos opciones siguientes en la hoja **Agregar la aplicación local**:

- **Autenticación previa**: Escriba **Azure Active Directory**.
- **Traducir direcciones URL en el cuerpo de la aplicación**: Elija **Sí**.

Las dos configuraciones son necesarias para que la aplicación funcione con Defender for Cloud Apps.

## <a name="test-the-on-premises-application"></a>Prueba de la aplicación local

Después de agregar la aplicación a Azure AD, siga los pasos de [Probar la aplicación](../app-proxy/application-proxy-add-on-premises-application.md#test-the-application) para agregar un usuario para pruebas y probar el inicio de sesión. 

## <a name="deploy-conditional-access-app-control"></a>Implementación del Control de aplicaciones de acceso condicional

Para configurar la aplicación con el Control de aplicaciones de acceso condicional, siga las instrucciones de [Implementación del control de aplicaciones de acceso condicional para aplicaciones de Azure AD](/cloud-app-security/proxy-deployment-aad).


## <a name="test-conditional-access-app-control"></a>Prueba del Control de aplicaciones de acceso condicional

Para probar la implementación de aplicaciones de Azure AD con Control de acceso condicional de aplicación, siga las instrucciones de [Probar la implementación de aplicaciones de Azure AD](/cloud-app-security/proxy-deployment-aad).
