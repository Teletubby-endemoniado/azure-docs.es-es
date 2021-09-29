---
title: 'Directivas de protección de aplicaciones con acceso condicional: Azure Active Directory'
description: Aprenda a establecer el uso obligatorio de directivas de protección de aplicaciones para el acceso a aplicaciones en la nube con acceso condicional en Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 07/20/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: spunukol, rosssmi
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcb64886b1d1c8b324863a762b0a302e9521dab1
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128605304"
---
# <a name="how-to-require-app-protection-policy-and-an-approved-client-app-for-cloud-app-access-with-conditional-access"></a>Procedimientos: Uso obligatorio de directivas de protección de aplicaciones y una aplicación aprobada por el cliente para el acceso a aplicaciones en la nube con acceso condicional

Los usuarios utilizan sus dispositivos móviles habitualmente para tareas personales y profesionales. A la vez que se aseguran de que el personal sigue siendo productivo, las organizaciones también quieren evitar la pérdida de datos de aplicaciones potencialmente no seguras. Con el acceso condicional, las organizaciones pueden restringir el acceso a aplicaciones cliente aprobadas (con capacidad para la autenticación moderna) con directivas de protección de aplicaciones de Intune aplicadas.

En este artículo se presentan tres escenarios a fin de configurar directivas de acceso condicional para recursos como Microsoft 365, Exchange Online y SharePoint.

