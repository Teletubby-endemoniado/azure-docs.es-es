---
title: 'Azure Cloud Services (clásico): Esquema de definición de Azure Cloud Services | Microsoft Docs'
description: El rol de trabajo de Azure se usa para el desarrollo generalizado y puede realizar el procesamiento en segundo plano para un rol web. Obtenga información sobre el esquema de rol de trabajo de Azure.
ms.topic: article
ms.service: cloud-services
ms.subservice: deployment-files
ms.date: 10/14/2020
author: hirenshah1
ms.author: hirshah
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 2c891cd11174645cdbc574c7526fd0011b397e11
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122823809"
---
# <a name="azure-cloud-services-classic-definition-workerrole-schema"></a>Esquema WorkerRole de definición de Azure Cloud Services (clásico)

[!INCLUDE [Cloud Services (classic) deprecation announcement](includes/deprecation-announcement.md)]

El rol de trabajo de Azure es un rol que resulta útil para el desarrollo generalizado; además, puede realizar procesamiento en segundo plano para un rol web.

La extensión predeterminada del archivo de definición de servicio es. csdef.

## <a name="basic-service-definition-schema-for-a-worker-role"></a>Esquema básico de definición de servicio para un rol de trabajo.
El formato básico del archivo de definición de servicio que contiene un rol de trabajo es el siguiente.

