---
title: Adición de una nueva actualización | Microsoft Docs
description: Guía paso a paso para agregar una nueva actualización en Device Update for IoT Hub.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 4/19/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: caa58c1affc9e4a9cc9d3ec5b63e0b396e289144
ms.sourcegitcommit: 901ea2c2e12c5ed009f642ae8021e27d64d6741e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2021
ms.locfileid: "132370429"
---
# <a name="add-an-update-to-device-update-for-iot-hub"></a>Adición de una actualización a Device Update for IoT Hub
Aprenda a agregar una nueva actualización en Device Update for IoT Hub.

## <a name="prerequisites"></a>Requisitos previos

* [Acceso a una instancia de IoT Hub con Device Update para IoT Hub habilitado](create-device-update-account.md). 
* Un dispositivo IoT (o simulador) [aprovisionado para Device Update](device-update-agent-provisioning.md) en IoT Hub.
* [PowerShell 5](/powershell/scripting/install/installing-powershell) o posterior (incluye las instalaciones de Linux, macOS y Windows).
* Exploradores admitidos:
  * [Microsoft Edge](https://www.microsoft.com/edge)
  * Google Chrome

> [!NOTE]
> Es posible que algunos de los datos que se envíen a este servicio se procesen en una región que no es en la que se creó esta instancia.

## <a name="obtain-an-update-for-your-devices"></a>Obtención de una actualización para sus dispositivos

Ahora que ha configurado Device Update y aprovisionado los dispositivos, necesitará los archivos de actualización que va a implementar en esos dispositivos.

Si ha adquirido dispositivos de un OEM o integrador de soluciones, es muy probable que esa organización le proporcione archivos de actualización, por lo que no necesitará crear las actualizaciones. Póngase en contacto con el OEM o el integrador de soluciones para saber cómo facilitan sus actualizaciones.

Si su organización ya crea software para los dispositivos que usa, ese mismo grupo será el que cree las actualizaciones de ese software. Al crear una actualización para implementarla con Device Update for IoT Hub, empiece con el [enfoque basado en imágenes o basado en paquetes](understand-device-update.md#support-for-a-wide-range-of-update-artifacts), en función de su escenario. Nota: Si desea crear sus propias actualizaciones pero no tiene experiencia, GitHub es una excelente opción para administrar el desarrollo. Puede almacenar y administrar el código fuente y realizar integración continua (CI) e implementación continua (CD) mediante [Acciones de GitHub](https://docs.github.com/en/actions/guides/about-continuous-integration).

## <a name="create-a-device-update-import-manifest"></a>Creación de un manifiesto de importación de Device Update

Si todavía no lo ha hecho, asegúrese de familiarizarse con los [conceptos de importación](import-concepts.md) básicos y pruebe a consultar un tutorial de importación [basada en imagen](device-update-raspberry-pi.md) o [basada en paquetes](device-update-ubuntu-agent.md).

1. Asegúrese de que los archivos de actualización se encuentran en un directorio al que se pueda acceder desde PowerShell.

2. Cree un archivo de texto denominado **AduUpdate.psm1** en el directorio donde se encuentra el archivo de imagen de actualización o el archivo de manifiesto APT. Después, abra el cmdlet de PowerShell [AduUpdate.psm1](https://github.com/Azure/iot-hub-device-update/tree/main/tools/AduCmdlets), copie el contenido en el archivo de texto y, a continuación, guarde el archivo de texto.

3. En PowerShell, navegue hasta el directorio en el que ha creado el cmdlet de PowerShell en el paso 2. Use la opción Copiar que aparece a continuación y péguela en PowerShell para ejecutar los comandos:

    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
    Import-Module .\AduUpdate.psm1
    ```

4. Ejecute los siguientes comandos, para lo que debe reemplazar los valores de los parámetros de ejemplo para generar un manifiesto de importación, un archivo JSON que describe la actualización:
    ```powershell
    $compat = New-AduUpdateCompatibility -DeviceManufacturer 'deviceManufacturer' -DeviceModel 'deviceModel'

    $importManifest = New-AduImportManifest -Provider 'updateProvider' -Name 'updateName' -Version 'updateVersion' `
                                            -UpdateType 'updateType' -InstalledCriteria 'installedCriteria' `
                                            -Compatibility $compat -Files 'updateFilePath(s)'

    $importManifest | Out-File '.\importManifest.json' -Encoding UTF8
    ```

    La tabla siguiente es una referencia rápida de cómo rellenar los parámetros anteriores. Si necesita más información, también puede consultar el [esquema del manifiesto de importación](import-schema.md) completo.

    | Parámetro | Descripción |
    | --------- | ----------- |
    | deviceManufacturer | El fabricante del dispositivo con el que la actualización es compatible, por ejemplo, Contoso. Debe coincidir con la [propiedad del dispositivo](./device-update-plug-and-play.md#device-properties) _manufacturer_.
    | deviceModel | El modelo del dispositivo con el que la actualización es compatible, por ejemplo, Tostador. Debe coincidir con la [propiedad del dispositivo](./device-update-plug-and-play.md#device-properties) _model_.
    | updateProvider | Entidad que está creando la actualización o que es directamente responsable de ella. Suele ser un nombre de empresa.
    | updateName | Identificador de una clase de actualizaciones. La clase puede ser cualquier cosa que elija. Suele ser un nombre de dispositivo o de modelo.
    | updateVersion | Número de versión que diferencia a esta actualización de otras con el mismo proveedor y el mismo nombre. No coincide con ninguna versión de un componente de software individual en el dispositivo (pero puede coincidir si elige uno).
    | updateType | <ul><li>Especifique `microsoft/swupdate:1` para la actualización de la imagen.</li><li>Especifique `microsoft/apt:1` para la actualización del paquete.</li></ul>
    | installedCriteria | Se usa durante la implementación para comparar la versión que ya está en el dispositivo con la versión de la actualización. La implementación de la actualización en el dispositivo devolverá un resultado con errores si el valor installedCriteria no coincide con la versión que está en el dispositivo.<ul><li>Para el tipo de actualización`microsoft/swupdate:1`, especifique el valor de SWVersion. </li><li>Para el tipo de actualización `microsoft/apt:1`, especifique el valor **name-version**, donde _name_ es el nombre del manifiesto APT y _version_ es la versión del manifiesto APT. Por ejemplo, contoso-iot-edge-1.0.0.0.
    | updateFilePath(s) | Ruta de acceso a los archivos de actualización en el equipo.


## <a name="review-the-generated-import-manifest"></a>Revisión del manifiesto de importación generado

A continuación se muestra una salida del manifiesto de ejemplo. En este ejemplo, hay dos archivos que componen esta actualización: un archivo .json y un archivo .zip. Si tiene alguna pregunta sobre cualquiera de los elementos, consulte el [esquema del manifiesto de importación](import-schema.md) completo. 
```json
{
  "updateId": {
    "provider": "Microsoft",
    "name": "Toaster",
    "version": "2.0"
  },
  "updateType": "microsoft/swupdate:1",
  "installedCriteria": "5",
  "compatibility": [
    {
      "deviceManufacturer": "Fabrikam",
      "deviceModel": "Toaster"
    },
    {
      "deviceManufacturer": "Contoso",
      "deviceModel": "Toaster"
    }
  ],
  "files": [
    {
      "filename": "file1.json",
      "sizeInBytes": 7,
      "hashes": {
        "sha256": "K2mn97qWmKSaSaM9SFdhC0QIEJ/wluXV7CoTlM8zMUo="
      }
    },
    {
      "filename": "file2.zip",
      "sizeInBytes": 11,
      "hashes": {
        "sha256": "gbG9pxCr9RMH2Pv57vBxKjm89uhUstD06wvQSioLMgU="
      }
    }
  ],
  "createdDateTime": "2020-10-08T03:32:52.477Z",
  "manifestVersion": "2.0"
}
```

## <a name="import-an-update"></a>Importación de una actualización

> [!NOTE]
> En las siguientes instrucciones se muestra cómo importar una actualización a través de la interfaz de usuario de Azure Portal. En su lugar, también puede usar las [API de Device Update for IoT Hub](#if-youre-importing-via-apis-instead) para importar una actualización.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a su instancia de IoT Hub con Device Update.

2. En el lado izquierdo de la página, seleccione "Actualizaciones de dispositivos" en "Administración de dispositivos automática".

   :::image type="content" source="media/import-update/import-updates-3.png" alt-text="Importación de actualizaciones" lightbox="media/import-update/import-updates-3.png":::

3. Verá varias pestañas en la parte superior de la pantalla. Seleccione la pestaña Actualizaciones.

   :::image type="content" source="media/import-update/updates-tab.png" alt-text="Actualizaciones" lightbox="media/import-update/updates-tab.png":::

4. Seleccione "+ Import New Update" (Importar nueva actualización) en el encabezado "Preparado para implementar".

   :::image type="content" source="media/import-update/import-new-update-2.png" alt-text="Importación de una nueva actualización" lightbox="media/import-update/import-new-update-2.png":::

5. Seleccione el icono de la carpeta o el cuadro de texto en "Select an Import Manifest File" (Seleccionar un archivo de manifiesto de importación). Verá un cuadro de diálogo para seleccionar archivos. Seleccione el manifiesto de importación que creó anteriormente mediante el cmdlet de PowerShell. Seleccione el icono de la carpeta o el cuadro de texto en "Select one or more update files" (Seleccionar uno o varios archivos de importación). Verá un cuadro de diálogo para seleccionar archivos. Seleccione los mismos archivos de actualización que incluyó al crear el manifiesto de importación.

   :::image type="content" source="media/import-update/select-update-files.png" alt-text="Seleccionar archivos de actualización" lightbox="media/import-update/select-update-files.png":::

6. Seleccione el icono de la carpeta o el cuadro de texto en "Select a storage container" (Seleccionar un contenedor de almacenamiento). Luego, seleccione la cuenta de almacenamiento adecuada. El contenedor de almacenamiento se usa para almacenar temporalmente los archivos de actualización.

   :::image type="content" source="media/import-update/storage-account.png" alt-text="Storage Account" lightbox="media/import-update/storage-account.png":::

7. Si ya ha creado un contenedor, puede volver a usarlo (de lo contrario, seleccione "+ Contenedor" para crear contenedor de almacenamiento para las actualizaciones).  Seleccione el contenedor que desee usar y haga clic en "Seleccionar".

   :::image type="content" source="media/import-update/container.png" alt-text="Seleccionar contenedor" lightbox="media/import-update/container.png":::

8. Seleccione "Enviar" para iniciar el proceso de importación.

   :::image type="content" source="media/import-update/publish-update.png" alt-text="Publicar actualización" lightbox="media/import-update/publish-update.png":::

9. Se inicia el proceso de importación y la pantalla cambia a la sección "Historial de importación". Seleccione "Actualizar" para ver el progreso hasta que finalice el proceso de importación (en función del tamaño de la actualización, esta operación puede tardar unos minutos o prolongarse mucho tiempo).

   :::image type="content" source="media/import-update/update-publishing-sequence-2.png" alt-text="Actualizar secuenciación de importación" lightbox="media/import-update/update-publishing-sequence-2.png":::

10. Cuando la columna Estado indique que la importación se ha realizado correctamente, seleccione el encabezado "Ready to Deploy" (Listo para implementar). Debería ver la actualización importada en la lista.

   :::image type="content" source="media/import-update/update-ready.png" alt-text="Estado del trabajo" lightbox="media/import-update/update-ready.png":::

## <a name="next-steps"></a>Pasos siguientes

[Creación de grupos](create-update-group.md)

[Información sobre los conceptos de la importación](import-concepts.md)

## <a name="if-youre-importing-via-apis-instead"></a>Si en su lugar va a importar a través de las API

Si quiere usar las [API de actualización de Device Update for IoT Hub](/rest/api/deviceupdate/updates) para importar una actualización en lugar de importarla a través de Azure Portal, tenga en cuenta lo siguiente:
  - Tendrá que cargar los archivos de actualización en una ubicación de Azure Blob Storage antes de llamar a las API de actualización.
  - Puede hacer referencia a esta [llamada API de ejemplo](import-schema.md#example-import-request-body) que usa el manifiesto de importación que creó anteriormente.
  - Si vuelve a usar la misma dirección URL de SAS durante las pruebas, puede que se produzcan errores cuando expire el token. Esto sucede cuando se envía el manifiesto de importación, así como el propio contenido de actualización.