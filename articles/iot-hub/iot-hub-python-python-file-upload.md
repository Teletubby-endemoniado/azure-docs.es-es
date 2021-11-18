---
title: Carga de archivos desde dispositivos a Azure IoT Hub con Python | Microsoft Docs
description: Cómo cargar archivos de un dispositivo a la nube mediante el SDK de dispositivo IoT de Azure para Python. Los archivos cargados se almacenan en un contenedor de blobs de Azure Storage.
author: eross-msft
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/18/2021
ms.author: lizross
ms.custom: mqtt, devx-track-python
ms.openlocfilehash: d12ab40ee2bf585607f9dd60670e607decc4113a
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132548986"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-python"></a>Carga de archivos de un dispositivo a la nube con IoT Hub (Python)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

En este tutorial, se describe cómo usar las [funcionalidades de carga de archivos de IoT Hub](iot-hub-devguide-file-upload.md) para cargar un archivo en [Azure Blob Storage](../storage/index.yml). En este tutorial se muestra cómo realizar las siguientes acciones:

* Proporcionar de forma segura un contenedor de almacenamiento para cargar un archivo.

* Usar el cliente de Python para cargar un archivo mediante IoT Hub.

En el inicio rápido sobre el [Envío de telemetría desde un dispositivo a un centro de IoT](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-python) se explica la funcionalidad básica de mensajería del dispositivo a la nube de IoT Hub. Sin embargo, en algunos casos no se pueden asignar fácilmente los datos de que los dispositivos envían en los mensajes de dispositivo a nube con un tamaño relativamente reducido que acepta Azure IoT Hub. Cuando necesite cargar archivos desde un dispositivo, todavía puede usar la seguridad y confiabilidad de IoT Hub.

Al final de este tutorial, ejecutará la aplicación de consola de Python:

* **FileUpload.py**, que carga un archivo en el almacenamiento con el SDK de dispositivo de Python.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

