---
title: Solucionar problemas del agente de Linux de Azure Log Analytics | Microsoft Docs
description: Se describen los síntomas, las causas y las soluciones de los problemas más comunes que surgen con el agente de Log Analytics para Linux en Azure Monitor.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/21/2021
ms.openlocfilehash: fce62f3bbe5a3eca29f89cb47c98df6485d1d123
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130216568"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-linux"></a>Cómo solucionar problemas relacionados con el agente de Log Analytics para Linux

En este artículo se proporciona información sobre los errores que es posible que experimente con el agente de Log Analytics para Linux en Azure Monitor y se sugieren posibles soluciones para resolverlos.

## <a name="log-analytics-troubleshooting-tool"></a>Herramienta de solución de problemas de Log Analytics

La herramienta de solución de problemas de Linux del agente de Log Analytics es un script diseñado para ayudar a buscar y diagnosticar problemas con el agente de Log Analytics. Se incluye automáticamente con el agente en la instalación. La ejecución de la herramienta debe ser el primer paso para diagnosticar un problema.

### <a name="how-to-use"></a>Cómo se usa

La herramienta de solución de problemas se puede ejecutar pegando el siguiente comando en una ventana de terminal en un equipo con el agente de Log Analytics: `sudo /opt/microsoft/omsagent/bin/troubleshooter`

### <a name="manual-installation"></a>Instalación manual

La herramienta de solución de problemas se incluye automáticamente al instalar el agente de Log Analytics. Sin embargo, si se produce algún error en la instalación, también se puede instalar manualmente siguiendo estos pasos.

