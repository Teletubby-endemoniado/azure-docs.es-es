---
title: Notas de la versión del agente de Azure File Sync | Microsoft Docs
description: Lea las notas de la versión del agente de Azure File Sync, que le permite centralizar los recursos compartidos de archivos de la organización en Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: conceptual
ms.date: 11/9/2021
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 27a5e1e1341b93a8172a179db1dc8596c103a184
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132716037"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Notas de la versión del agente de Azure File Sync
Azure File Sync le permite centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Las instalaciones de Windows Server se transforman en una memoria caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a los datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

En este artículo se tratan las notas de la versión de las versiones compatibles del agente de Azure File Sync.

## <a name="supported-versions"></a>Versiones compatibles
Se admiten las siguientes versiones de agentes de Azure File Sync:

| Hito | Número de versión del agente | Fecha de la versión | Status |
|----|----------------------|--------------|------------------|
| Versión V14: [KB5001872](https://support.microsoft.com/topic/92290aa1-75de-400f-9442-499c44c92a81)| 14.0.0.0 | 29 de octubre de 2021 | Compatible: lanzamiento como paquete piloto |
| Versión V13: [KB4588753](https://support.microsoft.com/topic/632fb833-42ed-4e4d-8abd-746bd01c1064)| 13.0.0.0 | 12 de julio de 2021 | Compatible |
| Versión V12.1: [KB4588751](https://support.microsoft.com/topic/497dc33c-d38b-42ca-8015-01c906b96132)| 12.1.0.0 | 20 de mayo de 2021 | Compatible |
| Versión V12: [KB4568585](https://support.microsoft.com/topic/b9605f04-b4af-4ad8-86b0-2c490c535cfd)| 12.0.0.0 | 26 de marzo de 2021 | Compatible |
| Versión V11.3: [KB4539953](https://support.microsoft.com/topic/f68974f6-bfdd-44f4-9659-bf2d8a696c26)| 11.3.0.0 | 7 de abril de 2021 | Compatible |
| Versión V11.2: [KB4539952](https://support.microsoft.com/topic/azure-file-sync-agent-v11-2-release-february-2021-c956eaf0-cd8e-4511-98c0-e5a1f2c84048)| 11.2.0.0 | 2 de febrero de 2021 | Compatible |
| Versión V11.1: [KB4539951](https://support.microsoft.com/help/4539951)| 11.1.0.0 | 4 de noviembre de 2020 | Compatible |

## <a name="unsupported-versions"></a>Versiones no admitidas
Las siguientes versiones de agentes de Azure File Sync han expirado y ya no se admiten:

| Hito | Número de versión del agente | Fecha de la versión | Status |
|----|----------------------|--------------|------------------|
| Versión V10 | 10.0.0.0 - 10.1.0.0 | N/D | No compatible: las versiones del agente expiraron el 28 de junio de 2021. |
| Versión V9 | 9.0.0.0 - 9.1.0.0 | N/D | No compatible: las versiones del agente expiraron el 16 de febrero de 2021. |
| Versión 8 | 8.0.0.0 | N/D | No compatible: las versiones del agente expiraron el 12 de enero de 2021. |
| Versión V7 | 7.0.0.0 - 7.2.0.0 | N/D | No compatible: las versiones del agente expiraron el 1 de septiembre de 2020. |
| Versión V6 | 6.0.0.0 - 6.3.0.0 | N/D | No compatible: las versiones del agente expiraron el 21 de abril de 2020. |
| Versión V5 | 5.0.2.0 - 5.2.0.0 | N/D | No compatibles: las versiones del agente expiraron el 18 de marzo de 2020 |
| Versión V4 | 4.0.1.0-4.3.0.0 | N/D | No compatibles: las versiones del agente expiraron el 6 de noviembre de 2019 |
| Versión V3 | 3.1.0.0 - 3.4.0.0 | N/D | No compatibles: las versiones del agente expiraron el 19 de agosto de 2019 |
| Agentes anteriores a la disponibilidad general | 1.1.0.0 - 3.0.13.0 | N/D | No compatibles: las versiones del agente expiraron el 1 de octubre de 2018 |

### <a name="azure-file-sync-agent-update-policy"></a>Directiva de actualización del agente de Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-14000"></a>Versión 14.0.0.0 del agente
Las notas siguientes se corresponden a la versión 14.0.0.0 del agente de Azure File Sync (publicada el 29 de octubre de 2021).

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado
- Se han reducido las transacciones cuando se ejecuta el trabajo de enumeración de cambios en la nube. 
    - Azure File Sync tiene un trabajo de enumeración de cambios en la nube que se ejecuta cada 24 horas para detectar los cambios realizados directamente en el recurso compartido de archivos de Azure y sincronizar esos cambios con los servidores de los grupos de sincronización. Hemos realizado mejoras para reducir el número de transacciones cuando se ejecuta este trabajo.

- Mejora de las instrucciones de desaprovisionamiento de puntos de conexión de servidor en el portal
    - Al quitar un punto de conexión de servidor desde del portal, ahora se proporcionan instrucciones paso a paso en función del motivo de la eliminación del punto de conexión de servidor, de modo que pueda evitar la pérdida de datos y asegurarse de que los datos están donde deben estar (un servidor o un recurso compartido de archivos de Azure). Esta característica también incluye nuevos cmdlets de PowerShell (Get-StorageSyncStatus y New-StorageSyncUploadSession) que puede usar en el servidor local para facilitar el proceso de desaprovisionamiento.

- Mejoras en el cmdlet Invoke-AzStorageSyncChangeDetection
    - Antes de la versión v14, si se realizaban cambios directamente en el recurso compartido de archivos de Azure, se podía usar el cmdlet Invoke-AzStorageSyncChangeDetection para detectar los cambios y sincronizarlos con los servidores del grupo de sincronización. Pero el cmdlet no se ejecutaba si la ruta especificada contenía más de 10 000 elementos. Se ha mejorado el cmdlet Invoke-AzStorageSyncChangeDetection y el límite de 10 000 elementos ya no se aplica al examinar todo el recurso compartido. Para más información, consulte la documentación sobre [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection).

- Otras mejoras
    - Ahora Azure File Sync se admite en la región Oeste de EE. UU. 3.
    - Se ha corregido un error que provocaba que el script FileSyncErrorsReport.ps1 no proporcionara la lista de todos los errores por elemento.
    - Se han reducido las transacciones cuando un archivo no se puede cargar de forma coherente debido a un error de sincronización por elemento.
    - Mejoras de confiabilidad y telemetría para la sincronización y la nube por niveles. 

### <a name="evaluation-tool"></a>Herramienta de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante la herramienta de evaluación de Azure File Sync. Esta herramienta es un cmdlet de Azure PowerShell que busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Para la instalación y las instrucciones de uso, consulte la sección [Herramienta de evaluación](file-sync-planning.md#evaluation-cmdlet) en la Guía de planeación. 

### <a name="agent-installation-and-server-configuration"></a>Instalación del agente y configuración del servidor
Para más información sobre cómo instalar y configurar el agente de Azure File Sync con Windows Server, vea [Planeamiento de una implementación de Azure File Sync](file-sync-planning.md) y [How to deploy Azure File Sync](file-sync-deployment-guide.md) (Implementación de Azure Files Sync).

- Se requiere reiniciar los servidores con una instalación del agente de Azure File Sync si la versión del agente es inferior a la versión 12.0.
- El paquete de instalación del agente debe instalarse con permisos elevados (administrador).
- El agente no es compatible con la opción de implementación de Nano Server.
- El agente solo se admite en Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2022.
- El agente requiere al menos 2 GB de memoria. Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina virtual debe configurarse con un mínimo de 2048 MiB de memoria. Para más información, consulte el artículo en el que se indican los [recursos del sistema recomendados](file-sync-planning.md#recommended-system-resources).
- El servicio del agente de sincronización de Storage (FileSyncSvc) no admite puntos de conexión de servidor ubicados en un volumen que tiene el directorio de información del volumen del sistema (SVI) comprimido. Esta configuración producirá resultados inesperados.

### <a name="interoperability"></a>Interoperabilidad
- Las aplicaciones antivirus, de copia de seguridad y de otro tipo que accedan a archivos organizados en niveles pueden provocar recuperaciones no deseadas, salvo que respeten el atributo sin conexión y pasen por alto la lectura del contenido de esos archivos. Para más información, vea [Solución de problemas de Azure File Sync](file-sync-troubleshoot.md).
- Los filtros de archivos del Administrador de recursos del servidor de archivos (FSRM) pueden provocar un gran número de errores de sincronización cuando los archivos están bloqueados debido al filtro de archivos.
- No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y esto puede provocar resultados inesperados. El agente de Azure File Sync se debe instalar después de implementar la imagen del servidor y completar la instalación mínima de la herramienta de preparación del sistema.

### <a name="sync-limitations"></a>Limitaciones de la sincronización
Los siguientes elementos no se sincronizan, pero el resto del sistema funciona con normalidad:
- Archivos con caracteres no admitidos. Consulte la [Guía de solución de problemas](file-sync-troubleshoot.md#handling-unsupported-characters) para obtener la lista de caracteres no admitidos.
- Archivos o directorios que terminan con un punto.
- Rutas de acceso de más de 2,048 caracteres.
- La parte de la lista de control de acceso del sistema (SACL) de un descriptor de seguridad que se utiliza para la auditoría.
- Atributos extendidos.
- Flujos de datos alternativos.
- Puntos de repetición de análisis.
- Vínculos físicos.
- La compresión (si se establece en un archivo de servidor) no se conserva al sincronizarse los cambios con ese archivo desde otros puntos de conexión.
- Los archivos cifrados con EFS (u otros cifrados con modo de usuario) que evitan que el servicio lea los datos.

    > [!Note]  
    > Azure File Sync siempre cifra los datos en tránsito. Los datos siempre se cifran en reposo en Azure.
 
### <a name="server-endpoint"></a>Punto de conexión de servidor
- Un punto de conexión de servidor solo se puede crear en un volumen NTFS. En este momento Azure File Sync no admite ReFS, FAT, FAT32 ni otros sistemas de archivos.
- Los niveles de nube no se admiten en el volumen del sistema. Para crear un punto de conexión de servidor en el volumen del sistema, deshabilite los niveles de la nube al crear el punto de conexión de servidor.
- Los clústeres de conmutación por error solo son compatibles con discos en clúster, pero no con volúmenes compartidos de clúster (CSV).
- No se puede anidar un punto de conexión de servidor. Puede coexistir en el mismo volumen en paralelo con otro punto de conexión.
- No almacene un archivo de paginación de aplicación o sistema operativo en una ubicación de punto de conexión de servidor.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
- Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión de nube una vez cada 24 horas. Para sincronizar inmediatamente los archivos que se modifican en el recurso compartido de archivos de Azure, se puede usar el cmdlet [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) de PowerShell para iniciar de forma manual la detección de cambios en el recurso compartido. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización.
- El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a un grupo de recursos, suscripción o inquilino de Azure AD diferentes. Después de mover el servicio de sincronización de almacenamiento o la cuenta de almacenamiento, debe dar acceso a la aplicación Microsoft.StorageSync a la cuenta de almacenamiento (consulte [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](file-sync-troubleshoot.md?tabs=portal1%252cportal#troubleshoot-rbac)).

    > [!Note]  
    > Al crear el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento deben estar en el mismo inquilino de Azure AD. Una vez creado el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento se pueden migrar a distintos inquilinos de Azure AD.

### <a name="cloud-tiering"></a>Niveles de nube
- Si un archivo en niveles se copia en otra ubicación mediante el uso de Robocopy, el archivo resultante no estará en niveles. El atributo sin conexión podría estar establecido porque Robocopy incluye incorrectamente dicho atributo en las operaciones de copia.
- Al copiar los archivos mediante Robocopy, utilice la opción /MIR para conservar las marcas de tiempo de archivo. Esto garantizará que los archivos antiguos se organicen por niveles antes que los archivos a los que se ha accedido recientemente.

## <a name="agent-version-13000"></a>Versión 13.0.0.0 del agente
Las notas siguientes corresponden a la versión 13.0.0.0 del agente de Azure File Sync publicada el 12 de julio de 2021.

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado
- Carga autoritativa  
    - La carga autoritativa es un nuevo modo disponible al crear el primer punto de conexión de servidor en un grupo de sincronización. Resulta útil para el escenario en el que la nube (recurso compartido de archivos de Azure) tiene algunos o la mayoría de los datos, pero no está actualizada y debe estar al día con los datos más recientes en el nuevo punto de conexión de servidor. Este es el caso en escenarios de migración sin conexión como DataBox, por ejemplo. Cuando se rellena un DataBox y se envía a Azure, los usuarios del servidor local seguirán cambiando, agregando o eliminando archivos en el servidor local. Esto hace que los datos de DataBox y, por tanto, el recurso compartido de archivos de Azure, no se puedan usar. Con la carga autoritativa, ahora puede decir al servidor y a la nube cómo resolver este caso y actualizar la nube sin problemas con los cambios más recientes en el servidor. 

        Independientemente de cómo hayan llegado los datos a la nube, este modo puede actualizar el recurso compartido de archivos de Azure si los datos proceden de la ubicación de coincidencia en el servidor. Asegúrese de evitar las grandes reestructuraciones de directorios entre la copia inicial en la nube y ponerse al día con la carga autoritativa. Esto garantizará que solo se transporten actualizaciones. Los cambios en los nombres de directorio harán que todos los archivos de estos directorios con nombre cambiado se vuelvan a cargar. Esta funcionalidad es comparable a la semántica de RoboCopy/MIR = origen reflejado en destino, incluida la eliminación de archivos en el destino que ya no existen en el origen. 
        
        La carga autoritativa reemplaza la característica "Transferencia de datos sin conexión" para la integración de DataBox con Azure File Sync a través de un recurso compartido de almacenamiento provisional. Ya no es necesario un recurso compartido de almacenamiento provisional para usar DataBox. Los nuevos trabajos de transferencia de datos sin conexión ya no se pueden iniciar con el agente de AFS V13. Los trabajos existentes en un servidor continuarán incluso con la actualización a la versión 13 del agente.

- Mejoras del portal para ver la enumeración de cambios en la nube y el progreso de la sincronización  
    - Cuando se crea un nuevo grupo de sincronización, cualquier punto de conexión de servidor conectado solo puede iniciar la sincronización cuando se completa la enumeración de cambios en la nube.  En caso de que ya existan archivos en el punto de conexión de nube (recurso compartido de archivos de Azure) de este grupo de sincronización, la enumeración de cambios del contenido en la nube puede tardar algún tiempo. Cuantos más elementos (archivos y carpetas) existan en el espacio de nombres, más tiempo puede tardar este proceso. Los administradores ahora podrán obtener el progreso de la enumeración de cambios en la nube en Azure Portal para calcular una hora estimada de finalización o sincronización con los servidores.

- Compatibilidad con el cambio de nombre del servidor  
    - Si se cambia el nombre de un servidor registrado, Azure File Sync mostrará el nuevo nombre de servidor en el portal. Si se cambió el nombre del servidor antes de la versión v13, el nombre del servidor en el portal ahora se actualizará para mostrar el nombre de servidor correcto.

- Compatibilidad con Windows Server 2022  
    - El agente de Azure File Sync ahora se admite en Windows Server 2022.

    > [!Note]  
    > Windows Server 2022 agrega compatibilidad con TLS 1.3, que actualmente no es compatible con Azure File Sync.  Si la [configuración de TLS](/windows-server/security/tls/tls-ssl-schannel-ssp-overview) se administra a través de la directiva de grupo, el servidor debe configurarse para admitir TLS 1.2. 

- Otras mejoras
    - Mejoras de confiabilidad para la sincronización, la nube por niveles y la enumeración de cambios en la nube.
    - Si se cambia un gran número de archivos en el servidor, la carga de sincronización se realiza ahora desde una instantánea de VSS que reduce los errores por elemento y los errores de sesión de sincronización. 
    - El cmdlet Invoke-StorageSyncFileRecall recuperará ahora todos los archivos en capas asociados a un punto de conexión de servidor, incluso si el archivo se ha movido fuera de la ubicación del punto de conexión del servidor. 
    - Explorer.exe ahora se excluye del seguimiento de la hora del último acceso de la nube por niveles.
    - Nueva telemetría (id. de evento 6664) para supervisar el progreso de limpieza de archivos huérfanos en capas después de quitar un punto de conexión de servidor con la nube por niveles habilitada.


### <a name="evaluation-tool"></a>Herramienta de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante la herramienta de evaluación de Azure File Sync. Esta herramienta es un cmdlet de Azure PowerShell que busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Para la instalación y las instrucciones de uso, consulte la sección [Herramienta de evaluación](file-sync-planning.md#evaluation-cmdlet) en la Guía de planeación. 

### <a name="agent-installation-and-server-configuration"></a>Instalación del agente y configuración del servidor
Para más información sobre cómo instalar y configurar el agente de Azure File Sync con Windows Server, vea [Planeamiento de una implementación de Azure File Sync](file-sync-planning.md) y [How to deploy Azure File Sync](file-sync-deployment-guide.md) (Implementación de Azure Files Sync).

- Se requiere reiniciar los servidores con una instalación del agente de Azure File Sync si la versión del agente es inferior a la versión 12.0.
- El paquete de instalación del agente debe instalarse con permisos elevados (administrador).
- El agente no es compatible con la opción de implementación de Nano Server.
- El agente solo se admite en Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2022.
- El agente requiere al menos 2 GB de memoria. Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina virtual debe configurarse con un mínimo de 2048 MiB de memoria. Para más información, consulte el artículo en el que se indican los [recursos del sistema recomendados](file-sync-planning.md#recommended-system-resources).
- El servicio del agente de sincronización de Storage (FileSyncSvc) no admite puntos de conexión de servidor ubicados en un volumen que tiene el directorio de información del volumen del sistema (SVI) comprimido. Esta configuración producirá resultados inesperados.

### <a name="interoperability"></a>Interoperabilidad
- Las aplicaciones antivirus, de copia de seguridad y de otro tipo que accedan a archivos organizados en niveles pueden provocar recuperaciones no deseadas, salvo que respeten el atributo sin conexión y pasen por alto la lectura del contenido de esos archivos. Para más información, vea [Solución de problemas de Azure File Sync](file-sync-troubleshoot.md).
- Los filtros de archivos del Administrador de recursos del servidor de archivos (FSRM) pueden provocar un gran número de errores de sincronización cuando los archivos están bloqueados debido al filtro de archivos.
- No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y esto puede provocar resultados inesperados. El agente de Azure File Sync se debe instalar después de implementar la imagen del servidor y completar la instalación mínima de la herramienta de preparación del sistema.

### <a name="sync-limitations"></a>Limitaciones de la sincronización
Los siguientes elementos no se sincronizan, pero el resto del sistema funciona con normalidad:
- Archivos con caracteres no admitidos. Consulte la [Guía de solución de problemas](file-sync-troubleshoot.md#handling-unsupported-characters) para obtener la lista de caracteres no admitidos.
- Archivos o directorios que terminan con un punto.
- Rutas de acceso de más de 2,048 caracteres.
- La parte de la lista de control de acceso del sistema (SACL) de un descriptor de seguridad que se utiliza para la auditoría.
- Atributos extendidos.
- Flujos de datos alternativos.
- Puntos de repetición de análisis.
- Vínculos físicos.
- La compresión (si se establece en un archivo de servidor) no se conserva al sincronizarse los cambios con ese archivo desde otros puntos de conexión.
- Los archivos cifrados con EFS (u otros cifrados con modo de usuario) que evitan que el servicio lea los datos.

    > [!Note]  
    > Azure File Sync siempre cifra los datos en tránsito. Los datos siempre se cifran en reposo en Azure.
 
### <a name="server-endpoint"></a>Punto de conexión de servidor
- Un punto de conexión de servidor solo se puede crear en un volumen NTFS. En este momento Azure File Sync no admite ReFS, FAT, FAT32 ni otros sistemas de archivos.
- Los niveles de nube no se admiten en el volumen del sistema. Para crear un punto de conexión de servidor en el volumen del sistema, deshabilite los niveles de la nube al crear el punto de conexión de servidor.
- Los clústeres de conmutación por error solo son compatibles con discos en clúster, pero no con volúmenes compartidos de clúster (CSV).
- No se puede anidar un punto de conexión de servidor. Puede coexistir en el mismo volumen en paralelo con otro punto de conexión.
- No almacene un archivo de paginación de aplicación o sistema operativo en una ubicación de punto de conexión de servidor.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
- Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión de nube una vez cada 24 horas. Para sincronizar inmediatamente los archivos que se modifican en el recurso compartido de archivos de Azure, se puede usar el cmdlet [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) de PowerShell para iniciar de forma manual la detección de cambios en el recurso compartido. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización.
- El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a un grupo de recursos, suscripción o inquilino de Azure AD diferentes. Después de mover el servicio de sincronización de almacenamiento o la cuenta de almacenamiento, debe dar acceso a la aplicación Microsoft.StorageSync a la cuenta de almacenamiento (consulte [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](file-sync-troubleshoot.md?tabs=portal1%252cportal#troubleshoot-rbac)).

    > [!Note]  
    > Al crear el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento deben estar en el mismo inquilino de Azure AD. Una vez creado el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento se pueden migrar a distintos inquilinos de Azure AD.

### <a name="cloud-tiering"></a>Niveles de nube
- Si un archivo en niveles se copia en otra ubicación mediante el uso de Robocopy, el archivo resultante no estará en niveles. El atributo sin conexión podría estar establecido porque Robocopy incluye incorrectamente dicho atributo en las operaciones de copia.
- Al copiar los archivos mediante Robocopy, utilice la opción /MIR para conservar las marcas de tiempo de archivo. Esto garantizará que los archivos antiguos se organicen por niveles antes que los archivos a los que se ha accedido recientemente.

## <a name="agent-version-12100"></a>Versión 12.1.0.0 del agente
Las notas de la versión siguientes corresponden a la versión 12.1.0.0 del agente de Azure File Sync publicada el 20 de mayo de 2021. Estas notas se suman a las notas de la versión enumeradas para la versión 12.0.0.0.

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado 
En la versión del agente v12.0 se corrigieron dos errores:
- La actualización automática del agente no puede actualizar el agente a una versión posterior.
- El script FileSyncErrorsReport.ps1 no proporciona la lista de errores por elemento.

## <a name="agent-version-12000"></a>Versión 12.0.0.0 del agente
Las notas siguientes corresponden a la versión 12.0.0.0 del agente de Azure File Sync (publicada el 26 de marzo de 2021).

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado
- Nueva experiencia del portal para configurar la directiva de acceso a la red y las conexiones de punto de conexión privado
    - Ahora puede usar el portal para deshabilitar el acceso al punto de conexión público del Servicio de sincronización de almacenamiento, y para aprobar, rechazar y quitar conexiones de punto de conexión privado. Para configurar la directiva de acceso a la red y las conexiones de punto de conexión privado, abra el portal del Servicio de sincronización de almacenamiento, vaya a la sección Configuración y haga clic en Red.
 
- Compatibilidad de nube por niveles con tamaños de clúster de volumen mayores que 64 KiB
    - La nube por niveles ahora admite tamaños de clúster de volumen de hasta 2 MiB en Server 2019. Para obtener más información, consulte [¿Cuál es el tamaño de archivo mínimo para organizar un archivo en niveles?](./file-sync-choose-cloud-tiering-policies.md#minimum-file-size-for-a-file-to-tier)
 
- Medición del ancho de banda y la latencia del servicio Azure File Sync y la cuenta de almacenamiento
    - El cmdlet Test-StorageSyncNetworkConnectivity ahora se puede usar para medir la latencia y el ancho de banda para el servicio Azure File Sync y la cuenta de almacenamiento. La latencia para el servicio Azure File Sync y la cuenta de almacenamiento se mide de manera predeterminada al ejecutar el cmdlet.  El ancho de banda de carga y descarga en la cuenta de almacenamiento se mide cuando se usa el parámetro "-MeasureBandwidth".
 
        Por ejemplo, para medir el ancho de banda y la latencia del servicio Azure File Sync y de la cuenta de almacenamiento, ejecute los siguientes comandos de PowerShell:
 
        ```powershell
        Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
        Test-StorageSyncNetworkConnectivity -MeasureBandwidth 
        ``` 
 
- Mejora en los mensajes de error en el portal cuando se produce un error en la creación del punto de conexión
    - Hemos prestado atención a los comentarios y hemos mejorado los mensajes de error y las instrucciones cuando se produce un error en la creación de un punto de conexión del servidor.
 
- Mejoras varias de rendimiento y confiabilidad
    - Se mejoró el rendimiento de la detección de cambios para detectar archivos que han cambiado en el recurso compartido de archivos de Azure.
    - Se mejoró el rendimiento para las sesiones de sincronización de conciliación. 
    - Se mejoró la sincronización para reducir los errores de ECS_E_SYNC_METADATA_KNOWLEDGE_SOFT_LIMIT_REACHED y ECS_E_SYNC_METADATA_KNOWLEDGE_LIMIT_REACHED.
    - Se corrigió un error que provoca datos dañados si la nube por niveles está habilitada y los archivos en capas se copian con Robocopy con el parámetro /B.
    - Se corrigió un error que puede provocar errores en el nivel de los archivos en Server 2019 si Desduplicación de datos está habilitada en el volumen.
    - Se corrigió un error que puede provocar que AFSDiag no pueda comprimir archivos si un archivo es mayor que 2 GiB.

### <a name="evaluation-tool"></a>Herramienta de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante la herramienta de evaluación de Azure File Sync. Esta herramienta es un cmdlet de Azure PowerShell que busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Para la instalación y las instrucciones de uso, consulte la sección [Herramienta de evaluación](file-sync-planning.md#evaluation-cmdlet) en la Guía de planeación. 

### <a name="agent-installation-and-server-configuration"></a>Instalación del agente y configuración del servidor
Para más información sobre cómo instalar y configurar el agente de Azure File Sync con Windows Server, vea [Planeamiento de una implementación de Azure File Sync](file-sync-planning.md) y [How to deploy Azure File Sync](file-sync-deployment-guide.md) (Implementación de Azure Files Sync).

- Se requiere un reinicio para los servidores que tienen una instalación existente del agente de Azure File Sync.
- El paquete de instalación del agente debe instalarse con permisos elevados (administrador).
- El agente no es compatible con la opción de implementación de Nano Server.
- El agente solo se admite en Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2.
- El agente requiere al menos 2 GB de memoria. Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina virtual debe configurarse con un mínimo de 2048 MiB de memoria. Para más información, consulte el artículo en el que se indican los [recursos del sistema recomendados](file-sync-planning.md#recommended-system-resources).
- El servicio del agente de sincronización de Storage (FileSyncSvc) no admite puntos de conexión de servidor ubicados en un volumen que tiene el directorio de información del volumen del sistema (SVI) comprimido. Esta configuración producirá resultados inesperados.

### <a name="interoperability"></a>Interoperabilidad
- Las aplicaciones antivirus, de copia de seguridad y de otro tipo que accedan a archivos organizados en niveles pueden provocar recuperaciones no deseadas, salvo que respeten el atributo sin conexión y pasen por alto la lectura del contenido de esos archivos. Para más información, vea [Solución de problemas de Azure File Sync](file-sync-troubleshoot.md).
- Los filtros de archivos del Administrador de recursos del servidor de archivos (FSRM) pueden provocar un gran número de errores de sincronización cuando los archivos están bloqueados debido al filtro de archivos.
- No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y esto puede provocar resultados inesperados. El agente de Azure File Sync se debe instalar después de implementar la imagen del servidor y completar la instalación mínima de la herramienta de preparación del sistema.

### <a name="sync-limitations"></a>Limitaciones de la sincronización
Los siguientes elementos no se sincronizan, pero el resto del sistema funciona con normalidad:
- Archivos con caracteres no admitidos. Consulte la [Guía de solución de problemas](file-sync-troubleshoot.md#handling-unsupported-characters) para obtener la lista de caracteres no admitidos.
- Archivos o directorios que terminan con un punto.
- Rutas de acceso de más de 2,048 caracteres.
- La parte de la lista de control de acceso del sistema (SACL) de un descriptor de seguridad que se utiliza para la auditoría.
- Atributos extendidos.
- Flujos de datos alternativos.
- Puntos de repetición de análisis.
- Vínculos físicos.
- La compresión (si se establece en un archivo de servidor) no se conserva al sincronizarse los cambios con ese archivo desde otros puntos de conexión.
- Los archivos cifrados con EFS (u otros cifrados con modo de usuario) que evitan que el servicio lea los datos.

    > [!Note]  
    > Azure File Sync siempre cifra los datos en tránsito. Los datos siempre se cifran en reposo en Azure.
 
### <a name="server-endpoint"></a>Punto de conexión de servidor
- Un punto de conexión de servidor solo se puede crear en un volumen NTFS. En este momento Azure File Sync no admite ReFS, FAT, FAT32 ni otros sistemas de archivos.
- Los niveles de nube no se admiten en el volumen del sistema. Para crear un punto de conexión de servidor en el volumen del sistema, deshabilite los niveles de la nube al crear el punto de conexión de servidor.
- Los clústeres de conmutación por error solo son compatibles con discos en clúster, pero no con volúmenes compartidos de clúster (CSV).
- No se puede anidar un punto de conexión de servidor. Puede coexistir en el mismo volumen en paralelo con otro punto de conexión.
- No almacene un archivo de paginación de aplicación o sistema operativo en una ubicación de punto de conexión de servidor.
- Si se cambia el nombre del servidor, no se actualiza en el portal.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
- Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión de nube una vez cada 24 horas. Para sincronizar inmediatamente los archivos que se modifican en el recurso compartido de archivos de Azure, se puede usar el cmdlet [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) de PowerShell para iniciar de forma manual la detección de cambios en el recurso compartido. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización.
- El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a un grupo de recursos, suscripción o inquilino de Azure AD diferentes. Después de mover el servicio de sincronización de almacenamiento o la cuenta de almacenamiento, debe dar acceso a la aplicación Microsoft.StorageSync a la cuenta de almacenamiento (consulte [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](file-sync-troubleshoot.md?tabs=portal1%252cportal#troubleshoot-rbac)).

    > [!Note]  
    > Al crear el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento deben estar en el mismo inquilino de Azure AD. Una vez creado el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento se pueden migrar a distintos inquilinos de Azure AD.

### <a name="cloud-tiering"></a>Niveles de nube
- Si un archivo en niveles se copia en otra ubicación mediante el uso de Robocopy, el archivo resultante no estará en niveles. El atributo sin conexión podría estar establecido porque Robocopy incluye incorrectamente dicho atributo en las operaciones de copia.
- Al copiar los archivos mediante Robocopy, utilice la opción /MIR para conservar las marcas de tiempo de archivo. Esto garantizará que los archivos antiguos se organicen por niveles antes que los archivos a los que se ha accedido recientemente.

## <a name="agent-version-11300"></a>Versión 11.3.0.0 del agente
Las notas de la versión siguientes corresponden a la versión 11.3.0.0 del agente de Azure File Sync publicada el 7 de abril de 2021. Estas notas se suman a las notas de la versión enumeradas para la versión 11.1.0.0.

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado 
Se corrigió un error que provoca datos dañados si la nube por niveles está habilitada y los archivos en capas se copian con Robocopy con el parámetro /B.

## <a name="agent-version-11200"></a>Versión 11.2.0.0 del agente
Las notas siguientes son para la versión 11.2.0.0 del agente de Azure File Sync publicada el 2 de febrero de 2021. Estas notas se suman a las notas de la versión enumeradas para la versión 11.1.0.0.

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado 
- Si se cancela una sesión de sincronización debido a un gran número de errores por elemento, la sincronización puede pasar por la reconciliación cuando se inicia una nueva sesión si el servicio Azure File Sync determina que se necesita una sesión de sincronización personalizada para corregir los errores por elemento.
- El registro de un servidor mediante el cmdlet Register-AzStorageSyncServer puede producir el error "Excepción no controlada".
- Nuevo cmdlet de PowerShell (Add-StorageSyncAllowedServerEndpointPath) para configurar las rutas de acceso de los puntos de conexión de servidor permitidas en un servidor. Este cmdlet es útil en escenarios en los que un Proveedor de soluciones en la nube (CSP) o un proveedor de servicios administra la implementación de Azure File Sync y el cliente desea configurar las rutas de acceso de los puntos de conexión de servidor permitidas en un servidor. Al crear un punto de conexión de servidor, si la ruta de acceso especificada no está en la lista de permitidos, se producirá un error en la creación del punto de conexión del servidor. Tenga en cuenta que se trata de una característica opcional y que, de forma predeterminada, se admiten todas las rutas de acceso al crear un punto de conexión de servidor.  

    
    - Para agregar una ruta de acceso de punto de conexión de servidor que esté permitida, ejecute los siguientes comandos de PowerShell en el servidor:

    ```powershell
    Import-Module 'C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll' -verbose
    Add-StorageSyncAllowedServerEndpointPath -Path <path>
    ```  

    - Para obtener la lista de rutas de acceso admitidas, ejecute el siguiente comando de PowerShell:
    
    ```powershell
    Get-StorageSyncAllowedServerEndpointPath
    ```     
    - Para quitar una ruta de acceso, ejecute el siguiente comando de PowerShell:
    
    ```powershell
    Remove-StorageSyncAllowedServerEndpointPath -Path <path>
    ```  
## <a name="agent-version-11100"></a>Versión 11.1.0.0 del agente
Las notas siguientes corresponden a la versión 11.1.0.0 del agente de Azure File Sync (publicada el 4 de noviembre de 2020).

### <a name="improvements-and-issues-that-are-fixed"></a>Mejoras y problemas que se han solucionado
- Nuevos modos de nube por niveles para controlar la descarga inicial y la recuperación proactiva
    - Modo de descarga inicial: ahora puede elegir cómo desea que los archivos se descarguen inicialmente en el nuevo punto de conexión de servidor. ¿Quiere todos sus archivos por niveles o la mayor cantidad posible de archivos descargados en su servidor con la última marca de tiempo modificada? ¡Puede hacerlo! ¿No puede usar la nube por niveles? Ahora puede optar por evitar los archivos por niveles en el sistema. Para obtener más información, consulte la sección [Creación de un punto de conexión de servidor](../file-sync/file-sync-deployment-guide.md?tabs=azure-portal%252cproactive-portal#create-a-server-endpoint) en la documentación de implementación de Azure File Sync.
    - Modo de recuperación proactiva: cada vez que se crea o se modifica un archivo, puede recuperarlo de forma proactiva en los servidores que especifique en el mismo grupo de sincronización. Esto hace que el archivo esté fácilmente disponible para su consumo en cada servidor que haya especificado. ¿Tiene equipos por todo el mundo trabajando con los mismos datos? Habilite la recuperación proactiva para que cuando el equipo llegue por la mañana, todos los archivos actualizados por un equipo en una zona horaria diferente se descarguen y estén listos para trabajar con ellos. Para más información, consulte la sección [Recuperación proactiva de archivos nuevos y modificados desde un recurso compartido de archivos de Azure](../file-sync/file-sync-deployment-guide.md?tabs=azure-portal%252cproactive-portal#proactively-recall-new-and-changed-files-from-an-azure-file-share) en la documentación de implementación de Azure File Sync.

- Excluir aplicaciones del seguimiento de la hora del último acceso de la nube por niveles Ahora puede excluir aplicaciones del seguimiento de la hora del último acceso. Cuando una aplicación tiene acceso a un archivo, la hora de último acceso para el archivo se actualiza en la base de datos de la nube por niveles. Las aplicaciones que examinan el sistema de archivos, como un antivirus, provocan que todos los archivos tengan la misma hora de último acceso, lo que afecta al momento en que los archivos se dividen en niveles.

    Para excluir las aplicaciones del seguimiento de la hora de último acceso, agregue el nombre del proceso al valor del Registro HeatTrackingProcessNameExclusionList, que se encuentra en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync.

    Ejemplo: reg ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync" /v HeatTrackingProcessNameExclusionList /t REG_MULTI_SZ /d "SampleApp.exe\0AnotherApp.exe" /f

    > [!Note]  
    > Los procesos Desduplicación de datos y Administrador de recursos del servidor de archivos (FSRM) se excluyen de forma predeterminada (codificación rígida) y la lista de exclusión de procesos se actualiza cada 5 minutos.

- Mejoras varias de rendimiento y confiabilidad
    - Se mejoró el rendimiento de la detección de cambios para detectar archivos que han cambiado en el recurso compartido de archivos de Azure.
    - Se mejoró el rendimiento de carga de sincronización.
    - La carga inicial se realiza ahora desde una instantánea de VSS, lo que reduce los errores por elemento y los errores de sesión de sincronización.
    - Se mejoró la confiabilidad de la sincronización para determinados patrones de E/S.
    - Se corrigió un error para evitar que la base de datos de sincronización retroceda en el tiempo en los clústeres de conmutación por error cuando se produce una conmutación por error.
    - Se mejoró el rendimiento de recuperación al obtener acceso a un archivo por niveles.

### <a name="evaluation-tool"></a>Herramienta de evaluación
Antes de implementar Azure File Sync, debe evaluar si es compatible con el sistema mediante la herramienta de evaluación de Azure File Sync. Esta herramienta es un cmdlet de Azure PowerShell que busca posibles problemas con el sistema de archivos y el conjunto de datos, tales como caracteres no admitidos o una versión de sistema operativo no compatible. Para la instalación y las instrucciones de uso, consulte la sección [Herramienta de evaluación](../file-sync/file-sync-planning.md#evaluation-cmdlet) en la Guía de planeación. 

### <a name="agent-installation-and-server-configuration"></a>Instalación del agente y configuración del servidor
Para más información sobre cómo instalar y configurar el agente de Azure File Sync con Windows Server, vea [Planeamiento de una implementación de Azure File Sync](../file-sync/file-sync-planning.md) y [How to deploy Azure File Sync](../file-sync/file-sync-deployment-guide.md) (Implementación de Azure Files Sync).

- El paquete de instalación del agente debe instalarse con permisos elevados (administrador).
- El agente no es compatible con la opción de implementación de Nano Server.
- El agente solo se admite en Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2.
- El agente requiere al menos 2 GB de memoria. Si el servidor se ejecuta en una máquina virtual con la memoria dinámica habilitada, la máquina virtual debe configurarse con un mínimo de 2048 MiB de memoria. Para más información, consulte el artículo en el que se indican los [recursos del sistema recomendados](../file-sync/file-sync-planning.md#recommended-system-resources).
- El servicio del agente de sincronización de Storage (FileSyncSvc) no admite puntos de conexión de servidor ubicados en un volumen que tiene el directorio de información del volumen del sistema (SVI) comprimido. Esta configuración producirá resultados inesperados.

### <a name="interoperability"></a>Interoperabilidad
- Las aplicaciones antivirus, de copia de seguridad y de otro tipo que accedan a archivos organizados en niveles pueden provocar recuperaciones no deseadas, salvo que respeten el atributo sin conexión y pasen por alto la lectura del contenido de esos archivos. Para más información, vea [Solución de problemas de Azure File Sync](../file-sync/file-sync-troubleshoot.md).
- Los filtros de archivos del Administrador de recursos del servidor de archivos (FSRM) pueden provocar un gran número de errores de sincronización cuando los archivos están bloqueados debido al filtro de archivos.
- No se admite la ejecución de sysprep en un servidor que tenga instalado el agente de Azure File Sync y esto puede provocar resultados inesperados. El agente de Azure File Sync se debe instalar después de implementar la imagen del servidor y completar la instalación mínima de la herramienta de preparación del sistema.

### <a name="sync-limitations"></a>Limitaciones de la sincronización
Los siguientes elementos no se sincronizan, pero el resto del sistema funciona con normalidad:
- Archivos con caracteres no admitidos. Consulte la [Guía de solución de problemas](../file-sync/file-sync-troubleshoot.md#handling-unsupported-characters) para obtener la lista de caracteres no admitidos.
- Archivos o directorios que terminan con un punto.
- Rutas de acceso de más de 2,048 caracteres.
- La parte de la lista de control de acceso del sistema (SACL) de un descriptor de seguridad que se utiliza para la auditoría.
- Atributos extendidos.
- Flujos de datos alternativos.
- Puntos de repetición de análisis.
- Vínculos físicos.
- La compresión (si se establece en un archivo de servidor) no se conserva al sincronizarse los cambios con ese archivo desde otros puntos de conexión.
- Los archivos cifrados con EFS (u otros cifrados con modo de usuario) que evitan que el servicio lea los datos.

    > [!Note]  
    > Azure File Sync siempre cifra los datos en tránsito. Los datos siempre se cifran en reposo en Azure.
 
### <a name="server-endpoint"></a>Punto de conexión de servidor
- Un punto de conexión de servidor solo se puede crear en un volumen NTFS. En este momento Azure File Sync no admite ReFS, FAT, FAT32 ni otros sistemas de archivos.
- Los niveles de nube no se admiten en el volumen del sistema. Para crear un punto de conexión de servidor en el volumen del sistema, deshabilite los niveles de la nube al crear el punto de conexión de servidor.
- Los clústeres de conmutación por error solo son compatibles con discos en clúster, pero no con volúmenes compartidos de clúster (CSV).
- No se puede anidar un punto de conexión de servidor. Puede coexistir en el mismo volumen en paralelo con otro punto de conexión.
- No almacene un archivo de paginación de aplicación o sistema operativo en una ubicación de punto de conexión de servidor.
- Si se cambia el nombre del servidor, no se actualiza en el portal.

### <a name="cloud-endpoint"></a>Punto de conexión de nube
- Azure File Sync admite realizar cambios directamente en el recurso compartido de archivos de Azure. Sin embargo, los cambios realizados en el recurso compartido de archivos de Azure primero deben ser detectados por un trabajo de detección de cambios de Azure File Sync. Se inicia un trabajo de detección de cambios para un punto de conexión de nube una vez cada 24 horas. Para sincronizar inmediatamente los archivos que se modifican en el recurso compartido de archivos de Azure, se puede usar el cmdlet [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) de PowerShell para iniciar de forma manual la detección de cambios en el recurso compartido. Además, los cambios realizados en un recurso compartido de archivos de Azure a través del protocolo de REST no actualizarán la hora de la última modificación de SMB y no se verán como cambios derivados de la sincronización.
- El servicio de sincronización del almacenamiento o la cuenta de almacenamiento se pueden mover a un grupo de recursos, suscripción o inquilino de Azure AD diferentes. Después de mover el servicio de sincronización de almacenamiento o la cuenta de almacenamiento, debe dar acceso a la aplicación Microsoft.StorageSync a la cuenta de almacenamiento (consulte [Asegúrese de que Azure File Sync tiene acceso a la cuenta de almacenamiento](../file-sync/file-sync-troubleshoot.md?tabs=portal1%252cportal#troubleshoot-rbac)).

    > [!Note]  
    > Al crear el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento deben estar en el mismo inquilino de Azure AD. Una vez creado el punto de conexión de nube, el servicio de sincronización de almacenamiento y la cuenta de almacenamiento se pueden migrar a distintos inquilinos de Azure AD.

### <a name="cloud-tiering"></a>Niveles de nube
- Si un archivo en niveles se copia en otra ubicación mediante el uso de Robocopy, el archivo resultante no estará en niveles. El atributo sin conexión podría estar establecido porque Robocopy incluye incorrectamente dicho atributo en las operaciones de copia.
- Al copiar los archivos mediante Robocopy, utilice la opción /MIR para conservar las marcas de tiempo de archivo. Esto garantizará que los archivos antiguos se organicen por niveles antes que los archivos a los que se ha accedido recientemente.
    > [!Warning]  
    > El modificador Robocopy/B no es compatible con Azure File Sync. El uso del modificador Robocopy/B con un punto de conexión del servidor de Azure File Sync como origen puede provocar daños en los archivos.