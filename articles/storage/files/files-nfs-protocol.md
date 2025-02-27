---
title: Recursos compartidos de archivos NFS en Azure Files
description: Obtenga información sobre los recursos compartidos de archivos hospedados en Azure Files mediante el protocolo Network File System (NFS).
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 11/16/2021
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 31aec4e32178b2f0ca7905c6d584df298dd2059a
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132524348"
---
# <a name="nfs-file-shares-in-azure-files"></a>Recursos compartidos de archivos NFS en Azure Files
Azure Files ofrece dos protocolos estándar del sector para el montaje de recursos compartidos de archivos de Azure: el protocolo [Bloque de mensajes del servidor (SMB)](/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) y el protocolo [Network File System (NFS)](https://en.wikipedia.org/wiki/Network_File_System). Azure Files le permite seleccionar el protocolo del sistema de archivos más adecuado para su carga de trabajo. Los recursos compartidos de archivos de Azure no admiten el acceso a un recurso compartido de archivos de Azure individual con los protocolos SMB y NFS, aunque se pueden crear recursos compartidos de archivos SMB y NFS dentro de la misma cuenta de almacenamiento. En general, Azure Files ofrece recursos compartidos de archivos de nivel empresarial que se pueden escalar verticalmente para satisfacer sus necesidades de almacenamiento y a los que pueden acceder simultáneamente miles de clientes.

En este artículo se describen los recursos compartidos de archivos NFS de Azure. Para obtener información acerca de los recursos compartidos de archivos SMB de Azure, consulte [Recursos compartidos de archivos SMB en Azure Files](files-smb-protocol.md).

> [!IMPORTANT]
>  Antes de usar recursos compartidos de archivos NFS, consulte [Solución de problemas de recursos compartidos de archivos NFS de Azure](storage-troubleshooting-files-nfs.md) para ver una lista de problemas conocidos.

## <a name="common-scenarios"></a>Escenarios frecuentes
Los recursos compartidos de archivos NFS se suelen usar en los escenarios siguientes:

- Almacenamiento de respaldo para aplicaciones basadas en Linux o UNIX, como aplicaciones de línea de negocio escritas con API del sistema de archivos Linux o POSIX (incluso aunque no requieran compatibilidad con POSIX).
- Cargas de trabajo que requieren recursos compartidos de archivos compatibles con POSIX, distinción entre mayúsculas y minúsculas o permisos de estilo Unix (UID/GID).
- Desarrollo de aplicaciones y servicios nuevos, sobre todo si esa aplicación o servicio tiene el requisito de E/S aleatoria y almacenamiento jerárquico. 

## <a name="features"></a>Características
- Sistema de archivos totalmente compatible con POSIX.
- Compatibilidad con vínculos físicos.
- Compatibilidad con vínculos simbólicos.
- Actualmente, los recursos compartidos de archivos NFS solo admiten la mayoría de las características de la [especificación del protocolo 4.1](https://tools.ietf.org/html/rfc5661). No se admiten algunas características, como delegaciones y devoluciones de llamada de todo tipo, la autenticación Kerberos y el cifrado en tránsito.


## <a name="security-and-networking"></a>Seguridad y redes
Todos los datos almacenados en Azure Files se cifran en reposo mediante el cifrado de servicio de almacenamiento (SSE) de Azure. El cifrado del servicio de almacenamiento funciona de forma similar a BitLocker en Windows: los datos se cifran bajo el nivel del sistema de archivos. Dado que los datos se cifran bajo el sistema de archivos del recurso compartido de archivos de Azure, ya que se codifican en el disco, no es necesario tener acceso a la clave subyacente en el cliente para leer o escribir en dicho recurso compartido. El cifrado en reposo se aplica a los protocolos SMB y NFS.

En el caso del cifrado en tránsito, Azure proporciona una capa de cifrado para todos los datos en tránsito entre los centros de datos de Azure mediante [MACSec](https://en.wikipedia.org/wiki/IEEE_802.1AE). Gracias a esto, se produce el cifrado cuando se transfieren datos entre centros de datos de Azure. A diferencia de Azure Files cuando usa el protocolo SMB, los recursos compartidos de archivos que utilizan el protocolo NFS no ofrecen autenticación basada en usuario. La autenticación de los recursos compartidos NFS se basa en las reglas de seguridad de red configuradas. Por eso, para tener la certeza de que solo se establecen conexiones seguras con su recurso compartido de NFS, debe usar puntos de conexión de servicio o puntos de conexión privados. Si desea acceder a recursos compartidos desde un entorno local, además de un punto de conexión privado, debe configurar una VPN o ExpressRoute. Las solicitudes que no tengan los siguientes orígenes se rechazarán:

- [Un punto de conexión privado](storage-files-networking-overview.md#private-endpoints)
- [Azure VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)
    - [Una VPN de punto a sitio (P2S)](../../vpn-gateway/point-to-site-about.md)
    - [De sitio a sitio](../../vpn-gateway/design.md#s2smulti)
- [ExpressRoute](../../expressroute/expressroute-introduction.md)
- [Un punto de conexión público restringido](storage-files-networking-overview.md#storage-account-firewall-settings)

Para más información sobre las opciones de red disponibles, consulte [Consideraciones de redes para Azure Files](storage-files-networking-overview.md).

## <a name="support-for-azure-storage-features"></a>Compatibilidad con las características de Azure Storage

En la tabla siguiente, se muestra el nivel actual de compatibilidad con características de Azure Storage en las cuentas que tienen habilitada la característica NFS 4.1. 

El estado de los elementos que aparecen en esta tabla puede cambiar con el tiempo a medida que se siga ampliando la compatibilidad.

| Característica de Azure Storage | Compatible con recursos compartidos NFS |
|-----------------|---------|
| [API REST del plano de administración de archivos](/rest/api/storagerp/file-shares) | ✔️ |
| [API REST del plano de datos de archivos](/rest/api/storageservices/file-service-rest-api)| ⛔ |
| Cifrado en reposo|   ✔️ |
| Cifrado en tránsito| ⛔ |
| [Tipos de redundancia LRS o ZRS](storage-files-planning.md#redundancy)|  ✔️ |
| [Puntos de conexión privados](storage-files-networking-overview.md#private-endpoints) | ✔️  |
| Montajes de subdirectorios|  ✔️ |
| [Concesión de acceso de red a redes virtuales de Azure específicas](storage-files-networking-endpoints.md#restrict-access-to-the-public-endpoint-to-specific-virtual-networks)|  ✔️  |
| [Concesión de acceso a determinadas direcciones IP](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#grant-access-from-an-internet-ip-range)| ⛔ |
| [Nivel Premium](storage-files-planning.md#storage-tiers) |  ✔️  |
| [Niveles estándar (nivel de acceso frecuente, nivel de acceso esporádico y transacción optimizados)](storage-files-planning.md#storage-tiers)| ⛔ |
| [Permisos POSIX](https://en.wikipedia.org/wiki/File-system_permissions#Notation_of_traditional_Unix_permissions)|  ✔️  |
| Uso de root_squash|  ✔️  |
| [Habilitación basada en identidad](storage-files-active-directory-overview.md) | ⛔ |
| [Eliminación temporal de recursos compartidos de archivos de Azure](storage-files-prevent-file-share-deletion.md) | ⛔  |
| [Azure File Sync](../file-sync/file-sync-introduction.md)| ⛔ |
| [Copia de seguridad de un recurso compartido de archivos de Azure](../../backup/azure-file-share-backup-overview.md)| ⛔ |
| [Instantáneas de recursos compartidos de archivos de Azure](storage-snapshots-files.md)| ⛔ |
| [Tipos de redundancia GRS o GZRS](storage-files-planning.md#redundancy)| ⛔ |
| [AzCopy](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)| ⛔ |
| Explorador de Azure Storage| ⛔ |
| Creación de recursos compartidos de archivos de NFS en cuentas de almacenamiento existentes*| ⛔ |
| Compatibilidad con más de 16 grupos| ⛔ |

## <a name="regional-availability"></a>Disponibilidad regional

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="performance"></a>Rendimiento
Los recursos compartidos de archivos de Azure NFS solo se ofrecen en recursos compartidos de archivos prémium, que almacenan datos en unidades de estado sólido (SSD). Las IOPS y el rendimiento de los recursos compartidos de NFS se escalan con la capacidad aprovisionada. Consulte la sección del [modelo aprovisionado](understanding-billing.md#provisioned-model) del artículo descripción de la facturación para comprender las fórmulas de IOPS, ráfagas de E/S y rendimiento. Las latencias de E/S promedio son de un número bajo de milisegundos inferior a 10 para los tamaños de E/S pequeños, mientras que las latencias de metadatos promedio son de un número alto de milisegundos inferior a 10. Las operaciones de metadatos de carga elevada, como la descompresión y las cargas de trabajo como WordPress, pueden tener latencias adicionales debido al gran número de operaciones de apertura y cierre.

## <a name="workloads"></a>Cargas de trabajo
> [!IMPORTANT]
> Antes de usar recursos compartidos de archivos NFS en producción, consulte [Solución de problemas de recursos compartidos de archivos NFS de Azure](storage-troubleshooting-files-nfs.md) para ver una lista de problemas conocidos.

NFS se ha validado para funcionar bien con cargas de trabajo como el nivel de aplicación de SAP, copias de seguridad de bases de datos, replicación de bases de datos, colas de mensajería, directorios particulares para servidores de archivos de uso general y repositorios de contenido para cargas de trabajo de aplicaciones.

Las siguientes cargas de trabajo tienen problemas conocidos. Consulte [Solución de problemas de recursos compartidos de archivos NFS de Azure](storage-troubleshooting-files-nfs.md) para ver una lista de problemas conocidos:
- Oracle Database experimentará incompatibilidad con su característica dNFS.


## <a name="next-steps"></a>Pasos siguientes
- [Creación de un recurso compartido de archivos NFS](storage-files-how-to-create-nfs-shares.md)
- [Comparación del acceso a Azure Files, Blob Storage y Azure NetApp Files con NFS](../common/nfs-comparison.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
