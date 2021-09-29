---
title: Importación y exportación de datos en Azure Cache for Redis
description: Obtenga información sobre cómo importar datos a Blob Storage y exportar datos desde este servicio con las instancias de Azure Cache for Redis de nivel Prémium.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/31/2017
ms.author: yegu
ms.openlocfilehash: 91991be65e132ebdcdc06fa5e3a7729debec7062
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124836955"
---
# <a name="import-and-export-data-in-azure-cache-for-redis"></a>Importación y exportación de datos en Azure Cache for Redis

Import/Export es una operación de administración de datos de Azure Cache for Redis. Permite importar datos en Azure Cache for Redis o exporta datos de Azure Cache for Redis mediante la importación y exportación de una instantánea de base de datos de Azure Cache for Redis (RDB) desde una memoria caché prémium a un blob en una cuenta de Azure Storage.

- **Exportar**: puede exportar las instantáneas de RDB de Azure Cache for Redis a un blob en páginas.
- **Importar**: puede importar las instantáneas de RDB de Azure Cache for Redis desde de un blob en páginas o un blob en bloques.

Import/Export permite migrar entre diferentes instancias de Azure Cache for Redis o rellenar la memoria caché de datos antes de su uso.

En este artículo se proporciona una guía para importar y exportar datos con Azure Cache for Redis y se responden las preguntas más frecuentes.

