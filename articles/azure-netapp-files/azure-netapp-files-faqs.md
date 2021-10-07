---
title: Preguntas más frecuentes acerca de Azure NetApp Files | Microsoft Docs
description: Revise las preguntas más frecuentes sobre Azure NetApp Files, como redes, seguridad, rendimiento, administración de capacidad y migración o protección de datos.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/27/2021
ms.author: b-juche
ms.openlocfilehash: 01a483e43429f45562cbb464e2b595023bd2ad5f
ms.sourcegitcommit: 61e7a030463debf6ea614c7ad32f7f0a680f902d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2021
ms.locfileid: "129091034"
---
# <a name="faqs-about-azure-netapp-files"></a>Preguntas más frecuentes acerca de Azure NetApp Files

En este artículo se responden algunas preguntas frecuentes sobre Azure NetApp Files. 

## <a name="networking-faqs"></a>Preguntas frecuentes sobre redes

### <a name="does-the-data-path-for-nfs-or-smb-go-over-the-internet"></a>¿La ruta de acceso de datos de NFS o SMB pasa a través de Internet?  

No. La ruta de acceso de datos de NFS o SMB no pasa a través de Internet. Azure NetApp Files es un servicio nativo de Azure que está implementado en Azure Virtual Network (VNet) en donde el servicio está disponible. Azure NetApp Files usa una subred delegada y aprovisiona una interfaz de red directamente en la red virtual (VNet). 

Consulte [Directrices para el planeamiento de red de Azure NetApp Files](./azure-netapp-files-network-topologies.md) para una información más detallada.  

### <a name="can-i-connect-a-vnet-that-i-already-created-to-the-azure-netapp-files-service"></a>¿Puedo conectar una red virtual que ya haya creado con el servicio Azure NetApp Files?

Sí, puede conectar redes virtuales que haya creado al servicio. 

Consulte [Directrices para el planeamiento de red de Azure NetApp Files](./azure-netapp-files-network-topologies.md) para una información más detallada.  

### <a name="can-i-mount-an-nfs-volume-of-azure-netapp-files-using-dns-fqdn-name"></a>¿Puedo montar un volumen NFS de Azure NetApp Files mediante el nombre FQDN de DNS?

Sí que puede, si crea las entradas DNS necesarias. Azure NetApp Files proporciona la dirección IP de servicio para el volumen aprovisionado. 

> [!NOTE] 
> Azure NetApp Files puede implementar direcciones IP adicionales para el servicio según sea necesario.  Puede que sea necesario actualizar las entradas DNS periódicamente.

### <a name="can-i-set-or-select-my-own-ip-address-for-an-azure-netapp-files-volume"></a>¿Puedo establecer o seleccionar mi propia dirección IP para un volumen de Azure NetApp Files?  

No. La asignación de direcciones IP a volúmenes de Azure NetApp Files es dinámica. No se admiten las asignaciones de IP estáticas. 

### <a name="does-azure-netapp-files-support-dual-stack-ipv4-and-ipv6-vnet"></a>¿Admite Azure NetApp Files la red virtual de pila dual (IPv4 e IPv6)?

No, actualmente Azure NetApp Files no admite la red virtual de pila dual (IPv4 e IPv6).  

### <a name="is-the-number-of-the-ip-addresses-using-azure-vmware-solutions-for-guest-os-mounts-limited-to-1000"></a>¿Está [limitado a 1000](azure-netapp-files-resource-limits.md#resource-limits) el número de direcciones IP que usan Azure VMware Solutions para los montajes de sistemas operativos invitados?

No. Azure VMware Solutions está detrás de una puerta de enlace de ER, lo que hace que se comporte de forma similar a los sistemas locales. El número de "hosts" e "invitados" de AVS no es visible para Azure NetApp Files, y el límite de 1000 direcciones IP no es aplicable.
 
## <a name="security-faqs"></a>Preguntas más frecuentes de seguridad

### <a name="can-the-network-traffic-between-the-azure-vm-and-the-storage-be-encrypted"></a>¿Se puede cifrar el tráfico de red entre la máquina virtual de Azure y el almacenamiento?

El tráfico de datos de Azure NetApp Files es seguro por diseño, porque no proporciona ningún punto de conexión público y el tráfico de datos permanece dentro de la red virtual propiedad del cliente. Los datos en tránsito no se cifran de forma predeterminada. Sin embargo, el tráfico de datos desde una máquina virtual de Azure (que ejecuta un cliente SMB o NFS) a Azure NetApp Files es tan seguro como cualquier otro tráfico de máquina virtual a máquina virtual de Azure. 

