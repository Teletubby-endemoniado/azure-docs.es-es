---
title: Consideraciones de almacenamiento de Azure Functions
description: Conozca los requisitos de almacenamiento de Azure Functions y aprenda a cifrar los datos almacenados.
ms.topic: conceptual
ms.date: 07/27/2020
ms.openlocfilehash: ad9e7979eddac3fc102d9fddae68c230a7418762
ms.sourcegitcommit: 2eac9bd319fb8b3a1080518c73ee337123286fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2021
ms.locfileid: "123259560"
---
# <a name="storage-considerations-for-azure-functions"></a>Consideraciones de almacenamiento de Azure Functions

Azure Functions necesita una cuenta de Azure Storage para crear una instancia de la aplicación de funciones. Esta aplicación de funciones puede utilizar los siguientes servicios de almacenamiento:


|Servicio de Storage  | Uso de Functions  |
|---------|---------|
| [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)     | Mantener el estado de los enlaces y las teclas de función.  <br/>También se utiliza en la [central de tareas de Durable Functions](durable/durable-functions-task-hubs.md). |
| [Archivos de Azure](../storage/files/storage-files-introduction.md)  | Recurso compartido de archivos que se utiliza para almacenar y ejecutar el código de la aplicación de funciones en un [plan de consumo](consumption-plan.md) y un [plan prémium](functions-premium-plan.md). <br/>Azure Files está configurado de forma predeterminada, pero puede crear una [aplicación sin Azure Files](#create-an-app-without-azure-files) en determinadas condiciones. |
| [Azure Queue Storage](../storage/queues/storage-queues-introduction.md)     | Se utiliza en la [central de tareas de Durable Functions](durable/durable-functions-task-hubs.md).   |
| [Azure Table Storage](../storage/tables/table-storage-overview.md)  |  Se utiliza en la [central de tareas de Durable Functions](durable/durable-functions-task-hubs.md).       |

> [!IMPORTANT]
> Cuando se usa el plan de hospedaje de consumo o premium, los archivos de configuración de enlaces y el código de la función se almacenan en Azure Files en la cuenta de almacenamiento principal. Si elimina la cuenta de almacenamiento principal, este contenido se suprimirá y no se podrá recuperar.

## <a name="storage-account-requirements"></a>Requisitos de la cuenta de almacenamiento

Al crear una aplicación de funciones, debe crear o vincular una cuenta de Azure Storage de uso general compatible con Blob, Queue y Table Storage. El motivo es que Azure Functions usa Azure Storage con ciertas operaciones; por ejemplo, para administrar los desencadenadores y ejecutar las funciones de registro. Algunas cuentas de almacenamiento no admiten el uso de colas y tablas; Estas cuentas incluyen cuentas de almacenamiento solo de blobs y Azure Premium Storage.

Para más información sobre los tipos de cuenta de almacenamiento, consulte [Introducción de los servicios Azure Storage](../storage/common/storage-introduction.md#core-storage-services). 

Aunque puede usar una cuenta de almacenamiento existente con la aplicación de funciones, debe asegurarse de que cumple estos requisitos. Las cuentas de almacenamiento creadas como parte del flujo de creación de aplicaciones de funciones en Azure Portal tienen la garantía de que satisfacen estos requisitos de la cuenta de almacenamiento. En el portal, las cuentas no admitidas se filtran al elegir una cuenta de almacenamiento existente durante la creación de una aplicación de funciones. En este flujo, solo se permite elegir cuentas de almacenamiento existentes en la misma región que la aplicación de funciones que está creando. Para más información, consulte [Ubicación de la cuenta de almacenamiento](#storage-account-location).

<!-- JH: Does using a Premium Storage account improve perf? -->

## <a name="storage-account-guidance"></a>Guía de la cuenta de almacenamiento

Cada aplicación de función requiere una cuenta de almacenamiento para funcionar. Si se elimina la cuenta, la aplicación de funciones no se ejecutará. Para más información, consulte este artículo sobre [la solución de problemas relacionados con el almacenamiento](functions-recover-storage-account.md). Estas otras consideraciones que se indican a continuación también se aplican a la cuenta de almacenamiento que se utiliza en las aplicaciones de funciones.

### <a name="storage-account-location"></a>Ubicación de la cuenta de almacenamiento

Para obtener el mejor rendimiento, la aplicación de función debe usar una cuenta de almacenamiento de la misma región, lo que reduce la latencia. En Azure Portal se aplica este procedimiento recomendado. Si, por alguna razón, necesita usar una cuenta de almacenamiento de una región diferente a la de la aplicación de funciones, debe crear la aplicación de función fuera del portal. 

### <a name="storage-account-connection-setting"></a>Configuración de la conexión de la cuenta de almacenamiento

La conexión de la cuenta de almacenamiento se mantiene en [el ajuste de la aplicación AzureWebJobsStorage](./functions-app-settings.md#azurewebjobsstorage). 

Cuando se generan las claves de almacenamiento, debe actualizarse la cadena de conexión de la cuenta de almacenamiento. [Más información sobre la administración de las claves de almacenamiento](../storage/common/storage-account-create.md).

### <a name="shared-storage-accounts"></a>Cuentas de almacenamiento compartidas

Una misma cuenta de almacenamiento puede estar compartida entre varias aplicaciones de funciones sin que se produzcan problemas. Por ejemplo, en Visual Studio, puede desarrollar varias aplicaciones con el Emulador de Azure Storage. En este caso, el emulador actúa como una cuenta de almacenamiento única. La cuenta de almacenamiento que se utiliza en la aplicación de funciones puede usarse también para almacenar los datos de la aplicación. Sin embargo, este enfoque no siempre resulta conveniente en los entornos de producción.

### <a name="optimize-storage-performance"></a>Optimización del rendimiento de almacenamiento

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>Cifrado de datos de almacenamiento

[!INCLUDE [functions-storage-encryption](../../includes/functions-storage-encryption.md)]

### <a name="in-region-data-residency"></a>Residencia de datos en la región

Cuando todos los datos del cliente deban permanecer dentro de una única región, la cuenta de almacenamiento asociada a la aplicación de funciones debe ser una que tenga [redundancia en la región](../storage/common/storage-redundancy.md). También se debe usar una cuenta de almacenamiento con redundancia en la región con [Azure Durable Functions](./durable/durable-functions-perf-and-scale.md#storage-account-selection).

Otros datos de clientes administrados por la plataforma solo se almacenan dentro de la región cuando se hospedan en una instancia de App Service Environment (ASE) con equilibrio de carga interno. Para más información, consulte [Redundancia de zona de ASE](../app-service/environment/zone-redundancy.md#in-region-data-residency).

## <a name="create-an-app-without-azure-files"></a>Creación de una aplicación sin Azure Files

Azure Files está configurado de forma predeterminada para planes Premium y Consumo para no Linux que sirvan como un sistema de archivos compartido en escenarios de gran escala. La plataforma usa el sistema de archivos para algunas características, como el streaming de registros, pero garantiza principalmente la coherencia de la carga útil de la función implementada. Cuando una aplicación se [implementa mediante una dirección URL de paquete externo](./run-functions-from-deployment-package.md), el contenido de la aplicación se sirve desde un sistema de archivos de solo lectura independiente, por lo que Azure Files se puede omitir si se desea. En tales casos, se proporciona un sistema de archivos que se puede escribir, pero no se garantiza que se comparta con todas las instancias de la aplicación de función.

Si Azure Files no se usa, debe tener en cuenta lo siguiente:

* Debe implementar desde una dirección URL del paquete externo.
* La aplicación no puede basarse en un sistema de archivos que se pueda escribir compartido
* La aplicación no puede usar el runtime de Functions v1
* Las experiencias de streaming de registros en clientes como Azure Portal tienen como valor predeterminado los registros del sistema de archivos. En su lugar, deben basarse en registros de Application Insights.

Si lo anterior se tiene en cuenta correctamente, puede crear la aplicación sin Azure Files. Cree la aplicación de función sin especificar la configuración de la aplicación `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` y `WEBSITE_CONTENTSHARE`.

## <a name="mount-file-shares"></a>Montaje de recursos compartidos de archivos

_Esta funcionalidad solo está disponible cuando se ejecuta en Linux._ 

Puede montar recursos compartidos de Azure Files existentes en las aplicaciones de funciones de Linux. Al montar un recurso compartido en la aplicación de funciones de Linux, puede aprovechar los modelos de aprendizaje automático existentes u otros datos en sus funciones. Puede usar el comando [`az webapp config storage-account add`](/cli/azure/webapp/config/storage-account#az_webapp_config_storage_account_add) para montar un recurso compartido existente en la aplicación de funciones de Linux. 

En este comando, `share-name` es el nombre del recurso compartido existente de Azure Files y `custom-id` puede ser cualquier cadena que defina de forma única el recurso compartido cuando se monta en la aplicación de funciones. Además, `mount-path` es la ruta de acceso desde la que se accede al recurso compartido en la aplicación de funciones. `mount-path` debe estar en el formato `/dir-name` y no puede comenzar con `/home`.

Para ver un ejemplo completo, consulte los scripts de [Creación de una aplicación de funciones de Python y montaje de un recurso compartido de Azure Files](scripts/functions-cli-mount-files-storage-linux.md). 

Actualmente, solo se admite `storage-type` de `AzureFiles`. Solo puede montar cinco recursos compartidos en una aplicación de funciones determinada. El montaje de un recurso compartido de archivos puede aumentar el tiempo de arranque en frío en al menos 200-300 ms o incluso más si la cuenta de almacenamiento se encuentra en una región diferente.

El recurso compartido montado está disponible para el código de función en el valor `mount-path` especificado. Por ejemplo, cuando `mount-path` es `/path/to/mount`, puede acceder al directorio de destino mediante las API del sistema de archivos, como en el siguiente ejemplo de Python:

```python
import os
...

files_in_share = os.listdir("/path/to/mount")
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las opciones de hospedaje de Azure Functions.

> [!div class="nextstepaction"]
> [Escalado y hospedaje de Azure Functions](functions-scale.md)
