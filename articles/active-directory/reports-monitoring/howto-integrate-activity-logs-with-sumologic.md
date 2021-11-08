---
title: Transmisión de los registros a SumoLogic mediante Azure Monitor | Microsoft Docs
description: Aprenda a integrar los registros de Azure Active Directory con SumoLogic mediante Azure Monitor.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7220d99c0c70a9e01095cadc293542396c32dc30
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131997331"
---
# <a name="integrate-azure-active-directory-logs-with-sumologic-using-azure-monitor"></a>Integración de los registros de Azure Active Directory con SumoLogic mediante Azure Monitor

En este artículo, aprenderá a integrar los registros de Azure Active Directory (Azure AD) con SumoLogic mediante Azure Monitor. En primer lugar, se enrutan los registros a un centro de eventos de Azure y, a continuación, se integra el centro de eventos con SumoLogic.

## <a name="prerequisites"></a>Prerrequisitos

Para usar esta característica, necesita:
* Un centro de eventos de Azure que contenga registros de actividad de Azure AD. Aprenda cómo [transmitir los registros de actividad a un centro de eventos](./tutorial-azure-monitor-stream-logs-to-event-hub.md). 
* Una suscripción habilitada para el inicio de sesión único en SumoLogic.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Pasos para integrar los registros de Azure AD con SumoLogic 

1. Primero, haga un [streaming de los registros de Azure AD a Azure Event Hubs (versión preliminar)](./tutorial-azure-monitor-stream-logs-to-event-hub.md).
2. Configure la instancia de SumoLogic para [recopilar registros de Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Instale la aplicación SumoLogic de Azure AD](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) para usar los paneles preconfigurados que proporcionan análisis en tiempo real de su entorno.

   ![Panel](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Pasos siguientes

* [Interpretación del esquema de registros de auditoría en Azure Monitor](./overview-reports.md)
* [Interpretación del esquema de registros de inicio de sesión en Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md)
* [Preguntas frecuentes y problemas conocidos](concept-activity-logs-azure-monitor.md#frequently-asked-questions)