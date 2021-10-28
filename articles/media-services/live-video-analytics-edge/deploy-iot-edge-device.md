---
title: Implementación de Azure Live Video Analytics en un dispositivo IoT Edge
description: En este artículo se enumeran los pasos para implementar Azure Live Video Analytics en el dispositivo IoT Edge. Hará esto, por ejemplo, si tiene acceso a una máquina Linux local o ha creado previamente una cuenta de Azure Media Services.
ms.topic: how-to
ms.date: 09/09/2020
ms.openlocfilehash: cf4dbd55314bae73d9082c8b45bfe728cb4c235a
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130225744"
---
# <a name="deploy-azure-live-video-analytics-on-an-iot-edge-device"></a>Implementación de Azure Live Video Analytics en un dispositivo IoT Edge

[!INCLUDE [redirect to Azure Video Analyzer](./includes/redirect-video-analyzer.md)]

En este artículo se enumeran los pasos que ayudarán a implementar Live Video Analytics en el dispositivo IoT Edge. Hará esto, por ejemplo, si tiene acceso a una máquina Linux local o ha creado previamente una cuenta de Azure Media Services.

> [!NOTE]
> La compatibilidad con dispositivos ARM64 está disponible en Live Video Analytics en compilaciones de IoT Edge `1.0.4` y más recientes.
> La compatibilidad con el entorno de ejecución de Azure IoT Edge en dispositivos ARM64 se encuentra en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

