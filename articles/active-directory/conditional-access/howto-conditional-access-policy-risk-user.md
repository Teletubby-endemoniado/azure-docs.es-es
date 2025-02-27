---
title: 'Acceso condicional basado en riesgo de usuario: Azure Active Directory'
description: Crear directivas de acceso condicional mediante el riesgo de usuario de Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 11/05/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: calebb, davidspo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9776a4f0342756e1e0e4ba015e2f281289ba90ac
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132708714"
---
# <a name="conditional-access-user-risk-based-conditional-access"></a>Acceso condicional: Acceso condicional basado en el riesgo del usuario

Microsoft trabaja con investigadores, cuerpos legales, varios equipos de seguridad de Microsoft y otros orígenes de confianza para buscar pares de nombre de usuario y contraseña filtrados. Las organizaciones con licencias de Azure AD Premium P2 pueden crear directivas de acceso condicional que incorporen [las detecciones de riesgos de los usuarios de Azure AD Identity Protection](../identity-protection/concept-identity-protection-risks.md#user-linked-detections). 

Hay dos ubicaciones donde se puede configurar esta directiva: Acceso condicional e Identity Protection. La configuración mediante una directiva de acceso condicional es el método preferido que proporciona más contexto, incluidos datos de diagnóstico mejorados, integración con el modo de solo informe, compatibilidad con Graph API y la posibilidad de utilizar otros atributos de acceso condicional en la directiva.

## <a name="template-deployment"></a>Implementación de plantilla

Las organizaciones pueden optar por implementar esta directiva mediante los pasos descritos a continuación o mediante las [plantillas de acceso condicional (versión preliminar)](concept-conditional-access-policy-common.md#conditional-access-templates-preview). 

## <a name="enable-with-conditional-access-policy"></a>Habilitar la directiva de acceso condicional

1. Inicie sesión en **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Seguridad** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. En **Excluir**, seleccione **Usuarios y grupos** y, luego, elija las cuentas de acceso de emergencia de la organización. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Todas las aplicaciones en la nube**.
1. En **Condiciones** > **Riesgo de usuario**, establezca **Configurar** en **Sí**. En **Configurar los niveles de riesgo de usuario necesarios para que se aplique la directiva**, seleccione **Alto** y después, **Listo**.
1. En **Controles de acceso** > **Conceder**, seleccione **Conceder acceso**, **Requerir cambio de contraseña** y **Seleccionar**.
1. Confirme la configuración y establezca **Habilitar directiva** en **Solo informe**.
1. Seleccione **Crear** para crear la directiva.

Después de confirmar la configuración mediante el [modo de solo informe](howto-conditional-access-insights-reporting.md), un administrador puede mover el botón de alternancia **Habilitar directiva** de **Solo informe** a **Activar**.

## <a name="enable-through-identity-protection"></a>Habilitar a través de Identity Protection

1. Inicie sesión en **Azure Portal**.
1. Seleccione **Todos los servicios** y, después, vaya a **Azure AD Identity Protection**.
1. Seleccione **Directiva de riesgo de usuario**.
1. En **Asignaciones**, seleccione **Usuarios**.
   1. En **Incluir**, seleccione **Todos los usuarios**.
   1. En **Excluir**, elija **Seleccionar usuarios excluidos**, elija las cuentas de acceso de emergencia de la organización y elija **Seleccionar**.
   1. Seleccione **Listo**.
1. En **Condiciones**, seleccione **Riesgo de usuario** y, luego, elija **Alto**.
   1. Seleccione **Seleccionar** y después, **Listo**.
1. En **Controles** > **Acceso**, elija **Permitir acceso** y, luego, seleccione **Requerir cambio de contraseña**.
   1. Elija **Seleccionar**.
1. Establezca **Aplicar directiva** en **Activado**.
1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[Acceso condicional basado en el riesgo de inicio de sesión](howto-conditional-access-policy-risk.md)

[Determinación del impacto mediante el modo de solo informe de acceso condicional](howto-conditional-access-insights-reporting.md)

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)

[¿Qué es Azure Active Directory Identity Protection?](../identity-protection/overview-identity-protection.md)