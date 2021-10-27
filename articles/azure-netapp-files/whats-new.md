---
title: Novedades de Azure NetApp Files | Microsoft Docs
description: Proporciona un resumen de las nuevas características y mejoras más recientes de Azure NetApp Files.
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
ms.topic: overview
ms.date: 10/14/2021
ms.author: b-juche
ms.openlocfilehash: 78f8b282e701e181dd3a2c137ba254227651b048
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130036865"
---
# <a name="whats-new-in-azure-netapp-files"></a>Novedades de Azure NetApp Files

Azure NetApp Files se actualiza periódicamente. En este artículo se proporciona un resumen de las nuevas características y mejoras más recientes. 
 
## <a name="october-2021"></a>Octubre de 2021

* La [replicación entre regiones de Azure NetApp Files](cross-region-replication-introduction.md) está ahora disponible con carácter general (GA)

    La característica de replicación entre regiones ya está disponible con carácter general. Ya no es necesario registrar la característica antes de usarla.

* [Características de red Estándar](configure-network-features.md) (versión preliminar)

    Azure NetApp Files ahora admite las características de red **Estándar** para volúmenes que los clientes han estado solicitando desde el principio. Esta funcionalidad ha sido posible gracias a una innovadora integración de hardware y software. Las características de red Estándar proporcionan una experiencia de redes virtuales mejorada mediante una variedad de características para una experiencia sin problemas y coherente, junto con la posición de seguridad de todas las cargas de trabajo, incluido Azure NetApp Files.
    
    Ahora puede elegir características de red *Estándar* o *Básicas* al crear un nuevo volumen de Azure NetApp Files. Al elegir las características de red Estándar, puede aprovechar las ventajas de las siguientes características admitidas para volúmenes y subredes delegadas de Azure NetApp Files:   
    * Aumento de los límites de IP para las redes virtuales con volúmenes de Azure NetApp Files al mismo nivel que las máquinas virtuales
    * Seguridad de red mejorada con compatibilidad con [grupos de seguridad de red](../virtual-network/network-security-groups-overview.md) en la subred delegada de Azure NetApp Files
    * Control de red mejorado con compatibilidad con [rutas definidas por el usuario](../virtual-network/virtual-networks-udr-overview.md#custom-routes) hacia y desde las subredes delegadas de Azure NetApp Files
    * Conectividad mediante la configuración de la puerta de enlace de VPN activo/activo
    * Conectividad de [FastPath de ExpressRoute](../expressroute/about-fastpath.md) a Azure NetApp Files

    Esta versión preliminar pública está disponible actualmente en **Centro-norte de EE. UU.** y se va a poner en marcha en otras regiones.  Manténgase en contacto para más información mediante [Actualizaciones de Azure](https://azure.microsoft.com/updates/) a medida que estén disponibles más regiones y características.  
 
    Para más información, consulte [Configuración de características de red para un volumen de Azure NetApp Files](configure-network-features.md).

## <a name="september-2021"></a>Septiembre de 2021

* [Copia de seguridad de Azure NetApp Files](backup-introduction.md) (versión preliminar)

    Las instantáneas en línea de Azure NetApp Files se han mejorado con la copia de seguridad de instantáneas. Con esta nueva funcionalidad de copia de seguridad, puede almacenar las instantáneas de Azure NetApp Files en un almacenamiento de Azure rentable y habilitado para ZRS de forma rápida y rentable, lo que protege aún más los datos de eliminaciones accidentales. La copia de seguridad de Azure NetApp Files amplía la tecnología de instantáneas integrada de ONTAP. Cuando las instantáneas se almacenan en Azure Storage, solo se copian y almacenan en un formato eficaz los bloques cambiados relacionados con instantáneas almacenadas anteriormente. Sin embargo, las instantáneas almacenadas se siguen representando en su totalidad y se pueden restaurar en un nuevo volumen de forma individual y directa, lo que elimina la necesidad de un proceso de recuperación incremental iterativo completamente incremental. Esta tecnología avanzada minimiza la cantidad de datos que hay que almacenar en Azure Storage, y recuperar de él, lo que ahorra costos de almacenamiento y transferencia de datos. También acorta el tiempo de almacenamiento de copias de seguridad, por lo que se puede lograr un objetivo de punto de restauración (RPO) más pequeño. Ahora puede optar por mantener un número mínimo de instantáneas en línea en el servicio Azure NetApp Files para cubrir las necesidades de recuperación de datos más inmediatas, casi instantáneas, y crear un historial más largo de instantáneas a un costo menor para fines de retención a largo plazo en el almacén de copia de seguridad de Azure NetApp Files. Para más información, consulte [Funcionamiento de las instantáneas de Azure NetApp Files](snapshots-introduction.md).

* Opción [**Administradores**](create-active-directory-connections.md#create-an-active-directory-connection) en las conexiones de Active Directory (versión preliminar)

    La página de conexiones de Active Directory ahora incluye un campo **Administradores**. Puede especificar los usuarios o grupos a los que se les darán privilegios de administrador en el volumen.

## <a name="august-2021"></a>Agosto de 2021

* Admite la [habilitación de la disponibilidad continua en volúmenes de SMB existentes](enable-continuous-availability-existing-SMB.md).

    Puede habilitar la característica de disponibilidad continua (CA) de SMB al [crear un nuevo volumen SMB](azure-netapp-files-create-volumes-smb.md#continuous-availability). Ahora también puede habilitar CA de SMB en un volumen SMB existente. Consulte [Habilitación de la disponibilidad continua en volúmenes de SMB existentes](enable-continuous-availability-existing-SMB.md).

* La [directiva de instantáneas](snapshots-manage-policy.md) ahora está disponible con carácter general (GA)  

    La característica de directiva de instantáneas ya está disponible con carácter general. Ya no es necesario registrar la característica antes de usarla.

* Directiva de exportación de [NFS `Chown Mode` y permisos de exportación de UNIX](configure-unix-permissions-change-ownership-mode.md) (versión preliminar)   

    Ahora puede establecer los permisos de Unix y las opciones del modo de cambio de propiedad (`Chown Mode`) en volúmenes NFS de Azure NetApp Files o volúmenes de protocolo dual con el estilo de seguridad de Unix. Puede especificar esta configuración durante la creación del volumen o después de la creación del volumen.   

    La funcionalidad de modo de cambio de propiedad (`Chown Mode`) permite establecer las funcionalidades de administración de propiedad de archivos y directorios. Puede especificar o modificar la configuración en la directiva de exportación de un volumen. Hay dos opciones disponibles para `Chown Mode`: *Restringido* (valor predeterminado), donde solo el usuario raíz puede cambiar la propiedad de los archivos y directorios, y *Sin restricciones*, donde los usuarios no raíz pueden cambiar la propiedad de los archivos y directorios que  poseen.   

    La funcionalidad de permisos de Unix de Azure NetApp Files permite especificar permisos de cambio para la ruta de acceso de montaje. 

    Estas nuevas características proporcionan opciones para mover el control de acceso de determinados archivos y directorios a manos del usuario de datos en lugar del operador de servicio.   

* [Volumen de protocolo doble (NFSv4 y SMB)](create-volumes-dual-protocol.md) (versión preliminar)   

    Azure NetApp Files ya admite el acceso de protocolo dual a volúmenes NFSv3 y SMB a partir de [julio de 2020](#july-2020). Ahora puede crear un volumen de Azure NetApp Files que permite el acceso simultáneo con protocolo doble (NFSv4 y SMB) con compatibilidad con la asignación de usuarios LDAP. Esta característica permite casos de uso en los que podría tener una carga de trabajo basada en Linux mediante NFSv4.1 para su acceso, y la carga de trabajo genera y almacena datos en un volumen de Azure NetApp Files. Al mismo tiempo, el personal debe usar clientes y software basados en Windows para analizar los datos recién generados desde el mismo volumen de Azure NetApp Files. La característica de acceso simultáneo con protocolo doble elimina la necesidad de copiar los datos generados por la carga de trabajo en un volumen independiente con un protocolo diferente para el análisis posterior, lo que supone un ahorro en los costos y en el tiempo de funcionamiento. Esta característica es gratuita (se sigue aplicando el costo de almacenamiento de Azure NetApp Files normal) y está disponible con carácter general. Más información en la documentación de [acceso simultáneo con protocolo doble NFSv4.1/SMB](create-volumes-dual-protocol.md).

## <a name="june-2021"></a>Junio de 2021  

* [Complementos del servicio de almacenamiento de Azure NetApp Files](storage-service-add-ons.md)

    La nueva opción de menú **Storage service add-ons** (Complementos del servicio de almacenamiento) de Azure NetApp Files proporciona un "panel de inicio" de Azure Portal para complementos de ecosistemas de terceros disponibles con el servicio de almacenamiento de Azure NetApp Files. Con esta nueva opción de menú del portal, puede entrar en una página de aterrizaje haciendo clic en un icono de complemento para acceder rápidamente al complemento.  

    Los **complementos de NetApp** son la primera categoría de complementos que se presentan en **Storage service add-ons** (Complementos del servicio de almacenamiento). Esta categoría proporcionan acceso a **NetApp Cloud Compliance**. Al hacer clic en **NetApp Cloud Compliance**, se abre un nuevo explorador que le dirige a la página de instalación del complemento. 

* [Grupo de capacidad de QoS manual](manual-qos-capacity-pool-introduction.md) disponible ahora con carácter general (GA)   

    La característica del grupo de capacidad de QoS manual ya está disponible con carácter general. Ya no es necesario registrar la característica antes de usarla. 

* [Compatibilidad de AD compartido en varias cuentas con una instancia de Active Directory por región y suscripción](create-active-directory-connections.md#shared_ad) (versión preliminar)   

    Hasta la fecha, Azure NetApp Files solo admite una única instancia de Active Directory (AD) por región, donde solo se podía configurar una única cuenta de NetApp para acceder a AD. La nueva característica de **AD compartido** permite que todas las cuentas de NetApp compartan una conexión de AD creada por una de las cuentas de NetApp que pertenecen a la misma suscripción y a la misma región. Por ejemplo, con esta característica, todas las cuentas de NetApp de la misma suscripción y región pueden usar la configuración común de AD para crear un volumen SMB, un volumen Kerberos NFSv4.1 o un volumen de protocolo dual. Cuando se use esta característica, la conexión de AD será visible en todas las cuentas de NetApp que estén en la misma suscripción y región.

## <a name="may-2021"></a>Mayo de 2021 

* La herramienta de instantáneas coherentes con la aplicación [(AzAcSnap)](azacsnap-introduction.md) de Azure NetApp Files ya está disponible con carácter general. 

    AzAcSnap es una herramienta de línea de comandos que permite simplificar la protección de datos para bases de datos de terceros (SAP HANA) en entornos de Linux (por ejemplo, SUSE y RHEL). Consulte las [notas de la versión de AzAcSnap](azacsnap-release-notes.md) para conocer los cambios más recientes sobre la herramienta.   

* [Compatibilidad con etiquetas de facturación del grupo de capacidad](manage-billing-tags.md)   

    Azure NetApp Files ahora admite etiquetas de facturación para ayudarle a hacer referencia cruzada al costo con unidades de negocio u otros consumidores internos. Las etiquetas de facturación se asignan en el nivel de grupo de capacidad y no en el de volumen, y aparecen en la factura del cliente.

* [ADDS LDAP sobre TLS](configure-ldap-over-tls.md) (versión preliminar) 

    De forma predeterminada, las comunicaciones LDAP entre aplicaciones cliente y servidor no se cifran. Esto significa que es posible usar un dispositivo o software de supervisión de red para ver las comunicaciones entre un cliente LDAP y los equipos servidor. Este escenario puede ser problemático en redes virtuales no aisladas o compartidas cuando se usa un enlace simple LDAP, ya que las credenciales (nombre de usuario y contraseña) usadas para enlazar el cliente LDAP al servidor LDAP se pasan a través de la red sin cifrar. LDAP sobre TLS (también conocido como LDAPS) es un protocolo que usa TLS para proteger la comunicación entre los clientes LDAP y los servidores LDAP. Azure NetApp Files ahora admite la comunicación segura entre un Dominio de Active Directory Server (ADDS) mediante LDAP sobre TLS. Azure NetApp Files ahora puede usar LDAP sobre TLS para configurar sesiones autenticadas entre los servidores LDAP integrados en Active Directory. Puede habilitar la característica LDAP sobre TLS para volúmenes de protocolo dual, SMB y NFS. De forma predeterminada, LDAP sobre TLS está deshabilitado en Azure NetApp Files.  

* Compatibilidad con [métricas](azure-netapp-files-metrics.md) de rendimiento    

    Azure NetApp Files agrega compatibilidad con las métricas siguientes:   
    * Métricas de rendimiento del grupo de capacidad
        * *Rendimiento del grupo asignado al volumen*
        * *Rendimiento consumido por el grupo*
        * *Porcentaje de rendimiento del grupo asignado al volumen*
        * *Porcentaje de rendimiento consumido del grupo*
    * Métricas de rendimiento de volumen
        * *Rendimiento asignado por volumen*
        * *Rendimiento consumido por volumen*
        * *Porcentaje de rendimiento consumido del volumen*

* Compatibilidad con el [cambio dinámico del nivel de servicio](dynamic-change-volume-service-level.md) de los volúmenes de replicación   

    Azure NetApp Files ahora admite el cambio dinámico del nivel de servicio de los volúmenes de origen y destino de replicación.

## <a name="april-2021"></a>Abril de 2021

* [Administración manual del volumen y el grupo de capacidad](volume-quota-introduction.md) (cuota máxima) 

    El comportamiento del aprovisionamiento del volumen y el grupo de capacidad de Azure NetApp Files ha cambiado a un mecanismo manual y controlable. La capacidad de almacenamiento de un volumen está limitada al tamaño establecido (cuota) del volumen. Cuando el consumo de volumen alcanza el máximo, ni el volumen ni el grupo de capacidad subyacente crecen automáticamente. En su lugar, el volumen recibirá una condición de "fuera de espacio". Sin embargo, puede [cambiar el tamaño del grupo de capacidad o un volumen](azure-netapp-files-resize-capacity-pools-or-volumes.md) según sea necesario. Debe [supervisar activamente la capacidad de un volumen](monitor-volume-capacity.md) y del grupo de capacidad subyacente.

    Este cambio de comportamiento es el resultado de las siguientes solicitudes clave indicadas por muchos usuarios:

    * Antes, los clientes de máquina virtual veían la capacidad de aprovisionamiento fino (100 TiB) de cualquier volumen al usar las herramientas de supervisión del espacio o la capacidad del sistema operativo.  Esta situación podría dar lugar a una visión inadecuada de la capacidad del cliente o la aplicación. Este comportamiento se ha corregido ya.  
    * El comportamiento anterior de crecimiento automático de los grupos de capacidad no daba a los propietarios de aplicaciones ningún control sobre el espacio del grupo de capacidad aprovisionado (y el costo asociado). Este comportamiento resultaba especialmente molesto en entornos en los que los "procesos descontrolados" podían saturarse y aumentar rápidamente la capacidad aprovisionada. Este comportamiento se ha corregido.  
    * Los usuarios quieren ver y mantener una correlación directa entre el tamaño del volumen (cuota) y el rendimiento. El comportamiento anterior permitía el crecimiento automático (implícito) de una suscripción en exceso de un volumen (capacidad) y un grupo de capacidad. Por ello, los usuarios no podían realizar una correlación directa hasta que se establecía, o restablecía, activamente la cuota del volumen. Este comportamiento se ha corregido ya.

    Los usuarios han solicitado control directo sobre la capacidad aprovisionada. Quieren controlar y equilibrar la capacidad de almacenamiento y la utilización. También quieren controlar el costo y obtener visibilidad tanto para las aplicaciones como para el cliente de la capacidad y rendimiento disponibles, utilizados y aprovisionados de los volúmenes de sus aplicaciones. Con este nuevo comportamiento, se ha habilitado toda esta funcionalidad.

* [Compatibilidad con recursos compartidos de disponibilidad continua (CA) de SMB para contenedores de perfiles de usuario de FSLogix](azure-netapp-files-create-volumes-smb.md#continuous-availability) (versión preliminar)  

    [FSLogix](/fslogix/overview) es un conjunto de soluciones que mejoran, habilitan y simplifican los entornos informáticos de Windows no persistentes. Las soluciones FSLogix son adecuadas para entornos virtuales en nubes tanto públicas como privadas. Las soluciones de FSLogix también se pueden usar para crear sesiones informáticas más portables al usar dispositivos físicos. FSLogix se puede usar para proporcionar acceso dinámico a contenedores de perfiles de usuario persistentes almacenados en un almacenamiento en red compartido SMB, incluido Azure NetApp Files. Para mejorar aún más la resistencia de FSLogix a los eventos de mantenimiento del servicio de almacenamiento, Azure NetApp Files ha ampliado la compatibilidad con la conmutación por error transparente de SMB mediante [recursos compartidos de disponibilidad continua (CA) de SMB en Azure NetApp Files](azure-netapp-files-create-volumes-smb.md#continuous-availability) para contenedores de perfiles de usuario. Consulte las [soluciones de Azure Virtual Desktop](azure-netapp-files-solution-architectures.md#windows-virtual-desktop) de Azure NetApp Files para obtener información adicional.  

* [Cifrado del protocolo SMB3](azure-netapp-files-create-volumes-smb.md#smb3-encryption) (versión preliminar) 

    Ahora puede habilitar el cifrado del protocolo SMB3 en Azure NetApp Files SMB y volúmenes de protocolo dual. Esta característica facilita el cifrado de datos SMB3 sobre la marcha mediante las conexiones de los [algoritmos AES-CCM en SMB 3.0 y AES-GCM en SMB 3.1.1](/windows-server/storage/file-server/file-server-smb-overview#features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607). Los clientes SMB que no usen el cifrado SMB3 no podrán acceder a este volumen. Los datos en reposo se cifrarán al margen de esta configuración. El cifrado SMB refuerza la seguridad. Sin embargo, podría afectar al cliente (sobrecarga de la CPU al cifrar y descifrar mensajes). También puede afectar al uso de los recursos de almacenamiento (disminución del rendimiento). Antes de implementar cargas de trabajo en producción, debería comprobar el efecto del cifrado en el rendimiento de las aplicaciones.

* [Asignación de usuarios mediante LDAP de Active Directory Domain Services (ADDS) con grupos ampliados para NFS](configure-ldap-extended-groups.md) (versión preliminar)   

    De forma predeterminada, Azure NetApp Files admite hasta 16 identificadores de grupo al administrar credenciales de usuario de NFS, tal como se define en [RFC 5531](https://tools.ietf.org/html/rfc5531). Con esta nueva funcionalidad ahora puede aumentar el máximo hasta 1024 si tiene usuarios que sean miembros de más grupos que los predeterminados. Para admitir esta funcionalidad, ahora también se pueden agregar volúmenes NFS a LDAP de ADDS, lo que permite a los usuarios de LDAP de Active Directory con entradas de grupos ampliadas (hasta 1024 grupos) acceder al volumen. 

## <a name="march-2021"></a>Marzo de 2021
 
* [Recursos compartidos con disponibilidad continua para SMB](azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) (versión preliminar)  

    La conmutación por error transparente de SMB facilita las operaciones de mantenimiento en el servicio Azure NetApp Files sin interrumpir la conectividad con las aplicaciones de servidor que almacenan los datos en volúmenes SMB y acceden a ellos. Para facilitar la conmutación por error transparente de SMB, Azure NetApp Files admite el uso de recursos compartidos con disponibilidad continua de SMB para aplicaciones de SQL Server a través de SMB en máquinas virtuales de Azure. Esta característica se admite actualmente en Windows SQL Server. Linux SQL Server no se admite actualmente. La activación de esta característica mejora considerablemente el rendimiento de SQL Server y ofrece ventajas de escalabilidad y costo para [implementaciones de una instancia única, una instancia de clúster de conmutación por error de Always-On y un grupo de disponibilidad Always-On](azure-netapp-files-solution-architectures.md#sql-server). Consulte las [ventajas de usar Azure NetApp Files para una implementación de SQL Server](solutions-benefits-azure-netapp-files-sql-server.md).

* [Cambio automático del tamaño de un volumen de destino de replicación entre regiones](azure-netapp-files-resize-capacity-pools-or-volumes.md#resize-a-cross-region-replication-destination-volume)

    En una relación de replicación entre regiones, el tamaño de un volumen de destino se cambia automáticamente en función del tamaño del volumen de origen. De este modo, no es necesario cambiar el tamaño del volumen de destino por separado. Este cambio de tamaño automático es aplicable cuando los volúmenes establecen una relación de replicación activa, o cuando el emparejamiento de replicación se interrumpe con la operación de resincronización. Para que esta característica funcione, debe asegurarse de tener suficiente espacio en los grupos de capacidad para los volúmenes de origen y de destino.

## <a name="december-2020"></a>Diciembre de 2020

* [Herramienta Azure Application Consistent Snapshot](azacsnap-introduction.md) (versión preliminar)    

    Herramienta Azure Application Consistent Snapshot (AzAcSnap) es una herramienta de línea de comandos que permite simplificar la protección de datos para bases de datos de terceros (SAP HANA) en entornos de Linux (por ejemplo, SUSE y RHEL).   

    AzAcSnap aprovecha las funcionalidades de replicación e instantánea de volumen de Azure NetApp Files y Azure (instancias grandes). Proporciona las siguientes ventajas:

    * Protección de datos coherente con la aplicación 
    * Administración del catálogo de base de datos 
    * Protección de volúmenes *ad hoc* 
    * Clonación de volúmenes de almacenamiento 
    * Compatibilidad con la recuperación ante desastres 

## <a name="november-2020"></a>Noviembre de 2020

* [Reversión de instantáneas](snapshots-revert-volume.md)

    La funcionalidad de reversión de instantáneas le permite revertir rápidamente un volumen al estado en que se encontraba cuando se tomó una instantánea determinada. En la mayoría de los casos, la reversión de un volumen es mucho más rápida que la restauración de archivos individuales a partir de una instantánea en el sistema de archivos activo. También requiere menos espacio que la restauración de una instantánea en un nuevo volumen.

## <a name="september-2020"></a>Septiembre de 2020

* [Replicación de Azure NetApp Files entre regiones](cross-region-replication-introduction.md) (versión preliminar)

  Azure NetApp Files ahora admite la replicación entre regiones. Con esta nueva funcionalidad de recuperación ante desastres, puede replicar los volúmenes de Azure NetApp Files de una región de Azure a otra de manera rápida y rentable, y así proteger los datos frente a errores regionales imprevistos. La replicación entre regiones de Azure NetApp Files aprovecha la tecnología SnapMirror® de NetApp; solo los bloques modificados se envían a través de la red en un formato comprimido y eficiente. Esta tecnología propietaria reduce la cantidad de datos necesarios para replicar entre regiones, lo que ahorra costos de transferencia de datos. También acorta el tiempo de replicación, por lo que puede lograr un objetivo de punto de restauración (RPO) más pequeño.

* [Grupo de capacidad de QoS manual](manual-qos-capacity-pool-introduction.md) (versión preliminar)  

    En un grupo de capacidad de QoS manual, puede asignar la capacidad y el rendimiento de un volumen de forma independiente. El rendimiento total de todos los volúmenes creados con un grupo de capacidad de QoS manual está limitado por el rendimiento total del grupo. Viene determinado por la combinación del tamaño del grupo y el rendimiento del nivel de servicio. También, el [tipo de QoS](azure-netapp-files-understand-storage-hierarchy.md#qos_types) de un grupo de capacidad puede ser automático (auto), que es el valor predeterminado. En un grupo de capacidad de QoS automático, el rendimiento se asigna automáticamente a los volúmenes del grupo, de forma proporcional a la cuota de tamaño asignada a los volúmenes.

* [Firma LDAP](create-active-directory-connections.md#create-an-active-directory-connection) (versión preliminar)   

    Azure NetApp Files admite ahora la firma LDAP para proteger las búsquedas de LDAP entre el servicio Azure NetApp Files y los controladores de dominio de Active Directory Domain Services especificados por el usuario. Esta funcionalidad actualmente está en su versión preliminar.

* [Cifrado AES para la autenticación de AD](create-active-directory-connections.md#create-an-active-directory-connection) (versión preliminar)

    Azure NetApp Files admite ahora el cifrado AES en la conexión LDAP con el controlador de dominio para permitir este tipo de cifrado en un volumen SMB. Esta funcionalidad actualmente está en su versión preliminar. 

* Nuevas [métricas](azure-netapp-files-metrics.md):   

    * Nuevas métricas de volumen: 
        * *Tamaño asignado del volumen*: Tamaño aprovisionado de un volumen
    * Nuevas métricas de grupo: 
        * *Tamaño asignado del grupo*: el tamaño aprovisionado del grupo. 
        * *Tamaño de instantánea total del grupo*: la suma del tamaño de la instantánea de todos los volúmenes del grupo.

## <a name="july-2020"></a>Julio de 2020

* [Volumen de protocolo doble (NFSv3 y SMB)](create-volumes-dual-protocol.md)

    Ahora puede crear un volumen de Azure NetApp Files que permite el acceso simultáneo con protocolo doble (NFS V3 y SMB) con compatibilidad con la asignación de usuarios LDAP. Esta característica permite casos de uso en los que puede tener una carga de trabajo basada en Linux que genera y almacena los datos en un volumen de Azure NetApp Files. Al mismo tiempo, el personal debe usar clientes y software basados en Windows para analizar los datos recién generados desde el mismo volumen de Azure NetApp Files. La característica de acceso simultáneo con protocolo doble elimina la necesidad de copiar los datos generados por la carga de trabajo en un volumen independiente con un protocolo diferente para el análisis posterior, lo que supone un ahorro en los costes y en el tiempo de funcionamiento. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y está disponible con carácter general. Más información en la [documentación de acceso simultáneo con protocolo doble](create-volumes-dual-protocol.MD).

* [Cifrado Kerberos de NFS v 4.1 en tránsito](configure-kerberos-encryption.MD)

    Azure NetApp Files admite ahora el cifrado de cliente NFS en los modos de Kerberos (krb5, krb5i y krb5p) con el cifrado AES-256, lo que le proporciona más seguridad para los datos. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y está disponible con carácter general. Más información en la [documentación de cifrado Kerberos de NFS v4.1](configure-kerberos-encryption.MD).

* [Cambio de nivel de servicio de volumen dinámico](dynamic-change-volume-service-level.MD) (versión preliminar) 

    Flexibilidad de promesas de la nube en los gastos de TI. Ahora puede cambiar el nivel de servicio de un volumen existente de Azure NetApp Files si lo mueve a otro grupo de capacidad que use el nivel de servicio que quiera para el volumen. Este cambio de nivel de servicio local para el volumen no requiere que se migren los datos. Tampoco afecta al acceso del plano de datos al volumen. Puede cambiar un volumen existente para que use un nivel de servicio superior para mejorar el rendimiento, o para que use un nivel de servicio inferior para la optimización de costos. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal). Se encuentra actualmente en versión preliminar. Para registrarse en la versión preliminar de la característica, siga la [documentación de cambio de nivel de servicio de volúmenes dinámicos](dynamic-change-volume-service-level.md).

* [Directiva de instantáneas de volumen](snapshots-manage-policy.md) (versión preliminar) 

    Azure NetApp Files le permite crear instantáneas de un momento dado de los volúmenes. Ahora, puede crear una directiva de instantáneas para que Azure NetApp Files cree automáticamente instantáneas de volumen con una frecuencia de su elección. Puede programar que se tomen instantáneas en ciclos por hora, día, semana o mes. También puede especificar el número máximo de instantáneas que se deben conservar como parte de la directiva de instantáneas. Esta característica es gratuita (se sigue aplicando el [costo de almacenamiento de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/) normal) y se encuentra actualmente en versión preliminar. Para registrarse en la versión preliminar de la característica, siga la [documentación de la directiva de instantáneas de volumen](snapshots-manage-policy.md).

* [Directiva de exportación de acceso raíz de NFS](azure-netapp-files-configure-export-policy.md)

    Azure NetApp Files ahora le permite especificar si la cuenta raíz puede acceder al volumen. 

* [Ocultar la ruta de acceso de la instantánea](snapshots-edit-hide-path.md)

    Azure NetApp Files le permite ahora especificar si un usuario puede ver el directorio `.snapshot` (clientes NFS) o la carpeta `~snapshot` (clientes SMB) en un volumen montado, y acceder a estos.

## <a name="may-2020"></a>Mayo de 2020

* [Usuarios de la directiva de copia de seguridad](create-active-directory-connections.md) (versión preliminar)

    Azure NetApp Files le permite incluir cuentas adicionales que requieran privilegios elevados para la cuenta de equipo creada para Azure NetApp Files. Se permitirá a las cuentas especificadas cambiar los permisos de NTFS en el nivel de archivo o carpeta. Por ejemplo, puede especificar una cuenta de servicio sin privilegios que se usa para migrar los datos a un recurso compartido de archivos de SMB en Azure NetApp Files. La característica Usuarios de la directiva de copia de seguridad se encuentra actualmente en la versión preliminar.

## <a name="next-steps"></a>Pasos siguientes
* [¿Qué es Azure NetApp Files?](azure-netapp-files-introduction.md)
* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md) 