[!INCLUDE [iot-hub-include-x509-ca-signed-file-upload-support-note](../../includes/iot-hub-include-x509-ca-signed-file-upload-support-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

* Asegúrese de que el puerto 8883 está abierto en el firewall. En el ejemplo de dispositivo de este artículo se usa el protocolo MQTT, que se comunica mediante el puerto 8883. Este puerto puede estar bloqueado en algunos entornos de red corporativos y educativos. Para más información y para saber cómo solucionar este problema, consulte el artículo sobre la [conexión a IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registro de un nuevo dispositivo en el centro de IoT

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-include-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Carga de un archivo desde una aplicación de dispositivo

En esta sección, creará la aplicación para dispositivos para cargar un archivo en IoT Hub.

1. En el símbolo del sistema, ejecute el siguiente comando para instalar el paquete **azure-iot-device**. Este paquete se usa para coordinar la carga de archivos con el centro de IoT.

    ```cmd/sh
    pip install azure-iot-device
    ```

1. En el símbolo del sistema, ejecute el siguiente comando para instalar el paquete [**azure.storage.blob**](https://pypi.org/project/azure-storage-blob/). Este paquete se usa para realizar la carga de archivos.

    ```cmd/sh
    pip install azure.storage.blob
    ```

1. Cree un archivo de prueba que se cargará en Blob Storage.

1. Con un editor de texto, cree un archivo **FileUpload.py** en la carpeta de trabajo.

1. Agregue las siguientes instrucciones y variables `import` al principio del archivo **FileUpload.py**.

    ```python
    import os
    from azure.iot.device import IoTHubDeviceClient
    from azure.core.exceptions import AzureError
    from azure.storage.blob import BlobClient

    CONNECTION_STRING = "[Device Connection String]"
    PATH_TO_FILE = r"[Full path to local file]"
    ```

1. En el archivo, sustituya `[Device Connection String]` por la cadena de conexión del dispositivo de IoT Hub. Reemplace `[Full path to local file]` por la ruta de acceso del archivo de prueba que creó o de cualquier archivo del dispositivo que quiera cargar.

1. Cree una función para cargar el archivo a Blob Storage:

    ```python
    def store_blob(blob_info, file_name):
        try:
            sas_url = "https://{}/{}/{}{}".format(
                blob_info["hostName"],
                blob_info["containerName"],
                blob_info["blobName"],
                blob_info["sasToken"]
            )

            print("\nUploading file: {} to Azure Storage as blob: {} in container {}\n".format(file_name, blob_info["blobName"], blob_info["containerName"]))

            # Upload the specified file
            with BlobClient.from_blob_url(sas_url) as blob_client:
                with open(file_name, "rb") as f:
                    result = blob_client.upload_blob(f, overwrite=True)
                    return (True, result)

        except FileNotFoundError as ex:
            # catch file not found and add an HTTP status code to return in notification to IoT Hub
            ex.status_code = 404
            return (False, ex)

        except AzureError as ex:
            # catch Azure errors that might result from the upload operation
            return (False, ex)
    ```

    Esta función analiza la estructura *blob_info* que se ha pasado para crear una dirección URL que se usa para inicializar un objeto [azure.storage.blob.BlobClient](/python/api/azure-storage-blob/azure.storage.blob.blobclient). Después, carga el archivo a Azure Blob Storage mediante este cliente.

1. Agregue el siguiente código para conectar el cliente y cargar el archivo:

    ```python
    def run_sample(device_client):
        # Connect the client
        device_client.connect()

        # Get the storage info for the blob
        blob_name = os.path.basename(PATH_TO_FILE)
        storage_info = device_client.get_storage_info_for_blob(blob_name)

        # Upload to blob
        success, result = store_blob(storage_info, PATH_TO_FILE)

        if success == True:
            print("Upload succeeded. Result is: \n") 
            print(result)
            print()

            device_client.notify_blob_upload_status(
                storage_info["correlationId"], True, 200, "OK: {}".format(PATH_TO_FILE)
            )

        else :
            # If the upload was not successful, the result is the exception object
            print("Upload failed. Exception is: \n") 
            print(result)
            print()

            device_client.notify_blob_upload_status(
                storage_info["correlationId"], False, result.status_code, str(result)
            )

    def main():
        device_client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)

        try:
            print ("IoT Hub file upload sample, press Ctrl-C to exit")
            run_sample(device_client)
        except KeyboardInterrupt:
            print ("IoTHubDeviceClient sample stopped")
        finally:
            # Graceful exit
            device_client.shutdown()


    if __name__ == "__main__":
        main()
    ```

    Este código crea un **IoTHubDeviceClient** y usa las siguientes API para administrar la carga de archivos con el centro de IoT:

    * **get_storage_info_for_blob** obtiene información del centro de IoT sobre la cuenta de almacenamiento vinculada que creó anteriormente. Esta información incluye el nombre de host, el nombre del contenedor, el nombre del blob y un token de SAS. La información de almacenamiento se pasa a la función **store_blob función de** (creada en el paso anterior), para que el elemento **BlobClient** en esa función pueda autenticarse con Azure Storage. El método **get_storage_info_for_blob** también devuelve un correlation_id, que se usa en el método **notify_blob_upload_status**. El correlation_id es la manera en que IoT Hub muestra en qué blob está trabajando.

    * **notify_blob_upload_status** notifica a IoT Hub del estado de la operación de almacenamiento de blobs. Se le pasa el correlation_id que obtuvo a través del método **get_storage_info_for_blob**. IoT Hub lo usa para notificar a cualquier servicio que pueda estar escuchando sobre el estado de la tarea de carga de archivos.

1. Guarde y cierre el archivo **FileUpload.py**.

## <a name="run-the-application"></a>Ejecución de la aplicación

Ahora está listo para ejecutar la aplicación.

1. En un símbolo del sistema de la carpeta de trabajo, ejecute el siguiente comando:

    ```cmd/sh
    python FileUpload.py
    ```

2. En la captura de pantalla siguiente se muestra la salida de la aplicación **FileUpload**:

    ![Salida de la aplicación simulated-device](./media/iot-hub-python-python-file-upload/run-device-app.png)

3. Puede usar el portal para ver el archivo cargado en el contenedor de almacenamiento que configuró:

    ![Archivo cargado](./media/iot-hub-python-python-file-upload/view-blob.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a usar la funcionalidad de carga de archivos de IoT Hub para simplificar la carga de archivos desde los dispositivos. Puede continuar explorando las características y los escenarios del centro de IoT con los siguientes artículos:

* [Creación de un centro de IoT mediante programación](iot-hub-rm-template-powershell.md)

* [Introducción a SDK para C](iot-hub-device-sdk-c-intro.md)

* [SDK de IoT de Azure](iot-hub-devguide-sdks.md)

Obtenga más información sobre Azure Blob Storage con los siguientes vínculos:

* [Documentación de Azure Blob Storage](../storage/blobs/index.yml)

* [Documentación de Azure Blob Storage para la API de Python](/python/api/overview/azure/storage-blob-readme)