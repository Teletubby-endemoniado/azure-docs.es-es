---
title: 'Reglas de nomenclatura de entidades de Azure Data Factory: versión 1'
description: Se describen las reglas de nomenclatura para las entidades de Data Factory v1.
author: dcstwh
ms.author: weetok
ms.reviewer: jburchel
ms.service: data-factory
ms.subservice: v1
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 80dbf128460ff09b0ba94a7a5fe7e6276b430c98
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128632393"
---
# <a name="rules-for-naming-azure-data-factory-entities"></a>Reglas para asignar nombres a entidades de Azure Data Factory
> [!NOTE]
> Este artículo se aplica a la versión 1 de Data Factory. Si utiliza la versión actual del servicio Data Factory, consulte las [reglas de nomenclatura de Data Factory](../naming-rules.md).

La tabla siguiente proporciona las reglas de nomenclatura para los artefactos de Factoría de datos.

| Nombre | Exclusividad del nombre | Comprobaciones de validación |
|:--- |:--- |:--- |
| Data Factory |Único en Microsoft Azure. Los nombres no distinguen mayúsculas de minúsculas, es decir, `MyDF` y `mydf` hacen referencia a la misma factoría de datos. |<ul><li>Cada factoría de datos está asociada exactamente a una suscripción de Azure.</li><li>Los nombres de objeto deben comenzar por una letra o un número, y pueden contener solo letras, números y el carácter de guión (-).</li><li>Los caracteres de guión (-) debe estar inmediatamente precedidos y seguidos por una letra o un número. No se permiten guiones consecutivos en los nombres de contenedor.</li><li>El nombre puede tener entre 3 y 63 caracteres.</li></ul> |
| Servicios, tablas o canalizaciones vinculados |Único en una factoría de datos. Los nombres no distinguen mayúsculas de minúsculas. |<ul><li>Número máximo de caracteres incluido en un nombre de tabla: 260.</li><li>Los nombres de objeto deben empezar con una letra, un número o un carácter de subrayado (_).</li><li>No se permiten los caracteres siguientes: ".", "+", "?", "/", "<", ">","*","%","&",":","\\".</li></ul> |
| Grupo de recursos |Único en Microsoft Azure. Los nombres no distinguen mayúsculas de minúsculas. |<ul><li>Número máximo de caracteres: 1000.</li><li>El nombre puede contener letras, dígitos y los siguientes caracteres: "-", "_", "," y "."</li></ul> |

