---
title: Conexión del origen de datos a Data Collector API de Microsoft Sentinel para ingerir datos | Microsoft Docs
description: Obtenga información sobre cómo conectar sistemas externos a Data Collector API de Microsoft Sentinel para ingerir sus datos de registro en registros personalizados del área de trabajo.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 733ce3565970a6f858edfe32386a21d7655345a6
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132721829"
---
# <a name="connect-your-data-source-to-the-microsoft-sentinel-data-collector-api-to-ingest-data"></a>Conexión del origen de datos a Data Collector API de Microsoft Sentinel para ingerir datos

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Las integraciones de API creadas por proveedores externos extraen datos de los orígenes de datos de sus productos y se conectan a [Data Collector API de Azure Monitor](../azure-monitor/logs/data-collector-api.md) para insertar los datos en tablas de registro personalizadas del área de trabajo de Microsoft Sentinel.

En su mayor parte, puede encontrar toda la información que necesita para configurar estos orígenes de datos para conectarse a Microsoft Sentinel en la documentación de cada proveedor.

Compruebe la sección del producto en la página [Referencia de conectores de datos](data-connectors-reference.md) para obtener instrucciones adicionales que puedan aparecer allí y para ver los vínculos a las instrucciones del proveedor.

> [!NOTE]
> Los datos se almacenan en la ubicación geográfica del área de trabajo en la que se ejecute Microsoft Sentinel.

## <a name="prerequisites"></a>Requisitos previos

- Debe tener permisos de lectura y escritura en el área de trabajo de Microsoft Sentinel.

- Debe tener permisos de lectura para las claves compartidas del área de trabajo. [Obtenga más información sobre las claves del área de trabajo](../azure-monitor/agents/agent-windows.md).

## <a name="configure-and-connect-your-data-source"></a>Configuración y conexión del origen de datos

1. En el portal de Microsoft Sentinel, seleccione **Conectores de datos** en el menú de navegación.

1. Seleccione la entrada del producto en la galería de conectores de datos y, luego, seleccione el botón **Abrir página del conector**.

1. Siga los pasos que aparecen en la página del conector o los vínculos a las instrucciones del proveedor que aparecen allí.

1. Cuando se le pida el identificador del área de trabajo y la clave principal, cópielos desde la página del conector de datos y péguelos en la configuración según las instrucciones del proveedor. Observe el ejemplo siguiente.

    :::image type="content" source="media/connect-rest-api-template/workspace-id-primary-key.png" alt-text="Identificador de área de trabajo y clave principal":::

## <a name="find-your-data"></a>Búsqueda de sus datos

Una vez que se establece una conexión correcta, los datos aparecen en **Registros** en la sección **CustomLogs**. Consulte la sección del producto en la página [Referencia de conectores de datos](data-connectors-reference.md) para los nombres de tabla.

Para consultar los datos del producto, use esos nombres de tabla en la consulta.

Los registros pueden tardar hasta 20 minutos en empezar a aparecer en Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a conectar orígenes de datos externos a Data Collector API de Microsoft Sentinel. Para sacar el máximo partido de las capacidades integradas en estos conectores de datos, seleccione la pestaña **Pasos siguientes** de la página del conector de datos. Allí encontrará algunas consultas de ejemplo, libros y plantillas de reglas de análisis predefinidos para que pueda empezar a buscar información útil.

Para obtener más información sobre Microsoft Sentinel, consulte los siguientes artículos:

- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](get-visibility.md).
- Empiece a [detectar amenazas con Microsoft Sentinel](detect-threats-built-in.md).
- [Use libros](monitor-your-data.md) para supervisar los datos.
