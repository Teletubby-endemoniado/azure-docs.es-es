---
title: Latencias de informes de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de la cantidad de tiempo necesaria para que los eventos de informes aparezcan en Azure Portal
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 807f3efbb76269a59b6c5f974e036754be197868
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131995357"
---
# <a name="azure-active-directory-reporting-latencies"></a>Latencias de informes de Azure Active Directory

La latencia es la cantidad de tiempo que tardan en mostrarse los datos de informes de Azure Active Directory (Azure AD) en [Azure Portal](https://portal.azure.com). En este artículo se muestra la latencia esperada de los distintos tipos de informes. 

## <a name="activity-reports"></a>Informes de actividad

Hay dos tipos de informes de actividad:

- [Inicios de sesión](concept-sign-ins.md): proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario
- [Registros de auditoría](concept-audit-logs.md): proporciona información de la actividad del sistema sobre los usuarios y grupos, las aplicaciones administradas y las actividades de directorio.

La tabla siguiente enumera la información de latencia para los informes de actividad. 

> [!NOTE]
> **Latencia (percentil 95)** hace referencia al momento en el que se notificará el 95 % de los registros, y **Latencia (percentil 99)** hace referencia al momento en el que se notificará el 99 % de los registros. 
>

| Informe | Latencia (percentil 95) |Latencia (percentil 99)|
| :-- | --- | --- |
| Registros de auditoría | 2 minutos  | 5 minutos  |
| Inicios de sesión | 2 minutos  | 5 minutos |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>¿En cuánto tiempo podré ver los datos de las actividades después de obtener una licencia prémium?

Si ya tiene datos de actividades con la licencia gratuita, entonces los verá inmediatamente al actualizar. Si no tiene ningún dato, tardarán uno o dos días en mostrarse en los informes después de que actualice a una licencia prémium.

## <a name="security-reports"></a>Informes de seguridad

Hay dos tipos de informes de seguridad:

- [Inicios de sesión peligrosos](../identity-protection/overview-identity-protection.md): un inicio de sesión peligroso es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario. 
- [Usuarios marcados en riesgo](../identity-protection/overview-identity-protection.md): un usuario en peligro es un indicador de una cuenta de usuario que puede haber estado en peligro. 

La tabla siguiente enumera la información de latencia para los informes de seguridad.

| Informe | Mínima | Average | Máxima |
| :-- | --- | --- | --- |
| Usuarios en riesgo          | 5 minutos   | 15 minutos  | 2 horas  |
| Inicios de sesión no seguros         | 5 minutos   | 15 minutos  | 2 horas  |

## <a name="risk-detections"></a>Detecciones de riesgo

Azure AD emplea algoritmos y heurística de aprendizaje automático adaptable para detectar acciones sospechosas que están relacionadas con las cuentas de usuario. Cada acción sospechosa detectada se almacena en un registro llamado **detección de riesgos**.

La tabla siguiente enumera la información de latencia para detecciones de riesgo.

| Informe | Mínima | Average | Máxima |
| :-- | --- | --- | --- |
| Inicios de sesión desde direcciones IP anónimas |5 minutos |15 minutos |2 horas |
| Inicios de sesión desde ubicaciones desconocidas |5 minutos |15 minutos |2 horas |
| Usuarios con credenciales perdidas |2 horas |4 horas |8 horas |
| Viaje imposible a ubicaciones inusuales |5 minutos |1 hora |8 horas  |
| Inicios de sesión desde dispositivos infectados |2 horas |4 horas |8 horas  |
| Inicios de sesión desde direcciones IP con actividad sospechosa |2 horas |4 horas |8 horas  |


## <a name="next-steps"></a>Pasos siguientes

* [Información general sobre informes de Azure AD](overview-reports.md)
* [Acceso mediante programación a los informes de Azure AD](concept-reporting-api.md)
* [Detecciones de riesgo de Azure Active Directory](../identity-protection/overview-identity-protection.md)