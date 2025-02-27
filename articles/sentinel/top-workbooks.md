---
title: Libros de Microsoft Sentinel de uso frecuente | Microsoft Docs
description: Obtenga información sobre los libros usados con más frecuencia para aprovechar los recursos integrados de Microsoft Sentinel más populares.
services: sentinel
cloud: na
documentationcenter: na
author: batamig
manager: rkarlin
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/09/2021
ms.author: bagol
ms.custom: ignite-fall-2021
ms.openlocfilehash: a1cc4d9eca7f55dfbd7de9da9c71959d1bcfad70
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132711503"
---
# <a name="commonly-used-microsoft-sentinel-workbooks"></a>Libros de Microsoft Azure Sentinel de uso frecuente

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

En la tabla siguiente se enumeran los libros integrados de Microsoft Sentinel que se usan con más frecuencia.

Acceda a los libros de Microsoft Sentinel en **Administración de amenazas** > **Libros** a la izquierda y, a continuación, busque el libro que desea usar. Para más información, consulte [Visualización y supervisión de los datos](monitor-your-data.md).

> [!TIP]
> Se recomienda implementar los libros asociados a los datos que va a ingerir. Los libros permiten una supervisión e investigación más amplias en función de los datos recopilados.
>
> Para más información, consulte [Conexión de orígenes de datos](connect-data-sources.md) y [Detección e implementación central de soluciones de Microsoft Sentinel integradas](sentinel-solutions-deploy.md).
>

