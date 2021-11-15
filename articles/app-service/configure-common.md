---
title: Configuración de aplicaciones en el portal
description: Aprenda a configurar las opciones comunes para una aplicación de App Service en Azure Portal. Configuración de aplicaciones, cadenas de conexión, plataforma, pila de lenguajes, contenedor, etc.
keywords: servicio de aplicación de Azure, aplicación web, configuración de la aplicación, variables de entorno
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.topic: article
ms.date: 12/07/2020
ms.custom: devx-track-csharp, seodec18
ms.openlocfilehash: ed6d0e397a8b0b6b8a3ad69e5dd91b4c60709e35
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130231845"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>Configurar una aplicación de App Service en Azure Portal

En este artículo se explica cómo configurar los parámetros comunes de aplicaciones web, back-end para dispositivos móviles o aplicación de API con [Azure Portal].

## <a name="configure-app-settings"></a>Configuración de aplicaciones

En App Service, las configuraciones de aplicaciones son variables que se pasan como variables de entorno al código de la aplicación. En el caso de las aplicaciones Linux y de los contenedores personalizados, App Service pasa la configuración de la aplicación al contenedor mediante la marca `--env` para establecer la variable de entorno en el contenedor. En cualquier caso, se insertan en el entorno de la aplicación al iniciar la aplicación. Al agregar, quitar o editar la configuración de la aplicación, App Service desencadena un reinicio de la aplicación. Los nombres de configuración de aplicaciones no pueden contener puntos (`.`). Si una configuración de aplicación contiene un punto, este se reemplaza por un carácter de subrayado en el contenedor.

En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. 

![Búsqueda de App Services](./media/configure-common/search-for-app-services.png)

En el menú izquierdo de la aplicación, haga clic en **Configuración** > **Configuración de la aplicación**.

![Configuración de la aplicación](./media/configure-common/open-ui.png)

Para los desarrolladores de ASP.NET y ASP.NET Core, la configuración de las opciones de aplicación en App Service es como configurarlas en `<appSettings>` en *Web.config* o *appsettings.json*, pero los valores de App Service reemplazan a los de *Web.config* o *appsettings.json*. Puede mantener la configuración de desarrollo (por ejemplo, la contraseña de MySQL local) de *Web.config* o *appsettings.json* y los secretos de producción (por ejemplo, la contraseña de base de datos de Azure MySQL) de forma segura en App Service. El mismo código usa la configuración de desarrollo cuando se depura localmente, y utiliza los secretos de producción cuando se implementa en Azure.

Del mismo modo, otras pilas de lenguaje obtienen la configuración de la aplicación como variables de entorno en tiempo de ejecución. Para obtener pasos específicos de la pila de lenguaje, consulte:

- [ASP.NET Core](configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](configure-language-nodejs.md#access-environment-variables)
- [PHP](configure-language-php.md#access-environment-variables)
- [Python](configure-language-python.md#access-environment-variables)
- [Java](configure-language-java.md#configure-data-sources)
- [Ruby](configure-language-ruby.md#access-environment-variables)
- [Contenedores personalizados](configure-custom-container.md#configure-environment-variables)

La configuración de la aplicación siempre se cifra cuando se almacena (cifrado en reposo).

> [!NOTE]
> La configuración de la aplicación también se puede resolver desde [Key Vault](../key-vault/index.yml) mediante las [referencias de Key Vault](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Mostrar valores ocultos

De forma predeterminada, los valores de configuración de la aplicación están ocultos en el portal por motivos de seguridad. Para ver un valor oculto de la configuración de una aplicación, haga clic en el campo **Valor** de dicha configuración. Para ver los valores de todas las configuraciones de aplicación, haga clic en el botón **Mostrar valor**.

### <a name="add-or-edit"></a>Agregar o editar

Para agregar una nueva configuración de aplicación, haga clic en **Nueva configuración de la aplicación**. En el cuadro de diálogo, puede [fijar la configuración a la ranura actual](deploy-staging-slots.md#which-settings-are-swapped).

Para editar una configuración, haga clic en el **Editar** situado a la derecha.

Cuando termine, haga clic en **Actualizar**. No olvide hacer clic en **Guardar** en la página **Configuración**.

> [!NOTE]
> En un servicio de aplicaciones de Linux predeterminado o en un contenedor de Linux personalizado, cualquier estructura de claves JSON anidada en el nombre de configuración de la aplicación, como `ApplicationInsights:InstrumentationKey`, debe configurarse en App Service como `ApplicationInsights__InstrumentationKey` para el nombre de clave. Es decir, los símbolos `:` deben reemplazarse por `__` (doble subrayado).
>

### <a name="edit-in-bulk"></a>Editar en masa

Para agregar o editar la configuración de la aplicación en masa, haga clic en el botón **Edición avanzada**. Cuando termine, haga clic en **Actualizar**. No olvide hacer clic en **Guardar** en la página **Configuración**.

La configuración de la aplicación tiene el formato JSON siguiente:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

### <a name="automate-app-settings-with-the-azure-cli"></a>Automatización de la configuración de la aplicación con la CLI de Azure

Puede usar la CLI de Azure para crear y administrar la configuración desde la línea de comandos.

- Asigne un valor a una configuración con [az webapp config app settings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set):

    ```azurecli-interactive
    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings <setting-name>="<value>"
    ```
        
    Reemplace `<setting-name>` con el nombre de la configuración y `<value>` con el valor que se le va a asignar. Este comando crea la configuración si aún no existe.
    
- Muestre todas las configuraciones y sus valores con [az webapp config appsettings list](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_list):
    
    ```azurecli-interactive
    az webapp config appsettings list --name <app-name> --resource-group <resource-group-name>
    ```
    
- Quite una o varias opciones de configuración con [az webapp config app settings delete](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_delete):

    ```azurecli-interactive
    az webapp config appsettings delete --name <app-name> --resource-group <resource-group-name> --setting-names {<names>}
    ```
    
    Reemplace `<names>` con una lista de nombres de configuración separados por espacios.

## <a name="configure-connection-strings"></a>Configurar cadenas de conexión

En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. En el menú izquierdo de la aplicación, haga clic en **Configuración** > **Configuración de la aplicación**.

![Configuración de la aplicación](./media/configure-common/open-ui.png)

Para los desarrolladores de ASP.NET y ASP.NET Core, la configuración de las cadenas de conexión en App Service es como configurarlas en `<connectionStrings>` en *Web.config*, pero los valores establecidos en App Service reemplazan a los de *Web.config*. Puede mantener la configuración de desarrollo (por ejemplo, un archivo de base de datos) en *Web.config* y los secretos de producción (por ejemplo, credenciales de SQL Database) seguros en App Service. El mismo código usa la configuración de desarrollo cuando se depura localmente, y utiliza los secretos de producción cuando se implementa en Azure.

En cambio, para otras pilas de lenguaje es mejor usar la [configuración de la aplicación](#configure-app-settings), dado que las cadenas de conexión requieren un formato especial en las claves de variable para acceder a los valores. 

> [!NOTE]
> Hay un caso en el que puede que quiera usar cadenas de conexión en lugar de la configuración de la aplicación para los lenguajes que no son .NET: la copia de seguridad de determinados tipos de bases de datos de Azure se realiza junto con la aplicación _solo_ si se configura una cadena de conexión para la base de datos en la aplicación de App Service. Para obtener más información, consulte [¿Qué se incluye en la copia de seguridad?](manage-backup.md#what-gets-backed-up) Si no necesita esta copia de seguridad automatizada, use la configuración de la aplicación.

En tiempo de ejecución, las cadenas de conexión están disponibles como variables de entorno, con los siguientes tipos de conexión como prefijo:

* SQLServer: `SQLCONNSTR_`  
* MySQL: `MYSQLCONNSTR_` 
* SQLAzure: `SQLAZURECONNSTR_` 
* Personalizado: `CUSTOMCONNSTR_`
* PostgreSQL: `POSTGRESQLCONNSTR_`  

Por ejemplo, se puede obtener acceso a una cadena de conexión de MySQL denominada *connectionstring1* como la variable de entorno `MYSQLCONNSTR_connectionString1`. Para obtener pasos específicos de la pila de lenguaje, consulte:

- [ASP.NET Core](configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](configure-language-nodejs.md#access-environment-variables)
- [PHP](configure-language-php.md#access-environment-variables)
- [Python](configure-language-python.md#access-environment-variables)
- [Java](configure-language-java.md#configure-data-sources)
- [Ruby](configure-language-ruby.md#access-environment-variables)
- [Contenedores personalizados](configure-custom-container.md#configure-environment-variables)

Las cadenas de conexión siempre se cifran cuando se almacenan (cifrado en reposo).

> [!NOTE]
> Las cadenas de conexión se pueden resolver desde [Key Vault](../key-vault/index.yml) mediante las [referencias de Key Vault](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Mostrar valores ocultos

De forma predeterminada, los valores de las cadenas de conexión están ocultos en el portal por motivos de seguridad. Para ver un valor oculto de una cadena de conexión, simplemente haga clic en el campo **Valor** de esa cadena. Para ver los valores de todas las cadenas de conexión, haga clic en el botón **Mostrar valor**.

### <a name="add-or-edit"></a>Agregar o editar

Para agregar una nueva cadena de conexión, haga clic en **Nueva cadena de conexión**. En el cuadro de diálogo, puede [fijar la cadena de conexión a la ranura actual](deploy-staging-slots.md#which-settings-are-swapped).

Para editar una configuración, haga clic en el **Editar** situado a la derecha.

Cuando termine, haga clic en **Actualizar**. No olvide hacer clic en **Guardar** en la página **Configuración**.

### <a name="edit-in-bulk"></a>Editar en masa

Para agregar o editar las cadenas de conexión en masa, haga clic en el botón **Edición avanzada**. Cuando termine, haga clic en **Actualizar**. No olvide hacer clic en **Guardar** en la página **Configuración**.

Las cadenas de conexión tienen el formato JSON siguiente:

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>Configurar las opciones generales

En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. En el menú izquierdo de la aplicación, seleccione **configuración** > **Configuración general**.

![Configuración general](./media/configure-common/open-general.png)

En este caso, puede configurar algunas opciones comunes para la aplicación. Algunas configuraciones requieren [escalar verticalmente hasta los planes de tarifa superiores](manage-scale-up.md).

- **Configuración de pila**: La pila de software para ejecutar la aplicación, incluidos el lenguaje y las versiones del SDK.

    En el caso de las aplicaciones de Linux y las aplicaciones de contenedor personalizadas, puede seleccionar la versión de Language Runtime y establecer un **comando de startup** opcional o un archivo de comandos de startup.

    ![Configuración general de los contenedores de Linux](./media/configure-common/open-general-linux.png)

- **Configuración de plataforma**: Le permite configurar opciones para la plataforma de alojamiento, incluidas:
    - **Valor de bits**: 32 bits o 64 bits. (El valor predeterminado es 32 bits para las instancias de App Service creadas en el portal).
    - **Protocolo Websocket**: para [ASP.NET SignalR] o [socket.io](https://socket.io/), por ejemplo.
    - **Always On**: mantiene la aplicación cargada, incluso cuando no hay tráfico. Cuando **Always On** no está activado (configuración predeterminada), la aplicación se descarga después de 20 minutos sin ninguna solicitud entrante. La aplicación descargada puede provocar una latencia alta para las nuevas solicitudes debido a su tiempo de preparación. Cuando **Always On** está activado, el equilibrador de carga front-end envía una solicitud GET a la raíz de la aplicación cada cinco minutos. El ping continuo impide que la aplicación se descargue.
    
        Always On es necesario en los WebJobs continuos o WebJobs que se desencadenan mediante una expresión CRON.
    - **Versión de canalización administrada**: el [modo de canalización] IIS. Establézcalo en **Clásico** si tiene una aplicación heredada que requiere una versión anterior de IIS.
    - **Versión de HTTP**: Establézcala en **2.0** para habilitar la compatibilidad con el protocolo [HTTPS/2](https://wikipedia.org/wiki/HTTP/2).
    > [!NOTE]
    > Los exploradores más modernos admiten el protocolo HTTP/2 sobre TLS únicamente, mientras que el tráfico no cifrado sigue usando HTTP/1.1. Para asegurarse de que los exploradores del cliente se conectan a la aplicación con HTTP/2, proteja el nombre DNS personalizado. Para más información, consulte [Protección de un nombre DNS personalizado con un enlace TLS/SSL en Azure App Service](configure-ssl-bindings.md).
    - **Afinidad ARR**: en una implementación de varias instancias, asegúrese de que el cliente esté enrutado a la misma instancia de la vida de la sesión. Puede establecer esta opción en **Desactivada** para las aplicaciones sin estado.
- **Depuración**: habilite la depuración remota para las aplicaciones [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug), [ASP.NET Core](/visualstudio/debugger/remote-debugging-azure) o [Node.js](configure-language-nodejs.md#debug-remotely). Esta opción se desactiva automáticamente después de 48 horas.
- **Certificados de cliente entrantes**: requiera certificados de cliente en [autenticación mutua](app-service-web-configure-tls-mutual-auth.md).

## <a name="configure-default-documents"></a>Configurar documentos predeterminados

Esta configuración solo es para las aplicaciones de Windows.

En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. En el menú izquierdo de la aplicación, seleccione **Configuración** > **Documentos predeterminados**.

![Documentos predeterminados](./media/configure-common/open-documents.png)

El documento predeterminado es la página web que se muestra en la dirección URL raíz de un sitio web. Se usa el primer archivo coincidente en la lista. Para agregar un nuevo documento predeterminado, haga clic en **Nuevo documento**. Recuerde hacer clic en **Guardar**.

Si la aplicación usa módulos que se enrutan en función de la dirección URL en lugar de servir contenido estático, no hay necesidad de documentos predeterminados.

## <a name="configure-path-mappings"></a>Configurar asignaciones de ruta de acceso

En [Azure Portal], busque y seleccione **App Services** y luego elija la aplicación. En el menú izquierdo de la aplicación, seleccione **Configuración** > **Asignaciones de ruta de acceso**.

![Asignaciones de ruta de acceso](./media/configure-common/open-path.png)

> [!NOTE] 
> La pestaña **Asignaciones de ruta de acceso** puede mostrar valores específicos del sistema operativo que difieren del ejemplo que se muestra aquí.

### <a name="windows-apps-uncontainerized"></a>Aplicaciones de Windows (sin contenedor)

Para aplicaciones de Windows, puede personalizar las asignaciones de controlador de IIS, así como las aplicaciones y directorios virtuales.

Las asignaciones de controlador permiten agregar procesadores de script personalizados para controlar solicitudes de extensiones de archivo específicas. Para agregar un controlador personalizado, haga clic en **Nueva asignación de controlador**. Configure el controlador de la manera siguiente:

- **Extensión**. La extensión de archivo que quiere gestionar, por ejemplo, *\*.php* o *handler.fcgi*.
- **Procesador de script**. La ruta de acceso absoluta del procesador de script. El procesador de script procesa las solicitudes a archivos que coincidan con esta extensión de archivo. Utilice la ruta de acceso `D:\home\site\wwwroot` para hacer referencia al directorio raíz de la aplicación.
- **Argumentos**. Argumentos opcionales de la línea de comandos para el procesador de script.

Cada aplicación tiene la ruta de acceso de la raíz predeterminada (`/`) asignada a `D:\home\site\wwwroot`, donde el código se implementa de forma predeterminada. Si es la raíz de la aplicación está en una carpeta diferente, o si el repositorio tiene más de una aplicación, puede editar o agregar aplicaciones y directorios virtuales aquí. 

En la pestaña **Asignaciones de ruta de acceso**, haga clic en **Nueva aplicación o directorio virtual**. 

- Para asignar un directorio virtual a una ruta de acceso física, deje activada la casilla **Directorio**. Especifique el directorio virtual y la ruta de acceso relativa (física) correspondiente a la raíz del sitio web (`D:\home`).
- Para marcar un directorio virtual como aplicación web, desactive la casilla **Directorio**.
  
  ![Casilla Directorio](./media/configure-common/directory-check-box.png)

### <a name="containerized-apps"></a>Aplicaciones en contenedores

También puede [agregar almacenamiento personalizado para la aplicación en contenedor](configure-connect-to-azure-storage.md). Las aplicaciones en contenedores incluyen todas las aplicaciones de Linux y también los contenedores personalizados de Windows y Linux que se ejecutan en App Service. Haga clic en **Nuevo montaje de Azure Storage** y configure el almacenamiento personalizado como sigue:

- **Name**: El nombre para mostrar.
- **Opciones de configuración**: **Básica** o **Avanzada**.
- **Cuentas de almacenamiento**: cuenta de almacenamiento con el contenedor que quiere.
- **Storage type** (Tipo de almacenamiento): **Azure Blobs** o **Azure Files**
  > [!NOTE]
  > Las aplicaciones de contenedor de Windows solo admiten Azure Files.
- **Contenedor de almacenamiento**: para la configuración básica, el contenedor que quiera.
- **Nombre del recurso compartido**: para la configuración avanzada, el nombre del recurso compartido.
- **Clave de acceso**: para la configuración avanzada, la clave de acceso.
- **Ruta de acceso de montaje**: La ruta de acceso absoluta en el contenedor para montar el almacenamiento personalizado.

Para obtener más información, consulte [Configuración de Azure Files en un contenedor de Windows en App Service](configure-connect-to-azure-storage.md).

## <a name="configure-language-stack-settings"></a>Configurar las opciones de pila de lenguaje

- [ASP.NET Core](configure-language-dotnetcore.md)
- [Node.js](configure-language-nodejs.md)
- [PHP](configure-language-php.md)
- [Python](configure-language-python.md)
- [Java](configure-language-java.md)
- [Ruby](configure-language-ruby.md)

## <a name="configure-custom-containers"></a>Configurar contenedores personalizados

Consulte [Configuración de un contenedor de Linux personalizado para Azure App Service](configure-custom-container.md).

## <a name="next-steps"></a>Pasos siguientes

- [Referencia de variables de entorno y configuración de la aplicación](reference-app-settings.md)
- [Configuración de un nombre de dominio personalizado en Azure App Service]
- [Configuración de entornos de ensayo en Azure App Service]
- [Protección de un nombre DNS personalizado con un enlace TLS/SSL en Azure App Service](configure-ssl-bindings.md)
- [Habilitar los registros de diagnóstico](troubleshoot-diagnostic-logs.md)
- [Escalado de una aplicación en Azure App Service]
- [Aspectos básicos de supervisión en Azure App Service]
- [Cambiar la configuración applicationHost.config por applicationHost.xdt](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Configuración de un nombre de dominio personalizado en Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Configuración de entornos de ensayo en Azure App Service]: ./deploy-staging-slots.md
[How to: Monitor web endpoint status]: ./web-sites-monitor.md
[Aspectos básicos de supervisión en Azure App Service]: ./web-sites-monitor.md
[modo de canalización]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Escalado de una aplicación en Azure App Service]: ./manage-scale-up.md