El protocolo NFSv3 no admite cifrado, por lo que estos datos en tránsito no se pueden cifrar. Sin embargo, existe la opción de habilitar el cifrado de datos en tránsito de NFSv4.1 y SMB3. El tráfico de datos entre los clientes de NFSv 4.1 y los volúmenes de Azure NetApp Files se puede cifrar mediante Kerberos con el cifrado AES-256. Para más información, consulte [Configuración del cifrado Kerberos de NFSv4.1 para Azure NetApp Files](configure-kerberos-encryption.md). El tráfico de datos entre los clientes de SMB3 y los volúmenes de Azure NetApp Files se puede cifrar mediante el algoritmo AES-CCM en SMB 3.0 y el algoritmo AES-GCM en conexiones SMB 3.1.1. Para más información, consulte [Creación de un volumen SMB para Azure NetApp Files](azure-netapp-files-create-volumes-smb.md). 

### <a name="can-the-storage-be-encrypted-at-rest"></a>¿Se puede cifrar el almacenamiento en reposo?

Todos los volúmenes de Azure NetApp Files se cifran mediante el estándar FIPS 140-2. El servicio Azure NetApp Files administra todas las claves. 

### <a name="is-azure-netapp-files-cross-region-replication-traffic-encrypted"></a>¿El tráfico de replicación entre regiones de Azure NetApp Files está cifrado?

La replicación entre regiones de Azure NetApp Files usa cifrado TLS 1.2 AES-256 GCM para cifrar todos los datos transferidos entre el volumen de origen y el de destino. Este cifrado es adicional al [cifrado MACSec de Azure](../security/fundamentals/encryption-overview.md) que está activado de manera predeterminada para todo el tráfico de Azure, incluida la replicación entre regiones de Azure NetApp Files. 

### <a name="how-are-encryption-keys-managed"></a>¿Cómo se administran las claves de cifrado? 

La administración de claves para Azure NetApp Files se controla mediante el servicio. Se genera una clave de cifrado de datos XTS-AES-256 única para cada volumen. Una jerarquía de claves de cifrado se usa para cifrar y proteger todas las claves de volumen. Estas claves de cifrado nunca se muestran ni se notifican en un formato sin cifrar. Las claves de cifrado se eliminan inmediatamente cuando se suprime un volumen.

La compatibilidad con las claves administradas por el cliente (Bring Your Own Key) mediante Azure Dedicated HSM está disponible de manera controlada en las regiones Este de EE. UU., Centro-sur de EE. UU., Oeste de EE. UU 2. y Centro y US Gov Virginia. Puede solicitar acceso en [anffeedback@microsoft.com](mailto:anffeedback@microsoft.com) . Las solicitudes se aprueban a medida que la capacidad va estando disponible.

### <a name="can-i-configure-the-nfs-export-policy-rules-to-control-access-to-the-azure-netapp-files-service-mount-target"></a>¿Puedo configurar las reglas de directivas de exportación NFS para controlar el acceso al destino de montaje del servicio Azure NetApp Files?

Sí, puede configurar hasta cinco reglas en una sola directiva de exportación NFS.

### <a name="does-azure-netapp-files-support-network-security-groups"></a>¿Azure NetApp Files admite grupos de seguridad de red?

No, actualmente no se pueden aplicar grupos de seguridad de red a la subred delegada de Azure NetApp Files o las interfaces de red creadas por el servicio.

### <a name="can-i-use-azure-rbac-with-azure-netapp-files"></a>¿Puedo usar RBAC de Azure con Azure NetApp Files?

Sí, Azure NetApp Files admite las características de RBAC de Azure. Junto con los roles de Azure integrados, puede [crear roles personalizados](../role-based-access-control/custom-roles.md) para Azure NetApp Files. 

