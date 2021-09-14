---
title: Optimización para el desarrollo y pruebas en Azure Cosmos DB
description: En este artículo se explican las opciones gratuitas que ofrece Azure Cosmos DB para el desarrollo y pruebas del servicio.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/26/2021
ms.openlocfilehash: 60f586e3f2ead0bde743f75d7810bf2d43c737af
ms.sourcegitcommit: 03f0db2e8d91219cf88852c1e500ae86552d8249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2021
ms.locfileid: "123028626"
---
# <a name="optimize-development-and-testing-cost-in-azure-cosmos-db"></a>Optimización del coste de desarrollo y pruebas en Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

En este artículo se describen las diferentes opciones para usar Azure Cosmos DB para desarrollo y pruebas de forma gratuita, así como técnicas para optimizar el costo en las cuentas de desarrollo o prueba.

## <a name="azure-cosmos-db-emulator-locally-downloadable-version"></a>Emulador de Azure Cosmos DB (versión descargable de forma local)

El [emulador de Azure Cosmos DB](local-emulator.md) es una versión que se puede descargar de forma local y que imita el servicio en la nube de Azure Cosmos DB. Puede escribir y probar el código que usan las API de Azure Cosmos DB incluso si no tiene conexión de red y sin que ello conlleve ningún costo. El emulador de Azure Cosmos DB proporciona un entorno local para propósitos de desarrollo con alta fidelidad al servicio en la nube. Gracias a él, puede desarrollar y probar su aplicación localmente y sin necesidad de crear una suscripción a Azure. Cuando esté listo para implementar su aplicación en la nube, actualice la cadena de conexión para conectarse al punto de conexión de Azure Cosmos DB en la nube; no es necesario que realice otras modificaciones. También puede [configurar una canalización de CI/CD con la tarea de compilación del emulador de Azure Cosmos DB](tutorial-setup-ci-cd.md) en Azure DevOps para ejecutar pruebas. Para comenzar, consulte el artículo referente al [emulador de Azure Cosmos DB](local-emulator.md).

## <a name="azure-cosmos-db-free-tier"></a>Nivel Gratis de Azure Cosmos DB

El nivel Gratis de Azure Cosmos DB facilita la introducción, el desarrollo y la prueba de las aplicaciones, o incluso la ejecución de pequeñas cargas de trabajo de producción de forma gratuita. Cuando el nivel gratis esté habilitado en una cuenta, obtendrá las primeras 1000 RU/s y 25 GB de almacenamiento en la cuenta gratuita.

