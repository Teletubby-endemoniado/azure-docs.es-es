---
title: Servicio de atestación de FPGFA de Azure
description: Servicio de atestación para las máquinas virtuales de la serie NP.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
ms.topic: conceptual
ms.date: 04/01/2021
ms.author: vikancha
ms.openlocfilehash: dba6962199f61eeb93dfb2f98e3e448c94ff633a
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128567107"
---
# <a name="fpga-attestation-for-azure-np-series-vms-preview"></a>Atestación de FPGA para las máquinas virtuales de Azure de la serie NP (versión preliminar)

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

El servicio de atestación de FPGA realiza una serie de validaciones en un archivo de punto de comprobación de diseño (llamado "netlist") generado por el conjunto de herramientas de Xilinx y genera un archivo que contiene la imagen validada (llamada "bitstream") que se puede cargar en la tarjeta FPGA Xilinx U250 en una máquina virtual de la serie NP.  

## <a name="prerequisites"></a>Requisitos previos  

Necesitará una suscripción de Azure y una cuenta de Azure Storage. La suscripción le proporciona acceso a Azure y la cuenta de almacenamiento se usa para almacenar los archivos de salida y el archivo netlist del servicio de atestación.  

Se proporcionan scripts de PowerShell y Bash para enviar solicitudes de atestación.   Los scripts usan la CLI de Azure, que se puede ejecutar en Windows y Linux. PowerShell se puede ejecutar en Windows, Linux y macOS.  

[Descarga de la CLI de Azure (obligatorio)](/cli/azure/install-azure-cli)

[Descarga de PowerShell para Windows, Linux y macOS (solo para scripts de PowerShell)](/powershell/scripting/install/installing-powershell)