```xml
<ServiceDefinition …>
  <WorkerRole name="<worker-role-name>" vmsize="<worker-role-size>" enableNativeCodeExecution="[true|false]">
    <Certificates>
      <Certificate name="<certificate-name>" storeLocation="[CurrentUser|LocalMachine]" storeName="[My|Root|CA|Trust|Disallow|TrustedPeople|TrustedPublisher|AuthRoot|AddressBook|<custom-store>" />
    </Certificates>
    <ConfigurationSettings>
      <Setting name="<setting-name>" />
    </ConfigurationSettings>
    <Endpoints>
      <InputEndpoint name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<local-port-number>" port="<port-number>" certificate="<certificate-name>" loadBalancerProbe="<load-balancer-probe-name>" />
      <InternalEndpoint name="<internal-endpoint-name" protocol="[http|tcp|udp|any]" port="<port-number>">
         <FixedPort port="<port-number>"/>
         <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
      </InternalEndpoint>
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">
         <AllocatePublicPortFrom>
            <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
         </AllocatePublicPortFrom>
      </InstanceInputEndpoint>
    </Endpoints>
    <Imports>
      <Import moduleName="[RemoteAccess|RemoteForwarder|Diagnostics]"/>
    </Imports>
    <LocalResources>
      <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />
    </LocalResources>
    <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />
    <Runtime executionContext="[limited|elevated]">
      <Environment>
         <Variable name="<variable-name>" value="<variable-value>">
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>
          </Variable>
      </Environment>
      <EntryPoint>
         <NetFxEntryPoint assemblyName="<name-of-assembly-containing-entrypoint>" targetFrameworkVersion="<.net-framework-version>"/>
         <ProgramEntryPoint commandLine="<application>" setReadyOnProcessStart="[true|false]"/>
      </EntryPoint>
    </Runtime>
    <Startup priority="<for-internal-use-only>">
      <Task commandLine="" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">
        <Environment>
         <Variable name="<variable-name>" value="<variable-value>">
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>
          </Variable>
        </Environment>
      </Task>
    </Startup>
    <Contents>
      <Content destination="<destination-folder-name>" >
        <SourceDirectory path="<local-source-directory>" />
      </Content>
    </Contents>
  </WorkerRole>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Elementos de esquema
El archivo de definición de servicio incluye estos elementos, que se describen más detalladamente en las sucesivas secciones de este tema:

[WorkerRole](#WorkerRole)

[ConfigurationSettings](#ConfigurationSettings)

[Configuración](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Extremos](#Endpoints)

[InputEndpoint](#InputEndpoint)

[InternalEndpoint](#InternalEndpoint)

[InstanceInputEndpoint](#InstanceInputEndpoint)

[AllocatePublicPortFrom](#AllocatePublicPortFrom)

[FixedPort](#FixedPort)

[FixedPortRange](#FixedPortRange)

[Certificados](#Certificates)

[Certificate](#Certificate)

[Importaciones](#Imports)

[Importar](#Import)

[Tiempo de ejecución](#Runtime)

[Entorno](#Environment)

[EntryPoint](#EntryPoint)

[NetFxEntryPoint](#NetFxEntryPoint)

[ProgramEntryPoint](#ProgramEntryPoint)

[Variable](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[Startup](#Startup)

[Task](#Task)

[Contents](#Contents)

[Contenido](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="workerrole"></a><a name="WorkerRole"></a> WorkerRole
El elemento `WorkerRole` describe un rol que resulta útil para el desarrollo generalizado; además, puede realizar procesamiento en segundo plano para un rol web. Un servicio puede contener cero o más roles de trabajo.

En la tabla siguiente se describen los atributos del elemento `WorkerRole`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. El nombre del rol de trabajo. El nombre del rol debe ser único.|
|enableNativeCodeExecution|boolean|Opcional. El valor predeterminado es `true`; de forma predeterminada están habilitadas la ejecución de código nativo y la plena confianza. Establezca este atributo en `false` para deshabilitar la ejecución de código nativo para el rol de trabajo y usar en su lugar la confianza parcial de Azure.|
|vmsize|string|Opcional. Establezca este valor para cambiar el tamaño de la máquina virtual que se asigna a este rol. El valor predeterminado es `Small`. Para obtener una lista de tamaños posibles de máquina virtual y sus atributos, consulte los [tamaños de máquina virtual para Cloud Services](cloud-services-sizes-specs.md).|

##  <a name="configurationsettings"></a><a name="ConfigurationSettings"></a> ConfigurationSettings
El elemento `ConfigurationSettings` describe la colección de valores de configuración de un rol de trabajo. Este elemento es el elemento primario del elemento `Setting`.

##  <a name="setting"></a>Configuración de <a name="Setting"></a>
El elemento `Setting` describe un par de nombre y valor que especifica un valor de configuración para una instancia de un rol.

En la tabla siguiente se describen los atributos del elemento `Setting`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. Un nombre único para el valor de configuración.|

Los valores de configuración de un rol son pares de nombre y valor que se declaran en el archivo de definición de servicio y se establecen en el archivo de configuración de servicio.

##  <a name="localresources"></a><a name="LocalResources"></a> LocalResources
El elemento `LocalResources` describe la colección de recursos de almacenamiento local de un rol de trabajo. Este elemento es el elemento primario del elemento `LocalStorage`.

##  <a name="localstorage"></a><a name="LocalStorage"></a> LocalStorage
El elemento `LocalStorage` identifica un recurso de almacenamiento local que proporciona espacio del sistema de archivos para el servicio en tiempo de ejecución. Un rol puede definir cero o más recursos de almacenamiento local.

> [!NOTE]
>  El elemento `LocalStorage` puede aparecer como un elemento secundario del elemento `WorkerRole` para permitir la compatibilidad con versiones anteriores de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `LocalStorage`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. Un nombre único para el almacén local.|
|cleanOnRoleRecycle|boolean|Opcional. Indica si se debe limpiar el almacén local cuando se reinicia el rol. El valor predeterminado es `true`.|
|sizeInMb|int|Opcional. La cantidad deseada de espacio de almacenamiento para asignar al almacén local, en MB. Si no se especifica, el espacio de almacenamiento predeterminado asignado es 100 MB. La cantidad mínima de espacio de almacenamiento que puede asignarse es 1 MB.<br /><br /> El tamaño máximo de los recursos locales depende del tamaño de máquina virtual. Para más información, consulte los [tamaños de máquina virtual para Cloud Services](cloud-services-sizes-specs.md).|

El nombre del directorio asignado al recurso de almacenamiento local corresponde al valor proporcionado para el atributo de nombre.

##  <a name="endpoints"></a><a name="Endpoints"></a> Endpoints
El elemento `Endpoints` describe la colección de puntos de conexión de entrada (externos), internos y de entrada de instancia de un rol. Este elemento es el elemento primario de los elementos `InputEndpoint`, `InternalEndpoint` y `InstanceInputEndpoint`.

Los puntos de conexión de entrada e internos se asignan por separado. Un servicio puede tener un total de veinticinco puntos de conexión de entrada, internos y de entrada de instancia que se pueden asignar entre los veinticinco roles permitidos en un servicio. Por ejemplo, si tiene cinco roles, puede asignar cinco puntos de conexión de entrada por rol, veinticinco puntos de conexión de entrada a un único rol o asignar un punto de conexión de entrada a cada veinticinco roles.

> [!NOTE]
>  Cada rol implementado requiere una instancia por rol. El aprovisionamiento predeterminado de una suscripción está limitado a veinte núcleos y, por tanto, a veinte instancias de un rol. Si la aplicación requiere más instancias que las que se proporcionan con el aprovisionamiento predeterminado, vea las secciones sobre [facturación, administración de suscripciones y compatibilidad con cuotas](https://azure.microsoft.com/support/options/) para más información sobre cómo aumentar la cuota.

##  <a name="inputendpoint"></a><a name="InputEndpoint"></a> InputEndpoint
El elemento `InputEndpoint` describe un punto de conexión externo a un rol de trabajo.

Puede definir varios puntos de conexión que sean una combinación de puntos de conexión HTTP, HTTPS, UDP y TCP. Puede especificar cualquier número de puerto que elija para un punto de conexión de entrada, pero los números de puerto especificados para cada rol del servicio deben ser únicos. Por ejemplo, si especifica que un rol use el puerto 80 para HTTP y el puerto 443 para HTTPS, podría especificar entonces que un segundo rol use el puerto 8080 para HTTP y el puerto 8043 para HTTPS.

En la tabla siguiente se describen los atributos del elemento `InputEndpoint`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. Un nombre único para el punto de conexión externo.|
|protocol|string|Necesario. El protocolo de transporte del punto de conexión externo. Los valores posibles para un rol de trabajo son `HTTP`, `HTTPS`, `UDP` o `TCP`.|
|port|int|Necesario. El puerto del punto de conexión externo. Puede especificar cualquier número de puerto que elija, pero los números de puerto especificados para cada rol del servicio deben ser únicos.<br /><br /> Los valores posibles oscilan entre 1 y 65535, ambos inclusive (versión 1.7 o posterior de Azure SDK).|
|certificado|string|Obligatorio para un punto de conexión HTTPS. El nombre de un certificado definido por un elemento `Certificate`.|
|localPort|int|Opcional. Especifica un puerto usado para las conexiones internas del punto de conexión. El atributo `localPort` asigna el puerto externo del punto de conexión a un puerto interno de un rol. Esto resulta de utilidad en escenarios donde un rol debe comunicarse con un componente interno en un puerto diferente del que se expone externamente.<br /><br /> Si no se especifica, el valor de `localPort` es el mismo que el del atributo `port`. Establezca el valor de `localPort` en "*" para asignar automáticamente un puerto sin asignar que se puede detectar mediante la API en tiempo de ejecución.<br /><br /> Los valores posibles oscilan entre 1 y 65535, ambos inclusive (versión 1.7 o posterior de Azure SDK).<br /><br /> El atributo `localPort` solo está disponible mediante la versión 1.3 o posterior de Azure SDK.|
|ignoreRoleInstanceStatus|boolean|Opcional. Cuando el valor de este atributo se establece en `true`, se omite el estado de un servicio y el equilibrador de carga no quita el punto de conexión. El establecimiento de este valor en `true` resulta de utilidad para depurar instancias ocupadas de un servicio. El valor predeterminado es `false`. **Nota:** Un punto de conexión puede seguir recibiendo tráfico aunque el rol no esté en un estado listo.|
|loadBalancerProbe|string|Opcional. El nombre del sondeo del equilibrador de carga asociado con el punto de conexión de entrada. Para más información, vea [Esquema LoadBalancerProbe](schema-csdef-loadbalancerprobe.md).|

##  <a name="internalendpoint"></a><a name="InternalEndpoint"></a> InternalEndpoint
El elemento `InternalEndpoint` describe un punto de conexión interno a un rol de trabajo. Un punto de conexión solo está disponible para otras instancias de rol que se ejecutan dentro del servicio; no está disponible para los clientes de fuera del servicio. Un rol de trabajo puede tener hasta cinco puntos de conexión HTTP, UDP o TCP.

En la tabla siguiente se describen los atributos del elemento `InternalEndpoint`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. Un nombre único para el punto de conexión interno.|
|protocol|string|Necesario. El protocolo de transporte del punto de conexión interno. Los valores posibles son `HTTP`, `TCP`, `UDP` o `ANY`.<br /><br /> Un valor de `ANY` especifica que se permite cualquier protocolo y cualquier puerto.|
|port|int|Opcional. El puerto usado para las conexiones de carga equilibrada internas del punto de conexión. Un punto de conexión de carga equilibrada usa dos puertos: uno para la dirección IP pública y el otro en la dirección IP privada. Normalmente, estos puertos se establecen en el mismo valor, pero puede elegir usar puertos diferentes.<br /><br /> Los valores posibles oscilan entre 1 y 65535, ambos inclusive (versión 1.7 o posterior de Azure SDK).<br /><br /> El atributo `Port` solo está disponible mediante la versión 1.3 o posterior de Azure SDK.|

##  <a name="instanceinputendpoint"></a><a name="InstanceInputEndpoint"></a> InstanceInputEndpoint
El elemento `InstanceInputEndpoint` describe un punto de conexión de entrada de instancia a un rol de trabajo. Un punto de conexión de entrada de instancia está asociado a una instancia de rol específica mediante el reenvío de puerto del equilibrador de carga. Cada punto de conexión de entrada de instancia se asigna a un puerto específico de un intervalo de puertos posibles. Este elemento es el elemento primario del elemento `AllocatePublicPortFrom`.

El elemento `InstanceInputEndpoint` solo está disponible cuando se usa la versión 1.7 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `InstanceInputEndpoint`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. Un nombre único para el punto de conexión.|
|localPort|int|Necesario. Especifica el puerto interno que todas las instancias de rol escucharán para recibir el tráfico de entrada reenviado desde el equilibrador de carga. El intervalo de valores posibles oscila entre 1 y 65535, ambos inclusive.|
|protocol|string|Necesario. El protocolo de transporte del punto de conexión interno. Los valores posibles son `udp` o `tcp`. Use `tcp` para el tráfico basado en http/https.|

##  <a name="allocatepublicportfrom"></a><a name="AllocatePublicPortFrom"></a> AllocatePublicPortFrom
El elemento `AllocatePublicPortFrom` describe el intervalo de puertos públicos que pueden usar los clientes externos para acceder a cada punto de conexión de entrada de instancia. El número de puerto público (VIP) se asigna de este intervalo y a cada punto de conexión de instancia de rol individual durante la implementación y la actualización de inquilinos. Este elemento es el elemento primario del elemento `FixedPortRange`.

El elemento `AllocatePublicPortFrom` solo está disponible cuando se usa la versión 1.7 o posterior de Azure SDK.

##  <a name="fixedport"></a><a name="FixedPort"></a> FixedPort
El elemento `FixedPort` especifica el puerto del punto de conexión interno, que permite conexiones de carga equilibrada en el punto de conexión.

El elemento `FixedPort` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `FixedPort`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|port|int|Necesario. El puerto del punto de conexión interno. Esto tiene el mismo efecto que establecer el valor mínimo y máximo de `FixedPortRange` en el mismo puerto.<br /><br /> Los valores posibles oscilan entre 1 y 65535, ambos inclusive (versión 1.7 o posterior de Azure SDK).|

##  <a name="fixedportrange"></a><a name="FixedPortRange"></a> FixedPortRange
El elemento `FixedPortRange` especifica el intervalo de puertos que se asignan al punto de conexión interno o al punto de conexión de entrada de instancia, y establece el puerto usado en las conexiones de carga equilibrada en el punto de conexión.

> [!NOTE]
>  El elemento `FixedPortRange` funciona de manera diferente según el elemento en el que reside. Cuando el elemento `FixedPortRange` está en el elemento `InternalEndpoint`, se abren todos los puertos del equilibrador de carga que se encuentran en el intervalo de los atributos min y max en todas las máquinas virtuales en las que se ejecuta el rol. Cuando el elemento `FixedPortRange` está en el elemento `InstanceInputEndpoint`, solo se abre un puerto del intervalo de los atributos min y max en cada máquina virtual donde se ejecuta el rol.

El elemento `FixedPortRange` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `FixedPortRange`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|Min|int|Necesario. El puerto mínimo del intervalo. Los valores posibles oscilan entre 1 y 65535, ambos inclusive (versión 1.7 o posterior de Azure SDK).|
|máx.|string|Necesario. El puerto máximo del intervalo. Los valores posibles oscilan entre 1 y 65535, ambos inclusive (versión 1.7 o posterior de Azure SDK).|

##  <a name="certificates"></a><a name="Certificates"></a> Certificados
El elemento `Certificates` describe la colección de certificados de un rol de trabajo. Este elemento es el elemento primario del elemento `Certificate`. Un rol puede tener cualquier número de certificados asociados. Para más información sobre cómo usar el elemento de certificados, vea cómo [modificar el archivo de definición de servicio con un certificado](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="certificate"></a><a name="Certificate"></a> Certificate
El elemento `Certificate` describe un certificado que está asociado a un rol de trabajo.

En la tabla siguiente se describen los atributos del elemento `Certificate`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. Un nombre para este certificado, que se usa para referirse a él cuando está asociado con un elemento `InputEndpoint` de HTTPS.|
|storeLocation|string|Necesario. La ubicación del almacén de certificados donde se puede encontrar este certificado en la máquina local. Los valores posibles son `CurrentUser` y `LocalMachine`.|
|storeName|string|Necesario. El nombre del almacén de certificados donde reside este certificado en la máquina local. Los valores posibles incluyen nombres de almacén integrados `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`, o cualquier otro nombre de almacén personalizado. Si se especifica un nombre de almacén personalizado, se crea automáticamente el almacén.|
|permissionLevel|string|Opcional. Especifica los permisos de acceso proporcionados a los procesos de rol. Si quiere que los procesos elevados puedan acceder a la clave privada, especifique entonces el permiso `elevated`. El permiso `limitedOrElevated` permite que todos los procesos del rol accedan a la clave privada. Los valores posibles son `limitedOrElevated` o `elevated`. El valor predeterminado es `limitedOrElevated`.|

##  <a name="imports"></a><a name="Imports"></a> Imports
El elemento `Imports` describe una colección de módulos de importación para un rol de trabajo que agregan componentes al sistema operativo invitado. Este elemento es el elemento primario del elemento `Import`. Este elemento es opcional y un rol solo puede tener un bloque de tiempo de ejecución.

El elemento `Imports` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

##  <a name="import"></a><a name="Import"></a> Import
El elemento `Import` especifica un módulo para agregar al sistema operativo invitado.

El elemento `Import` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `Import`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|moduleName|string|Necesario. El nombre del módulo que se va a importar. Los módulos de importación válidos son:<br /><br /> -   RemoteAccess<br />-   RemoteForwarder<br />-   Diagnostics<br /><br /> Los módulos RemoteAccess y RemoteForwarder permiten configurar la instancia de rol para las conexiones a Escritorio remoto. Para más información, vea cómo [habilitar la conexión a Escritorio remoto](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> El módulo Diagnostics permite recopilar datos de diagnóstico para una instancia de rol.|

##  <a name="runtime"></a><a name="Runtime"></a> Runtime
El elemento `Runtime` describe una colección de configuraciones de variables de entorno para un rol de trabajo que controlan el entorno en tiempo de ejecución del proceso de host de Azure. Este elemento es el elemento primario del elemento `Environment`. Este elemento es opcional y un rol solo puede tener un bloque de tiempo de ejecución.

El elemento `Runtime` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `Runtime`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|executionContext|string|Opcional. Especifica el contexto en el que se inicia el proceso del rol. El contexto predeterminado es `limited`.<br /><br /> -   `limited`: el proceso se inicia sin necesidad de privilegios de administrador.<br />-   `elevated`: el proceso se inicia con privilegios de administrador.|

##  <a name="environment"></a><a name="Environment"></a> Environment
El elemento `Environment` describe una colección de configuraciones de variables de entorno para un rol de trabajo. Este elemento es el elemento primario del elemento `Variable`. Un rol puede tener cualquier número de conjunto de variables de entorno.

##  <a name="variable"></a><a name="Variable"></a> Variable
El elemento `Variable` especifica una variable de entorno para establecer en el sistema operativo invitado.

El elemento `Variable` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `Variable`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|name|string|Necesario. El nombre de la variable de entorno que se establece.|
|value|string|Opcional. El valor que se establece para la variable de entorno. Debe incluir un atributo de valor o un elemento `RoleInstanceValue`.|

##  <a name="roleinstancevalue"></a><a name="RoleInstanceValue"></a> RoleInstanceValue
El elemento `RoleInstanceValue` especifica la xPath de la que se recupera el valor de la variable.

En la tabla siguiente se describen los atributos del elemento `RoleInstanceValue`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|xpath|string|Opcional. Ruta de acceso de ubicación de la configuración de implementación de la instancia. Para más información, vea las [variables de configuración con XPath](cloud-services-role-config-xpath.md).<br /><br /> Debe incluir un atributo de valor o un elemento `RoleInstanceValue`.|

##  <a name="entrypoint"></a><a name="EntryPoint"></a> EntryPoint
El elemento `EntryPoint` especifica el punto de entrada de un rol. Este elemento es el elemento primario del elemento `NetFxEntryPoint`. Estos elementos le permiten especificar una aplicación que no sea la predeterminada WaWorkerHost.exe para que actúe como el punto de entrada del rol.

El elemento `EntryPoint` solo está disponible cuando se usa la versión 1.5 o posterior de Azure SDK.

##  <a name="netfxentrypoint"></a><a name="NetFxEntryPoint"></a> NetFxEntryPoint
El elemento `NetFxEntryPoint` especifica el programa que se ejecutará para un rol.

> [!NOTE]
>  El elemento `NetFxEntryPoint` solo está disponible cuando se usa la versión 1.5 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `NetFxEntryPoint`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|assemblyName|string|Necesario. La ruta de acceso y el nombre de archivo del ensamblado que contiene el punto de entrada. La ruta de acceso es relativa a la carpeta **\\%ROLEROOT%\Approot** (no especifique **\\%ROLEROOT%\Approot** en `commandLine`, se da por supuesto). **%ROLEROOT%** es una variable de entorno que mantiene Azure y representa la ubicación de la carpeta raíz del rol. La carpeta **\\%ROLEROOT%\Approot** representa la carpeta de la aplicación del rol.|
|targetFrameworkVersion|string|Necesario. La versión de .NET Framework en la que se compiló el ensamblado. Por ejemplo, `targetFrameworkVersion="v4.0"`.|

##  <a name="programentrypoint"></a><a name="ProgramEntryPoint"></a> ProgramEntryPoint
El elemento `ProgramEntryPoint` especifica el programa que se ejecutará para un rol. El elemento `ProgramEntryPoint` le permite especificar un punto de entrada de programa que no se basa en un ensamblado. NET.

> [!NOTE]
>  El elemento `ProgramEntryPoint` solo está disponible cuando se usa la versión 1.5 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `ProgramEntryPoint`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|commandLine|string|Necesario. La ruta de acceso, el nombre de archivo y los argumentos de línea de comandos del programa que se va a ejecutar. La ruta de acceso es relativa a la carpeta **%ROLEROOT%\Approot** (no especifique **%ROLEROOT%\Approot** en commandLine, porque ya se da por supuesto). **%ROLEROOT%** es una variable de entorno que mantiene Azure y representa la ubicación de la carpeta raíz del rol. La carpeta **%ROLEROOT%\Approot** representa la carpeta de aplicaciones para el rol.<br /><br /> Si el programa finaliza, el rol se recicla, así que establezca el programa normalmente para que se siga ejecutando, en lugar de ser un programa que se inicie y ejecute una tarea finita.|
|setReadyOnProcessStart|boolean|Necesario. Especifica si la instancia de rol espera a que el programa de línea de comandos indique que se inicie. En este momento, este valor debe establecerse en `true`. El valor `false` está reservado para un uso futuro.|

##  <a name="startup"></a><a name="Startup"></a> Startup
El elemento `Startup` describe una colección de tareas que se ejecutan cuando se inicia el rol. Este elemento es el elemento primario del elemento `Variable`. Para más información sobre el uso de las tareas de inicio de rol, vea [cómo configurar las tareas de inicio](cloud-services-startup-tasks.md). Este elemento es opcional y un rol solo puede tener un bloque de inicio.

En la tabla siguiente se describen los atributos del elemento `Startup`.

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|priority|int|Solo para uso interno.|

##  <a name="task"></a><a name="Task"></a> Task
El elemento `Task` especifica la tarea de inicio que tiene lugar cuando se inicia el rol. Las tareas de inicio pueden usarse para realizar tareas que preparan el rol para ejecutar la instalación de componentes de software o ejecutar otras aplicaciones. Las tareas se ejecutan en el orden en que aparecen en el bloque del elemento `Startup`.

El elemento `Task` solo está disponible cuando se usa la versión 1.3 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `Task`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|commandLine|string|Necesario. Un script, como un archivo CMD, que contiene los comandos que se van a ejecutar. Los comandos de inicio y los archivos por lotes se deben guardar en formato ANSI. Los formatos de archivo que establecen un marcador de orden de bytes al inicio del archivo no se procesarán correctamente.|
|executionContext|string|Especifica el contexto en el que se ejecuta el script.<br /><br /> -   `limited` [valor predeterminado]: se ejecuta con los mismos privilegios que el rol que hospeda el proceso.<br />-   `elevated`: se ejecuta con privilegios de administrador.|
|taskType|string|Especifica el comportamiento de ejecución del comando.<br /><br /> -   `simple` [valor predeterminado]: el sistema espera a que se cierre la tarea antes de iniciar otra.<br />-   `background`: el sistema no espera a que se cierre la tarea.<br />-   `foreground`: se parece a background, excepto que el rol no se reinicia hasta que todas las tareas de foreground se cierran.|

##  <a name="contents"></a><a name="Contents"></a> Contenido
El elemento `Contents` describe la colección de contenido de un rol de trabajo. Este elemento es el elemento primario del elemento `Content`.

El elemento `Contents` solo está disponible cuando se usa la versión 1.5 o posterior de Azure SDK.

##  <a name="content"></a><a name="Content"></a> Contenido
El elemento `Content` define la ubicación de origen del contenido que se copiará en la máquina virtual de Azure y la ruta de acceso de destino en la que se copia.

El elemento `Content` solo está disponible cuando se usa la versión 1.5 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `Content`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|destination|string|Necesario. Ubicación en la máquina virtual de Azure en la que se coloca el contenido. Esta ubicación es relativa a la carpeta **%ROLEROOT%\Approot**.|

Este elemento es el elemento primario del elemento `SourceDirectory`.

##  <a name="sourcedirectory"></a><a name="SourceDirectory"></a> SourceDirectory
El elemento `SourceDirectory` define el directorio local desde el que se copia el contenido. Use este elemento para especificar el contenido local para copiar en la máquina virtual de Azure.

El elemento `SourceDirectory` solo está disponible cuando se usa la versión 1.5 o posterior de Azure SDK.

En la tabla siguiente se describen los atributos del elemento `SourceDirectory`:

| Atributo | Tipo | Descripción |
| --------- | ---- | ----------- |
|path|string|Necesario. Ruta de acceso absoluta o relativa de un directorio local cuyo contenido se copiará en la máquina virtual de Azure. Se admite la expansión de variables de entorno en la ruta de acceso de directorio.|

## <a name="see-also"></a>Consulte también
[Cloud Service (classic) Definition Schema](schema-csdef-file.md) (Esquema de definición de servicio en la nube [clásico])