El nivel Gratis se mantiene indefinidamente durante la vigencia de la cuenta e incluye todas las [ventajas y características](introduction.md#key-benefits) de una cuenta de Azure Cosmos DB normal, incluidos el almacenamiento ilimitado y el rendimiento (RU/s), los Acuerdos de Nivel de Servicio, la alta disponibilidad, la distribución global llave en mano en todas las regiones de Azure y mucho más. Puede crear una cuenta de nivel Gratis mediante Azure Portal, la CLI, PowerShell y una plantilla de Resource Manager. Para más información, consulte el artículo sobre cómo [crear una cuenta de nivel Gratis](free-tier.md) y la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="try-azure-cosmos-db-for-free"></a>Pruebe gratis Azure Cosmos DB

La opción para [probar Azure Cosmos DB gratis](https://azure.microsoft.com/try/cosmosdb/) es una experiencia sin costo alguno que le permite experimentar con Azure Cosmos DB en la nube sin registrarse para obtener una cuenta de Azure ni utilizar una tarjeta de crédito. Las cuentas de prueba de Azure Cosmos DB están disponibles por tiempo limitado (30 días actualmente). Aún así, se pueden renovar en cualquier momento. Las cuentas de prueba de Azure Cosmos DB facilitan la evaluación de Azure Cosmos DB, crear y probar una aplicación o utilizar los artículos de inicio rápido o tutoriales. También puede crear una demostración, realizar pruebas unitarias o incluso crear una cuenta de varias regiones y ejecutar una aplicación en ella sin incurrir en ningún costo. En una cuenta de prueba de Azure Cosmos DB, puede tener una base de datos de rendimiento compartida con un máximo de 25 contenedores y 20 000 RU/s de rendimiento, o un contenedor con hasta 5000 RU/s. Para comenzar, consulte la página [Probar Azure Cosmos DB de forma gratuita](https://azure.microsoft.com/try/cosmosdb/).

## <a name="azure-free-account"></a>Cuenta gratuita de Azure

Azure Cosmos DB se incluye en la [cuenta gratuita de Azure](https://azure.microsoft.com/free), que ofrece créditos y recursos de Azure de forma gratuita durante un período de tiempo determinado. Específicamente para Azure Cosmos DB, esta cuenta gratuita ofrece 25 GB de almacenamiento y 400 RU de rendimiento aprovisionado para todo el año. Esta experiencia permite a cualquier desarrollador probar fácilmente las características de Azure Cosmos DB o integrarlo con otros servicios de Azure sin ningún costo. Junto con la cuenta gratuita de Azure obtendrá un saldo de 200 USD para gastar durante los primeros 30 días. No se le cobrará nada hasta que decida actualizar la suscripción; incluso si comienza a usar los servicios. Para comenzar, visite la página de la [cuenta gratuita de Azure](https://azure.microsoft.com/free).

## <a name="azure-cosmos-db-serverless"></a>Azure Cosmos DB sin servidor

[Azure Cosmos DB sin servidor](serverless.md) le permite usar su cuenta de Azure Cosmos en un modo basado en el consumo, donde solo se cobran las unidades de solicitud que las operaciones de base de datos consumen y el almacenamiento consumido por los datos. No existe un cargo mínimo en el uso de Azure Cosmos DB en modo sin servidor. Dado que elimina el concepto de capacidad aprovisionada, es más adecuado para las actividades de desarrollo o pruebas específicamente cuando la base de datos está inactiva en la mayoría de los casos.

## <a name="use-shared-throughput-databases"></a>Uso de bases de datos de rendimiento compartido

En una [base de datos de rendimiento compartida](set-throughput.md#set-throughput-on-a-database), todos los contenedores dentro de la base de datos comparten el rendimiento aprovisionado (RU/s) de la base de datos. Por ejemplo, si aprovisiona una base de datos con 400 RU/s y tiene cuatro contenedores, los cuatro contenedores compartirán los 400 RU/s. En un entorno de desarrollo o pruebas, donde se puede tener acceso a cada contenedor con menos frecuencia y, por tanto, requerir un valor inferior al mínimo de 400 RU/s, la colocación de los contenedores en una base de datos de rendimiento compartida puede ayudar a optimizar el costo.

Por ejemplo, supongamos que la cuenta de desarrollo o de prueba tiene cuatro contenedores. Si crea cuatro contenedores con un rendimiento dedicado (mínimo de 400 RU/s), el total de RU/s será de 1600 RU/s. Por el contrario, si crea una base de datos de rendimiento compartida (mínimo 400 RU/s) y coloca los contenedores allí, el total de RU/s será de solo 400 RU/s. En general, las bases de datos de rendimiento compartido son excelentes para escenarios en los que no es necesario un rendimiento garantizado en ningún contenedor individual.  Aprenda más sobre las [bases de datos de rendimiento compartido](set-throughput.md#set-throughput-on-a-database).

## <a name="next-steps"></a>Pasos siguientes

Puede comenzar a usar el emulador o las cuentas gratuitas de Azure Cosmos DB con los siguientes artículos:

* Obtenga más información sobre [la factura de Azure Cosmos DB](understand-your-bill.md).
* Obtenga más información sobre [Azure Cosmos DB sin servidor](serverless.md).
* Obtenga más información sobre la [optimización del costo de la capacidad del rendimiento](optimize-cost-throughput.md).
* Obtenga más información sobre la [optimización del costo del almacenamiento](optimize-cost-storage.md).
* Obtenga más información sobre la [optimización del costo de la lectura y la escritura](optimize-cost-reads-writes.md).
* Obtenga más información sobre la [optimización del costo de las consultas](./optimize-cost-reads-writes.md).
* Obtenga más información sobre la [optimización del costo de las cuentas de Azure Cosmos de varias regiones](optimize-cost-regions.md).
* ¿Intenta planear la capacidad de una migración a Azure Cosmos DB? Para ello, puede usar información sobre el clúster de bases de datos existente.
    * Si lo único que sabe es el número de núcleos virtuales y servidores del clúster de bases de datos existente, lea sobre el [cálculo de unidades de solicitud mediante núcleos o CPU virtuales](convert-vcore-to-request-unit.md). 
    * Si conoce las tasas de solicitudes típicas de la carga de trabajo de la base de datos actual, obtenga información sobre el [cálculo de unidades de solicitud mediante la herramienta de planeamiento de capacidad de Azure Cosmos DB](estimate-ru-with-capacity-planner.md).