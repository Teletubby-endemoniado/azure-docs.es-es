---
title: azcopy | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7153eb1ee29ea63a17e614f49f287945869c7a7e
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128680155"
---
# <a name="azcopy"></a>azcopy

AzCopy es una herramienta de línea de comandos que mueve datos dentro y fuera de Azure Storage. Vea el artículo [Introducción a AzCopy](storage-use-azcopy-v10.md) para descargar AzCopy y obtener información sobre las formas de proporcionar credenciales de autorización para el servicio de almacenamiento.

## <a name="synopsis"></a>Sinopsis

El formato general de los comandos es: `azcopy [command] [arguments] --[flag-name]=[flag-value]`.

Para notificar problemas o para más información acerca de la herramienta, consulte [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy).

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Tutorial: Migración de datos locales a un almacenamiento en la nube con AzCopy](storage-use-azcopy-migrate-on-premises-data.md)
- [Transferencia de datos con AzCopy y Blob Storage](./storage-use-azcopy-v10.md#transfer-data)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)

## <a name="options"></a>Opciones

**--cap-mbps**  (float)  Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.

**--help** Ayuda de azcopy

**--output-type**  (string) Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es `text`. (`text` por defecto)

**--trusted-microsoft-suffixes** (string) Especifica sufijos de dominio adicionales en los que se pueden enviar tokens de inicio de sesión de Azure Active Directory.  El valor predeterminado es " *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net". Los valores que se muestran aquí se agregan al valor predeterminado. Por seguridad, solo debe poner aquí dominios de Microsoft Azure. Separe las entradas con punto y coma.

## <a name="see-also"></a>Consulte también

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [azcopy bench](storage-ref-azcopy-bench.md)
- [azcopy copy](storage-ref-azcopy-copy.md)
- [azcopy doc](storage-ref-azcopy-doc.md)
- [azcopy env](storage-ref-azcopy-env.md)
- [azcopy jobs](storage-ref-azcopy-jobs.md)
- [azcopy jobs clean](storage-ref-azcopy-jobs-clean.md)
- [azcopy jobs list](storage-ref-azcopy-jobs-list.md)
- [azcopy jobs remove](storage-ref-azcopy-jobs-remove.md)
- [azcopy jobs resume](storage-ref-azcopy-jobs-resume.md)
- [azcopy jobs show](storage-ref-azcopy-jobs-show.md)
- [azcopy list](storage-ref-azcopy-list.md)
- [azcopy login](storage-ref-azcopy-login.md)
- [azcopy logout](storage-ref-azcopy-logout.md)
- [azcopy make](storage-ref-azcopy-make.md)
- [azcopy remove](storage-ref-azcopy-remove.md)
- [azcopy sync](storage-ref-azcopy-sync.md)
