---
title: Solución de problemas de detección de servidores en curso, realización de inventario de software y detección de SQL
description: Obtenga ayuda para detectar servidores, realizar inventario de software y detectar SQL y aplicaciones web.
author: Vikram1988
ms.author: vibansa
ms.manager: abhemraj
ms.topic: troubleshooting
ms.date: 07/01/2020
ms.openlocfilehash: ffa77200a3f0341e74a049b7db3196ef1d726e73
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123423882"
---
# <a name="troubleshoot-ongoing-server-discovery-software-inventory-and-sql-and-web-apps-discovery"></a>Solución de problemas de detección de servidores en curso, realización de inventario de software y detección de SQL y aplicaciones web

Este artículo se le ayuda a solucionar problemas relacionados con la detección de servidores en curso, el inventario de software y la detección de bases de datos e instancias de SQL Server.

## <a name="discovered-servers-not-showing-in-the-portal"></a>Los servidores detectados no se muestran en el portal

Este error aparece cuando aún no se ven los servidores en el portal y el estado de detección es **Detección en curso**.
 
### <a name="remediation"></a>Corrección

Si los servidores no aparecen en el portal, espere unos minutos, ya que se tarda aproximadamente 15 minutos en detectar los servidores que se ejecutan en un servidor vCenter. Cada host de Hyper-V agregado al dispositivo tarda 2 minutos en detectar los servidores que se ejecutan en el host y 1 minuto en detectar cada servidor agregado al dispositivo físico.

Si el estado sigue sin cambiar:

- Seleccione **Actualizar** en la pestaña **Servidores** para ver el recuento de los servidores detectados en las herramientas Azure Migrate: Discovery and assessment y Azure Migrate: Server Migration.

Si el paso anterior no funciona y detecta servidores de VMware:

1. Compruebe que la cuenta de vCenter especificada tiene los permisos definidos correctamente y que tiene acceso al menos a un servidor.
1. En VMware, compruebe si se ha concedido acceso en el nivel de carpeta de VM de vCenter a la cuenta de vCenter. Azure Migrate no puede detectar servidores en VMware. [Obtenga más información](set-discovery-scope.md) sobre el ámbito de la detección.

## <a name="server-data-not-updating-in-the-portal"></a>Los datos de los servidores no se actualizan en el portal

Este error se produce si los servidores detectados no aparecen en el portal o si los datos de estos están obsoletos. 

### <a name="remediation"></a>Corrección

Espere unos minutos, ya que los cambios realizados en los datos de configuración de los servidores detectados tardan hasta 30 minutos en mostrarse en el portal, y los cambios en los datos del inventario de software pueden tardar algunas horas en aparecer. Si no hay datos pasado ese tiempo, actualice el portal y siga estos pasos:

1. En **Windows, Linux y SQL Servers** > **Azure Migrate: Discovery and assessment**, seleccione **Información general**.
1. En **Administrar**, seleccione **Dispositivos**.
1. Seleccione **Refresh services** (Actualizar servicios).
Espere a que finalice la operación de actualización. Ahora debería ver la información actualizada.

## <a name="deleted-servers-appear-in-the-portal"></a>Los servidores eliminados aparecen en el portal

Este error se produce cuando los servidores eliminados siguen apareciendo en el portal.

### <a name="remediation"></a>Corrección

Si los datos continúan apareciendo, espere 30 minutos y siga estos pasos:

1. En **Windows, Linux y SQL Servers** > **Azure Migrate: Discovery and assessment**, seleccione **Información general**.
1. En **Administrar**, seleccione **Dispositivos**.
1. Seleccione **Refresh services** (Actualizar servicios).
Espere a que finalice la operación de actualización. Ahora debería ver la información actualizada.

## <a name="you-imported-a-csv-but-see-discovery-is-in-progress"></a>He importado un archivo CSV pero ve "Detección en curso"

Este estado aparece si se produce un error en la carga de CSV debido a un error de validación.

### <a name="remediation"></a>Corrección

Importe de nuevo el archivo CSV. Descargue el informe de errores de la carga anterior y siga la guía de corrección del archivo para corregir los errores. Descargue el informe de errores en la sección **Detalles de importación** de la página **Discover servers** (Detectar servidores).

## <a name="you-dont-see-software-inventory-details-after-you-update-guest-credentials"></a>Tras actualizar las credenciales de invitado no verá los detalles del inventario de software

Este error se produce cuando los detalles del inventario no aparecen, ni siquiera después de actualizar las credenciales de invitado.

### <a name="remediation"></a>Corrección

El descubrimiento de inventario de software se ejecuta una vez cada 24 horas. Este proceso puede tardar unos minutos, en función del número de servidores detectados. Si quiere ver los detalles de inmediato, realice la actualización como se indica a continuación:

1. En **Windows, Linux y SQL Servers** > **Azure Migrate: Discovery and assessment**, seleccione **Información general**.
1. En **Administrar**, seleccione **Dispositivos**.
1. Seleccione **Refresh services** (Actualizar servicios).
Espere a que finalice la operación de actualización. Ahora debería ver la información actualizada.

## <a name="unable-to-export-software-inventory"></a>No se puede exportar el inventario de software

Este error se produce cuando no tiene privilegios de Colaborador.

### <a name="remediation"></a>Corrección

Asegúrese de que el usuario que descarga el inventario del portal tiene privilegios de colaborador en la suscripción.

## <a name="common-software-inventory-errors"></a>Errores comunes del inventario de software

Azure Migrate admite la detección del inventario de software mediante la herramienta Azure Migrate: Discovery and assessment. Actualmente solo se admite para VMware. [Obtenga más información](how-to-discover-applications.md) sobre los requisitos para la detección del inventario de software.

En la siguiente tabla se resume la lista de errores del inventario de software.

> [!Note]
> También se pueden encontrar los mismos errores con el análisis de dependencias sin agente, ya que sigue la misma metodología que el inventario de software para recopilar los datos necesarios.