* Un dispositivo x86-64 o ARM64 que ejecute uno de los [sistemas operativos Linux admitidos](../../iot-edge/support.md#operating-systems).
* Suscripción de Azure en la que tenga [privilegios de propietario](../../role-based-access-control/built-in-roles.md#owner).
* [Creación y configuración de IoT Hub](../../iot-hub/iot-hub-create-through-portal.md).
* [Registro de un dispositivo de IoT Edge](../../iot-edge/how-to-provision-single-device-linux-symmetric.md#register-your-device).
* [Instalación del entorno de ejecución de Azure IoT Edge en sistemas Linux basados en Debian](../../iot-edge/how-to-provision-single-device-linux-symmetric.md)
* [Creación de una cuenta de Azure Media Services](../latest/account-create-how-to.md)

    * Use una de estas regiones: Este de EE. UU. 2, Este de EE. UU., Centro de EE. UU., Centro-norte de EE. UU., Japón Oriental, Oeste de EE. UU., Oeste de EE. UU. '2, Centro-oeste de EE. UU., Este de Canadá, Sur de Reino Unido, Centro de Francia, Sur de Francia, Norte de Suiza, Oeste de Suiza y Japón Occidental.
    * Se recomienda usar cuentas de almacenamiento v2 de uso general (GPv2).

## <a name="configuring-azure-resources-for-using-live-video-analytics"></a>Configuración de recursos de Azure para usar Live Video Analytics

### <a name="create-custom-azure-resource-manager-role"></a>Creación de un rol de Azure Resource Manager personalizado

Vea [Creación de un rol personalizado de Azure Resource Manager](create-custom-azure-resource-manager-role-how-to.md) y asígnelo a una entidad de servicio para el uso de Live Video Analytics.

### <a name="set-up-a-premium-streaming-endpoint"></a>Configuración de un punto de conexión de streaming premium

Si tiene previsto usar Live Video Analytics para grabar vídeo continuamente en la nube y, posteriormente, usar las [API de consulta](playback-recordings-how-to.md#query-api) antes de reproducirlo, se recomienda actualizar el servicio multimedia para que use un [punto de conexión de streaming premium](../latest/stream-streaming-endpoint-concept.md#types).  

Se trata de un paso opcional. Se puede usar este comando de la CLI de Azure para ello:

```azurecli
az ams streaming-endpoint scale --resource-group $RESOURCE_GROUP --account-name $AMS_ACCOUNT -n default --scale-units 1
```

Se puede usar este comando para iniciar el punto de conexión de streaming: 

> [!IMPORTANT]
> La suscripción comenzará a facturarse en este momento.

```azurecli
az ams streaming-endpoint start --resource-group $RESOURCE_GROUP --account-name $AMS_ACCOUNT -n default --no-wait
```

Siga los pasos del artículo [Acceso a las API de Media Services](../latest/access-api-howto.md?tabs=portal) para obtener las credenciales de acceso a las API de Media Services, y seleccione la pestaña Portal.

## <a name="create-and-use-local-user-account-for-deployment"></a>Creación y uso de una cuenta de usuario local para la implementación
Para ejecutar el módulo Live Video Analytics en IoT Edge, cree una cuenta de usuario local que tenga el menor número de privilegios posible. Como ejemplo, ejecute los comandos siguientes en la máquina Linux:

```
sudo groupadd -g 1010 localusergroup
sudo useradd --home-dir /home/edgeuser --uid 1010 --gid 1010 lvaedgeuser
```

## <a name="granting-permissions-to-device-storage"></a>Concesión de permisos para el almacenamiento de dispositivos

Ahora que ha creado una cuenta de usuario local: 

* Necesitará una carpeta local para almacenar los datos de configuración de la aplicación. Cree una carpeta, conceda permisos a la cuenta de usuario local y escriba en esa carpeta con los comandos siguientes:

```
sudo mkdir -p /var/lib/azuremediaservices
sudo chown -R lvaedgeuser /var/lib/azuremediaservices
```

* También necesitará una carpeta para [grabar vídeos en un archivo local](event-based-video-recording-concept.md#video-recording-based-on-events-from-other-sources). Use los comandos siguientes para crear una carpeta local para el mismo:

```
sudo mkdir -p /var/media
sudo chown -R lvaedgeuser /var/media
```

## <a name="deploy-live-video-analytics-edge-module"></a>Implementación del módulo Edge en Live Video Analytics

Live Video Analytics en IoT Edge expone propiedades de módulos gemelos que se documentan en [Esquema de configuración de módulos gemelos](module-twin-configuration-schema.md). 

### <a name="deploy-using-the-azure-portal"></a>Implementación mediante Azure Portal

Azure Portal le guía en la creación de un manifiesto de implementación y la inserción de la implementación en un dispositivo de IoT Edge.  
#### <a name="select-your-device-and-set-modules"></a>Selección del dispositivo y establecimiento de módulos

1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/) y vaya a IoT Hub.
1. Seleccione **IoT Edge** en el menú.
1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.
1. Seleccione **Set modules** (Establecer módulos).

#### <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Azure Portal cuenta con un asistente que le guía en la creación de un manifiesto de implementación. Presenta tres pasos organizados en pestañas: **Módulos**, **Rutas** y **Revisar y crear**.

#### <a name="add-modules"></a>Adición de módulos

1. En la sección **Módulos IoT Edge** de la página, haga clic en la lista desplegable **Agregar** y seleccione **Agregar módulo IoT Edge** para mostrar la página **Agregar módulo IoT Edge**.
1. En la pestaña **Configuración del módulo**, proporcione un nombre para el módulo y especifique el URI de imagen del contenedor:   
    Ejemplos:
    
    * **Nombre del módulo IoT Edge**: lvaEdge
    * **URI de la imagen**: mcr.microsoft.com/media/live-video-analytics:2.0.    
    
    ![Captura de pantalla que muestra la pestaña Configuración del módulo.](./media/deploy-iot-edge-device/add.png)
    
    > [!TIP]
    > No seleccione **Agregar** hasta que haya especificado los valores en las pestañas **Configuración del módulo**, **Opciones de creación del contenedor** y **Configuración de módulos gemelos**, tal como se describe en este procedimiento.
    
    > [!WARNING]
    > Azure IoT Edge distingue mayúsculas de minúsculas al realizar llamadas a los módulos. Tome nota de la cadena exacta que utilice como nombre de módulo.

1. Abra la pestaña **Variables de entorno**.
   
   Agregue los siguientes valores en los cuadros de entrada que vea ![Variables de entorno](./media/deploy-iot-edge-device/environment-variables.png) 

1. Abra la pestaña **Opciones de creación del contenedor**.

    ![Opciones de creación del contenedor](./media/deploy-iot-edge-device/container-create-options.png)
 
    Copie y pegue el siguiente código JSON en el cuadro para limitar el tamaño de los archivos de registro que genera el módulo.
    
    ```    
    {
        "HostConfig": {
            "LogConfig": {
                "Type": "",
                "Config": {
                    "max-size": "10m",
                    "max-file": "10"
                }
            },
            "Binds": [
               "/var/lib/azuremediaservices:/var/lib/azuremediaservices",
               "/var/media:/var/media"
            ]
        }
    }
    ````
   
   La sección "Enlaces" del archivo JSON tiene 2 entradas:
   1. "/var/lib/azuremediaservices:/var/lib/azuremediaservices": se usa para enlazar los datos de configuración persistentes de la aplicación desde el contenedor y almacenarlos en el dispositivo perimetral.
   1. "/var/media:/var/media": esto enlaza las carpetas de elementos multimedia entre el dispositivo perimetral y el contenedor. Se usa para almacenar las grabaciones de vídeo cuando se ejecuta una topología de gráficos multimedia que admite el almacenamiento de clips de vídeo en el dispositivo perimetral.
   
1. En la pestaña **Configuración de módulos gemelos**, copie el siguiente código JSON y péguelo en el cuadro.
 
    ![Configuración de gemelos](./media/deploy-iot-edge-device/twin-settings.png)

    Live Video Analytics en IoT Edge requiere un conjunto de propiedades gemelas obligatorias para poder ejecutarse, tal como se muestra en [Esquema de configuración de módulos gemelos](module-twin-configuration-schema.md).  

    El código JSON que se debe especificar en el cuadro de edición de Configuración de módulos gemelos tendrá el siguiente aspecto:    
    ```
    {
        "applicationDataDirectory": "/var/lib/azuremediaservices",
        "azureMediaServicesArmId": "/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/microsoft.media/mediaservices/{AMS-account-name}",
        "aadTenantId": "{the-ID-of-your-tenant}",
        "aadServicePrincipalAppId": "{the-ID-of-the-service-principal-app-for-ams-account}",
        "aadServicePrincipalSecret": "{secret}"
    }
    ```
    Estas son propiedades **necesarias** y, para el código JSON anterior:  
    * {subscriptionID}: id. de suscripción de Azure.
    * {resourceGroupName}: grupo de recursos al que pertenece la cuenta de Servicio multimedia.
    * {AMS-account-name}: nombre de la cuenta de Media Services.
    
    Para obtener los demás valores, consulte [Acceso a la API de Azure Media Services](../latest/access-api-howto.md?tabs=portal) y seleccione la pestaña Portal.  
    * aadTenantId: id. del inquilino. Es el mismo que el de "AadTenantId" del vínculo anterior.
    * aadServicePrincipalAppId: id. de la aplicación de la entidad de servicio de la cuenta de Servicio multimedia. Es el mismo que el de "AadClientId" del vínculo anterior.
    * aadServicePrincipalSecret: contraseña de la entidad de servicio. Es el mismo que el de "AadSecret" del vínculo anterior.

    Más abajo se muestran algunas propiedades adicionales **recomendadas** que se pueden agregar al código JSON y ayudarán a supervisar el módulo. Para obtener más información, vea [Supervisión y registro](monitoring-logging.md):
    
    ```
    "diagnosticsEventsOutputName": "lvaEdgeDiagnostics",
    "OperationalEventsOutputName": "lvaEdgeOperational",
    "logLevel": "Information",
    "logCategories": "Application,Events"
    ```
    
    A continuación se muestran algunas propiedades **opcionales** que se pueden agregar en el código JSON:
    
    ```
    "aadEndpoint": "https://login.microsoftonline.com",
    "aadResourceId": "https://management.core.windows.net/",
    "armEndpoint": "https://management.azure.com/",
    "allowUnsecuredEndpoints": true
    ```

   > [!Note]
   > La propiedad gemela **allowUnsecuredEndpoints** se establece como true para los tutoriales y las guías de inicio rápido.   
   Se debe establecer esta propiedad en **false** cuando se ejecute en el entorno de producción. Esto garantizará que la aplicación bloqueará todos los puntos de conexión no seguros y, para poder ejecutar las topologías de gráfico, se necesitarán credenciales de conexión válidas.  
   
    Seleccione Agregar para incorporar las propiedades de módulos gemelos.
1. Seleccione **Siguiente: Rutas** para continuar con la sección de rutas.
    Especifique las rutas.

Mantenga las rutas predeterminadas y seleccione **Siguiente: Revisar y crear** para continuar en la sección de revisión.

#### <a name="review-deployment"></a>Revisión de la implementación

La sección de revisión le muestra el manifiesto de implementación en formato JSON que se ha creado en función de las selecciones de las dos secciones anteriores. También hay dos módulos declarados que no se han agregado: "$edgeAgent" y "$edgeHub". Estos dos módulos constituyen el entorno de ejecución de IoT Edge y son valores predeterminados necesarios en todas las implementaciones.

Revise la información de implementación y seleccione Crear.

### <a name="verify-your-deployment"></a>Comprobación de la implementación

Después de crear la implementación, regresará a la página IoT Edge de IoT Hub.

1. Seleccione el dispositivo IoT Edge elegido como destino con la implementación para abrir sus detalles.
2. En los detalles del dispositivo, compruebe que el módulo de almacenamiento de blobs aparece como **Especificado en la implementación y Notificado por el dispositivo**.

Puede tardar unos minutos para que el módulo se inicie en el dispositivo y, a continuación, notifique a IoT Hub. Actualice la página para ver el estado actualizado.
Código de estado: 200: OK significa que [el entorno de ejecución de Azure IoT Edge](../../iot-edge/iot-edge-runtime.md) es correcto y funciona bien.

![Captura de pantalla que muestra un valor de estado de un entorno de ejecución de IoT Edge.](./media/deploy-iot-edge-device/status.png)

#### <a name="invoke-a-direct-method"></a>Invocación de un método directo

Después, vamos a probar el ejemplo con la invocación de un método directo. Lea [Métodos directos de Live Video Analytics en IoT Edge](direct-methods.md) para conocer todos los métodos directos que proporciona nuestro módulo IvaEdge.

1. Al hacer clic en el módulo Edge que se ha creado, irá a su página de configuración.  

    ![Captura de pantalla que muestra la página de configuración de un módulo perimetral.](./media/deploy-iot-edge-device/modules.png)
1. Haga clic en la opción de menú Método directo.

    > [!NOTE] 
    > Tendrá que agregar un valor en las secciones Cadena de conexión, tal como se puede ver en la página actual. No es necesario ocultar ni cambiar nada en la sección **Nombre de configuración**. Es correcto dejar que sea pública.

    ![Método directo](./media/deploy-iot-edge-device/module-details.png)
1. Después, escriba "GraphTopologyList" en el cuadro Nombre del método.
1. Luego, copie y pegue la carga de JSON siguiente en el cuadro de carga.
    
    ```
    {
        "@apiVersion" : "2.0"
    }
    ```
1. Haga clic en la opción "Invocar método" situada en la parte superior de la página.

    ![Métodos directos](./media/deploy-iot-edge-device/direct-method.png)
1. Debería ver un mensaje de estado 200 en el cuadro de resultados.

    ![Mensaje de estado 200](./media/deploy-iot-edge-device/connection-timeout.png) 

## <a name="next-steps"></a>Pasos siguientes

Pruebe [Inicio rápido: Introducción: Live Video Analytics on IoT Edge](get-started-detect-motion-emit-events-quickstart.md#deploy-modules-on-your-edge-device)

> [!TIP]
> Si continúa con el inicio rápido anterior, al invocar los métodos directos mediante Visual Studio Code, utilizará el dispositivo que se agregó a IoT Hub a través de este artículo, en lugar del predeterminado `lva-sample-device`.
