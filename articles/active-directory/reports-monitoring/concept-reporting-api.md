---
title: Introducción a la API de generación de informes de Azure AD | Microsoft Docs
description: Introducción a la API de generación informes de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 01/21/2021
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19ce15222d71986667d359fc68ff1f61b1c931b1
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130253625"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Introducción a la API de generación informes de Azure Active Directory

Azure Active Directory proporciona diversos [informes](overview-reports.md) que contienen información útil para las aplicaciones, como sistemas SIEM, auditoría y herramientas de inteligencia empresarial. 

Mediante Microsoft Graph API para informes de Azure AD, puede acceder mediante programación a los datos por medio de un conjunto de API basadas en REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas.

En este artículo se proporciona información general de la API de informes, como las maneras de obtener acceso a ella.

Si tiene problemas, consulte [Obtención de soporte técnico para Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md).

## <a name="prerequisites"></a>Prerrequisitos

Para acceder a la API de informes, con o sin la intervención del usuario, necesita:

1. Asignar roles (Lector de seguridad Reader, Administrador de seguridad y Administrador global)
2. Registro de una aplicación
3. Concesión de permisos
4. Recopilación de configuraciones

Para obtener instrucciones detalladas, consulte [Requisitos previos para acceder a la API de generación de informes de Azure AD](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="api-endpoints"></a>Puntos de conexión de API 

El punto de conexión de Microsoft Graph API para registros de auditoría es `https://graph.microsoft.com/v1.0/auditLogs/directoryAudits` y el punto de conexión de Microsoft Graph API para inicios de sesión es `https://graph.microsoft.com/v1.0/auditLogs/signIns`. Para más información, consulte la [referencia de API de auditoría](/graph/api/resources/directoryaudit) y la [referencia de API de inicio de sesión](/graph/api/resources/signIn).

Puede utilizar la [API de detecciones de riesgos Identity Protection](/graph/api/resources/identityprotection-root) para acceder mediante programación a las detecciones de seguridad con Microsoft Graph. Para obtener más información, consulte [Introducción a Azure Active Directory Identity Protection y Microsoft Graph](../identity-protection/howto-identity-protection-graph-api.md). 
  
También puede usar la [API de registros de aprovisionamiento](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta&preserve-view=true) para acceder mediante programación a los eventos de aprovisionamiento del inquilino. 

## <a name="apis-with-microsoft-graph-explorer"></a>API con el Probador de Microsoft Graph

Puede usar el [Probador de Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer) para comprobar los datos de inicio de sesión de la API de auditoría. Asegúrese de iniciar sesión en su cuenta con ambos botones de inicio de sesión en la interfaz de usuario del Probador de Graph y establezca los permisos **AuditLog.Read.All** y **Directory.Read.All** para el inquilino tal como se muestra a continuación.   

![Explorador de Graph](./media/concept-reporting-api/graph-explorer.png)

![Modificación de la interfaz de usuario de permisos](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Uso de certificados para acceder a la API de generación de informes de Azure AD 

Si planea recuperar datos de informes sin la intervención del usuario, use la API de generación de informes de Azure AD con certificados.

Para instrucciones detalladas, consulte [Obtención de datos mediante la API de generación de informes de Azure Active Directory con certificados](tutorial-access-api-with-certificates.md).

## <a name="next-steps"></a>Pasos siguientes

 * [Requisitos previos para acceder a la API de informes](howto-configure-prerequisites-for-reporting-api.md) 
 * [Obtención de datos mediante la API de informes de Azure AD con certificados](tutorial-access-api-with-certificates.md)
 * [Solución de errores de la API de generación de informes de Azure AD](troubleshoot-graph-api.md)