Para obtener una lista completa de los permisos de Azure NetApp Files, consulte las operaciones del proveedor de recursos de Azure para [`Microsoft.NetApp`](../role-based-access-control/resource-provider-operations.md#microsoftnetapp).

### <a name="are-azure-activity-logs-supported-on-azure-netapp-files"></a>¿Se admiten los registros de actividad de Azure en Azure NetApp Files?

Azure NetApp Files es un servicio nativo de Azure. Todas las API PUT, POST y DELETE se registran en Azure NetApp Files. Por ejemplo, los registros muestran actividades como quién creó la instantánea, quién modificó el volumen, etc.

Para obtener una lista completa de las operaciones de API, consulte [API REST de Azure NetApp Files](/rest/api/netapp/).

### <a name="can-i-use-azure-policies-with-azure-netapp-files"></a>¿Puedo usar directivas de Azure con Azure NetApp Files?

Sí, puede crear [directivas de Azure personalizadas](../governance/policy/tutorials/create-custom-policy-definition.md). 

Sin embargo, no puede crear directivas de Azure (directivas de nomenclatura personalizadas) en la interfaz de Azure NetApp Files. Consulte [Instrucciones para el planeamiento de red de Azure NetApp Files](azure-netapp-files-network-topologies.md#considerations).

### <a name="when-i-delete-an-azure-netapp-files-volume-is-the-data-deleted-safely"></a>Cuando elimino un volumen de Azure NetApp Files, ¿los datos se eliminan de forma segura? 

La eliminación de un volumen de Azure NetApp Files se realiza mediante programación con efecto inmediato. La operación de eliminación incluye la eliminación de las claves usadas para cifrar datos en reposo. No hay ningún escenario que permita la recuperación de un volumen eliminado una vez que la operación de eliminación se ejecuta correctamente (mediante interfaces como Azure Portal y la API).

## <a name="performance-faqs"></a>Preguntas más frecuentes sobre rendimiento

### <a name="what-should-i-do-to-optimize-or-tune-azure-netapp-files-performance"></a>¿Qué debo hacer para optimizar o ajustar el rendimiento de Azure NetApp Files?

Puede realizar las siguientes acciones según los requisitos de rendimiento: 
- Asegúrese de que la máquina virtual tiene el tamaño apropiado.
- Habilite redes aceleradas para la máquina virtual.
- Seleccione el nivel de servicio deseado y el tamaño de grupo de capacidad.
- Cree un volumen con el tamaño de cuota deseada para la capacidad y el rendimiento.

### <a name="how-do-i-convert-throughput-based-service-levels-of-azure-netapp-files-to-iops"></a>¿Cómo puedo convertir los niveles de servicio basado en el rendimiento de Azure NetApp Files a IOPS?

Puede convertir MB/s a IOPS mediante el uso de la siguiente fórmula:  

`IOPS = (MBps Throughput / KB per IO) * 1024`

### <a name="how-do-i-change-the-service-level-of-a-volume"></a>¿Cómo puedo cambiar el nivel de servicio de un volumen?

Puede cambiar el nivel de servicio de un volumen existente al moverlo a otro grupo de capacidad que use el [nivel de servicio](azure-netapp-files-service-levels.md) que quiere para dicho volumen. Consulte [Cambio dinámico del nivel de servicio de un volumen](dynamic-change-volume-service-level.md). 

### <a name="how-do-i-monitor-azure-netapp-files-performance"></a>¿Cómo se puede supervisar el rendimiento de Azure NetApp Files?

Azure NetApp Files proporciona métricas de rendimiento del volumen. También puede usar Azure Monitor para supervisar las métricas de uso de Azure NetApp Files.  Consulte [Métricas de Azure NetApp Files](azure-netapp-files-metrics.md) para obtener la lista de métricas de rendimiento de Azure NetApp Files.

### <a name="whats-the-performance-impact-of-kerberos-on-nfsv41"></a>¿Cuál es el impacto en el rendimiento de Kerberos en NFSv4.1?

Consulte [Impacto en el rendimiento de Kerberos en los volúmenes de NFSv4.1](performance-impact-kerberos.md) para obtener información sobre las opciones de seguridad de NFSv4.1, los vectores de rendimiento que se hayan probado y el impacto esperado en el rendimiento. 

### <a name="does-azure-netapp-files-support-smb-direct"></a>¿Azure NetApp Files admite SMB directo?

No, Azure NetApp Files no admite SMB directo. 

### <a name="is-nic-teaming-supported-in-azure"></a>¿Se admite la formación de equipos NIC en Azure?

La formación de equipos NIC no se admite en Azure. Aunque se admiten varias interfaces de red en las máquinas virtuales de Azure, representan una construcción lógica más que física. Como tal, no proporcionan tolerancia a errores.  Además, el ancho de banda disponible para una máquina virtual de Azure se calcula para el propio equipo y no para ninguna interfaz de red individual.

### <a name="are-jumbo-frames-supported"></a>¿Se admiten tramas gigantes?

No se admiten tramas gigantes con máquinas virtuales de Azure.

## <a name="nfs-faqs"></a>Preguntas más frecuentes sobre NFS

### <a name="i-want-to-have-a-volume-mounted-automatically-when-an-azure-vm-is-started-or-rebooted--how-do-i-configure-my-host-for-persistent-nfs-volumes"></a>Quiero tener un volumen montado automáticamente cuando se inicia o se reinicia una máquina virtual de Azure.  ¿Cómo tengo que configurar mi host para los volúmenes persistentes de NFS?

Para que un volumen NFS se monte automáticamente al inicio o reinicio de una máquina virtual, agregue una entrada al archivo `/etc/fstab` en el host. 

Consulte [Montaje o desmontaje de un volumen para máquinas virtuales Windows o Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) para consultar los detalles.  

### <a name="what-nfs-version-does-azure-netapp-files-support"></a>¿Qué versión de NFS admite Azure NetApp Files?

Azure NetApp Files admite NFSv3 y NFSv4.1. Se pueden [crear volúmenes](azure-netapp-files-create-volumes.md) con cualquier versión de NFS. 

### <a name="how-do-i-enable-root-squashing"></a>¿Cómo se puede habilitar root squashing?

El usuario puede especificar si la cuenta raíz tendrá o no acceso al volumen por medio de la directiva de exportación del volumen. Para más información, consulte [Configuración de la directiva de exportación para un volumen NFS](azure-netapp-files-configure-export-policy.md).

### <a name="can-i-use-the-same-file-path-volume-creation-token-for-multiple-volumes"></a>¿Puedo usar la misma ruta de acceso de archivo (token de creación de volumen) para varios volúmenes?

Sí, puede hacerlo. Sin embargo, la ruta de acceso de archivo debe usarse en una suscripción o región distinta.   

Por ejemplo, puede crear un volumen llamado denominado `vol1`. A continuación, crea otro volumen también llamado `vol1` en un grupo de capacidad distinto, pero en la misma suscripción y región. En este caso, el uso del mismo nombre de volumen `vol1` producirá un error. Para usar la misma ruta de acceso de archivo, el nombre debe estar en otra región o suscripción.

### <a name="when-i-try-to-access-nfs-volumes-through-a-windows-client-why-does-the-client-take-a-long-time-to-search-folders-and-subfolders"></a>Cuando intento acceder a volúmenes NFS a través de un cliente de Windows, ¿por qué el cliente tarda mucho tiempo en buscar carpetas y subcarpetas?

Asegúrese de que `CaseSensitiveLookup` está habilitado en el cliente de Windows para acelerar la búsqueda de carpetas y subcarpetas:

1. Use el siguiente comando de PowerShell para habilitar CaseSensitiveLookup:   
    `Set-NfsClientConfiguration -CaseSensitiveLookup 1`    
2. Monte el volumen en el servidor de Windows.   
    Ejemplo:   
    `Mount -o rsize=1024 -o wsize=1024 -o mtype=hard \\10.x.x.x\testvol X:*`

### <a name="how-does-azure-netapp-files-support-nfsv41-file-locking"></a>¿Cómo admite Azure NetApp Files el bloqueo de archivos de NFSv4.1? 

En el caso de los clientes de NFSv4.1, Azure NetApp Files admite el mecanismo de bloqueo de archivos de NFSv4.1 que mantiene el estado de todos los bloqueos de archivos en un modelo basado en concesión. 

Por RFC 3530, Azure NetApp Files define un período de concesión único para todos los estados de un cliente NFS. Si el cliente no renueva su concesión dentro del período definido, el servidor liberará todos los estados asociados a la concesión del cliente.  

Por ejemplo, si un cliente que monta un volumen deja de responder o se bloquea más allá de los tiempos de espera, se liberarán los bloqueos. El cliente puede renovar su concesión de forma explícita o implícita realizando operaciones como leer un archivo.   

Un período de gracia define un período de procesamiento especial en el que los clientes pueden intentar reclamar su estado de bloqueo durante una recuperación del servidor. El tiempo de espera predeterminado para las concesiones es de 30 segundos con un período de gracia de 45 segundos. Transcurrido ese tiempo, se liberará la concesión del cliente.   

## <a name="smb-faqs"></a>Preguntas más frecuentes de SMB

### <a name="which-smb-versions-are-supported-by-azure-netapp-files"></a>¿Qué versiones de SMB son compatibles con Azure NetApp Files?

Azure NetApp Files admite SMB 2.1 y SMB 3.1 (que incluye compatibilidad con SMB 3.0).    

### <a name="is-an-active-directory-connection-required-for-smb-access"></a>¿Es necesaria una conexión de Active Directory para el acceso SMB? 

Sí, tiene que crear una conexión de Active Directory antes de implementar un volumen de SMB. Para que la conexión tenga éxito la subred delegada de Azure NetApp Files tiene que tener acceso a los controladores de dominio especificados.  Consulte [Crear un volumen de SMB](./azure-netapp-files-create-volumes-smb.md) para más información. 

### <a name="how-many-active-directory-connections-are-supported"></a>¿Cuántas conexiones de Active Directory se admiten?

Solo puede configurar una conexión de Active Directory (AD) por suscripción y por región. Para más información, consulte [Requisitos para las conexiones de Active Directory](create-active-directory-connections.md#requirements-for-active-directory-connections). 

Sin embargo, puede asignar varias cuentas de NetApp que se encuentran en la misma suscripción y la misma región a un servidor de AD común creado en una de las cuentas de NetApp. Consulte [Asignación de varias cuentas de NetApp de la misma suscripción y región a una conexión de AD](create-active-directory-connections.md#shared_ad). 

### <a name="does-azure-netapp-files-support-azure-active-directory"></a>¿ Azure NetApp Files admite Azure Active Directory? 

Se admite tanto [Azure Active Directory (AD) Domain Services](../active-directory-domain-services/overview.md) como [Active Directory Domain Services (AD DS)](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview). Puede usar controladores de dominio de Active Directory existentes con Azure NetApp Files. Los controladores de dominio pueden residir en Azure como máquinas virtuales o en el entorno local mediante ExpressRoute o VPN S2S. En este momento, Azure NetApp Files no admite la unión a AD para [Azure Active Directory](https://azure.microsoft.com/resources/videos/azure-active-directory-overview/).

Si usa Azure NetApp Files con Azure Active Directory Domain Services, la ruta de acceso de la unidad organizativa es `OU=AADDC Computers` cuando configura Active Directory para su cuenta de NetApp.

### <a name="what-versions-of-windows-server-active-directory-are-supported"></a>¿Qué versiones de Windows Server Active Directory se admiten?

Azure NetApp Files admite las versiones 2008r2SP1-2019 de Windows Server de Active Directory Domain Services.

### <a name="im-having-issues-connecting-to-my-smb-share-what-should-i-do"></a>Tengo problemas para conectarme al recurso compartido de SMB. ¿Cuál debo hacer?

Como procedimiento recomendado, establezca la tolerancia máxima para la sincronización del reloj del equipo en cinco minutos. Para más información, consulte [Tolerancia máxima para la sincronización del reloj del equipo](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj852172(v=ws.11)). 

### <a name="can-i-manage-smb-shares-sessions-and-open-files-through-computer-management-console-mmc"></a>¿Puedo administrar `SMB Shares`, `Sessions` y `Open Files` a través de la consola de administración de equipos (MMC)?

Actualmente no se admite la administración de `SMB Shares`, `Sessions` y `Open Files` a través de la consola de administración de equipos (MMC).

### <a name="how-can-i-obtain-the-ip-address-of-an-smb-volume-via-the-portal"></a>¿Cómo puedo obtener la dirección IP de un volumen SMB a través del portal?

Use el vínculo de la **vista JSON** en el panel Información general del volumen y busque el identificador **startIP** en **properties** -> **mountTargets**.

### <a name="can-an-azure-netapp-files-smb-share-act-as-an-dfs-namespace-dfs-n-root"></a>¿Puede un recurso compartido de SMB de Azure NetApp Files actuar como raíz del espacio de nombres DFS (DFS-N)?

No. Sin embargo, los recursos compartidos de SMB de Azure NetApp Files pueden actuar como destino de la carpeta del espacio de nombres DFS (DFS-N).   
Para usar un recurso compartido de SMB de Azure NetApp Files como destino de la carpeta de DFS-N, proporcione la ruta de acceso de montaje de convención de nomenclatura universal (UNC) del recurso compartido de SMB de Azure NetApp Files mediante el procedimiento para [agregar destino de la carpeta DFS](/windows-server/storage/dfs-namespaces/add-folder-targets#to-add-a-folder-target).  

### <a name="can-the-smb-share-permissions-be-changed"></a>¿Se pueden cambiar los permisos del recurso compartido de SMB?   

No, los permisos del recurso compartido no se pueden cambiar. Pero sí se pueden cambiar los permisos NTFS del volumen `root` mediante el procedimiento para [permisos de carpetas y archivos NTFS](azure-netapp-files-create-volumes-smb.md#ntfs-file-and-folder-permissions). 

## <a name="capacity-management-faqs"></a>Preguntas más frecuentes sobre la administración de la capacidad

### <a name="how-do-i-monitor-usage-for-capacity-pool-and-volume-of-azure-netapp-files"></a>¿Cómo puedo supervisar el uso de grupo de capacidad y volumen de Azure NetApp Files? 

Azure NetApp Files proporciona métricas de uso de grupo de capacidad y volumen. También puede usar Azure Monitor para supervisar el uso de Azure NetApp Files. Consulte [Métricas de Azure NetApp Files](azure-netapp-files-metrics.md) para más información. 

### <a name="how-do-i-determine-if-a-directory-is-approaching-the-limit-size"></a>¿Cómo puedo determinar si un directorio está llegando al tamaño límite?

Puede usar el comando `stat` desde un cliente para ver si un directorio está llegando al [límite de tamaño máximo](azure-netapp-files-resource-limits.md#resource-limits) de los metadatos del directorio (320 MB).
Consulte [Límites de recursos para Azure NetApp Files](azure-netapp-files-resource-limits.md#directory-limit) para el límite y el cálculo. 

<!-- 
You can use the `stat` command from a client to see whether a directory is approaching the maximum size limit for directory metadata (320 MB).   

For a 320-MB directory, the number of blocks is 655360, with each block size being 512 bytes.  (That is, 320x1024x1024/512.)  This number translates to approximately 4 million files maximum for a 320-MB directory. However, the actual number of maximum files might be lower, depending on factors such as the number of files containing non-ASCII characters in the directory. As such, you should use the `stat` command as follows to determine whether your directory is approaching its limit.  

Examples:

```console
[makam@cycrh6rtp07 ~]$ stat bin
File: 'bin'
Size: 4096            Blocks: 8          IO Block: 65536  directory

[makam@cycrh6rtp07 ~]$ stat tmp
File: 'tmp'
Size: 12288           Blocks: 24         IO Block: 65536  directory
 
[makam@cycrh6rtp07 ~]$ stat tmp1
File: 'tmp1'
Size: 4096            Blocks: 8          IO Block: 65536  directory
```
--> 

### <a name="does-snapshot-space-count-towards-the-usable--provisioned-capacity-of-a-volume"></a>¿Se cuenta el espacio de instantáneas en la capacidad utilizable o aprovisionada de un volumen?

Sí, la [capacidad de instantáneas consumida](azure-netapp-files-cost-model.md#capacity-consumption-of-snapshots) cuenta para el espacio aprovisionado del volumen. En caso de que el volumen se llene, considere la posibilidad de tomar las siguientes medidas:

* [Cambie el tamaño del volumen](azure-netapp-files-resize-capacity-pools-or-volumes.md).
* [Quite las instantáneas más antiguas](snapshots-delete.md) para liberar espacio en el volumen de hospedaje. 

### <a name="does-azure-netapp-files-support-auto-grow-for-volumes-or-capacity-pools"></a>¿Azure NetApp Files admite el crecimiento automático en los volúmenes o grupos de capacidad?

No, los volúmenes y los grupos de capacidad de Azure NetApp Files no crecen automáticamente al llenarse. Vea [Modelo de costo de Azure NetApp Files](azure-netapp-files-cost-model.md).   

Puede usar la herramienta admitida por la comunidad [ANFCapacityManager de Logic Apps](https://github.com/ANFTechTeam/ANFCapacityManager) para administrar las reglas de alertas basadas en la capacidad. La herramienta puede aumentar automáticamente los tamaños de los volúmenes para evitar que se queden sin espacio.

### <a name="does-the-destination-volume-of-a-replication-count-towards-hard-volume-quota"></a>¿El volumen de destino de una replicación cuenta para la cuota de volumen estricta?  

No, el volumen de destino de una replicación no cuenta para la cuota de volumen estricta.

### <a name="can-i-manage-azure-netapp-files-through-azure-storage-explorer"></a>¿Puedo administrar Azure NetApp Files mediante Explorador de Azure Storage?

No. Explorador de Azure Storage no es compatible con Azure NetApp Files.

## <a name="data-migration-and-protection-faqs"></a>Preguntas frecuentes sobre migración y protección de datos

### <a name="how-do-i-migrate-data-to-azure-netapp-files"></a>¿Cómo se pueden migrar los datos a Azure NetApp Files?
Azure NetApp Files proporciona volúmenes SMB y NFS.  Puede usar cualquier herramienta de copia basada en archivos para migrar datos al servicio. 

NetApp ofrece una solución basada en SaaS, [NetApp Cloud Sync](https://cloud.netapp.com/cloud-sync-service).  La solución le permite replicar datos de NFS o SMB en los recursos compartidos de SMB o exportaciones de NFS de Azure NetApp Files. 

También puede utilizar una amplia gama de herramientas gratuitas para copiar los datos. Para NFS, puede usar las herramientas de cargas de trabajo, como [rsync](https://rsync.samba.org/examples.html) para copiar y sincronizar datos de origen en un volumen de Azure NetApp Files. Para SMB, puede usar las cargas de trabajo [robocopy](/windows-server/administration/windows-commands/robocopy) de la misma manera.  Estas herramientas también pueden replicar los permisos de archivo o carpeta. 

Los requisitos para la migración de datos desde un entorno local a Azure NetApp Files son los siguientes: 

- Asegúrese de que Azure NetApp Files está disponible en la región de Azure de destino.
- Valide la conectividad de red entre el origen y la dirección IP del volumen de destino de Azure NetApp Files. La transferencia de datos entre el entorno local y el servicio Azure NetApp Files se admite a través de ExpressRoute.
- Cree el volumen de destino de Azure NetApp Files.
- Transfiera los datos de origen al volumen de destino mediante la herramienta de copia de archivo que prefiera.

### <a name="where-does-azure-netapp-files-store-customer-data"></a>¿Dónde almacena Azure NetApp Files los datos de los clientes?   

De forma predeterminada, los datos permanecen dentro de la región donde se implementan los volúmenes de Azure NetApp Files. Sin embargo, puede optar por replicar los datos por volumen en las regiones de destino disponibles mediante la [replicación entre regiones](cross-region-replication-introduction.md).

### <a name="how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region"></a>¿Cómo puedo crear una copia de Azure NetApp Files en otra región de Azure?
    
Azure NetApp Files proporciona volúmenes SMB y NFS.  Cualquier herramienta de copia basada en archivos puede usarse para replicar datos entre regiones de Azure. 

La funcionalidad de [replicación entre regiones](cross-region-replication-introduction.md) permite replicar datos de forma asincrónica desde un volumen de Azure NetApp Files (origen) de una región en otro volumen de Azure NetApp Files (destino) de otra región.  Además, puede [crear un nuevo volumen mediante una instantánea de un volumen existente](snapshots-restore-new-volume.md).

NetApp ofrece una solución basada en SaaS, [NetApp Cloud Sync](https://cloud.netapp.com/cloud-sync-service).  La solución le permite replicar datos de NFS o SMB en los recursos compartidos de SMB o exportaciones de NFS de Azure NetApp Files. 

También puede utilizar una amplia gama de herramientas gratuitas para copiar los datos. Para NFS, puede usar las herramientas de cargas de trabajo, como [rsync](https://rsync.samba.org/examples.html) para copiar y sincronizar datos de origen en un volumen de Azure NetApp Files. Para SMB, puede usar las cargas de trabajo [robocopy](/windows-server/administration/windows-commands/robocopy) de la misma manera.  Estas herramientas también pueden replicar los permisos de archivo o carpeta. 

Los requisitos para la replicación de un volumen de Azure NetApp Files en otra región de Azure son los siguientes: 
- Asegúrese de que Azure NetApp Files está disponible en la región de Azure de destino.
- Valide la conectividad de red entre redes virtuales en cada región. Actualmente, no se admite el emparejamiento global entre redes virtuales.  Puede establecer la conectividad entre redes virtuales mediante la vinculación con un circuito de ExpressRoute o el uso de una conexión VPN de sitio a sitio. 
- Cree el volumen de destino de Azure NetApp Files.
- Transfiera los datos de origen al volumen de destino mediante la herramienta de copia de archivo que prefiera.

### <a name="is-migration-with-azure-data-box-supported"></a>¿Se admite la migración con Azure Data Box?

No. En la actualidad, Azure Data Box no es compatible con Azure NetApp Files. 

### <a name="is-migration-with-azure-importexport-service-supported"></a>¿Se admite la migración con el servicio Azure Import/Export?

No. En la actualidad, el servicio Azure Import/Export no es compatible con Azure NetApp Files.

## <a name="azure-netapp-files-backup-faqs"></a>Preguntas frecuentes sobre la copia de seguridad de archivos de Azure NetApp Files

En esta sección se responden preguntas sobre la característica de [copia de seguridad de Azure NetApp Files](backup-introduction.md). 

### <a name="when-do-my-backups-occur"></a>¿Cuándo se producen mis copias de seguridad?   

Las copias de seguridad de Azure NetApp Files se inician dentro de un período de tiempo aleatorio después de que se especifique la frecuencia de una directiva de copia de seguridad. Por ejemplo, las copias de seguridad semanales se inician el domingo dentro de un intervalo asignado aleatoriamente después de las 12:00 a.m., medianoche. Los usuarios no pueden modificar este intervalo en este momento. La copia de seguridad de la línea base se inicia en cuanto se asigna la directiva de copia de seguridad al volumen.

### <a name="what-happens-if-a-backup-operation-encounters-a-problem"></a>¿Qué ocurre si surge un problema en una operación de copia de seguridad?

Si se produce un problema durante una operación de copia de seguridad, la copia de seguridad de Azure NetApp Files reintenta la operación automáticamente, sin necesidad de la interacción del usuario. Si los reintentos siguen sin funcionar, la copia de seguridad de Azure NetApp Files informará del error de la operación.

### <a name="can-i-change-the-location-or-storage-tier-of-my-backup-vault"></a>¿Puedo cambiar la ubicación o el nivel de almacenamiento de mi almacén de copia de seguridad?

No, Azure NetApp Files administra automáticamente la ubicación de los datos de copia de seguridad en Azure Storage y el usuario no puede modificar esta ubicación ni el nivel de acceso de Azure.

### <a name="what-types-of-security-are-provided-for-the-backup-data"></a>¿Qué tipos de seguridad se proporcionan para los datos de copia de seguridad?

Azure NetApp Files el cifrado AES de 256 bits durante la codificación de los datos de copia de seguridad recibidos. Además, los datos cifrados se transmiten de forma segura a Azure Storage mediante conexiones HTTPS TLSv1.2. La copia de seguridad de Azure NetApp Files depende de la funcionalidad en reposo del cifrado integrado de la cuenta de Azure Storage para almacenar los datos de copia de seguridad.

### <a name="what-happens-to-the-backups-when-i-delete-a-volume-or-my-netapp-account"></a>¿Qué ocurre con las copias de seguridad cuando se elimina un volumen o una cuenta de NetApp? 

 Al eliminar un volumen de Azure NetApp Files, se conservan las copias de seguridad. Si no desea que se conserven las copias de seguridad, deshabilítelas antes de eliminar el volumen. Cuando se elimina una cuenta de NetApp, las copias de seguridad se conservan y se muestran en otras cuentas de NetApp de la misma suscripción, por lo que sigue estando disponible para la restauración. Si elimina todas las cuentas de NetApp de una suscripción, debe asegurarse de deshabilitar las copias de seguridad antes de eliminar todos los volúmenes de todas las cuentas de NetApp.

### <a name="whats-the-systems-maximum-backup-retries-for-a-volume"></a>¿Cuál es el número máximo de reintentos de copia de seguridad del sistema para un volumen?  

El sistema realiza diez reintentos al procesar un trabajo de copia de seguridad programado. Si se produce un error en el trabajo, el sistema genera un error en la operación de copia de seguridad. En el caso de las copias de seguridad programadas (basadas en la directiva configurada), el sistema intenta hacer una copia de seguridad de los datos una vez cada hora. Si hay nuevas instantáneas disponibles que no se han transferido (o no se han podido realizar en el último intento), esas instantáneas se considerarán para la transferencia. 

## <a name="product-faqs"></a>Preguntas más frecuentes sobre productos

### <a name="can-i-use-azure-netapp-files-nfs-or-smb-volumes-with-azure-vmware-solution-avs"></a>¿Puedo usar volúmenes NFS o SMB de Azure NetApp Files con Azure VMware Solution (AVS)?

Puede montar volúmenes NFS de Azure NetApp Files en máquinas virtuales Windows o Linux de AVS. Puede asignar recursos compartidos de SMB de Azure NetApp Files en máquinas virtuales Windows de AVS. Para obtener más información, consulte [Azure NetApp Files con Azure VMware Solution]( ../azure-vmware/netapp-files-with-azure-vmware-solution.md).  

### <a name="what-regions-are-supported-for-using-azure-netapp-files-nfs-or-smb-volumes-with-azure-vmware-solution-avs"></a>¿Qué regiones se admiten para usar volúmenes NFS o SMB de Azure NetApp Files con Azure VMware Solution (AVS)?

El uso de volúmenes NFS o SMB de Azure NetApp Files (ANF) con AVS para *montajes de sistemas operativos invitados* se admite en [todas las regiones habilitadas para AVS y ANF](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware,netapp).

### <a name="does-azure-netapp-files-work-with-azure-policy"></a>¿Azure NetApp Files funciona con Azure Policy?

Sí. Azure NetApp Files es un servicio de primera entidad. Se adhiere totalmente a los estándares del proveedor de recursos de Azure. Por lo tanto, Azure NetApp Files se puede integrar en Azure Policy a través de *definiciones de directiva personalizadas*. Para obtener información sobre cómo implementar directivas personalizadas para Azure NetApp Files, consulte [Azure Policy ya disponible para Azure NetApp Files](https://techcommunity.microsoft.com/t5/azure/azure-policy-now-available-for-azure-netapp-files/m-p/2282258) en la comunidad tecnológica de Microsoft. 

### <a name="which-unicode-character-encoding-is-supported-by-azure-netapp-files-for-the-creation-and-display-of-file-and-directory-names"></a>¿Qué codificación de caracteres Unicode admite Azure NetApp Files para la creación y la visualización de nombres de archivo y directorio?   

Azure NetApp Files solo admite nombres de archivo y directorio codificados con el formato de codificación de caracteres Unicode UTF-8 para volúmenes NFS y SMB.

Si intenta crear archivos o directorios con nombres que usan caracteres adicionales o pares suplentes, como caracteres no regulares y emojis que no son compatibles con UTF-8, se producirá un error en la operación. En este caso, un error de un cliente Windows podría ser: "El nombre de archivo especificado no es válido o es demasiado largo. Especifique otro nombre de archivo". 

## <a name="next-steps"></a>Pasos siguientes  

- [Preguntas más frecuentes acerca de Microsoft Azure ExpressRoute](../expressroute/expressroute-faqs.md)
- [Preguntas más frecuentes acerca de Azure Virtual Network](../virtual-network/virtual-networks-faq.md)
- [Creación de una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md)
- [Azure Data Box](../databox/index.yml)
- [Preguntas más frecuentes sobre el rendimiento de SMB para Azure NetApp Files](azure-netapp-files-smb-performance.md)
