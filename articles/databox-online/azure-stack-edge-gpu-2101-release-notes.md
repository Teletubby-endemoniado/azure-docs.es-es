---
title: Notas de la versión de Azure Stack Edge Pro 2101
description: Aquí se describen las incidencias críticas pendientes y las resoluciones de Azure Stack Edge Pro versión 2101.
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/08/2021
ms.author: alkohli
ms.openlocfilehash: 44aa20dcdf4cc4d87fa868ce38855b525caa8a98
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129707509"
---
# <a name="azure-stack-edge-2101-release-notes"></a>Notas de la versión de Azure Stack Edge 2101

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

En las notas de la versión siguientes se identifican las incidencias críticas pendientes e incidencias resueltas de la versión 2101 de los dispositivos Azure Stack Edge. Estas notas de la versión se aplican a los dispositivos de Azure Stack Edge Pro con GPU, Azure Stack Edge Pro R y Azure Stack Edge Mini R. Las características y las incidencias que corresponden a un modelo específico se indican siempre que proceda.

Las notas de la versión se actualizan continuamente y se van agregando a medida que se descubren problemas críticos que requieren una solución alternativa. Antes de implementar el dispositivo, revise detenidamente la información que encontrará en las notas de la versión.

Este artículo se aplica a la versión **Azure Stack Edge 2101**, correspondiente al número de versión de software **2.2.1473.2521**.

## <a name="whats-new"></a>Novedades

Ya están disponibles las siguientes características nuevas en la versión 2101 de Azure Stack Edge. 