- [Escenario 1: Las aplicaciones de Microsoft 365 requieren aplicaciones aprobadas con directivas de protección de aplicaciones](#scenario-1-microsoft-365-apps-require-approved-apps-with-app-protection-policies)
- [Escenario 2: Las aplicaciones del explorador requieren aplicaciones aprobadas con directivas de protección de aplicaciones](#scenario-2-browser-apps-require-approved-apps-with-app-protection-policies).
- [Escenario 3: Exchange Online y SharePoint requieren una aplicación cliente aprobada y una directiva de protección de aplicaciones](#scenario-3-exchange-online-and-sharepoint-require-an-approved-client-app-and-app-protection-policy)

En el acceso condicional, estas aplicaciones cliente se conocen como directivas protegidas con una directiva de protección de aplicaciones. Puede encontrar más información sobre las directivas de protección de aplicaciones en el artículo [Introducción a las directivas de protección de aplicaciones](/intune/apps/app-protection-policy).

> [!WARNING]
> No todas las aplicaciones se admiten como aplicaciones aprobadas ni admiten directivas de protección de aplicaciones. Para obtener una lista de aplicaciones cliente aptas, consulte [Requisito de directiva de protección de aplicaciones](concept-conditional-access-grant.md#require-app-protection-policy).

> [!NOTE]
> "Requerir uno de los controles seleccionados" en Conceder controles es como una cláusula OR. Esta cláusula se usa dentro de la directiva para que los usuarios puedan utilizar aplicaciones que admitan los controles de concesión **Requerir directiva de protección de aplicaciones** o **Requerir aplicación cliente aprobada**. **Requerir la directiva de protección de aplicaciones** se aplica cuando la aplicación admite ese control de concesión. Para más información sobre las aplicaciones que admiten el control de concesión **Requerir directiva de protección de aplicaciones**, consulte el [requisito de la directiva de protección de aplicaciones](concept-conditional-access-grant.md#require-app-protection-policy).

## <a name="scenario-1-microsoft-365-apps-require-approved-apps-with-app-protection-policies"></a>Escenario 1: Las aplicaciones de Microsoft 365 requieren aplicaciones aprobadas con directivas de protección de aplicaciones

En este escenario, Contoso ha decidido que todo el acceso móvil a los recursos de Microsoft 365 debe usar aplicaciones cliente aprobadas, como Outlook Mobile y OneDrive, protegidas por una directiva de protección de aplicaciones antes de recibir acceso. Todos sus usuarios ya inician sesión con credenciales de Azure AD y tienen licencias asignadas que incluyen Azure AD Premium P1 o P2 y Microsoft Intune.

Las organizaciones deben completar los pasos siguientes para requerir el uso de una aplicación cliente aprobada en dispositivos móviles.

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Microsoft 365**

1. Inicie sesión en **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios** o **Usuarios y grupos** específicos a los que desee aplicar esta directiva. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Office 365**.
1. En **Condiciones**, seleccione **Plataformas de dispositivo**.
   1. Establezca **Configurar** en **Sí**.
   1. Incluya **Android** e **iOS**.
1. En **Condiciones**, seleccione **Aplicaciones cliente**.
   1. Establezca **Configurar** en **Sí**.
   1. Seleccione **Aplicaciones móviles y aplicaciones de escritorio** y anule la selección de todo lo demás.
1. En **Controles de acceso** > **Conceder**, seleccione las opciones siguientes:
   - **Requerir aplicación cliente aprobada**
   - **Requerir la directiva de protección de aplicaciones**
   - **Requerir uno de los controles seleccionados**
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear y habilitar la directiva.

**Paso 2: Configuración de una directiva de acceso condicional de Azure AD para Exchange Online con Active Sync (EAS)**

Para la directiva de acceso condicional descrita en este paso, configure los componentes siguientes:

1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios** o **Usuarios y grupos** específicos a los que desee aplicar esta directiva. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Office 365 Exchange Online**.
1. En **Condiciones**, seleccione **Aplicaciones cliente**:
   1. Establezca **Configurar** en **Sí**.
   1. Seleccione **Clientes de Exchange ActiveSync** y anule la selección de todo lo demás.
1. En **Controles de acceso** > **Conceder**, seleccione **Conceder acceso**, **Requerir directiva de protección de aplicaciones** y **Seleccionar**.
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear y habilitar la directiva.

**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**

Revise el artículo [Creación y asignación de directivas de protección de aplicaciones](/intune/apps/app-protection-policies) para conocer los pasos para crear directivas de protección de aplicaciones para Android e iOS. 

## <a name="scenario-2-browser-apps-require-approved-apps-with-app-protection-policies"></a>Escenario 2: Las aplicaciones del explorador requieren aplicaciones aprobadas con directivas de protección de aplicaciones.

En este escenario, Contoso ha decidido que todo el acceso de exploración web móvil a los recursos de Microsoft 365 debe usar una aplicación cliente aprobaba, como Edge para iOS y Android, protegida por una directiva de protección de aplicaciones antes de recibir acceso. Todos sus usuarios ya inician sesión con credenciales de Azure AD y tienen licencias asignadas que incluyen Azure AD Premium P1 o P2 y Microsoft Intune.

Las organizaciones deben completar los pasos siguientes para requerir el uso de una aplicación cliente aprobada en dispositivos móviles.

**Paso 1: Configuración de una directiva de acceso condicional de Azure AD para Microsoft 365**

1. Inicie sesión en **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios** o **Usuarios y grupos** específicos a los que desee aplicar esta directiva. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Office 365**.
1. En **Condiciones**, seleccione **Plataformas de dispositivo**.
   1. Establezca **Configurar** en **Sí**.
   1. Incluya **Android** e **iOS**.
1. En **Condiciones**, seleccione **Aplicaciones cliente**.
   1. Establezca **Configurar** en **Sí**.
   1. Seleccione **Explorador** y anule la selección de todo lo demás.
1. En **Controles de acceso** > **Conceder**, seleccione las opciones siguientes:
   - **Requerir aplicación cliente aprobada**
   - **Requerir la directiva de protección de aplicaciones**
   - **Requerir uno de los controles seleccionados**
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear y habilitar la directiva.

**Paso 2: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android**

Revise el artículo [Creación y asignación de directivas de protección de aplicaciones](/intune/apps/app-protection-policies) para conocer los pasos para crear directivas de protección de aplicaciones para Android e iOS. 

## <a name="scenario-3-exchange-online-and-sharepoint-require-an-approved-client-app-and-app-protection-policy"></a>Escenario 3: Exchange Online y SharePoint requieren una aplicación cliente aprobada y una directiva de protección de aplicaciones

En este escenario, Contoso ha decidido que los usuarios solo pueden acceder al correo electrónico y a los datos de SharePoint en dispositivos móviles, siempre que usen una aplicación cliente aprobada, como Outlook Mobile, protegida por una directiva de protección de aplicaciones antes de recibir el acceso. Todos sus usuarios ya inician sesión con credenciales de Azure AD y tienen licencias asignadas que incluyen Azure AD Premium P1 o P2 y Microsoft Intune.

Las organizaciones deben completar los tres pasos siguientes para requerir el uso de una aplicación cliente aprobada en dispositivos móviles y clientes de Exchange ActiveSync.

**Paso 1: Directiva para los clientes de autenticación moderna basada en Android e iOS que requieren el uso de una aplicación cliente aprobada y una directiva de protección de aplicaciones al acceder a Exchange Online y SharePoint.**

1. Inicie sesión en **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios** o **Usuarios y grupos** específicos a los que desee aplicar esta directiva. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Office 365 Exchange Online** y **Office 365 SharePoint Online**.
1. En **Condiciones**, seleccione **Plataformas de dispositivo**.
   1. Establezca **Configurar** en **Sí**.
   1. Incluya **Android** e **iOS**.
1. En **Condiciones**, seleccione **Aplicaciones cliente**.
   1. Establezca **Configurar** en **Sí**.
   1. Seleccione **Aplicaciones móviles y aplicaciones de escritorio** y anule la selección de todo lo demás.
1. En **Controles de acceso** > **Conceder**, seleccione las opciones siguientes:
   - **Requerir aplicación cliente aprobada**
   - **Requerir la directiva de protección de aplicaciones**
   - **Requerir uno de los controles seleccionados**
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear y habilitar la directiva.

**Paso 2: Directiva para los clientes de Exchange ActiveSync que requieren el uso de una aplicación cliente aprobada.**

1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios** o **Usuarios y grupos** específicos a los que desee aplicar esta directiva. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Office 365 Exchange Online**.
1. En **Condiciones**, seleccione **Aplicaciones cliente**:
   1. Establezca **Configurar** en **Sí**.
   1. Seleccione **Clientes de Exchange ActiveSync** y anule la selección de todo lo demás.
1. En **Controles de acceso** > **Conceder**, seleccione **Conceder acceso**, **Requerir directiva de protección de aplicaciones** y **Seleccionar**.
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear y habilitar la directiva.

**Paso 3: Configuración de la directiva de protección de aplicaciones de Intune para aplicaciones cliente de iOS y Android.**

Revise el artículo [Creación y asignación de directivas de protección de aplicaciones](/intune/apps/app-protection-policies) para conocer los pasos para crear directivas de protección de aplicaciones para Android e iOS. 

## <a name="next-steps"></a>Pasos siguientes

[¿Qué es el acceso condicional?](overview.md)

[Componentes de acceso condicional](concept-conditional-access-policies.md)

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