1. Asegúrese de que el [depurador de proyectos de GNU (GDB)](https://www.gnu.org/software/gdb/) está instalado en la máquina, ya que el solucionador de problemas depende de él.
2. Copie el paquete del solucionador de problemas en el equipo: `wget https://raw.github.com/microsoft/OMS-Agent-for-Linux/master/source/code/troubleshooter/omsagent_tst.tar.gz`
3. Desempaquete el paquete: `tar -xzvf omsagent_tst.tar.gz`
4. Ejecute la instalación manual: `sudo ./install_tst`

### <a name="scenarios-covered"></a>Escenarios descritos

A continuación se muestra una lista de escenarios comprobados por la herramienta de solución de problemas:

1. El agente tiene un estado incorrecto, el latido no funciona correctamente.
2. El agente no se inicia, no se puede conectar a los servicios de Log Analytics.
3. El syslog del agente no funciona.
4. El agente tiene un uso elevado de CPU/memoria.
5. El agente tiene problemas de instalación.
6. Los registros personalizados del agente no funcionan.
7. Recopilación de registros del agente

Para obtener más información, consulte la [documentación de GitHub](https://github.com/microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting-Tool.md).

 > [!NOTE]
 > Ejecute la herramienta de recopilador de registros cuando experimente un problema. Disponer inicialmente de los registros ayudará a nuestro equipo de soporte técnico a solucionar el problema más rápido.

## <a name="purge-and-re-install-the-linux-agent"></a>Purga y reinstalación del agente de Linux

Hemos visto que una reinstalación limpia del agente corregirá la mayoría de los problemas. De hecho, esta puede ser la primera sugerencia del equipo de soporte técnico: que el agente entre en un estado no dañado. La ejecución del solucionador de problemas, la recopilación de registros y el intento de una reinstalación limpia ayudarán a resolver los problemas más rápidamente.

1. Descargue el script de purga:
- `$ wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/purge_omsagent.sh`
2. Ejecute el script de purga (con permisos sudo):
- `$ sudo sh purge_omsagent.sh`

## <a name="important-log-locations-and-log-collector-tool"></a>Ubicaciones de registro importantes y herramienta de recopilador de registros

 Archivo | Path
 ---- | -----
 Archivo de registro del agente de Log Analytics para Linux | `/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
 Archivo de registro de configuración del agente de Log Analytics | `/var/opt/microsoft/omsconfig/omsconfig.log`

 Se recomienda usar nuestra herramienta de recopilador de registros para recuperar registros importantes para solucionar problemas o antes de enviar un problema de GitHub. Puede leer más acerca de la herramienta y cómo ejecutarla [aquí](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md).

## <a name="important-configuration-files"></a>Archivos de configuración importantes

 Category | Ubicación del archivo
 ----- | -----
 syslog | `/etc/syslog-ng/syslog-ng.conf`, `/etc/rsyslog.conf` o `/etc/rsyslog.d/95-omsagent.conf`
 Rendimiento, Nagios, Zabbix, salida de Log Analytics y agente general | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`
 Configuraciones adicionales | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/*.conf`

 > [!NOTE]
 > Edición de archivos de configuración para los contadores de rendimiento, y Syslog se sobrescribe si la colección se configura desde el [menú de datos Configuración avanzada de Log Analytics](../agents/agent-data-sources.md#configuring-data-sources) en Azure Portal para el área de trabajo. Con el fin de deshabilitar la configuración para todos los agentes, deshabilite la colección en **Configuración avanzada** de Log Analytics; en el caso de un único agente, ejecute lo siguiente: `sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable && sudo rm /etc/opt/omi/conf/omsconfig/configuration/Current.mof* /etc/opt/omi/conf/omsconfig/configuration/Pending.mof*`

## <a name="installation-error-codes"></a>Códigos de error de instalación

| Código de error | Significado |
| --- | --- |
| NOT_DEFINED | Dado que las dependencias necesarias no están instaladas, no se instalará el complemento de auoms auditd. No se pudo instalar auoms, instale el paquete auditd. |
| 2 | Opción no válida proporcionada a la agrupación de shell. Ejecute `sudo sh ./omsagent-*.universal*.sh --help` para el uso |
| 3 | Ninguna opción proporcionada a la agrupación de shell. Ejecute `sudo sh ./omsagent-*.universal*.sh --help` para el uso. |
| 4 | Tipo de paquete no válido o configuración de proxy no válida; los paquetes omsagent-*rpm*.sh solo pueden instalarse en sistemas basados en RPM, y los paquetes omsagent-*deb*.sh solo pueden instalarse en sistemas basados en Debian. Se recomienda usar el instalador universal de la [versión más reciente](../vm/monitor-virtual-machine.md#agents). Además, revise para comprobar la configuración de proxy. |
| 5 | La agrupación de shell se debe ejecutar como raíz, o bien se devolvió el error 403 durante la incorporación. Ejecute el comando con `sudo`. |
| 6 | Arquitectura de paquete no válida, o bien se devolvió el error 200 durante la incorporación; los paquetes omsagent-\*x64.sh solo pueden instalarse en sistemas de 64 bits, y los paquetes omsagent-\*x86.sh solo pueden instalarse en sistemas de 32 bits. Descargue el paquete correcto para su arquitectura de la [versión más reciente](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/latest). |
| 17 | No se pudo instalar el paquete de OMS. Examine el resultado del comando para conocer el error raíz. |
| 18 | Error en la instalación del paquete OMSConfig. Examine el resultado del comando para conocer el error raíz. |
| 19 | No se pudo instalar el paquete de OMI. Examine el resultado del comando para conocer el error raíz. |
| 20 | No se pudo instalar el paquete de SCX. Examine el resultado del comando para conocer el error raíz. |
| 21 | No se pudieron instalar los kits del proveedor. Examine el resultado del comando para conocer el error raíz. |
| 22 | No se pudo instalar el paquete integrado. Examine el resultado del comando para conocer el error raíz |
| 23 | El paquete de SCX u OMI ya está instalado. Use `--upgrade` en lugar de `--install` para instalar la agrupación de shell. |
| 30 | Error de agrupación interno. Registre un [problema de GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) con los detalles del resultado. |
| 55 | Versión de openssl no compatible, o bien no se puede conectar con Azure Monitor, o dpkg está bloqueado, o falta el programa curl. |
| 61 | Falta la biblioteca ctypes de Python. Instale la biblioteca ctypes de Python o el paquete (python-ctypes). |
| 62 | Falta el programa tar, instale tar. |
| 63 | Falta el programa sed, instale sed. |
| 64 | Falta el programa curl, instale curl. |
| 65 | Falta el programa gpg, instale gpg. |

## <a name="onboarding-error-codes"></a>Códigos de error de incorporación

| Código de error | Significado |
| --- | --- |
| 2 | Opción no válida proporcionada al script omsadmin. Ejecute `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` para el uso. |
| 3 | Configuración no válida proporcionada al script omsadmin. Ejecute `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` para el uso. |
| 4 | Proxy no válido proporcionado al script omsadmin. Compruebe el proxy y consulte nuestra [documentación para usar un proxy HTTP](./log-analytics-agent.md#firewall-requirements). |
| 5 | Error HTTP 403 recibido de Azure Monitor. Vea el resultado completo del script omsadmin para obtener más información. |
| 6 | Error HTTP distinto de 200 recibido de Azure Monitor. Vea el resultado completo del script omsadmin para obtener más información. |
| 7 | No se puede conectar a Azure Monitor. Vea el resultado completo del script omsadmin para obtener más información. |
| 8 | Error en la incorporación al área de trabajo de Log Analytics. Vea el resultado completo del script omsadmin para obtener más información. |
| 30 | Error interno de script. Registre un [problema de GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) con los detalles del resultado. |
| 31 | Error al generar identificador de agente. Registre un [problema de GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) con los detalles del resultado. |
| 32 | Error al generar los certificados. Vea el resultado completo del script omsadmin para obtener más información. |
| 33 | Error al generar la metaconfiguración para omsconfig. Registre un [problema de GitHub](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) con los detalles del resultado. |
| 34 | El script de generación de la metaconfiguración no está presente. Vuelva a intentar la incorporación con `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key>`. |

## <a name="enable-debug-logging"></a>Habilitación del registro de depuración

### <a name="oms-output-plugin-debug"></a>Depuración del complemento de salida de OMS

 FluentD permite niveles de registro específicos del complemento, lo que le permite especificar distintos niveles de registro para entradas y salidas. Para especificar un nivel de registro distinto para la salida de OMS, edite la configuración del agente general en `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.

 En el complemento de salida de OMS, antes del final del archivo de configuración, cambie la propiedad `log_level` de `info` a `debug`:

```
<match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

El registro de depuración permite ver cargas por lotes a Azure Monitor separadas por tipo, número de elementos de datos y tiempo de envío:

*Ejemplo de registro con depuración habilitada:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

### <a name="verbose-output"></a>Salida detallada

En lugar de usar el complemento de salida de OMS, también puede generar la salida de elementos de datos directamente a `stdout`, que es visible en el archivo de registro del agente de Log Analytics para Linux.

En el archivo de configuración del agente general de Log Analytics en `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, convierta en comentario el complemento de salida de OMS agregando `#` al principio de cada línea:

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Debajo del complemento de salida, quite la marca de comentario de la siguiente sección quitando el `#` delante de cada línea:

```
<match **>
  type stdout
</match>
```

## <a name="issue--unable-to-connect-through-proxy-to-azure-monitor"></a>Problema:  No es posible establecer la conexión a través del proxy a Azure Monitor.

### <a name="probable-causes"></a>Causas probables

* El proxy especificado durante la incorporación era incorrecto.
* Los puntos de conexión de servicio de Azure Monitor y Azure Automation no están en la lista de permitidos en el centro de datos.

### <a name="resolution"></a>Solución

1. Vuelva a incorporase a Azure Monitor con el agente de Log Analytics para Linux mediante el siguiente comando con la opción `-v` habilitada. Así se permite la salida detallada del agente que se conecta a través del proxy a Azure Monitor. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key> -p <Proxy Conf> -v`

2. Revise la sección [Actualizar la configuración de proxy](agent-manage.md#update-proxy-settings) para comprobar que el agente se haya configurado correctamente para comunicarse a través de un servidor proxy.

3. Asegúrese de que los puntos de conexión descritos en la lista de [requisitos de firewall de red](./log-analytics-agent.md#firewall-requirements) de Azure Monitor se agregan correctamente a una lista de permitidos. Si usa Azure Automation, en el vínculo también se incluyen los pasos de configuración de red necesarios.

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problema: Recibe un error 403 al intentar incorporarse

### <a name="probable-causes"></a>Causas probables

* La fecha y la hora son incorrectas en el servidor Linux
* El identificador y la clave de área de trabajo usados son incorrectos

### <a name="resolution"></a>Solución

1. Compruebe la hora en el servidor Linux con el comando date. Si la hora es +/-15 minutos de la hora actual, la incorporación produce un error. Para corregir este problema, actualice la fecha o la zona horaria del servidor Linux.
2. Compruebe que ha instalado la versión más reciente del agente de Log Analytics para Linux.  La versión más reciente notifica ahora si el desfase temporal está causando el error de incorporación.
3. Vuelva a realizar la incorporación con el identificador de área de trabajo y la clave de área de trabajo correctos según las instrucciones de instalación de este artículo.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problema: Ve los errores 404 y 500 en el archivo de registro justo después de la incorporación

Se trata de un problema conocido que se produce con la primera carga de datos de Linux en un área de trabajo de Log Analytics. Esto no afecta a los datos que se envían ni a la experiencia del servicio.

## <a name="issue-you-see-omiagent-using-100-cpu"></a>Problema: Se indica el proceso omiagent usa el 100 % de la CPU

### <a name="probable-causes"></a>Causas probables

Una regresión en el paquete nss-pem [v1.0.3 5.el7](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-7.el7.x86_64.rpm.html) causó un problema de rendimiento grave, que hemos observado con frecuencia en las distribuciones Redhat/Centos 7.x. Para más información sobre este problema, consulte la siguiente documentación: Bug [1667121 Performance regression in libcurl](https://bugzilla.redhat.com/show_bug.cgi?id=1667121) (Error 1667121 Regresión del rendimiento en libcurl).

Los errores relacionados con el rendimiento no se producen constantemente y son muy difíciles de reproducir. Si experimenta este problema con omiagent, debe usar el script omiHighCPUDiagnostics.sh, que recopilará el seguimiento de pila de omiagent cuando se supere un umbral determinado.

1. Descarga del script <br/>
`wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/LogCollector/source/omiHighCPUDiagnostics.sh`

2. Ejecute el diagnóstico durante 24 horas con un umbral de la CPU del 30 % <br/>
`bash omiHighCPUDiagnostics.sh --runtime-in-min 1440 --cpu-threshold 30`

3. La pila de llamadas se volcará en el archivo omiagent_trace. Si observa muchas llamadas a las funciones Curl y NSS, realice los pasos de resolución siguientes.

### <a name="resolution-step-by-step"></a>Resolución (paso a paso)

1. Actualice el paquete nss-pem a [v1.0.3 5.el7_6.1](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-7.el7.x86_64.rpm.html). <br/>
`sudo yum upgrade nss-pem`

2. Si el paquete nss-pem no está disponible para la actualización (sucede principalmente en Centos), degrade curl a la versión 7.29.0-46. Si por error ejecuta la "actualización de yum", curl se actualizará a la versión 7.29.0-51 y volverá a producirse el problema. <br/>
`sudo yum downgrade curl libcurl`

3. Reinicie OMI: <br/>
`sudo scxadmin -restart`

## <a name="issue-you-are-not-seeing-forwarded-syslog-messages"></a>Problema: No ve los mensajes de Syslog reenviados

### <a name="probable-causes"></a>Causas probables

* La configuración aplicada al servidor Linux no permite la recopilación de las utilidades enviadas ni de los niveles de registro.
* Syslog no se reenvía correctamente al servidor Linux.
* El número de mensajes que se reenvía por segundo es demasiado grande para la configuración básica del agente de Log Analytics para Linux

### <a name="resolution"></a>Solución

* Compruebe que la configuración de Syslog en el área de trabajo de Log Analytics tenga todas las utilidades y los niveles de registro correctos. Revise [Configuración de Syslog en Azure Portal](data-sources-syslog.md#configure-syslog-in-the-azure-portal).
* Compruebe que los demonios de mensajería de Syslog nativos (`rsyslog`, `syslog-ng`) puedan recibir los mensajes reenviados.
* Compruebe la configuración de firewall en el servidor de Syslog para asegurarse de que no se estén bloqueando los mensajes.
* Simule un mensaje de Syslog en Log Analytics con el comando `logger`.
  * `logger -p local0.err "This is my test message"`

## <a name="issue-you-are-receiving-errno-address-already-in-use-in-omsagent-log-file"></a>Problema: Recibe el error de que la dirección Errno ya está en uso en el archivo de registro de omsagent

Si ve `[error]: unexpected error error_class=Errno::EADDRINUSE error=#<Errno::EADDRINUSE: Address already in use - bind(2) for "127.0.0.1" port 25224>` en omsagent.log.

### <a name="probable-causes"></a>Causas probables

Este error indica que la extensión Diagnostics de Linux (LAD) está instalada en paralelo con la extensión de VM Linux de Log Analytics, y usa el mismo puerto para la colección de datos de syslog que omsagent.

### <a name="resolution"></a>Solución

1. Como raíz, ejecute los comandos siguientes (tenga en cuenta que 25224 es un ejemplo y es posible que en su entorno vea que LAD usa otro número de puerto):

    ```
    /opt/microsoft/omsagent/bin/configure_syslog.sh configure LAD 25229

    sed -i -e 's/25224/25229/' /etc/opt/microsoft/omsagent/LAD/conf/omsagent.d/syslog.conf
    ```

    A continuación, tiene que editar el archivo de configuración correcto, `rsyslogd` o `syslog_ng`, y cambiar la configuración relacionada con LAD para escribir en el puerto 25229.

2. Si la VM ejecuta `rsyslogd`, el archivo que debe modificar es: `/etc/rsyslog.d/95-omsagent.conf` (si existe, de lo contrario, `/etc/rsyslog`). Si la VM ejecuta `syslog_ng`, el archivo que debe modificar es: `/etc/syslog-ng/syslog-ng.conf`.
3. Reinicie omsagent `sudo /opt/microsoft/omsagent/bin/service_control restart`.
4. Reinicie el servicio syslog.

## <a name="issue-you-are-unable-to-uninstall-omsagent-using-purge-option"></a>Problema: No puede desinstalar omsagent con la opción purge

### <a name="probable-causes"></a>Causas probables

* La extensión Diagnostics de Linux está instalada
* La extensión Diagnostics de Linux se instaló y desinstaló, pero todavía aparece un error que indica que mdsd usa omsagent, por lo que no se puede quitar.

### <a name="resolution"></a>Solución

1. Desinstale la extensión Diagnostics de Linux (LAD).
2. Quite los archivos de la extensión Diagnostics de Linux de la máquina si están presentes en la siguiente ubicación: `/var/lib/waagent/Microsoft.Azure.Diagnostics.LinuxDiagnostic-<version>/` y `/var/opt/microsoft/omsagent/LAD/`.

## <a name="issue-you-cannot-see-data-any-nagios-data"></a>Problema: No ve ningún dato de Nagios

### <a name="probable-causes"></a>Causas probables

* El usuario omsagent no tiene permisos para leer el archivo de registro de Nagios
* No se quitó la marca de comentario de origen y filtro y de Nagios del archivo omsagent.conf

### <a name="resolution"></a>Solución

1. Para agregar el usuario omsagent para leer del archivo de Nagios, siga estas [instrucciones](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts).
2. En el archivo de configuración general del agente de Log Analytics para Linux en `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`, asegúrese de que se haya quitado la marca de comentarios **tanto** del origen como del filtro de Nagios.

    ```
    <source>
      type tail
      path /var/log/nagios/nagios.log
      format none
      tag oms.nagios
    </source>

    <filter oms.nagios>
      type filter_nagios_log
    </filter>
    ```

## <a name="issue-you-are-not-seeing-any-linux-data"></a>Problema: No ve ningún dato de Linux

### <a name="probable-causes"></a>Causas probables

* Error de incorporación a Azure Monitor
* La conexión a Azure Monitor está bloqueada
* La máquina virtual se reinició
* El paquete de OMI se actualizó manualmente a una versión más reciente en comparación con la que instaló el paquete del agente de Log Analytics para Linux
* OMI está detenido, se está bloqueando el agente de OMS
* Error *class not found* de los registros de recursos de DSC en el archivo de registro `omsconfig.log`
* Se está haciendo la copia de seguridad de los datos del agente de Log Analytics
* DSC registra *Current configuration does not exist. Execute Start-DscConfiguration command with -Path parameter to specify a configuration file and create a current configuration first.* en el archivo de registro `omsconfig.log`, pero no existe ningún mensaje de registro sobre las operaciones de `PerformRequiredConfigurationChecks`.

### <a name="resolution"></a>Solución

1. Instale todas las dependencias como el paquete auditd.
2. Para comprobar si la incorporación a Azure Monitor se realizó correctamente, asegúrese de que existe el archivo siguiente: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.  Si no existe, repita la incorporación con las [instrucciones](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) de la línea de comandos de omsadmin.sh.
4. Si usa un proxy, compruebe los pasos anteriores para solucionar problemas con un proxy.
5. En algunos sistemas de distribución de Azure, el demonio de servidor OMI omid no se inicia después de reiniciar la máquina virtual. Esto provocará que no se muestren los datos relacionados con la solución de Audit, ChangeTracking ni UpdateManagement. La solución alternativa consiste en iniciar manualmente el servidor omi ejecutando `sudo /opt/omi/bin/service_control restart`.
6. Después de que el paquete OMI se actualice manualmente a una versión más reciente, debe reiniciarse manualmente para que el agente de Log Analytics siga funcionando. Este paso es necesario para algunas distribuciones donde el servidor OMI no se inicia automáticamente tras su actualización. Ejecute `sudo /opt/omi/bin/service_control restart` para reiniciar OMI.
* En algunas situaciones, OMI puede detenerse. El agente de OMS puede entrar en un estado bloqueado en espera de OMI, bloqueando toda la recopilación de datos. El proceso del agente de OMS se ejecutará, pero no habrá ninguna actividad, como evidenciará que no haya ninguna nueva línea de registro (como los latidos enviados) presente en `omsagent.log`. Reinicie OMI con `sudo /opt/omi/bin/service_control restart` para recuperar el agente.
7. Si ve el error *class not found* en el recurso DSC en omsconfig.log, ejecute `sudo /opt/omi/bin/service_control restart`.
8. En algunos casos, cuando el agente de Log Analytics para Linux no puede comunicarse con Azure Monitor, se crea una copia de seguridad de los datos del agente del tamaño de búfer total: 50 MB. Se debe reiniciar el agente ejecutando el siguiente comando `/opt/microsoft/omsagent/bin/service_control restart`.

    > [!NOTE]
    > Este problema se ha corregido en la versión 1.1.0-28 y posteriores del agente.
    >

* Si el archivo de registro `omsconfig.log` no indica que las operaciones `PerformRequiredConfigurationChecks` se ejecutan periódicamente en el sistema, podría haber un problema con el servicio o con el trabajos cron. Asegúrese de que exista el trabajo cron en `/etc/cron.d/OMSConsistencyInvoker`. De ser necesario, ejecute los siguientes comandos para crear el trabajo cron:

    ```
    mkdir -p /etc/cron.d/
    echo "*/15 * * * * omsagent /opt/omi/bin/OMSConsistencyInvoker >/dev/null 2>&1" | sudo tee /etc/cron.d/OMSConsistencyInvoker
    ```

    Además, asegúrese de que el servicio cron se esté ejecutando. Puede usar `service cron status` con Debian, Ubuntu, SUSE, o `service crond status` con RHEL, CentOS, Oracle Linux para comprobar el estado de este servicio. Si el servicio no existe, puede instalar los archivos binarios e iniciar el servicio mediante lo siguiente:

    **Ubuntu/Debian**

    ```
    # To Install the service binaries
    sudo apt-get install -y cron
    # To start the service
    sudo service cron start
    ```

    **SUSE**

    ```
    # To Install the service binaries
    sudo zypper in cron -y
    # To start the service
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

    **RHEL/CeonOS**

    ```
    # To Install the service binaries
    sudo yum install -y crond
    # To start the service
    sudo service crond start
    ```

    **Oracle Linux**

    ```
    # To Install the service binaries
    sudo yum install -y cronie
    # To start the service
    sudo service crond start
    ```

## <a name="issue-when-configuring-collection-from-the-portal-for-syslog-or-linux-performance-counters-the-settings-are-not-applied"></a>Problema: Al configurar la colección desde el portal para los contadores de rendimiento de Syslog o Linux, la configuración no se aplica

### <a name="probable-causes"></a>Causas probables

* El agente de Log Analytics para Linux no ha seleccionado la configuración más reciente
* No se aplicó la configuración cambiada en el portal

### <a name="resolution"></a>Solución

**Información previa:** `omsconfig` es el agente de configuración del agente de Log Analytics para Linux que cada cinco minutos busca una nueva configuración del portal. Esta configuración se aplica luego en los archivos de configuración del agente de Log Analytics para Linux ubicados en /etc/opt/microsoft/omsagent/conf/omsagent.conf.

* En algunos casos, es posible que el agente de configuración del agente de Log Analytics para Linux no pueda comunicarse con el servicio de configuración del portal y, por tanto, no se aplique la configuración más reciente.
  1. Para comprobar que el agente `omsconfig` está instalado, ejecute `dpkg --list omsconfig` o `rpm -qi omsconfig`.  Si no está instalado, vuelva a instalar la versión más reciente del agente de Log Analytics para Linux.

  2. Para comprobar que el agente `omsconfig` puede comunicarse con Azure Monitor, ejecute el siguiente comando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`. Este comando devuelve la configuración que el agente recupera del servicio, incluida la configuración de Syslog, los contadores de rendimiento de Linux y los registros personalizados. Si se produce un error en este comando, ejecute el siguiente comando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`. Este comando fuerza al agente omsconfig a comunicarse con Azure Monitor y a recuperar la configuración más reciente.

## <a name="issue-you-are-not-seeing-any-custom-log-data"></a>Problema: No ve ningún dato de registro personalizado

### <a name="probable-causes"></a>Causas probables

* Error de incorporación a Azure Monitor.
* No se ha seleccionado la opción **Aplicar la configuración que aparece a continuación a mis máquinas con Linux**.
* omsconfig no ha recuperado la configuración del registro personalizado más reciente del servicio.
* El usuario `omsagent` del agente de Log Analytics para Linux no puede acceder al registro personalizado debido a permisos o a que no se encuentra.  Puede que vea los errores siguientes:
* `[DATETIME] [warn]: file not found. Continuing without tailing it.`
* `[DATETIME] [error]: file not accessible by omsagent.`
* Un problema conocido con la condición de carrera se ha corregido en el agente de Log Analytics para Linux versión 1.1.0-217

### <a name="resolution"></a>Solución

1. Para comprobar si la incorporación a Azure Monitor se realizó correctamente, asegúrese de que existe el archivo siguiente: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`. Si no existe:

  1. Repita la incorporación con las [instrucciones](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) de la línea de comandos de omsadmin.sh.
  2. En **Configuración avanzada** en Azure Portal, asegúrese de que la opción **Aplicar la configuración que aparece a continuación a mis máquinas con Linux** esté habilitada.

2. Para comprobar que el agente `omsconfig` puede comunicarse con Azure Monitor, ejecute el siguiente comando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`.  Este comando devuelve la configuración que el agente recupera del servicio, incluida la configuración de Syslog, los contadores de rendimiento de Linux y los registros personalizados. Si se produce un error en este comando, ejecute el siguiente comando `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`. Este comando fuerza al agente omsconfig a comunicarse con Azure Monitor y a recuperar la configuración más reciente.

**Información previa:** En lugar de que el agente de Log Analytics para Linux se ejecute como usuario con privilegios, `root`, el agente se ejecuta como el usuario `omsagent`. En la mayoría de los casos, se deben conceder permisos explícitos al usuario para que lea determinados archivos. Para conceder permiso al usuario `omsagent`, ejecute los siguientes comandos:

1. Agregue el usuario `omsagent` a un grupo específico `sudo usermod -a -G <GROUPNAME> <USERNAME>`.
2. Concédale acceso de lectura universal al archivo necesario `sudo chmod -R ugo+rx <FILE DIRECTORY>`.

Existe un problema conocido con la condición de carrera en el agente de Log Analytics para Linux antes de la versión 1.1.0-217. Después de actualizar al agente más reciente, ejecute el siguiente comando para obtener la versión más reciente del complemento de salida: `sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`.

## <a name="issue-you-are-trying-to-reonboard-to-a-new-workspace"></a>Problema: Intenta repetir la incorporación en una nueva área de trabajo

Cuando intenta repetir la incorporación de un agente en una nueva área de trabajo, la configuración del agente de Log Analytics debe limpiarse antes de repetir la incorporación. Para limpiar la configuración anterior del agente, ejecute la agrupación de shell con `--purge`

```
sudo sh ./omsagent-*.universal.x64.sh --purge
```

Or

```
sudo sh ./onboard_agent.sh --purge
```

Puede continuar con la repetición de la incorporación después de usar la opción `--purge`

## <a name="log-analytics-agent-extension-in-the-azure-portal-is-marked-with-a-failed-state-provisioning-failed"></a>La extensión del agente de Log Analytics en Azure Portal está marcada con un estado de error: error de aprovisionamiento.

### <a name="probable-causes"></a>Causas probables

* Se quitó el agente de Log Analytics del sistema operativo
* El servicio de agente de Log Analytics está inactivo, deshabilitado o no está configurado

### <a name="resolution"></a>Solución

Siga los pasos a continuación para corregir el problema.
1. Quite la extensión de Azure Portal.
2. Instale el agente siguiendo las [instrucciones](../vm/monitor-virtual-machine.md).
3. Reinicie el agente con el comando siguiente: `sudo /opt/microsoft/omsagent/bin/service_control restart`.
* Espere unos minutos y el estado de aprovisionamiento cambia a **Aprovisionamiento realizado correctamente**.

## <a name="issue-the-log-analytics-agent-upgrade-on-demand"></a>Problema: Actualización a petición del agente de Log Analytics

### <a name="probable-causes"></a>Causas probables

Los paquetes de agente de Log Analytics en el host no están desactualizados.

### <a name="resolution"></a>Solución

Siga los pasos a continuación para corregir el problema.

1. Busque la versión más reciente en esta [página](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/).
2. Descargue script de instalación (1.4.2-124 como versión de ejemplo):

    ```
    wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.2-124/omsagent-1.4.2-124.universal.x64.sh
    ```

3. Para actualizar los paquetes, ejecute `sudo sh ./omsagent-*.universal.x64.sh --upgrade`.

## <a name="issue-installation-is-failing-saying-python2-cannot-support-ctypes-even-though-python3-is-being-used"></a>Problema: la instalación produce un error que dice que Python2 no admite ctypes, a pesar de que se está utilizando Python3

### <a name="probable-causes"></a>Causas probables

Hay un problema conocido por el que, si el idioma de la máquina virtual no es inglés, se producirá un error al comprobar qué versión de Python se está utilizando. Esto hace que el agente siempre asuma que se usa Python2 y que se produzca un error si no hay Python2.

### <a name="resolution"></a>Resolución

Cambie el idioma del entorno de la máquina virtual a inglés:

```
export LANG=en_US.UTF-8
```