|Nombre del libro  |Description  |
|---------|---------|
|**Eficacia de los análisis**     |  Proporciona información sobre la eficacia de las reglas de análisis para ayudarle a lograr un mejor rendimiento de SOC. <br><br>Para obtener más información, consulte [Kit de herramientas para los SOC controlados por datos](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).|
|**Azure Activity** (Actividad de Azure)     |     Proporciona conclusiones ampliadas de la actividad de Azure de su organización mediante el análisis y la correlación de todos los eventos y operaciones de usuario. <br><br>Para más información, consulte [Auditoría con registros de actividad de Azure](audit-sentinel-data.md#auditing-with-azure-activity-logs).    |
|**Registros de auditoría de Azure AD**     |  Usa los registros de auditoría de Azure Active Directory para proporcionar conclusiones sobre los escenarios de Azure AD. <br><br>Para obtener más información, consulte [Inicio rápido: Introducción a Microsoft Sentinel](get-visibility.md).     |
|**Registros de auditoría, actividad e inicio de sesión de Azure AD**     |   Proporciona conclusiones sobre los datos de auditoría, actividad e inicio de sesión de Azure Active Directory con un libro. Muestra la actividad, como inicios de sesión por ubicación, dispositivo, motivo del error, acción del usuario, etc. <br><br> Tanto los administradores de seguridad como de Azure pueden usar este libro.   |
|**Registros de inicio de sesión de Azure AD**     | Usa los registros de inicio de sesión de Azure AD para proporcionar conclusiones sobre los escenarios de Azure AD.        |
|**Cybersecurity Maturity Model Certification (CMMC)**     |   Proporciona un mecanismo para ver las consultas de registros alineadas con los controles de CMMC en la cartera de Microsoft, incluidas las ofertas de seguridad de Microsoft, Office 365, Teams, Intune, Windows Virtual Desktop, etc. <br><br>Para obtener más información, consulte [Libro de Cybersecurity Maturity Model Certification (CMMC) en versión preliminar pública](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-cybersecurity-maturity-model-certification-cmmc/ba-p/2111184).|
|**Supervisión del estado de la recopilación de datos** / **Supervisión del uso**     |  Proporciona información sobre el estado de ingesta de datos del área de trabajo, como el tamaño de ingesta, la latencia y el número de registros por origen. Permite consultar los monitores y detectar anomalías para ayudarle a determinar el estado de recopilación de datos de las áreas de trabajo. <br><br>Para más información, consulte [Supervisión del estado de los conectores de datos con este libro de Microsoft Sentinel](monitor-data-connector-health.md).    |
|**Analizador de eventos**     |  Permite explorar, auditar y acelerar el análisis de registros de eventos de Windows, incluidos todos los atributos y detalles de los eventos, como la seguridad, la aplicación, el sistema, la instalación, el servicio de directorio y DNS, entre otros.       |
|**Exchange Online**     |Proporciona información sobre Microsoft Exchange online mediante el seguimiento y el análisis de todas las operaciones de Exchange y las actividades de usuario.         |
|**Identidad y acceso**     |   Proporciona información sobre las operaciones de identidad y acceso en el uso de productos de Microsoft, a través de registros de seguridad que incluyen registros de auditoría e inicio de sesión.     |
|**Información general sobre incidentes**     |   Diseñado para ayudar con la evaluación de prioridades y la investigación mediante el suministro de información detallada sobre un incidente, como información general, datos de entidad, tiempo de evaluación de prioridades, tiempo de mitigación y comentarios. <br><br>Para obtener más información, consulte [Kit de herramientas para los SOC controlados por datos](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).      |
|<a name="investigation-insights"></a>**Conclusiones de la investigación**     | Proporciona a los analistas conclusiones sobre incidentes, marcadores y datos de entidad. Las consultas comunes y las visualizaciones detalladas pueden ayudar a los analistas a investigar actividades sospechosas.     |
|**Microsoft Defender for Cloud Apps: registros de detección**     |   Proporciona información detallada sobre las aplicaciones en la nube que se usan en su organización, así como conclusiones sobre las tendencias de uso y datos detallados para usuarios y aplicaciones específicos.  <br><br>Para obtener más información, consulte [Conexión de datos de Microsoft Defender for Cloud Apps](./data-connectors-reference.md#microsoft-defender-for-cloud-apps).|
|**Libro de MITRE ATT&CK**     |   Proporciona detalles sobre la cobertura de MITRE ATT&CK para Microsoft Sentinel.      |
|**Office 365**     | Proporciona conclusiones de Office 365 mediante el seguimiento y el análisis de todas las operaciones y actividades. Explore en profundidad los datos de SharePoint, OneDrive, Teams y Exchange.       |
|**alertas de seguridad**     |  Proporciona un panel de alertas de seguridad para las alertas del entorno de Microsoft Sentinel. <br><br>Para más información, consulte [Creación automática de incidentes a partir de alertas de seguridad de Microsoft](create-incidents-from-alerts.md).      |
|**Eficiencia de las operaciones de seguridad**     |  Diseñado para los administradores del centro de operaciones de seguridad (SOC) para ver las métricas de eficacia general y las medidas relacionadas con el rendimiento de su equipo. <br><br>Para obtener más información, consulte [Mejora en la administración de SOC con métricas de incidentes](manage-soc-with-incident-metrics.md).  |
|**Inteligencia sobre amenazas**     | Proporciona conclusiones sobre los indicadores de amenazas, como el tipo y la gravedad de las amenazas, la actividad de amenazas a lo largo del tiempo y la correlación con otros orígenes de datos, incluidos los firewalls y Office 365.  <br><br>Para más información, consulte [Inteligencia sobre amenazas en Microsoft Sentinel](understand-threat-intelligence.md).      |
|**Confianza cero (TIC3.0)**     |  Proporciona una visualización automatizada de los principios básicos de Confianza cero, que se cruzan con el [marco de conexiones a Internet de confianza](https://www.cisa.gov/trusted-internet-connections).   <br><br>Para obtener más información, consulte el blog de introducción del libro [Confianza cero (TIC 3.0)](https://techcommunity.microsoft.com/t5/public-sector-blog/announcing-the-azure-sentinel-zero-trust-tic3-0-workbook/ba-p/2313761).  |
