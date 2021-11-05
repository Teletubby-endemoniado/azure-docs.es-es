---
title: 'Microsoft Defender para Storage: ventajas y características'
description: Obtenga información sobre las ventajas y características de Microsoft Defender para Storage.
author: memildin
ms.author: memildin
ms.date: 02/04/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 6bc972f1e548ba94d591d3c000a83f40b8c1c507
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131009828"
---
# <a name="introduction-to-microsoft-defender-for-storage"></a>Introducción a Microsoft Defender para Storage

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

**Microsoft Defender para Storage** es una capa de inteligencia de seguridad nativa de Azure que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o vulnerarlas. Utiliza las funcionalidades avanzadas de inteligencia artificial de seguridad y [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) para ofrecer recomendaciones y alertas de seguridad contextuales.

Las alertas de seguridad se desencadenan cuando se producen anomalías en una actividad. Estas alertas aparecen en Microsoft Defender for Cloud y también se envían por correo electrónico a los administradores de las suscripciones, con detalles de actividad sospechosa y recomendaciones sobre cómo investigar y solucionar las amenazas.

## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponibilidad general (GA)|
|Precios:|**Microsoft Defender para Storage** se factura como se muestra en la [página de precios](https://azure.microsoft.com/pricing/details/security-center/).|
|Tipos de almacenamiento protegido:|[Blob Storage](https://azure.microsoft.com/services/storage/blobs/)<br>[Archivos de Azure](../storage/files/storage-files-introduction.md)<br>[Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)|
|Nubes:|:::image type="icon" source="./media/icons/yes-icon.png"::: Nubes comerciales<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure Government<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure China 21Vianet|
|||


## <a name="what-are-the-benefits-of-microsoft-defender-for-storage"></a>¿Cuáles son las ventajas de Microsoft Defender para Storage?

Microsoft Defender para Storage proporciona:

- **Seguridad nativa de Azure**: con la habilitación en un clic, Defender para Storage protege los datos almacenados en Azure Blob Storage, Azure File Storage y Data Lake Storage. Como servicio nativo de Azure, Defender para Storage proporciona seguridad centralizada en todos los recursos de datos administrados por Azure y se integra con otros servicios de seguridad de Azure, como Microsoft Sentinel.
- **Conjunto de detecciones enriquecidas**: con tecnología de inteligencia sobre amenazas de Microsoft, las detecciones de Defender para Storage abarcan las principales amenazas de Storage, como el acceso anónimo, las credenciales en peligro, la ingeniería social, el abuso de privilegios y el contenido malintencionado.
- **Respuesta a escala**: las herramientas de automatización de Defender for Cloud facilitan la prevención y la respuesta a las amenazas identificadas. Más información en [Automatización de respuestas a desencadenadores de Defender for Cloud](workflow-automation.md)

:::image type="content" source="media/defender-for-storage-introduction/defender-for-storage-high-level-overview.png" alt-text="Información general de las características de Microsoft Defender para Storage.":::


## <a name="what-kind-of-alerts-does-microsoft-defender-for-storage-provide"></a>¿Qué tipos de alertas ofrece Microsoft Defender para Storage?

Las alertas de seguridad se desencadenan en los escenarios siguientes:

- **Patrones de acceso sospechosos**: como el acceso correcto desde un nodo de salida de Tor o desde una dirección IP considerada sospechosa por Microsoft Threat Intelligence.
- **Actividades sospechosas**: como la extracción de datos anómala o el cambio inusual de permisos de acceso.
- **Carga de contenido malintencionado**: como archivos de malware potenciales (basados en el análisis de reputación de hash) u hospedaje de contenido de suplantación de identidad (phishing).

Las alertas incluyen detalles acerca del incidente que las desencadenó, así como recomendaciones acerca de cómo investigar y corregir las amenazas. Las alertas se pueden exportar no solo a Microsoft Sentinel, sino también a cualquier otra SIEM de terceros o cualquier otra herramienta externa.

> [!TIP]
> Se recomienda [configurar Microsoft Defender para Storage](../storage/common/azure-defender-storage-configure.md?tabs=azure-security-center) en el nivel de suscripción, pero también se puede [configurar en cuentas de almacenamiento individuales](../storage/common/azure-defender-storage-configure.md?tabs=azure-portal).


## <a name="what-is-hash-reputation-analysis-for-malware"></a>¿Qué es el análisis de reputación de hash para malware?

Para determinar si un archivo cargado es sospechoso, Microsoft Defender para Storage usa el análisis de reputación de hash compatible con la [inteligencia sobre amenazas de Microsoft](https://go.microsoft.com/fwlink/?linkid=2128684). Las herramientas de protección contra amenazas no examinan los archivos cargados, sino que examinan los registros de almacenamiento y comparan los valores hash de los archivos recién cargados con los de virus conocidos, troyanos, spyware y ransomware. 

Cuando se sospecha que un archivo contiene malware, Defender for Cloud muestra una alerta y, opcionalmente, puede enviar un correo electrónico al propietario del almacenamiento para que elimine el archivo sospechoso. Para configurar esta eliminación automática de archivos que el análisis de reputación de hash indica que contienen malware, implemente una [automatización del flujo de trabajo para desencadenar alertas que contengan el "malware potencial cargado en una cuenta de almacenamiento"](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-respond-to-potential-malware-uploaded-to-azure-storage/ba-p/1452005).

> [!NOTE]
> Para habilitar las funcionalidades de protección contra amenazas de Cloud, debe habilitar las características de seguridad en la suscripción que contenga las cargas de trabajo aplicables.
>
> Puede habilitar **Microsoft Defender para Storage** en el nivel de suscripción o de recursos.

## <a name="trigger-a-test-alert-for-microsoft-defender-for-storage"></a>Desencadenamiento de una alerta de prueba para Microsoft Defender para Storage

Para probar las alertas de seguridad de Microsoft Defender para Storage en su entorno, genere la alerta "Access from a Tor exit node to a storage account" (Acceso desde un nodo de salida de Tor a una cuenta de almacenamiento) con los pasos siguientes:

1. Abra una cuenta de almacenamiento con Microsoft Defender para Storage habilitado.
1. En la barra lateral, seleccione "Contenedores" y abra un contenedor existente o cree uno.

    :::image type="content" source="media/defender-for-storage-introduction/opening-storage-container.png" alt-text="Apertura de un contenedor de blobs desde una cuenta de Azure Storage." lightbox="media/defender-for-storage-introduction/opening-storage-container.png":::

1. Cargue un archivo en ese contenedor.

    > [!CAUTION]
    > No cargue un archivo que contenga datos confidenciales.

1. Use el menú contextual del archivo cargado para seleccionar "Generar SAS".

    :::image type="content" source="media/defender-for-storage-introduction/generate-sas.png" alt-text="Opción Generar SAS de un archivo en un contenedor de blobs.":::

1. Deje las opciones predeterminadas y seleccione **Generar URL y token de SAS**.

1. Copie la dirección URL de SAS generada.

1. En la máquina local, abra el explorador Tor.

    > [!TIP]
    > Puede descargar Tor desde el sitio del proyecto Tor [https://www.torproject.org/download/](https://www.torproject.org/download/).

1. En el explorador Tor, vaya a la dirección URL de SAS.

1. Descargue el archivo que cargó en el paso 3.

    Al cabo de dos horas, recibirá la siguiente alerta de seguridad de Defender for Cloud:

    :::image type="content" source="media/defender-for-storage-introduction/tor-access-alert-storage.png" alt-text="Alerta de seguridad respecto al acceso desde un nodo de salida de Tor.":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre Microsoft Defender para Storage.

Para obtener material relacionado, consulte los siguientes artículos: 

- Tanto si Defender for Cloud genera una alerta como si la recibe desde un producto de seguridad diferente, puede exportarla. Para exportar las alertas a Microsoft Sentinel, a cualquier SIEM de terceros o a cualquier otra herramienta externa, siga las instrucciones de [Exportación de alertas a un SIEM](continuous-export.md).
- [Habilitar la protección avanzada de Azure Defender para Storage](../storage/common/azure-defender-storage-configure.md)
- [Lista de alertas de Microsoft Defender para Storage](alerts-reference.md#alerts-azurestorage)
- [Funcionalidades de inteligencia sobre amenazas de Microsoft](https://go.microsoft.com/fwlink/?linkid=2128684)