Deberá tener autorizado el identificador de inquilino y de suscripción para los envíos al servicio de atestación. Visite [https://aka.ms/AzureFPGAAttestationPreview](https://aka.ms/AzureFPGAAttestationPreview) para solicitar acceso. 

## <a name="building-your-design-for-attestation"></a>Creación del diseño para la atestación  

El conjunto de herramientas de Xilinx preferido para la creación de diseños es Vitis 2020.2. Se pueden usar los archivos netlist que se crearon con una versión anterior del conjunto de herramientas y que sigan siendo compatibles con la versión 2020.2. Asegúrese de que ha cargado el shell correcto con el que se va a realizar la compilación. La versión admitida actualmente es xilinx_u250_gen3x16_xdma_2_1_202010_1. Los archivos auxiliares se pueden descargar desde la sala de Xilinx Alveo. 

Debe incluir el siguiente argumento en Vitis (línea de comandos v++) para compilar un archivo xclbin que contenga un archivo netlist en lugar de un archivo bitstream.   

`--advanced.param compiler.acceleratorBinaryContent=dcp`

## <a name="logging-into-azure"></a>Inicio de sesión en Azure  

Antes de realizar cualquier operación con Azure, debe iniciar sesión en Azure y establecer la suscripción que está autorizada para llamar al servicio. Use los comandos `az login` y `az account set –s <Sub ID or Name>` para este fin. Aquí se documenta información adicional sobre este proceso: [Inicio de sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli). Use la opción **Iniciar sesión de forma interactiva** o **Iniciar sesión con credenciales** en la línea de comandos.  

## <a name="creating-a-storage-account-and-blob-container"></a>Creación de una cuenta de almacenamiento y un contenedor de blobs  

El archivo netlist se debe cargar en un contenedor de blobs de Azure Storage para que el servicio de atestación pueda acceder a él.  

Consulte esta página para más información sobre la creación de la cuenta, un contenedor y la carga del archivo netlist como un blob en ese contenedor: [https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-cli](../storage/blobs/storage-quickstart-blobs-cli.md).  

También puede usar Azure Portal para hacerlo.  

## <a name="upload-your-netlist-file-to-azure-blob-storage"></a>Carga del archivo netlist en Azure Blob Storage  

Hay varias maneras de copiar el archivo: a continuación se muestra un ejemplo del uso del cmdlet az storage upload. Los comandos az se ejecutan en Linux y Windows. Puede elegir cualquier nombre para el nombre del "blob", pero asegúrese de conservar la extensión `xclbin`.

`az storage blob upload --account-name <storage account to receive netlist> --container-name <blob container name> --name <blob filename> --file <local file with netlist>`

## <a name="download-the-attestation-scripts"></a>Descarga de los scripts de atestación  

Los scripts de validación se pueden descargar desde el siguiente contenedor de blobs de Azure Storage:  

[https://fpgaattestation.blob.core.windows.net/validationscripts/validate.zip](https://fpgaattestation.blob.core.windows.net/validationscripts/validate.zip)

El archivo zip tiene dos scripts de PowerShell, uno para enviar y otro para supervisar, mientras que el tercer archivo es un script de Bash que realiza ambas funciones.  

## <a name="running-the-attestation-scripts"></a>Ejecución de los scripts de atestación  

Para ejecutar los scripts, debe proporcionar el nombre de la cuenta de almacenamiento, el nombre del contenedor de blobs donde se almacena el archivo netlist y el nombre del archivo netlist. También debe crear una firma de acceso compartido (SAS) de servicio que conceda acceso de lectura y escritura al contenedor (no al archivo netlist). El servicio de atestación usa esta firma de acceso compartido para realizar una copia local del archivo netlist y escribir los archivos de salida resultantes del proceso de validación en el contenedor.  

Aquí encontrará información general sobre las firmas de acceso compartido con información específica sobre la SAS de servicio. La página de firma de acceso compartido de servicio incluye una advertencia importante sobre la protección de la firma de acceso compartido generada.  Lea la advertencia para comprender la necesidad de proteger la firma de acceso compartido del uso malintencionado o imprevisto.  

Puede generar una firma de acceso compartido para el contenedor mediante el cmdlet az storage container generate-sas. Especifique una hora de expiración en formato UTC que sea al menos unas horas después de la hora del envío; aproximadamente 6 horas deben ser más que suficientes.  

Si desea usar directorios virtuales, debe incluir la jerarquía de directorios como parte del argumento del contenedor. Por ejemplo, si tiene un contenedor llamado "netlists" y tiene un directorio virtual llamado "image1" que contiene el blob del archivo netlist, debe especificar "netlists/image1" como nombre del contenedor. Anexe cualquier nombre de directorio adicional para especificar una jerarquía más profunda. 

### <a name="powershell"></a>PowerShell   

```powershell
$sas=$(az storage container generate-sas --account-name <storage acct name> --name <blob container name> --https-only --permissions rwc --expiry <e.g., 2021-01-07T17:00Z> --output tsv)

.\Validate-FPGAImage.ps1 -StorageAccountName <storage acct name> -Container <blob container name> -BlobContainerSAS $sas -NetlistName <netlist blob filename>
```

### <a name="bash"></a>Bash  

```bash
sas=az storage container generate-sas --account-name <storage acct name> --name <blob container name> --https-only --permissions rwc --expiry <2021-01-07T17:00Z> --output tsv  

validate-fpgaimage.sh --storage-account <storage acct name> --container <blob container name> --netlist-name <netlist blob filename> --blob-container-sas $sas
``` 

## <a name="checking-on-the-status-of-your-submission"></a>Comprobación del estado del envío  

El servicio de atestación devolverá el identificador de orquestación del envío. Los scripts de envío inician automáticamente la supervisión del envío mediante sondeos de su finalización. El identificador de orquestación es la forma principal de revisar lo que ha ocurrido con el envío, por lo que debe conservarlo por si se produce un problema. Como puntos de referencia, la atestación tarda unos 30 minutos en completarse para un archivo netlist pequeño (300 MB de tamaño); un archivo de 1,6 GB tarda una hora. 

Puede llamar al script Monitor-Validation.ps1 en cualquier momento para obtener el estado y los resultados de la atestación; para ello, proporcione el identificador de orquestación como argumento:  

`.\Monitor-Validation.ps1 -OrchestrationId <orchestration ID>`

Como alternativa, puede enviar una solicitud HTTP POST al punto de conexión del servicio de atestación:  

`https://fpga-attestation.azurewebsites.net/api/ComputeFPGA_HttpGetStatus`

El cuerpo de la solicitud debe contener el identificador de la suscripción, el identificador de inquilino y el identificador de orquestación de la solicitud de atestación:  

```json
{  
  "OrchestrationId": "<orchestration ID>",  
  "ClientSubscriptionId": "<your subscription ID>",  
  "ClientTenantId": "<your tenant ID>"
}
```

## <a name="post-validation-steps"></a>Pasos posteriores a la validación

El servicio escribirá su salida en el contenedor. Si la fase de validación se realiza correctamente, el contenedor tendrá el archivo netlist original (abc.xclbin), un archivo bitstream (abc.bit.xclbin), un archivo que identifica la ubicación privada del archivo bitstream almacenado (abc.azure.xclbin) y cuatro archivos de registro: uno para el proceso de inicio (abc-log.txt) y otros para las tres fases que realizan la validación en paralelo. Estos archivos se llaman *logPhaseX.txt, donde X es el número de la fase. El archivo azure.xclbin se utiliza en la máquina virtual para señalizar la carga de la imagen validada en el dispositivo U250. 

Si se produce un error de validación, se escribe el archivo error-*.txt, que indica el paso con errores. Compruebe también los archivos de registro si el registro de errores indica que se produjo un error en la atestación. Al ponerse en contacto con nosotros para obtener soporte técnico, asegúrese de incluir todos estos archivos como parte de la solicitud de soporte técnico junto con el identificador de orquestación.  

Puede usar Azure Portal para crear el contenedor, así como para cargar el archivo netlist y descargar los archivos bitstream y de registro. El envío de una solicitud de atestación y la supervisión del progreso mediante el portal no se admiten en este momento y se deben realizar mediante scripts, como se ha descrito anteriormente.