> [!IMPORTANT]
> La importación y exportación solo está disponible para las memorias caché del [nivel Premium](cache-overview.md#service-tiers).
>
>

## <a name="import"></a>Importar

Utilice la importación para traer los archivos RDB compatibles de Redis desde cualquier servidor de Redis que se ejecute en cualquier nube o entorno, incluidas las instancias de Redis que se ejecutan en Linux, Windows o cualquier proveedor de nube como, por ejemplo, Amazon Web Services. La importación de datos supone una manera fácil de crear una caché con datos rellenados previamente. Durante el proceso de importación, Azure Cache for Redis carga los archivos RDB desde Azure Storage en la memoria y, luego, inserta las claves en la memoria caché.

> [!NOTE]
> Antes de comenzar la operación de importación, asegúrese de que los archivos de base de datos de Redis (RDB) se cargan en los blobs en páginas o en bloques de Azure Storage, en la misma región y suscripción que la instancia de Azure Cache for Redis. Para obtener información, vea [Introducción al Almacenamiento de blobs de Azure](../storage/blobs/storage-quickstart-blobs-dotnet.md). Si exportó el archivo RDB mediante la característica [Azure Cache for Redis Export](#export), el archivo RDB ya estará almacenado en un blob en páginas y listo para la importación.
>
>

1. Para importar uno o varios blobs de caché exportados, [vaya a la caché](cache-configure.md#configure-azure-cache-for-redis-settings) en Azure Portal y seleccione **Importar datos** en el **menú Recurso**.

    ![Importar datos](./media/cache-how-to-import-export-data/cache-import-data.png)
2. Seleccione **Elegir blobs** y, después, la cuenta de almacenamiento que contiene los datos que desea importar.

    ![Selección de la cuenta de almacenamiento](./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png)
3. Seleccione el contenedor con los datos que desea importar.

    ![Elegir contenedor](./media/cache-how-to-import-export-data/cache-import-choose-container.png)
4. Seleccione uno o varios blobs para importar; para ello, elija el área a la izquierda del nombre del blob y, a continuación, **Seleccionar**.

    ![Elegir blobs](./media/cache-how-to-import-export-data/cache-import-choose-blobs.png)
5. Seleccione **Importar** para comenzar el proceso de importación.

   > [!IMPORTANT]
   > Los clientes de la memoria caché no pueden tener acceso a esta durante el proceso de importación, y los datos existentes en la memoria caché se eliminan.
   >
   >

    ![Importar](./media/cache-how-to-import-export-data/cache-import-blobs.png)

    Puede supervisar el progreso de la operación de importación siguiendo las notificaciones de Azure Portal o viendo los eventos en el [registro de auditoría](../azure-monitor/essentials/activity-log.md).

    ![Progreso de la importación](./media/cache-how-to-import-export-data/cache-import-data-import-complete.png)

## <a name="export"></a>Exportación

La exportación permite exportar los datos almacenados en Azure Cache for Redis a archivos RDB compatibles. Puede utilizar esta característica para mover datos desde una instancia de Azure Cache for Redis a otra o a otro servidor de Redis. Durante el proceso de exportación, se crea un archivo temporal en la máquina virtual que hospeda la instancia del servidor de Azure Cache for Redis. A continuación, el archivo se carga en la cuenta de almacenamiento designada. Una vez completada la operación de exportación (de manera correcta o incorrecta), se elimina el archivo temporal.

1. Para exportar el contenido actual de la memoria caché al almacenamiento, [vaya a la caché](cache-configure.md#configure-azure-cache-for-redis-settings) en Azure Portal y seleccione **Exportar datos** en el **menú Recurso**.

    ![En el panel de navegación de contoso5premium, la opción Exportar datos de la lista Administración aparece resaltada.](./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png)
2. Seleccione **Elegir contenedor de almacenamiento** y seleccione la cuenta de almacenamiento que desee. La cuenta de almacenamiento debe encontrarse en la misma región y suscripción que la memoria caché.

   > [!IMPORTANT]
   >
   > - La exportación funciona con blobs en páginas que son compatibles con las cuentas de almacenamiento del modelo clásico y de Resource Manager.
   > - Azure Cache for Redis no admite la exportación a cuentas de almacenamiento de ADLS Gen2.
   > - aunque en este momento no lo son con las cuentas de Blob Storage.
   >
   > Para más información, vea [Introducción a las cuentas de Azure Storage](../storage/common/storage-account-overview.md).
   >

    ![Cuenta de almacenamiento](./media/cache-how-to-import-export-data/cache-export-data-choose-account.png)
3. Elija el contenedor de blobs que desee y, a continuación, elija **Seleccionar**. Para usar un contenedor nuevo, seleccione **Agregar contenedor** para agregarlo primero y, después, selecciónelo en la lista.

    ![En Contenedores de contoso55, la opción + Contenedor aparece resaltada. Hay un contenedor en la lista, cachesaves, que aparece seleccionado y resaltado. La opción Selección está seleccionada y resaltada.](./media/cache-how-to-import-export-data/cache-export-data-container.png)
4. Escriba un **prefijo de nombre de blob** y seleccione **Exportar**  para iniciar el proceso de exportación. El prefijo del nombre del blob se utiliza como prefijo para los nombres de los archivos generados por esta operación de exportación.

    ![Exportación](./media/cache-how-to-import-export-data/cache-export-data.png)

    Puede supervisar el progreso de la operación de exportación siguiendo las notificaciones de Azure Portal o viendo los eventos en el [registro de auditoría](../azure-monitor/essentials/activity-log.md).

    ![Exportación de datos completa](./media/cache-how-to-import-export-data/cache-export-data-export-complete.png)

    Las memorias caché permanecen disponibles para su uso durante el proceso de exportación.

## <a name="importexport-faq"></a>P+F de Import/Export

Esta sección contiene las preguntas más frecuentes acerca de la característica Importación/Exportación.

- [¿Con qué planes de tarifa se puede usar Importación/Exportación?](#what-pricing-tiers-can-use-importexport)
- [¿Puedo importar datos desde cualquier servidor de Redis?](#can-i-import-data-from-any-redis-server)
- [¿Qué versiones de RDB puedo importar?](#what-rdb-versions-can-i-import)
- [¿La memoria caché estará disponible durante una operación de Import/Export?](#is-my-cache-available-during-an-importexport-operation)
- [¿Puedo utilizar Import/Export con un clúster de Redis?](#can-i-use-importexport-with-redis-cluster)
- [¿Cómo funciona la importación y exportación con una configuración de bases de datos personalizada?](#how-does-importexport-work-with-a-custom-databases-setting)
- [¿En qué se diferencia Importación/Exportación de la persistencia de Redis?](#how-is-importexport-different-from-redis-persistence)
- [¿Puedo automatizar Importación/Exportación mediante PowerShell, la CLI u otros clientes de administración?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
- [He recibido un error de tiempo de espera durante la operación de importación/exportación. ¿Qué significa esto?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
- [Aparece un error al exportar los datos a Azure Blob Storage. ¿Qué ha ocurrido?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>¿Con qué planes de tarifa se puede usar Importación/Exportación?

Importación/Exportación solo está disponible en el plan de tarifa Premium.

### <a name="can-i-import-data-from-any-redis-server"></a>¿Puedo importar datos desde cualquier servidor de Redis?

Sí, puede importar los datos exportados desde instancias de Azure Cache for Redis y puede importar archivos RDB desde cualquier servidor de Redis que se ejecute en cualquier nube o entorno, como Linux, Windows, o proveedores en la nube, como Amazon Web Services. Para importar estos datos, cargue el archivo RDB del servidor Redis que desee en una página o blob en bloque en una cuenta de almacenamiento de Azure. Después, impórtelo en la instancia de Azure Cache for Redis prémium. Por ejemplo, puede exportar los datos de la caché de producción e importarlos en una memoria caché que se usa como parte de un entorno de ensayo para pruebas o migración.

> [!IMPORTANT]
> Para poder importar correctamente los datos exportados desde servidores de Redis distintos a Azure Cache for Redis cuando se usa un blob en páginas, el tamaño de dicho blob debe ajustarse al límite de 512 bytes. Para ver un ejemplo para realizar cualquier relleno de bytes necesario, consulte [Sample page blog upload](https://github.com/JimRoberts-MS/SamplePageBlobUpload) (Carga de blog en páginas de ejemplo).
>
>

### <a name="what-rdb-versions-can-i-import"></a>¿Qué versiones de RDB puedo importar?

Azure Cache for Redis admite la importación RDB hasta la versión 7 de RDB.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>¿La memoria caché estará disponible durante una operación de Import/Export?

- **Exportación** : las memorias caché permanecen disponibles y puede seguir usándolas durante una operación de exportación.
- **Importación** : las memorias caché dejan de estar disponibles cuando se inicia una operación de importación y vuelven a estar disponibles para su uso cuando la operación de importación finaliza.

### <a name="can-i-use-importexport-with-redis-cluster"></a>¿Puedo utilizar Import/Export con un clúster de Redis?

Sí, y puede importar/exportar entre una caché en clústeres y una caché que no esté en clústeres. Puesto que el clúster de Redis [solo admite la base de datos 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), solo se importarán los datos de la base de datos 0. Cuando se importan datos de caché en clúster, las claves se redistribuyen entre las particiones del clúster.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>¿Cómo funciona la importación y exportación con una configuración de bases de datos personalizada?

Algunos planes de tarifa tienen diferentes [límites de bases de datos](cache-configure.md#databases), por lo que hay algunas consideraciones al importar si ha configurado un valor personalizado para el parámetro `databases` al crear la memoria caché.

- Al importar a un plan de tarifa con un límite de `databases` menor que el nivel desde el que exportó:
  - Si usa el número predeterminado de `databases`, que es 16 para todos los planes de tarifa, no se pierden datos.
  - Si utiliza un número personalizado de `databases` que está dentro de los límites para el plan al que va a importar, no se pierden datos.
  - Si los datos exportados contenían datos en una base de datos que supera los límites del nuevo plan, no se importan los datos de esas bases de datos superiores.

### <a name="how-is-importexport-different-from-redis-persistence"></a>¿En qué se diferencia Import/Export de la persistencia de Redis?

Azure Cache for Redis permite mantener datos almacenados en Redis en Azure Storage. Cuando la persistencia se configura, Azure Cache for Redis conserva en el disco una instantánea de los datos de la caché en un formato binario de Redis con una frecuencia de copia de seguridad que se puede configurar. Si se produce un evento catastrófico que deshabilita tanto la caché de réplica como la principal, los datos de la caché se restauran automáticamente con la instantánea más reciente. Para más información, vea [How to configure data persistence for a Premium Azure Cache for Redis](cache-how-to-premium-persistence.md) (Configuración de la persistencia de datos para una instancia de Azure Cache for Redis de nivel Prémium).

Import/Export permite traer datos a Azure Cache for Redis o exportar desde este servicio. No configura la copia de seguridad y restauración mediante la persistencia de Redis.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>¿Puedo automatizar Importación/Exportación mediante PowerShell, la CLI u otros clientes de administración?

Sí, para instrucciones de PowerShell, consulte [Importación de una instancia de Azure Cache for Redis](cache-how-to-manage-redis-cache-powershell.md#to-import-an-azure-cache-for-redis) y [Exportación de una instancia de Azure Cache for Redis](cache-how-to-manage-redis-cache-powershell.md#to-export-an-azure-cache-for-redis).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>He recibido un error de tiempo de espera durante la operación de Import/Export. ¿Qué significa?

A la izquierda, si permanece en la hoja **Importar datos** o **Exportar datos** durante más de 15 minutos antes de iniciar la operación, recibirá un mensaje de error similar al siguiente ejemplo:

```azcopy
The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.
```

Para solucionarlo, inicie la operación de importación o exportación antes de que transcurran 15 minutos.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Aparece un error al exportar los datos a Azure Blob Storage. ¿Qué ha ocurrido?

La exportación solo funciona con archivos RDB almacenados como blobs en páginas. No se admiten otros tipos de blob en este momento, incluidas las cuentas de Blob Storage con niveles de acceso frecuente y esporádico. Para más información, vea [Introducción a las cuentas de Azure Storage](../storage/common/storage-account-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las características de Azure Cache for Redis.

- [Niveles de servicio de Azure Cache for Redis](cache-overview.md#service-tiers)
