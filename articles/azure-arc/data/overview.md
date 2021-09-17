---
title: ¿Qué son los servicios de datos habilitados para Azure Arc?
description: Presentación de los servicios de datos habilitados para Azure Arc
ms.custom: references_regions
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 07/30/2021
ms.topic: overview
ms.openlocfilehash: 5a2bd61c2e59e5933361cc5d64462ba50a12d836
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121737114"
---
# <a name="what-are-azure-arc-enabled-data-services"></a>¿Qué son los servicios de datos habilitados para Azure Arc?

Azure Arc permite ejecutar servicios de datos de Azure en el entorno local, en el perímetro y en nubes públicas con Kubernetes y la infraestructura de su elección.

Actualmente están disponibles los siguientes servicios de datos habilitados para Azure Arc: 

- Instancia administrada de SQL
- Hiperescala de PostgreSQL (versión preliminar)

Para ver una introducción a cómo Servicios de datos habilitados para Azure Arc admiten entornos de trabajo híbridos, consulte este vídeo:

> [!VIDEO https://channel9.msdn.com/Shows//Inside-Azure-for-IT/Choose-the-right-data-solution-for-your-hybrid-environment/player?format=ny]

## <a name="always-current"></a>Siempre actual

Los servicios de datos habilitados para Azure Arc, como la instancia administrada de SQL habilitada para Azure Arc e Hiperescala de PostgreSQL habilitado para Azure Arc, reciben actualizaciones con frecuencia, como revisiones de mantenimiento y nuevas características similares a la experiencia de Azure. Usted recibe las actualizaciones de Microsoft Container Registry y debe definir las cadencias de implementación de acuerdo con sus directivas. De este modo, las bases de datos locales pueden mantenerse al día a la vez que mantiene su control. Dado que los servicios de datos habilitados para Azure Arc son de suscripción, dejarán de producirse situaciones de finalización de soporte técnico para las bases de datos.

## <a name="elastic-scale"></a>Escalado elástico

La elasticidad similar a la nube local le permite escalar y reducir verticalmente las bases de datos de forma dinámica al igual que en Azure, en función de la capacidad disponible de la infraestructura. Esta funcionalidad puede ser útil en los escenarios flexibles con necesidades volátiles, incluidos aquellos escenarios que requieren la ingesta y consulta de datos en tiempo real, a cualquier escala, con un tiempo de respuesta inferior a un segundo. Además, también puede escalar horizontalmente las instancias de base de datos mediante la opción de implementación única de hiperescala de Azure Database for PostgreSQL con la opción Hiperescala. Esta funcionalidad proporciona a las cargas de trabajo de datos un aumento adicional en la optimización de la capacidad, mediante el uso de lecturas y escrituras de escalabilidad *horizontal* únicas.

## <a name="self-service-provisioning"></a>Aprovisionamiento de autoservicio

Azure Arc también proporciona otras ventajas en la nube, como la implementación y automatización rápidas a gran escala. Gracias a la orquestación basada en Kubernetes, puede implementar una base de datos en cuestión segundos mediante las herramientas GUI o CLI.

## <a name="unified-management"></a>Administración unificada

Gracias a herramientas conocidas como Azure Portal, Azure Data Studio y la CLI de Azure (`az`) con la extensión `arcdata`, ahora puede obtener una vista unificada de todos los recursos de datos implementados con Azure Arc. No solo puede ver y administrar una variedad de bases de datos relacionales en todo el entorno y Azure, sino que también puede obtener registros y datos de telemetría de las API de Kubernetes para analizar la capacidad y el estado de la infraestructura subyacente. Además de contar con la supervisión del rendimiento y el análisis de registros localizados, ahora puede aprovechar Azure Monitor para obtener información operativa completa en todo el estado.

[!INCLUDE [use-insider-azure-data-studio](includes/use-insider-azure-data-studio.md)]

## <a name="disconnected-scenario-support"></a>Compatibilidad con los escenarios sin conexión

Muchos de los servicios, como el aprovisionamiento de autoservicio, las copias de seguridad automatizadas y la restauración y la supervisión se pueden ejecutar localmente en la infraestructura con una conexión directa con Azure o sin ninguna. La conexión directa a Azure abre opciones adicionales para la integración con otros servicios de Azure, como Azure Monitor y la capacidad de usar las API de Azure Portal y Azure Resource Manager desde cualquier lugar del mundo para administrar los servicios de datos habilitados para Azure Arc.

## <a name="supported-regions"></a>Regiones admitidas

En la tabla siguiente se describen los escenarios compatibles actualmente con los servicios de datos habilitados para Arc.

|Regiones de Azure  |Modo de conexión directa  |Modo conectado indirecto  |
|---------|---------|---------|
|Este de EE. UU.|Disponible|Disponible
|Este de EE. UU. 2|Disponible|Disponible
|Oeste de EE. UU. 2|Disponible|Disponible
|Centro de EE. UU.|No disponible|Disponible
|Centro-sur de EE. UU.|Disponible|Disponible
|Sur de Reino Unido 2|Disponible|Disponible
|Centro de Francia|Disponible|Disponible
|Oeste de Europa |Disponible |Disponible
|Norte de Europa|Disponible|Disponible
|Japón Oriental|No disponible|Disponible
|Centro de Corea del Sur|No disponible|Disponible
|Este de Asia|No disponible|Disponible
|Sudeste de Asia|Disponible|Disponible
|Este de Australia|Disponible|Disponible

## <a name="next-steps"></a>Pasos siguientes

> **¿Solo desea realizar algunas pruebas?**  
> Empiece a trabajar rápidamente con el [Inicio rápido de Azure Arc](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) en Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) o en una máquina virtual de Azure.
>
>Además, implemente [Jumpstart ArcBox](https://azurearcjumpstart.io/azure_jumpstart_arcbox/), un espacio aislado fácil de implementar para Azure Arc. ArcBox está diseñado para ser completamente independiente dentro de una sola suscripción de Azure y un grupo de recursos, lo que facilitará la obtención de toda la tecnología habilitada para Azure Arc disponible sin más que una suscripción de Azure disponible.

[Instalación de las herramientas de cliente](install-client-tools.md)

[Planeamiento de la implementación Azure Arc servicios de datos](plan-azure-arc-data-services.md) (requiere instalar primero las herramientas de cliente)

[Creación de una instancia administrada de Azure SQL en Azure Arc](create-sql-managed-instance.md) (requiere primero la creación de un controlador de datos de Azure Arc).

[Creación de un grupo de servidores de Hiperescala de Azure Database for PostgreSQL en Azure Arc](create-postgresql-hyperscale-server-group.md) (requiere primero la creación de un controlador de datos de Azure Arc).