- **Disponibilidad general de los dispositivos de Azure Stack Edge Pro R y Azure Stack Edge Mini R**: a partir de esta versión, estarán disponibles los dispositivos de Azure Stack Edge Pro R y Azure Stack Edge Mini R. Para obtener más información, consulte [¿Qué es Azure Stack Edge Pro R?](azure-stack-edge-pro-r-overview.md) y [¿Qué es Azure Stack Edge Mini R?](azure-stack-edge-mini-r-overview.md).  
- **Administración en la nube de Virtual Machines**: a partir de esta versión, puede crear y administrar las máquinas virtuales en el dispositivo a través de Azure Portal. Para más información, consulte [Implementación de VM a través de Azure Portal](azure-stack-edge-gpu-deploy-virtual-machine-portal.md).
- **Integración con Azure Monitor**: ya puede usar Azure Monitor para supervisar contenedores desde las aplicaciones de proceso que se ejecutan en el dispositivo. El almacén de métricas de Azure Monitor no se admite en esta versión. Para más información, consulte [Habilitación de Azure Monitor en el dispositivo](azure-stack-edge-gpu-enable-azure-monitor.md).
- **Registro de contenedor de Edge**: en esta versión, hay disponible un registro de contenedor de Edge que proporciona un repositorio en el perímetro del dispositivo. Puede usar este registro para almacenar y administrar las imágenes de contenedor. Para más información, consulte [Habilitación del registro de contenedor de Edge](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md). 
- **Red privada virtual (VPN)** : use la VPN para proporcionar otra capa de cifrado para los datos que fluyen entre los dispositivos y la nube. VPN solo está disponible en Azure Stack Edge Pro R y Azure Stack Edge Mini R. Para obtener más información, consulte [Configuración de la VPN en el dispositivo](azure-stack-edge-mini-r-configure-vpn-powershell.md). 
- **Rotación de las claves de cifrado en reposo**: ahora puede rotar las claves de cifrado en reposo que se usan para proteger las unidades en el dispositivo. Esta característica solo está disponible para los dispositivos de Azure Stack Edge Pro R y Azure Stack Edge Mini R. Para más información, consulte [Giro de las claves de cifrado en reposo](azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#manage-access-to-device-data).
- **Registro proactivo**: a partir de esta versión, puede habilitar la recopilación proactiva de registros en el dispositivo en función de los indicadores de estado del sistema para ayudar a solucionar de forma eficaz cualquier problema del dispositivo. Para obtener más información, consulte [Recopilación proactiva de registros en el dispositivo](azure-stack-edge-gpu-proactive-log-collection.md).


## <a name="known-issues-in-2101-release"></a>Problemas conocidos de la versión 2101

En la tabla siguiente se proporciona un resumen de los problemas conocidos de la versión 2101.

| No. | Característica | Problema | Soluciones alternativas o comentarios |
| --- | --- | --- | --- |
|**1.**|Características en vista previa |Para esta versión, las siguientes características: Azure Resource Manager local, VM, administración en la nube de VM, Kubernetes habilitado para Azure Arc, VPN para Azure Stack Edge Pro R y Azure Stack Edge Mini R, servicio de varios procesos (MPS) para Azure Stack Edge Pro con GPU, están todas disponibles en versión preliminar.  |Estas características estarán disponibles con carácter general en versiones posteriores. |
|**2.**|Panel de Kubernetes | No se admite el punto de conexión *HTTPS* del panel de Kubernetes con el certificado SSL. | |
|**3.**|Kubernetes |El registro de contenedor de Edge no funciona cuando el proxy web está habilitado.|La funcionalidad estará disponible en una versión posterior. |
|**4.**|Kubernetes |El registro de contenedor de Edge no funciona con los módulos de IoT Edge.| |
|**5.**|Kubernetes |Kubernetes no admite ":" en los nombres de las variables de entorno que utilizan las aplicaciones .NET. Esto también es necesario para que el módulo de IoT Edge de Event Grid funcione en el dispositivo de Azure Stack Edge y en otras aplicaciones. Para más información, consulte [Documentación de ASP.NET Core](/aspnet/core/fundamentals/configuration/?tabs=basicconfiguration&view=aspnetcore-3.1&preserve-view=true#environment-variables).|Reemplace ":" por un guion bajo doble. Para más información, consulte [Incidencia de Kubernetes](https://github.com/kubernetes/kubernetes/issues/53201).|
|**6.** |Azure Arc + clúster de Kubernetes |De manera predeterminada, cuando el recurso `yamls` se elimina del repositorio de Git, no se eliminan los recursos correspondientes del clúster de Kubernetes.  |Para permitir la eliminación de recursos cuando se eliminen del repositorio de Git, establezca `--sync-garbage-collection` en Arc OperatorParams. Para obtener más información, consulte [Eliminación de una configuración](../azure-arc/kubernetes/tutorial-use-gitops-connected-cluster.md#additional-parameters). |
|**7.**|NFS |Las aplicaciones que usan montajes de recursos compartidos de NFS en el dispositivo para escribir datos deben usar escritura exclusiva. Así se garantiza que las escrituras se realizan en el disco.| |
|**8.**|Configuración del proceso |Se produce un error en la configuración del proceso en configuraciones de red en las que las puertas de enlace o los conmutadores o enrutadores responden a las solicitudes del Protocolo de resolución de direcciones (ARP) para sistemas que no existen en la red.| |
|**9.**|Proceso y Kubernetes |Si Kubernetes se configura en primer lugar en el dispositivo, notifica todas las GPU disponibles. Por lo tanto, no es posible crear VM de Azure Resource Manager mediante GPU después de configurar Kubernetes. |Si el dispositivo tiene dos GPU, puede crear una máquina virtual que use la GPU y, a continuación, configurar Kubernetes. En este caso, Kubernetes usará el resto de la GPU disponible. |


## <a name="known-issues-from-previous-releases"></a>Problemas conocidos de las versiones anteriores

En la tabla siguiente se proporciona un resumen de los problemas conocidos que se arrastran de las versiones anteriores.

| No. | Característica | Problema | Soluciones alternativas o comentarios |
| --- | --- | --- | --- |
|**1.**|Azure Stack Edge Pro y Azure SQL | La creación de una base de datos SQL requiere acceso de administrador. |Realice los pasos siguientes, en lugar de los pasos 1 a 2 del [Tutorial: Almacenamiento de datos en el perímetro con bases de datos de SQL Server](../iot-edge/tutorial-store-data-sql-server.md#create-the-sql-database). <ul><li>En la interfaz de usuario local del dispositivo, habilite la interfaz de proceso. Seleccione **Proceso > Número de puerto > Habilitar para el proceso > Aplicar**.</li><li>Descargue `sqlcmd` en la máquina cliente desde la [utilidad sqlcmd](/sql/tools/sqlcmd-utility) </li><li>Conéctese a la dirección IP de la interfaz de proceso (el puerto que se habilitó) y agregue `,1401` al final de la dirección.</li><li>El comando tendrá un aspecto similar al siguiente: `sqlcmd -S {Interface IP},1401 -U SA -P "Strong!Passw0rd"`.</li>Tras este cambio, los pasos 3 y 4 de la documentación actual deben ser idénticos. </li></ul> |
|**2.**|Actualizar| No se admiten los cambios incrementales en los blobs restaurados mediante la funcionalidad de **actualización**. |En los puntos de conexión de blob, las actualizaciones parciales de los blobs después de la funcionalidad de actualización pueden dar lugar a que las actualizaciones no se carguen en la nube. Por ejemplo, supongamos una secuencia de acciones como la siguiente:<ul><li>Crear un blob en la nube. O bien, eliminar del dispositivo un blob cargado previamente.</li><li>Actualizar el blob desde la nube en el dispositivo mediante la funcionalidad de actualización.</li><li>Actualizar solo una parte del blob mediante las API REST de Azure SDK.</li></ul>Estas acciones pueden dar lugar a que las secciones actualizadas del blob no se actualicen en la nube. <br>**Solución alternativa**: use herramientas como robocopy o la copia normal de archivos con el Explorador o la línea de comandos para reemplazar los blobs completos.|
|**3.**|Limitaciones|En una limitación, si no se permiten nuevas escrituras en el dispositivo, las escrituras realizadas por el cliente NFS generarán el error "Permiso denegado".| El error se mostrará como se indica a continuación:<br>`hcsuser@ubuntu-vm:~/nfstest$ mkdir test`<br>mkdir: no se puede crear el directorio 'test': Permiso denegado|
|**4.**|Ingesta de Blob Storage|Si usa AzCopy versión 10 para la ingesta de Blob Storage, ejecute AzCopy con el siguiente argumento: `Azcopy <other arguments> --cap-mbps 2000`.| Si no se proporcionan estos límites para AzCopy, se podría enviar un gran número de solicitudes al dispositivo, lo que provocaría problemas con el servicio.|
|**5.**|Cuentas de almacenamiento en capas|Cuando se usan cuentas de almacenamiento en capas, se aplica lo siguiente:<ul><li> Solo se admiten blobs en bloques. No se admiten los blobs en páginas.</li><li>No hay compatibilidad con la API de copia o instantánea.</li><li> No se admite la ingesta de cargas de trabajo de Hadoop mediante `distcp`, ya que utiliza la operación de copia de forma intensiva.</li></ul>||
|**6.**|Conexión de recurso compartido NFS|Si varios procesos se están copiando en el mismo recurso compartido y no se utiliza el atributo `nolock`, es posible que aparezcan errores durante la copia.|Para copiar archivos en el recurso compartido NFS, se debe usar el atributo `nolock` con el comando mount. Por ejemplo: `C:\Users\aseuser mount -o anon \\10.1.1.211\mnt\vms Z:`.|
|**7.**|Clúster de Kubernetes|Cuando se aplica una actualización en el dispositivo que ejecuta un clúster de Kubernetes, las máquinas virtuales de Kubernetes se reinician. En este caso, solo se restauran automáticamente después de una actualización los pods que se implementan con las réplicas especificadas.  |Si ha creado pods individuales fuera de un controlador de replicación sin especificar un conjunto de réplicas, estos pods no se restaurarán automáticamente después de la actualización del dispositivo. Tendrá que restaurarlos de forma manual.<br>Los pods que se eliminan o finalizan por alguna razón, como un error en un nodo o una actualización del nodo que genere problemas, se sustituyen por conjuntos de réplicas. Por este motivo, es conveniente que utilice un conjunto de réplicas aunque la aplicación solamente necesite un único pod.|
|**8.**|Clúster de Kubernetes|Kubernetes solo se admite en Azure Stack Edge Pro con Helm v3 o una versión posterior. Para más información, consulte las [preguntas frecuentes: eliminación de Tiller](https://v3.helm.sh/docs/faq/).|
|**9.**|Kubernetes habilitado para Azure Arc |En el caso de la versión GA, se ha actualizado Kubernetes habilitado para Azure Arc de la versión 0.1.18 a la 0.2.9. Dado que la actualización de Kubernetes habilitado para Azure Arc no es compatible con el dispositivo Azure Stack Edge, tendrá que volver a implementar Kubernetes habilitado para Azure Arc.|Siga estos pasos:<ol><li>[Aplique las actualizaciones del software del dispositivo y Kubernetes](azure-stack-edge-gpu-install-update.md).</li><li>Conéctese a la [interfaz de PowerShell del dispositivo](azure-stack-edge-gpu-connect-powershell-interface.md).</li><li>Quite el agente de Azure Arc existente. Escriba: `Remove-HcsKubernetesAzureArcAgent`.</li><li>Implemente [Azure Arc en un nuevo recurso](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md). No use un recurso de Azure Arc existente.</li></ol>|
|**10.**|Kubernetes habilitado para Azure Arc|No se admiten implementaciones de Azure Arc si hay un proxy web configurado en el dispositivo Azure Stack Edge Pro.||
|**11.**|Kubernetes |El puerto 31000 está reservado para el panel de Kubernetes. El puerto 31001 está reservado para el registro de contenedor de Edge. De forma similar, en la configuración predeterminada, las direcciones IP 172.28.0.1 y 172.28.0.10 se reservan para el servicio Kubernetes y el servicio DNS principal, respectivamente.|No use las direcciones IP reservadas.|
|**12.**|Kubernetes |Actualmente, Kubernetes no permite servicios LoadBalancer con varios protocolos. Por ejemplo, un servicio DNS que tuviese que escuchar en TCP y UDP. |Para solucionar esta limitación de Kubernetes con MetalLB, se pueden crear dos servicios, uno para TCP y otro para UDP, en el mismo selector del pod. Estos servicios usan la misma clave de uso compartido y el mismo valor de spec.loadBalancerIP para compartir la misma dirección IP. También puede compartir las direcciones IP si tiene más servicios que direcciones IP disponibles. <br> Para obtener más información, consulte [Uso compartido de las direcciones IP](https://metallb.universe.tf/usage/#ip-address-sharing).|
|**13.**|Clúster de Kubernetes|Es posible que los módulos de Azure IoT Edge Marketplace existentes necesiten modificaciones para que se ejecuten en IoT Edge en el dispositivo Azure Stack Edge.|Para más información, consulte el tema sobre modificación de los módulos de Azure IoT Edge de Marketplace para que se ejecuten en un dispositivo Azure Stack Edge.<!-- insert link-->|
|**14.**|Kubernetes |En un dispositivo Azure Stack Edge, no se admiten los montajes de enlace basados en archivos con Azure IoT Edge en Kubernetes.|IoT Edge usa una capa de traducción para traducir las opciones `ContainerCreate` a construcciones de Kubernetes. La creación de asignaciones de `Binds` al directorio `hostpath` y los posteriores montajes de los enlaces basados en archivos no se pueden enlazar a rutas de acceso en los contenedores de IoT Edge. Si es posible, asigne el directorio principal.|
|**15.**|Kubernetes |Si trae sus propios certificados para IoT Edge y los agrega al dispositivo Azure Stack Edge después de que el proceso se haya configurado en el dispositivo, los nuevos certificados no se seleccionarán.|Para solucionar este problema, debe cargar los certificados antes de configurar el proceso en el dispositivo. Si el proceso ya está configurado, [Conéctese a la interfaz de PowerShell del dispositivo y ejecute los comandos de IoT Edge](azure-stack-edge-gpu-connect-powershell-interface.md#use-iotedge-commands). Reinicie los pods `iotedged` y `edgehub`.|
|**16.**|Certificados |En determinados casos, el estado del certificado en la interfaz de usuario local puede tardar varios segundos en actualizarse. |Los escenarios siguientes en la interfaz de usuario local pueden verse afectados.<ul><li>La columna **Estado** en la página **Certificados**.</li><li>El icono **Seguridad** en la página de **Introducción**.</li><li>El mosaico **Configuración** en la página de **Información general**.</li></ul>  |
|**17.**|IoT Edge |Los módulos implementados a través de IoT Edge no pueden usar la red de host. | |
|**18.**|Compute + Kubernetes |Compute/Kubernetes no admite el proxy web NTLM. ||
|**19.**|Kubernetes + actualización |Las versiones de software anteriores, como las de 2008, tienen un problema de actualización de condición de carrera que hace que la actualización genere un error con ClusterConnectionException. |El uso de las compilaciones más recientes debería solucionar esta incidencia. Si sigue viendo este problema, vuelva a intentar la actualización y debería funcionar.|
|**20**|Internet Explorer|Si las características de seguridad mejorada están habilitadas, es posible que no pueda acceder a las páginas de la interfaz de usuario web local. | Deshabilite la seguridad mejorada y reinicie el explorador.|


<!--|**18.**|Azure Private Edge Zone (Preview) |There is a known issue with Virtual Network Function VM if the VM was created on Azure Stack Edge device running earlier preview builds such as 2006/2007b and then the device was updated to 2009 GA release. The issue is that the VNF information can't be retrieved or any new VNFs can't be created unless the VNF VMs are deleted before the device is updated.  |Before you update Azure Stack Edge device to 2009 release, use the PowerShell command `get-mecvnf` followed by `remove-mecvnf <VNF guid>` to remove all Virtual Network Function VMs one at a time. After the upgrade, you will need to redeploy the same VNFs.|-->


## <a name="next-steps"></a>Pasos siguientes

- [Actualización de dispositivos](azure-stack-edge-gpu-install-update.md)