| **Error** | **Causa** | **Acción** |
|--|--|--|
| **9000**: no se puede detectar el estado de las herramientas de VMware en el servidor. | Es posible que las herramientas de VMware no estén instaladas en el servidor o que la versión instalada esté dañada. | Asegúrese de que las herramientas de VMware posteriores a la versión 10.2.1 estén instaladas y en ejecución en el servidor. |
| **9001**: las herramientas de VMware no están instaladas en el servidor. | Es posible que las herramientas de VMware no estén instaladas en el servidor o que la versión instalada esté dañada. | Asegúrese de que las herramientas de VMware posteriores a la versión 10.2.1 estén instaladas y en ejecución en el servidor. |
| **9002**: las herramientas de VMware no están en ejecución en el servidor. | Es posible que las herramientas de VMware no estén instaladas en el servidor o que la versión instalada esté dañada. | Asegúrese de que las herramientas de VMware posteriores a la versión 10.2.0 estén instaladas y en ejecución en el servidor. |
| **9003**: no se admite el tipo de sistema operativo que se ejecuta en el servidor. | El sistema operativo que se ejecuta en el servidor no es Windows ni Linux. | Solo se admiten los tipos de SO Windows y Linux. Si el servidor utiliza Windows o Linux, compruebe el tipo de sistema operativo especificado en vCenter Server. |
| **9004**: el servidor no está en estado de ejecución. | El servidor está apagado. | Asegúrese de que el servidor esté en ejecución. |
| **9005**: no se admite el tipo de sistema operativo que se ejecuta en el servidor. | El sistema operativo que se ejecuta en el servidor no es Windows ni Linux. | Solo se admiten los tipos de SO Windows y Linux. Actualmente, no se admite el sistema operativo \<FetchedParameter>. |
| **9006**: la dirección URL necesaria para descargar el archivo de metadatos de detección del servidor está vacía. | Este podría ser un problema transitorio porque el agente de detección del dispositivo no funciona según lo previsto. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **9007**: no se encuentra en el servidor el proceso que ejecuta el script para recopilar los metadatos. | Este podría ser un problema transitorio porque el agente de detección del dispositivo no funciona según lo previsto. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **9008**: no se puede recuperar el estado del proceso que se ejecuta en el servidor para recopilar los metadatos. | Este problema podría ser transitorio debido a un error interno. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **9009:** Control de cuentas de usuario (UAC) de Windows impide la ejecución de operaciones de detección en el servidor. | La configuración de Control de cuentas de usuario de Windows restringe la detección desde el servidor de las aplicaciones instaladas. | En el servidor afectado, disminuya el nivel de la configuración de **Control de cuentas de usuario** en el Panel de control. |
| **9010**: el servidor está apagado. | El servidor está apagado. | Asegúrese de que el servidor esté encendido. |
| **9011**: no se encuentra en el servidor el archivo que contiene los metadatos detectados. | Este problema podría ser transitorio debido a un error interno. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **9012**: el archivo que contiene los metadatos detectados en el servidor está vacío. | Este problema podría ser transitorio debido a un error interno. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **9013**: cada vez que se inicia sesión en el servidor, se crea un perfil de usuario temporal nuevo. | Cada vez que se inicia sesión en el servidor, se crea un perfil de usuario temporal nuevo. | Envíe una incidencia de soporte técnico de Microsoft para recibir ayuda para solucionar este problema. |
| **9014**: no se puede recuperar el archivo que contiene los metadatos detectados debido a un error detectado en el host ESXi. Código de error: %ErrorCode; detalles: %ErrorMessage | Se encontró un error en el host ESXi \<HostName>. Código de error: %ErrorCode; detalles: %ErrorMessage | Asegúrese de que el puerto 443 esté abierto en el host ESXi en el que se ejecuta el servidor.<br/><br/> [Obtenga más información](troubleshoot-discovery.md#error-9014-httpgetrequesttoretrievefilefailed) sobre cómo corregir el problema.|
| **9015**: la cuenta de usuario de VCenter Server que se proporcionó para la detección de servidores no tiene habilitados los privilegios de operaciones de invitado. | Los privilegios necesarios para las operaciones de invitado no están habilitados en la cuenta de usuario de VCenter Server. | Asegúrese de que la cuenta de usuario de vCenter Server tiene habilitados los privilegios para **Máquinas virtuales** > **Operaciones de invitado**: para interactuar con el servidor y extraer los datos necesarios. <br/><br/> [Obtenga más información](tutorial-discover-vmware.md#prepare-vmware) sobre cómo configurar la cuenta de vCenter Server con los privilegios necesarios. |
| **9016**: no se pueden detectar los metadatos porque el agente de operaciones de invitado del servidor no está actualizado. | Las herramientas de VMware no están instaladas en el servidor o la versión instalada no está actualizada. | Asegúrese de que las herramientas de VMware estén instaladas, actualizadas y en ejecución en el servidor. La versión de las herramientas de VMware debe ser la versión 10.2.1 o posterior. |
| **9017**: no se encuentra en el servidor el archivo que contiene los metadatos detectados. | Este problema podría ser transitorio debido a un error interno. | Envíe una incidencia de soporte técnico de Microsoft para recibir ayuda para solucionar este problema. |
| **9018**: PowerShell no está instalado en el servidor. | PowerShell no se encuentra en el servidor. | Asegúrese de que PowerShell 2.0 o posterior esté instalado en el servidor. <br/><br/> [Obtenga más información](troubleshoot-discovery.md#error-9018-powershellnotfound) sobre cómo corregir el problema.|
| **9019**: no se pueden detectar los metadatos porque hay errores de operaciones de invitado en el servidor. | No se han podido realizar las operaciones del invitado de VMware en el servidor. El problema se encontró al probar las credenciales siguientes en el servidor: ```<FriendlyNameOfCredentials>.``` | Asegúrese de que las credenciales del servidor proporcionadas en el dispositivo sean válidas y que el nombre de usuario especificado en las credenciales esté en formato de nombre principal de usuario (UPN). (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **9020**: no se puede crear el archivo necesario para contener los metadatos detectados en el servidor. | El rol asociado a las credenciales proporcionadas en el dispositivo o en una directiva de grupo local restringe la creación del archivo en la carpeta necesaria. El problema se encontró al probar las credenciales siguientes en el servidor: ```<FriendlyNameOfCredentials>.``` | 1. Compruebe si las credenciales proporcionadas en el dispositivo tienen permiso de creación de archivos en la carpeta \<folder path/folder name> del servidor. <br/>2. Si las credenciales proporcionadas en el dispositivo no tienen los permisos necesarios, proporcione otro conjunto de credenciales o edite uno existente. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **9021**: no se puede crear el archivo necesario para contener los metadatos detectados en la ruta de acceso correcta del servidor. | Las herramientas de VMware informan de una ruta de acceso de archivo incorrecta para crear el archivo. | Asegúrese de que las herramientas de VMware posteriores a la versión 10.2.0 estén instaladas y en ejecución en el servidor. |
| **9022**: se ha denegado el acceso para ejecutar el cmdlet Get-WmiObject en el servidor. | El rol asociado a las credenciales proporcionadas en el dispositivo o una directiva de grupo local restringe el acceso al objeto WMI. El problema se encontró al probar las credenciales siguientes en el servidor: \<FriendlyNameOfCredentials>. | 1. Compruebe si las credenciales proporcionadas en el dispositivo tienen privilegios de administrador para crear archivos y tienen WMI habilitado. <br/> 2. Si las credenciales del dispositivo no tienen los permisos necesarios, proporcione otro conjunto de credenciales o edite uno existente. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate).<br/><br/> [Obtenga más información](troubleshoot-discovery.md#error-9022-getwmiobjectaccessdenied) sobre cómo corregir el problema.|
| **9023**: no se puede ejecutar PowerShell porque valor de la variable de entorno %SystemRoot% está vacío. | El valor de la variable de entorno %SystemRoot% está vacío para el servidor. | 1. Compruebe si la variable de entorno devuelve un valor vacío mediante la ejecución del comando echo %systemroot% en el servidor afectado. <br/> 2. Si el problema persiste, envíe una incidencia de soporte técnico de Microsoft. |
| **9024**: no se puede realizar la detección porque el valor de la variable de entorno %TEMP% está vacío. | El valor de la variable de entorno %TEMP% está vacío para el servidor. | 1. Compruebe si la variable de entorno devuelve un valor vacío mediante la ejecución del comando echo %temp% en el servidor afectado. <br/> 2. Si el problema persiste, envíe una incidencia de soporte técnico de Microsoft. |
| **9025**: no se puede realizar la detección porque PowerShell está dañado en el servidor. | PowerShell está dañado en el servidor. | Vuelva a instalar PowerShell y compruebe que esté en ejecución en el servidor afectado. |
| **9026**: no se pueden ejecutar operaciones de invitado en el servidor. | El estado actual del servidor no permite que se ejecuten las operaciones de invitado. | 1. Asegúrese de que el servidor afectado esté en ejecución.<br/> 2. Si el problema persiste, envíe una incidencia de soporte técnico de Microsoft. |
| **9027**: no se pueden detectar los metadatos porque el agente de operaciones de invitado no está en ejecución en el servidor. | No es posible establecer contacto con el agente de operaciones de invitado en el servidor. | Asegúrese de que las herramientas de VMware posteriores a la versión 10.2.0 estén instaladas y en ejecución en el servidor. |
| **9028**: no se puede crear el archivo necesario para contener los metadatos detectados debido a almacenamiento insuficiente en el servidor. | No hay espacio de almacenamiento suficiente en el disco del servidor. | Asegúrese de que haya espacio disponible suficiente en el almacenamiento en disco del servidor afectado. |
| **9029**: las credenciales proporcionadas en el dispositivo no tienen permisos de acceso para ejecutar PowerShell. | Las credenciales proporcionadas en el dispositivo no tienen permisos de acceso para ejecutar PowerShell. El problema se encontró al probar las credenciales siguientes en el servidor: \<FriendlyNameOfCredentials>. | 1. Asegúrese de que las credenciales del dispositivo puedan acceder a PowerShell en el servidor.<br/> 2. Si las credenciales del dispositivo no tienen el acceso necesario, proporcione otro conjunto de credenciales o edite uno existente. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **9030**: no se pueden recopilar los metadatos detectados porque el host ESXi en el que se hospeda el servidor está desconectado. | El host ESXi en el que reside el servidor está desconectado. | Asegúrese de que el host ESXi que ejecuta servidor esté conectado. |
| **9031**: no se pueden recopilar los metadatos detectados porque el host ESXi en el que se hospeda el servidor no responde. | El host ESXi en el que reside el servidor tiene un estado no válido. | Asegúrese de que el host ESXi que ejecuta el servidor esté conectado y en ejecución. |
| **9032**: no se puede realizar la detección debido a un error interno. | El problema que se encontró se debe a un error interno. | Siga los pasos de [este sitio web](troubleshoot-discovery.md#error-9032-invalidrequest) para corregir el problema. Si el problema persiste, abra un caso de soporte técnico de Microsoft. |
| **9033**: no se puede realizar la detección porque el nombre de usuario de las credenciales proporcionadas en el dispositivo para el servidor tiene caracteres que no son válidos. | Las credenciales proporcionadas en el dispositivo contienen caracteres no válidos en el nombre de usuario. El problema se encontró al probar las credenciales siguientes en el servidor: \<FriendlyNameOfCredentials>. | Asegúrese de que las credenciales proporcionadas en el dispositivo no tengan caracteres no válidos en el nombre de usuario. Vuelva al administrador de configuración del dispositivo para editar las credenciales. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **9034**: no se puede realizar la detección porque el nombre de usuario de las credenciales proporcionadas en el dispositivo para el servidor no está en formato UPN. | Las credenciales del dispositivo no tienen el nombre de usuario en formato UPN. El problema se encontró al probar las credenciales siguientes en el servidor: \<FriendlyNameOfCredentials>. | Asegúrese de que las credenciales del dispositivo tengan el nombre de usuario en formato UPN. Vuelva al administrador de configuración del dispositivo para editar las credenciales. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **9035**: no se puede realizar la detección porque el modo de lenguaje de PowerShell no está configurado correctamente. | El modo de lenguaje de PowerShell no está establecido en **Lenguaje completo**. | Asegúrese de que el modo de lenguaje de PowerShell esté establecido en **Lenguaje completo**. |
| **9036**: no se puede realizar la detección porque el nombre de usuario de las credenciales proporcionadas en el dispositivo para el servidor no está en formato UPN. | Las credenciales del dispositivo no tienen el nombre de usuario en formato UPN. El problema se encontró al probar las credenciales siguientes en el servidor: \<FriendlyNameOfCredentials>. | Asegúrese de que las credenciales del dispositivo tengan el nombre de usuario en formato UPN. Vuelva al administrador de configuración del dispositivo para editar las credenciales. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **9037**: la recopilación de metadatos está temporalmente en pausa debido al tiempo de respuesta elevado del servidor. | El servidor está tardando demasiado tiempo en responder. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **10000**: no se admite el tipo de sistema operativo que se ejecuta en el servidor. | El sistema operativo que se ejecuta en el servidor no es Windows ni Linux. | Solo se admiten los tipos de SO Windows y Linux. Actualmente, no se admite el sistema operativo \<GuestOSName>. |
| **10001**: no se encuentra en el servidor el script necesario para recopilar los metadatos de detección. | Es posible que el script necesario para realizar la detección se haya eliminado o quitado de la ubicación esperada. | Envíe una incidencia de soporte técnico de Microsoft para recibir ayuda para solucionar este problema. |
| **10002**: se agotó el tiempo de espera de las operaciones de detección en el servidor. | Este podría ser un problema transitorio porque el agente de detección del dispositivo no funciona según lo previsto. | El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si no se soluciona, siga los pasos de [este sitio web](troubleshoot-discovery.md#error-10002-scriptexecutiontimedoutonvm) para corregir el problema. Si el problema persiste, abra un caso de soporte técnico de Microsoft.|
| **10003**: el proceso que ejecuta las operaciones de detección se cerró con un error. | El proceso que ejecuta las operaciones de detección se cerró de manera abrupta debido a un error.| El problema se debe resolver automáticamente en el ciclo siguiente en un plazo de 24 horas. Si el problema persiste, envíe un caso de soporte técnico de Microsoft. |
| **10004**: no se proporcionaron credenciales en el dispositivo para el tipo de sistema operativo del servidor. | No se agregaron en el dispositivo las credenciales para el tipo de sistema operativo del servidor. | 1. Asegúrese de agregar en el dispositivo las credenciales para el tipo de sistema operativo del servidor afectado.<br/> 2. Ahora puede agregar varias credenciales de servidor en el dispositivo. |
| **10005**: las credenciales proporcionadas en el dispositivo para el servidor no son válidas. | Las credenciales proporcionadas en el dispositivo no son válidas. El problema se encontró al probar las credenciales siguientes en el servidor: ```\<FriendlyNameOfCredentials>.``` | 1. Asegúrese de que las credenciales proporcionadas en el dispositivo sean válidas y de que se pueda acceder al servidor con las credenciales.<br/> 2. Ahora puede agregar varias credenciales de servidor en el dispositivo.<br/> 3. Vuelva al administrador de configuración del dispositivo para proporcionar otro conjunto de credenciales o editar uno existente. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). <br/><br/> [Obtenga más información](troubleshoot-discovery.md#error-10005-guestcredentialnotvalid) sobre cómo corregir el problema.|
| **10006**: no se admite el tipo de sistema operativo que se ejecuta en el servidor. | El sistema operativo que se ejecuta en el servidor no es Windows ni Linux. | Solo se admiten los tipos de SO Windows y Linux. Actualmente, no se admite el sistema operativo \<GuestOSName>. |
| **10007**: no se pueden procesar los metadatos detectados desde el servidor. | Se produjo un error al analizar el contenido del archivo que contiene los metadatos detectados. | Envíe una incidencia de soporte técnico de Microsoft para recibir ayuda para solucionar este problema. |
| **10008**: no se puede crear el archivo necesario para contener los metadatos detectados en el servidor. | El rol asociado a las credenciales proporcionadas en el dispositivo o en una directiva de grupo local restringe la creación del archivo en la carpeta necesaria. El problema se encontró al probar las credenciales siguientes en el servidor: ```<FriendlyNameOfCredentials>.``` | 1. Compruebe si las credenciales proporcionadas en el dispositivo tienen permisos de creación de archivos en la carpeta \<folder path/folder name> del servidor.<br/> 2. Si las credenciales proporcionadas en el dispositivo no tienen los permisos necesarios, proporcione otro conjunto de credenciales o edite uno existente. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **10009**: no se pueden escribir los metadatos detectados en el archivo del servidor. | El rol asociado a las credenciales proporcionadas en el dispositivo o una directiva de grupo local restringe la escritura en el archivo en el servidor. El problema se encontró al probar las credenciales siguientes en el servidor: \<FriendlyNameOfCredentials>. | 1. Compruebe si las credenciales proporcionadas en el dispositivo tienen permisos de escritura en el archivo en la carpeta <ruta de acceso a la carpeta/nombre de la carpeta> en el servidor.<br/> 2. Si las credenciales proporcionadas en el dispositivo no tienen los permisos necesarios, proporcione otro conjunto de credenciales o edite uno existente. (Busque en las posibles causas el nombre descriptivo de las credenciales que probó Azure Migrate). |
| **10010**: no se puede realizar la detección porque en el servidor falta el comando %CommandName; necesario para recopilar algunos metadatos. | El paquete que contiene el comando %CommandName; no está instalado en el servidor. | Asegúrese de que el paquete que contiene el comando %CommandName; esté instalado en el servidor. |
| **10011:** las credenciales proporcionadas en el dispositivo se usaron para iniciar y cerrar una sesión interactiva. | El inicio y cierre de una sesión interactiva fuerza la descarga de las claves del Registro en el perfil de la cuenta que se usa. Esta condición hace que las claves no estén disponibles para usarlas en el futuro. | Utilice los métodos de resolución que se documentan en [este sitio web](/sharepoint/troubleshoot/administration/800703fa-illegal-operation-error#resolutionus/sharepoint/troubleshoot/administration/800703fa-illegal-operation-error). |
| **10012**: no se han proporcionado credenciales en el dispositivo para el servidor. | No se han proporcionado credenciales para el servidor o se han proporcionado credenciales de dominio con un nombre de dominio incorrecto en el dispositivo. [Más información](troubleshoot-discovery.md#error-10012-credentialnotprovided) sobre la causa de este error. | 1. Asegúrese de que se proporcionaron credenciales en el dispositivo para el servidor y de que se pueda acceder al servidor con las credenciales. <br/> 2. Ahora puede agregar varias credenciales de servidores en el dispositivo. Vuelva al administrador de configuración del dispositivo para proporcionar las credenciales del servidor.|

## <a name="error-9014-httpgetrequesttoretrievefilefailed"></a>Error 9014: HTTPGetRequestToRetrieveFileFailed

### <a name="cause"></a>Causa
El problema se produce cuando el agente de detección de VMware del dispositivo intenta descargar el archivo de salida que contiene los datos de dependencias del sistema de archivos del servidor mediante el host ESXi en el que se hospeda el servidor.

### <a name="remediation"></a>Corrección
- Puede probar la conectividad TCP con el host ESXi _(nombre proporcionado en el mensaje de error)_ en el puerto 443 (debe estar abierto en hosts ESXi para extraer datos de dependencia) desde el dispositivo. Para ello, abra PowerShell en el servidor del dispositivo y ejecute el comando siguiente:
 
   ````
   Test -NetConnection -ComputeName <Ip address of the ESXi host> -Port 443
   ````

- Si el comando devuelve una conectividad correcta, vaya al **proyecto de Azure Migrate** > **Discovery and assessment** > **Overview** > **Manage** > **Appliances** (Detección y evaluación > Información general > Administrar > Dispositivos), seleccione el nombre del dispositivo y, luego, **Refresh services** (Actualizar servicios).

## <a name="error-9018-powershellnotfound"></a>Error 9018: PowerShellNotFound

### <a name="cause"></a>Causa
El error suele aparecer para servidores que ejecutan Windows Server 2008 o una versión anterior.

### <a name="remediation"></a>Corrección
Instale la versión de PowerShell necesaria (2.0 o posterior) en esta ubicación del servidor: ($SYSTEMROOT)\System32\WindowsPowershell\v1.0\powershell.exe. [Más información](/powershell/scripting/windows-powershell/install/installing-windows-powershell) sobre cómo instalar PowerShell en Windows Server.

Después de instalar la versión de PowerShell necesaria, compruebe si se resolvió el error; para ello, siga los pasos que se indican en [este sitio web](troubleshoot-discovery.md#mitigation-verification-by-using-vmware-powercli).

## <a name="error-9022-getwmiobjectaccessdenied"></a>Error 9022: GetWMIObjectAccessDenied

### <a name="remediation"></a>Corrección
Asegúrese de que la cuenta de usuario proporcionada en el dispositivo tenga acceso al espacio de nombres WMI y a los subespacios de nombres. Para establecer el acceso:

1.  Vaya al servidor que informa de este error.
1. Busque y seleccione **Ejecutar** en el menú **Inicio**. En el cuadro de diálogo **Ejecutar**, escriba **wmimgmt.msc** en el cuadro de texto **Abrir** y seleccione **Entrar**.
1. Se abre la consola wmimgmt, en la que puede encontrar **Control WMI (local)** en el panel izquierdo. Haga clic con el botón derecho y seleccione **Propiedades** en el menú.
1. En el cuadro de diálogo **Propiedades: Control WMI (local)** , seleccione la pestaña **Seguridad**.
1. Seleccione **Seguridad** para abrir el diálogo **Seguridad para ROOT**.
1. Seleccione el botón **Opciones avanzadas** para abrir el cuadro de diálogo **Configuración de seguridad avanzada para Root**. 
1. Seleccione el botón **Agregar** para abrir el cuadro de diálogo **Entrada de permiso para Root**.
1. Haga clic en **Seleccionar una entidad de seguridad** para abrir el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos**.
1. Seleccione los nombres de usuario o grupos a los que desea conceder acceso a WMI y haga clic en **Aceptar**.
1. Asegúrese de conceder permisos de ejecución y seleccione **Este espacio de nombres y subespacios de nombres** en la lista desplegable **Se aplica a**.
1. Seleccione **Aplicar** para guardar la configuración y cerrar todos los cuadros de diálogo.

Después de obtener el acceso necesario, compruebe si se resolvió el error; para ello, siga los pasos que se indican en [este sitio web](troubleshoot-discovery.md#mitigation-verification-by-using-vmware-powercli).

## <a name="error-9032-invalidrequest"></a>Error 9032: InvalidRequest

### <a name="cause"></a>Causa
Los motivos de este problema pueden ser varios. Una razón es cuando el nombre de usuario proporcionado (credenciales del servidor) en el administrador de configuración del dispositivo tiene caracteres XML no válidos. Los caracteres no válidos provocan un error al analizar la solicitud SOAP.

### <a name="remediation"></a>Corrección
- Asegúrese de que el nombre de usuario de las credenciales del servidor no tenga caracteres XML no válidos y esté en formato username@domain.com. Este formato se conoce popularmente como formato UPN.
- Después de editar las credenciales en el dispositivo, compruebe si se resolvió el error; para ello, siga los pasos que se indican en [este sitio web](troubleshoot-discovery.md#mitigation-verification-by-using-vmware-powercli).


## <a name="error-10002-scriptexecutiontimedoutonvm"></a>Error 10002: ScriptExecutionTimedOutOnVm

### <a name="cause"></a>Causa
- Este error se produce cuando el servidor funciona lento o no responde y se empieza a agotar el tiempo de espera del script ejecutado para extraer los datos de dependencias.
- Una vez que el agente de detección encuentra este error en el servidor, el dispositivo deja de intentar realizar el análisis de dependencias sin agente en el servidor a fin de evitar sobrecargar el servidor que no responde.
- Seguirá viendo el error hasta que compruebe el problema con el servidor y reinicie el servicio de detección.

### <a name="remediation"></a>Corrección

1. Inicie sesión en el servidor en el que se encontró este error.
1. Ejecute los siguientes comandos en PowerShell:

   ````
   Get-WMIObject win32_operatingsystem;
   Get-WindowsFeature  | Where-Object {$_.InstallState -eq 'Installed' -or ($_.InstallState -eq $null -and $_.Installed -eq 'True')};
   Get-WmiObject Win32_Process;
   netstat -ano -p tcp | select -Skip 4;
   ````
1. Si los comandos generan el resultado en unos segundos, vaya al **proyecto de Azure Migrate** > **Discovery and assessment** > **Overview** > **Manage** > **Appliances** (Detección y evaluación > Información general > Administrar > Dispositivos), seleccione el nombre del dispositivo y, luego, **Refresh services** (Actualizar servicios).
1. Si se está agotando el tiempo de espera de los comandos sin generar ninguna salida, debe realizar lo siguiente:

   - Averigüe qué proceso consume una gran cantidad de memoria o CPU en el servidor.
   - Intentar proporcionar más núcleos o memoria a ese servidor y volver a ejecutar los comandos.

## <a name="error-10005-guestcredentialnotvalid"></a>Error 10005: GuestCredentialNotValid

### <a name="remediation"></a>Corrección
- Para garantizar la validez de las credenciales _(el nombre descriptivo que se proporciona en el error)_ , seleccione **Revalidate credentials** (Revalidar credenciales) en el administrador de configuración del dispositivo.
- Asegúrese de que puede iniciar sesión en el servidor afectado con la misma credencial proporcionada en el dispositivo.
- Puede intentarlo mediante el uso de otra cuenta de usuario (del mismo dominio, en caso de que el servidor esté unido a un dominio) de ese servidor en lugar de la cuenta de administrador.
- El problema puede producirse cuando la comunicación entre el catálogo global y el controlador de dominio está rota. Para comprobar este problema, cree una nueva cuenta de usuario en el controlador de dominio y proporcione esta cuenta en el dispositivo. Es posible que también tenga que reiniciar el controlador de dominio.
- Después de llevar a cabo los pasos de corrección, compruebe si se resolvió el error; para ello, siga los pasos que se indican en [este sitio web](troubleshoot-discovery.md#mitigation-verification-by-using-vmware-powercli).

## <a name="error-10012-credentialnotprovided"></a>Error 10012: CredentialNotProvided

### <a name="cause"></a>Causa
Este error se produce cuando se ha proporcionado una credencial de dominio con un nombre de dominio incorrecto en el administrador de configuración del dispositivo. Por ejemplo, si ha especificado una credencial de dominio con nombre de usuario user@abc.com, pero ha proporcionado el nombre de dominio como def.com, esas credenciales no se utilizarán si el servidor está conectado a def.com y recibirá este mensaje de error.

### <a name="remediation"></a>Corrección
- Vaya al administrador de configuración del dispositivo para agregar una credencial de servidor o editar una existente, tal como se explica en la causa.
- Después de llevar a cabo los pasos de corrección, compruebe si se resolvió el error; para ello, siga los pasos que se indican en [este sitio web](troubleshoot-discovery.md#mitigation-verification-by-using-vmware-powercli).

## <a name="mitigation-verification-by-using-vmware-powercli"></a>Comprobación de la mitigación con VMware PowerCLI
Después de usar los pasos de mitigación para los errores anteriores, compruebe si la mitigación ha funcionado mediante la ejecución de algunos comandos de PowerCLI desde el servidor del dispositivo. Si los comandos se ejecutan correctamente, significa que el problema se ha resuelto. De lo contrario, compruebe y siga los pasos de corrección de nuevo.

1. Ejecute los comandos siguientes para configurar PowerCLI en el servidor del dispositivo:
   ````
   Install-Module -Name VMware.PowerCLI -AllowClobber
   Set-PowerCLIConfiguration -InvalidCertificateAction Ignore
   ````
1. Conéctese a vCenter Server desde el dispositivo; para ello, proporcione la dirección IP de vCenter Server en el comando y las credenciales en el símbolo del sistema:
   ````
   Connect-VIServer -Server <IPAddress of vCenter Server>
   ````
1. Conéctese al servidor de destino desde el dispositivo; para ello, especifique el nombre y las credenciales del servidor (tal como se han proporcionado en el dispositivo):
   ````
   $vm = get-VM <VMName>
   $credential = Get-Credential
   ````
1. En el caso del inventario de software, ejecute los siguientes comandos para ver si obtiene una salida correcta:

   - Para servidores Windows:

      ```` 
        Invoke-VMScript -VM $vm -ScriptText "powershell.exe 'Get-WMIObject win32_operatingsystem'" -GuestCredential $credential

        Invoke-VMScript -VM $vm -ScriptText "powershell.exe Get-WindowsFeature" -GuestCredential $credential
      ```` 
   - Para servidores Linux:
      ````
      Invoke-VMScript -VM $vm -ScriptText "ls" -GuestCredential $credential
      ````
1. En el caso del análisis de dependencias sin agente, ejecute los siguientes comandos para ver si obtiene una salida correcta:

   - Para servidores Windows:

      ```` 
      Invoke-VMScript -VM $vm -ScriptText "powershell.exe 'Get-WmiObject Win32_Process'" -GuestCredential $credential 
  
      Invoke-VMScript -VM $vm -ScriptText "powershell.exe 'netstat -ano -p tcp'" -GuestCredential $credential 
      ```` 
   - Para servidores Linux:
      ````
      Invoke-VMScript -VM $vm -ScriptText "ps -o pid,cmd | grep -v ]$" -GuestCredential $credential

      Invoke-VMScript -VM $vm -ScriptText "netstat -atnp | awk '{print $4,$5,$7}'" -GuestCredential $credential
      ````
1. Después de comprobar que la mitigación funcionó, vaya al **proyecto de Azure Migrate** > **Discovery and assessment** > **Overview** > **Manage** > **Appliances** (Detección y evaluación > Información general > Administrar > Dispositivos), seleccione el nombre del dispositivo y, luego, **Refresh services** (Actualizar servicios) para iniciar un ciclo de detección nuevo.

## <a name="discovered-sql-server-instances-and-databases-not-in-the-portal"></a>Las instancias y bases de datos de SQL Server detectadas no aparecen en el portal

Una vez que haya iniciado la detección en el dispositivo, puede tardar hasta 24 horas en empezar a mostrar los datos de inventario en el portal.

Si no ha proporcionado las credenciales de autenticación de Windows o SQL Server en el administrador de configuración del dispositivo, agregue las credenciales para que el dispositivo pueda usarlas para conectarse a las instancias de SQL Server correspondientes.

Una vez conectado el dispositivo, este recopila datos de configuración y rendimiento de las instancias y bases de datos de SQL Server. Los datos de configuración de SQL Server se actualizan una vez cada 24 horas y los datos de rendimiento se capturan cada 30 segundos. Cualquier cambio en las propiedades de la instancia y bases de datos de SQL Server, como el estado de la base de datos y el nivel de compatibilidad, puede tardar hasta 24 horas en actualizarse en el portal.

## <a name="sql-server-instance-is-showing-up-in-a-not-connected-state-on-the-portal"></a>La instancia de SQL Server se muestra en el estado "Sin conexión" en el portal

Para ver los problemas encontrados durante la detección de instancias y bases de datos de SQL Server, seleccione el estado **Sin conexión** en la columna de estado de la conexión de la página **Servidores detectados** del proyecto.

La creación de una evaluación sobre los servidores que contienen instancias de SQL que no se detectaron por completo o que se encuentran en estado sin conexión, puede dar lugar a que se marque la preparación como **Desconocida**.

## <a name="common-sql-server-instances-and-database-discovery-errors"></a>Instancias de SQL Server comunes y errores de detección de base de datos

Azure Migrate admite la detección de instancias y bases de datos de SQL Server que se ejecutan en máquinas locales mediante Azure Migrate: Discovery and assessment. Actualmente solo se admite la detección de SQL para VMware. Para comenzar, consulte el tutorial de [detección](tutorial-discover-vmware.md).

Los errores de detección de SQL típicos se resumen en la siguiente tabla.

| **Error** | **Causa** | **Acción** | **Guía**
|--|--|--|--|
|**30000**: las credenciales asociadas a esta instancia de SQL Server no funcionan.|Las credenciales asociadas manualmente no son válidas o las que se han asociado de forma automática ya no pueden obtener acceso a SQL Server.|Agregue las credenciales de SQL Server en el dispositivo y espere hasta el siguiente ciclo de detección de SQL o fuerce la actualización.| - |
|**30001**: no se puede conectar a SQL Server desde el dispositivo.|1. El dispositivo no tiene una línea de visión de red para SQL Server.<br/>2. El firewall está bloqueando la conexión entre SQL Server y el dispositivo.|1. Haga que se pueda acceder a SQL Server desde el dispositivo.<br/>2. Permita las conexiones entrantes del dispositivo a SQL Server.| - | 
|**30003**: el certificado no es de confianza.|No hay ningún certificado de confianza instalado en el equipo que ejecuta SQL Server.|Configure un certificado de confianza en el servidor. [Más información](/troubleshoot/sql/connect/error-message-when-you-connect).| [Vista](/troubleshoot/sql/connect/error-message-when-you-connect) |
|**30004**: permisos insuficientes.|Este error puede producirse debido a la falta de permisos necesarios para examinar las instancias de SQL Server. |Conceda el rol sysadmin a las credenciales o la cuenta proporcionada en el dispositivo para detectar las instancias y bases de datos de SQL Server. [Más información](/sql/t-sql/statements/grant-server-permissions-transact-sql).| [Vista](/sql/t-sql/statements/grant-server-permissions-transact-sql) |
|**30005**: el inicio de sesión de SQL Server no pudo conectarse debido a un problema con su base de datos maestra predeterminada.|Bien la propia base de datos no es válida o el inicio de sesión no tiene permiso CONNECT para la base de datos.|Use ALTER LOGIN para establecer la base de datos predeterminada en la base de datos maestra.<br/>Conceda el rol sysadmin a las credenciales o la cuenta proporcionada en el dispositivo para detectar las instancias y bases de datos de SQL Server. [Más información](/sql/relational-databases/errors-events/mssqlserver-4064-database-engine-error).| [Vista](/sql/relational-databases/errors-events/mssqlserver-4064-database-engine-error) |
|**30006**: el inicio de sesión de SQL Server no se puede usar con la autenticación de Windows.|1. El inicio de sesión puede ser de SQL Server, pero el servidor solo acepta la autenticación de Windows.<br/>2. Intenta conectarse mediante la autenticación de SQL Server, pero el inicio de sesión que ha utilizado no existe en SQL Server.<br/>3. El inicio de sesión puede utilizar la autenticación de Windows, pero el inicio de sesión es una entidad de seguridad de Windows no reconocida. Una entidad de seguridad de Windows no reconocida significa que Windows no puede comprobar el inicio de sesión. Este problema podría deberse a que el inicio de sesión de Windows procede de un dominio que no es de confianza.|Si intenta conectarse mediante la autenticación de SQL Server, compruebe que SQL Server está configurado en modo de autenticación mixta y que el inicio de sesión de SQL Server existe.<br/>Si intenta establecer conexión usando la autenticación de Windows, compruebe que ha iniciado sesión correctamente en el dominio correcto. [Más información](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error).| [Vista](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error) |
|**30007**: la contraseña ha expirado.|La contraseña de la cuenta expiró.|Es posible que la contraseña de inicio de sesión de SQL Server haya expirado. Restablezca la contraseña o amplíe la fecha de expiración de la contraseña. [Más información](/sql/relational-databases/native-client/features/changing-passwords-programmatically).| [Vista](/sql/relational-databases/native-client/features/changing-passwords-programmatically) |
|**30008**: es necesario cambiar la contraseña.|Se debe cambiar la contraseña de la cuenta.|Cambie la contraseña de la credencial proporcionada para la detección de SQL Server. [Más información](/previous-versions/sql/sql-server-2008-r2/cc645934(v=sql.105)).| [Vista](/previous-versions/sql/sql-server-2008-r2/cc645934(v=sql.105)) |
|**30009**: error interno.|Se produjo un error interno al detectar las instancias y bases de datos de SQL Server. |Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.| - |
|**30010**: no se ha encontrado ninguna base de datos.|No se encuentra ninguna base de datos de la instancia de servidor seleccionada.|Conceda el rol sysadmin a las credenciales o la cuenta proporcionada en el dispositivo para detectar las bases de datos de SQL Server.| - |
|**30011**: se produjo un error interno al evaluar una instancia o base de datos de SQL.|Se produjo un error interno al realizar una valoración.|Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.| - |
|**30012**: error de la conexión SQL.|1. El firewall en el servidor ha rechazado la conexión.<br/>2. El servicio SQL Server Browser (sqlbrowser) no se inició.<br/>3. SQL Server no respondió a la solicitud del cliente porque es probable que el servidor no se haya iniciado.<br/>4. El cliente de SQL Server no se puede conectar al servidor. Este error se puede producir porque el servidor no está configurado para aceptar conexiones remotas.<br/>5. El cliente de SQL Server no se puede conectar al servidor. Este error se puede producir porque el cliente no puede resolver el nombre del servidor o porque el nombre del servidor no es correcto.<br/>6. Los protocolos de canalizaciones con nombre o TCP no están habilitados.<br/>7. El nombre de la instancia de SQL Server especificado no es válido.|Use [esta guía de usuario interactiva](https://go.microsoft.com/fwlink/?linkid=2153317) para solucionar problemas de conectividad. Espere 24 horas después de seguir los pasos de la guía para que se actualicen los datos en el servicio. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.| [Vista](https://go.microsoft.com/fwlink/?linkid=2153317) |
|**30013**: se produjo un error al establecer una conexión con la instancia de SQL Server.|1. El nombre de SQL Server no se puede resolver desde el dispositivo.<br/>2. SQL Server no permite conexiones remotas.|Si puede hacer ping a SQL Server desde el dispositivo, espere 24 horas para comprobar si este problema se resuelve automáticamente. En caso contrario, póngase en contacto con el servicio de soporte técnico de Microsoft. [Más información](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error).| [Vista](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error) |
|**30014**: el nombre de usuario o la contraseña no son válidos.| Este error puede producirse debido a un error de autenticación que implica una contraseña o un nombre de usuario incorrectos.|Especifique una credencial con un nombre de usuario y una contraseña válidos. [Más información](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error).| [Vista](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error) |
|**30015**: se produjo un error interno al detectar la instancia de SQL Server.|Se produjo un error interno al detectar la instancia de SQL Server.|Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.| - |
|**30016**: no se pudo establecer la conexión con la instancia "%instance;" porque se agotó el tiempo de espera.| Este problema puede suceder si el firewall del servidor rechaza la conexión.|Compruebe si el firewall de SQL Server está configurado para aceptar conexiones. Si el error persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. [Más información](/sql/relational-databases/errors-events/mssqlserver-neg2-database-engine-error).| [Vista](/sql/relational-databases/errors-events/mssqlserver-neg2-database-engine-error) |
|**30017**: error interno.|Excepción no controlada.|Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.| - |
|**30018**: error interno.|Se produjo un error interno al recopilar datos, por ejemplo, el tamaño de TempDB o el tamaño de los archivos de la instancia de SQL.|Espere 24 horas y póngase en contacto con el servicio de soporte técnico de Microsoft si el problema persiste.| - |
|**30019**: error interno.|Se produjo un error interno al recopilar las métricas de rendimiento (por ejemplo, el uso de la memoria) de una base de datos o una instancia.|Espere 24 horas y póngase en contacto con el servicio de soporte técnico de Microsoft si el problema persiste.| - |

## <a name="common-web-apps-discovery-errors"></a>Errores comunes de detección de aplicaciones web

Azure Migrate admite la detección de aplicaciones web ASP.NET que se ejecutan en máquinas locales mediante Azure Migrate: Discovery and assessment. Actualmente solo se admite la detección de aplicaciones web para VMware. Para comenzar, consulte el tutorial de [detección](tutorial-discover-vmware.md).

En la siguiente tabla se resumen los errores típicos de detección de aplicaciones web.

| **Error** | **Causa** | **Acción** |
|--|--|--|
| **40001:** La característica Consola de administración de IIS no está habilitada. | La detección de aplicaciones web de IIS usa la API de administración que se incluye con la versión local de IIS para leer la configuración de IIS. Esta API está disponible cuando está habilitada la característica Consola de administración de IIS. Esta característica no está habilitada o la versión del sistema operativo no es compatible con la detección de aplicaciones web de IIS.| Asegúrese de que el rol Servidor web (IIS), incluida la característica Consola de administración de IIS (que forma parte de las herramientas de administración), está habilitado y de que el sistema operativo del servidor es Windows Server 2008 R2, o cualquier versión posterior.|
| **40002:** no se puede conectar al servidor desde el dispositivo. | Se produjo un error en la conexión al servidor porque las credenciales de inicio de sesión no son válidas o la máquina no está disponible. | Asegúrese de que las credenciales de inicio de sesión proporcionadas para el servidor son correctas y de que el servidor está en línea y acepta conexiones remotas de PowerShell de WS-Management. |
| **40003:** no os posible conectarse al servidor porque las credenciales no son válidas. | No se pudo establecer conexión con el servidor porque las credenciales de inicio de sesión no son válidas. | Asegúrese de que las credenciales de inicio de sesión proporcionadas para el servidor son correctas y de que la comunicación remota de PowerShell de WS-Management está habilitada. |
| **40004:** No se puede acceder a la configuración de IIS. | No tiene permisos o los permisos son insuficientes. | Confirme que las credenciales de usuario proporcionadas para el servidor son credenciales de nivel de administrador y que el usuario tiene permiso para acceder a los archivos del directorio de IIS (%windir%\System32\inetsrv) y del directorio de configuración del servidor de IIS (%windir%\System32\inetsrv\config). |
| **40005:** No se puede completar la detección de IIS. | Se produjo un error al completar la detección en la máquina virtual. Este problema puede producirse porque hay problemas con el acceso a la configuración en el servidor. El error fue "%detailedMessage;". | Confirme que las credenciales de usuario proporcionadas para el servidor son credenciales de nivel de administrador y que el usuario tiene permiso para acceder a los archivos del directorio de IIS (%windir%\System32\inetsrv) y del directorio de configuración del servidor de IIS (%windir%\System32\inetsrv\config). Póngase en contacto con el servicio de soporte técnico de Microsoft e indique los detalles del error. |
| **40006:** Excepción sin categoría. | Nuevo escenario de error. | Póngase en contacto con el soporte técnico de Microsoft y proporciónele los detalles y registros del error. Los registros se pueden encontrar en el servidor del dispositivo, en la ruta de acceso C:\ProgramData\Microsoft Azure\Logs. |
| **40007:** No se encontró ninguna aplicación web para el servidor web. | El servidor web no tiene ninguna aplicación hospedada. | Compruebe la configuración del servidor web. |

## <a name="next-steps"></a>Pasos siguientes

Configuración de un dispositivo para [VMware](how-to-set-up-appliance-vmware.md), [Hyper-V](how-to-set-up-appliance-hyper-v.md), o [servidores físicos](how-to-set-up-appliance-physical.md).
