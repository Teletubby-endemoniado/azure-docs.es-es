---
title: Exploración de los datos y el modelo en Windows
titleSuffix: Azure Data Science Virtual Machine
description: Realice tareas de exploración y modelado de datos en Windows Data Science Virtual Machine.
services: machine-learning
ms.service: data-science-vm
ms.custom: devx-track-python, devx-track-azurepowershell
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: b855f8fd464335e368e70f193f27d9baf5a10b11
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128567509"
---
# <a name="data-science-with-a-windows-data-science-virtual-machine"></a>Ciencia de datos con una instancia de Data Science Virtual Machine para Windows

Windows Data Science Virtual Machine (DSVM) es un entorno de desarrollo de ciencia de datos eficaz en el que puede realizar tareas de exploración y modelado de datos. El entorno incluye ya en su compilación varias herramientas populares de análisis de datos que le permitirán comenzar fácilmente su análisis de implementaciones locales, híbridas o en la nube. 

DSVM trabaja estrechamente con los servicios de Azure. Puede leer y procesar los datos que ya están almacenados en Azure, Azure Synapse (anteriormente SQL DW), Azure Data Lake, Azure Storage o Azure Cosmos DB. También puede aprovechar otras herramientas de análisis como Azure Machine Learning.

En este artículo, aprenderá a usar DSVM para realizar tareas de ciencia de datos e interactuar con otros servicios de Azure. Estas son algunas de las tareas que puede hacer en DSVM:

- Usar un cuaderno de Jupyter Notebook para experimentar con los datos en un explorador mediante Python 2, Python 3 y Microsoft R. (Microsoft R es una versión de R preparada para las empresas y diseñada para ofrecer rendimiento).
- Explorar datos y desarrollar modelos localmente en DSVM con Microsoft Machine Learning Server y Python.
- Administrar los recursos de Azure mediante Azure Portal o PowerShell.
- Ampliar el espacio de almacenamiento y compartir código o conjuntos de datos de gran escala entre todo el equipo al crear un recurso compartido de Azure Files como una unidad que se puede montar en DSVM.
- Compartir código con su equipo mediante GitHub. Acceder al repositorio mediante los clientes de Git preinstalados: Git Bash y Git GUI.
- Acceder a servicios de análisis y datos de Azure, como Azure Blob Storage, Azure Cosmos DB, Azure Synapse (anteriormente SQL DW) y Azure SQL Database.
- Crear informes y un panel con la instancia de Power BI Desktop que está preinstalada en DSVM e implementarlos en la nube.

- Instalar herramientas adicionales en la máquina virtual.   

> [!NOTE]
> A muchos de los servicios de almacenamiento y análisis de datos que se enumeran en este artículo se les aplican cargos de uso adicionales. Para obtener información detallada, consulte la página de [precios de Azure](https://azure.microsoft.com/pricing/).
> 
> 

## <a name="prerequisites"></a>Prerrequisitos

* Necesita una suscripción de Azure. Puede [suscribirse a una evaluación gratuita](https://azure.microsoft.com/free/).
* Las instrucciones para el aprovisionamiento de Data Science Virtual Machine en Azure Portal están disponibles en [Creación de una máquina virtual](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]


## <a name="use-jupyter-notebooks"></a>Uso de Jupyter Notebooks
Jupyter Notebook proporciona un IDE basado en el explorador para explorar y modelar datos. En un cuaderno de Jupyter Notebook se pueden usar Python 2, Python 3 o R.

Para iniciar el cuaderno de Jupyter Notebook, seleccione el icono **Jupyter Notebook** en el menú **Inicio** o en el escritorio. En el símbolo del sistema de DSVM, también puede ejecutar el comando ```jupyter notebook``` desde el directorio donde tiene cuadernos existentes o donde quiere crear cuadernos.  

Después de iniciar Jupyter, vaya al directorio `/notebooks` para ver los cuadernos de ejemplo que vienen incluidos en DSVM. Ahora puede:

* Seleccionar el cuaderno para ver el código.
* Seleccionar Mayús+Entrar para ejecutar cada celda.
* Seleccionar **Celda** > **Ejecutar** para ejecutar todo el cuaderno.
* Seleccionar el icono de Jupyter (esquina superior izquierda), después el botón **Nuevo** situado a la derecha y, finalmente, elegir el idioma del cuaderno (también conocido como kernels) para crear un cuaderno.   

> [!NOTE]
> Actualmente se admiten los kernels de Python 2.7, Python 3.6, R, Julia y PySpark en Jupyter. El kernel de R admite la programación en R de código abierto y en Microsoft R.   
> 
> 

Cuando esté en el cuaderno, podrá explorar los datos, así como compilar y probar el modelo con las bibliotecas que elija.

## <a name="explore-data-and-develop-models-with-microsoft-machine-learning-server"></a>Exploración de datos y desarrollo de modelos con Microsoft Machine Learning Server

> [!NOTE]
> El soporte técnico para Machine Learning Server independiente finalizó el 1 de julio de 2021. Como consecuencia, después del 30 de junio se eliminó de las imágenes de DSVM. Las implementaciones existentes seguirán teniendo acceso al software, pero debido a que se ha alcanzado la fecha de finalización del soporte técnico, no habrá soporte técnico para él después del 1 de julio de 2021.

Puede utilizar lenguajes como R y Python para realizar el análisis de datos directamente en DSVM.

Para R, puede usar un IDE como RStudio que se puede encontrar en el menú Inicio o en el escritorio. O bien puede usar Herramientas de R para Visual Studio. Microsoft ha aportado bibliotecas adicionales además de la de CRAN R de código abierto para habilitar el análisis escalable y la capacidad de analizar datos mayores que el tamaño de memoria permitido en el análisis de fragmentos en paralelo. 

En el caso de Python, puede usar un IDE como Visual Studio Community Edition, que ya tiene preinstalada la extensión Herramientas de Python para Visual Studio (PTVS). De forma predeterminada, solo Python 3.6, el entorno raíz de Conda, se configura en PTVS. Siga estos pasos para habilitar Anaconda Python 2.7:

1. Cree entornos personalizados para cada versión; para ello, vaya a **Herramientas** > **Herramientas de Python** > **Entornos de Python** y seleccione **+ Personalizado** en Visual Studio Community Edition.
1. Incluya una descripción y establezca la ruta de acceso del prefijo del entorno como **c:\anaconda\envs\python2** para Anaconda Python 2.7.
1. Seleccione **Detectar automáticamente** > **Aplicar** para guardar el entorno.

Para consultar más detalles sobre cómo crear entornos de Python, vea la [documentación de PTVS](/visualstudio/python/).

Ya tiene todo configurado para crear un proyecto de Python. Vaya a **Archivo** > **Nuevo** > **Proyecto** > **Python** y seleccione el tipo de aplicación de Python que va a compilar. Puede establecer el entorno de Python del proyecto actual en la versión que quiera (Python 2.7 o 3.6) si hace clic con el botón derecho en **Entornos de Python** y selecciona **Agregar o quitar entornos de Python**. Puede encontrar más información sobre cómo trabajar con PTVS en la [documentación del producto](/visualstudio/python/).



## <a name="manage-azure-resources"></a>Administración de recursos de Azure
DSVM no solo le permite compilar su solución de análisis de forma local en la máquina virtual, sino que también le permite acceder a los servicios de la plataforma en la nube de Azure. Azure proporciona varios servicios de proceso, almacenamiento, análisis de datos y de otra índole que puede administrar desde su DSVM y a los que puede acceder desde dicho entorno.

Para administrar los recursos de la nube y la suscripción de Azure, tiene dos opciones:
+ Vaya a [Azure Portal](https://portal.azure.com) en el explorador.

+ Use scripts de PowerShell. Ejecute Azure PowerShell desde un acceso directo del escritorio o desde el menú **Inicio**. Consulte la [documentación de Microsoft Azure PowerShell](../../azure-resource-manager/management/manage-resources-powershell.md) para obtener más información. 

## <a name="extend-storage-by-using-shared-file-systems"></a>Extensión del almacenamiento mediante sistemas de archivos compartidos
Los científicos de datos pueden compartir grandes conjuntos de datos, código u otros recursos dentro del equipo. DSVM tiene aproximadamente 45 GB de espacio disponible. Para ampliar el almacenamiento, puede usar Azure Files y montarlo en una o varias instancias de DSVM, o bien acceder a él mediante la API REST. También puede usar [Azure Portal](../../virtual-machines/windows/attach-managed-disk-portal.md) o [Azure PowerShell](../../virtual-machines/windows/attach-disk-ps.md) para agregar discos de datos dedicados adicionales. 

> [!NOTE]
> El espacio máximo en el recurso compartido de Azure Files es de 5 TB. El límite de tamaño de cada archivo es de 1 TB. 

Puede usar este script en Azure PowerShell para crear un recurso compartido de Azure Files:

```powershell
# Authenticate to Azure.
Connect-AzAccount
# Select your subscription
Get-AzSubscription –SubscriptionName "<your subscription name>" | Select-AzSubscription
# Create a new resource group.
New-AzResourceGroup -Name <dsvmdatarg>
# Create a new storage account. You can reuse existing storage account if you want.
New-AzStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
# Set your current working storage account
Set-AzCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

# Create an Azure Files share
$s = New-AzStorageShare <<teamsharename>>
# Create a directory under the file share. You can give it any name
New-AzStorageDirectory -Share $s -Path <directory name>
# List the share to confirm that everything worked
Get-AzStorageFile -Share $s
```

Ahora que ha creado un recurso compartido de Azure Files, puede montarlo en cualquier máquina virtual de Azure. Le recomendamos que ponga la máquina virtual en el mismo centro de datos de Azure que la cuenta de almacenamiento para evitar la latencia y los cobros por la transferencia de datos. Estos son los comandos de Azure PowerShell para montar la unidad en DSVM:

```powershell
# Get the storage key of the storage account that has the Azure Files share from the Azure portal. Store it securely on the VM to avoid being prompted in the next command.
cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

# Mount the Azure Files share as drive Z on the VM. You can choose another drive letter if you want.
net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>
```

Ya puede acceder a esta unidad del mismo modo que a cualquier otra unidad normal de la máquina virtual.

## <a name="share-code-in-github"></a>Código compartido en GitHub
GitHub es un repositorio de código donde puede encontrar ejemplos de código y orígenes para diferentes herramientas mediante las tecnologías que comparte la comunidad de desarrolladores. Utiliza Git como tecnología para realizar un seguimiento de las versiones de los archivos de código y almacenarlas. Además, GitHub es una plataforma donde podrá crear su propio repositorio para almacenar el código compartido y la documentación de su equipo, implementar el control de versiones y controlar quién tiene acceso para ver el código y contribuir en él. 

Para más información sobre el uso de Git, visite las [páginas de ayuda de GitHub](https://help.github.com/). Puede usar GitHub como uno de los métodos para colaborar con su equipo, usar el código desarrollado por la comunidad y aportar código de vuelta a ella.

DSVM incluye herramientas de cliente en la línea de comandos y en la GUI para poder acceder al repositorio de GitHub. La herramienta de línea de comandos que funciona con Git y GitHub se denomina Git Bash. Visual Studio está instalado en DSVM y cuenta con las extensiones de Git. Puede encontrar los iconos de estas herramientas en el menú **Inicio** y en el escritorio.

Para descargar código de un repositorio de GitHub, debe usar el comando ```git clone```. Por ejemplo, para descargar el repositorio de ciencia de datos que publica Microsoft en el directorio actual, puede ejecutar el siguiente comando en Git Bash:

```bash
git clone https://github.com/Azure/DataScienceVM.git
```

En Visual Studio, puede realizar la misma operación de clonación. En la captura de pantalla siguiente, se muestra cómo acceder a las herramientas de Git y GitHub en Visual Studio:

![Captura de Visual Studio con la conexión de GitHub mostrada](./media/vm-do-ten-things/VSGit.png)

En los recursos disponibles en github.com, puede encontrar más información sobre cómo usar Git para trabajar con el repositorio de GitHub. La [hoja de referencia rápida](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) puede resultar un recurso útil.

## <a name="access-azure-data-and-analytics-services"></a>Acceso a servicios de análisis y datos de Azure
### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Blob Storage es un servicio de almacenamiento en nube confiable y económico para muchos y pocos datos. En esta sección se describe cómo mover los datos a Blob Storage y cómo acceder a los datos almacenados en un blob de Azure.

#### <a name="prerequisites"></a>Prerrequisitos

* Cree una cuenta de Azure Blob Storage desde [Azure Portal](https://portal.azure.com).

   ![Captura de pantalla del proceso de creación de una cuenta de almacenamiento en Azure Portal](./media/vm-do-ten-things/create-azure-blob.png)

* Confirme que la herramienta de línea de comandos AzCopy se ha instalado previamente: ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. El directorio que contiene el archivo azcopy.exe ya se encuentra en la variable de entorno PATH para que no tenga que escribir la ruta de acceso completa del comando al ejecutar esta herramienta. Para obtener más información sobre la herramienta AzCopy, consulte la [documentación de AzCopy](../../storage/common/storage-use-azcopy-v10.md).
* Inicie el Explorador de Azure Storage desde aquí. Puede descargarlo desde la [página web del Explorador de Storage](https://storageexplorer.com/). 

   ![Captura de pantalla del Explorador de Azure Storage con acceso a una cuenta de almacenamiento](./media/vm-do-ten-things/AzureStorageExplorer_v4.png)

#### <a name="move-data-from-a-vm-to-an-azure-blob-azcopy"></a>Traslado de datos de una máquina virtual a un blob de Azure: AzCopy

Para mover datos entre los archivos locales y Blob Storage, puede usar AzCopy en la línea de comandos o en PowerShell:

```powershell
AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt
```

Reemplace **C:\myfolder** por la ruta de acceso al lugar donde está almacenado su archivo, **mystorageaccount** por el nombre de su cuenta de Blob Storage, **mycontainer** por el nombre del contenedor y **storage account key** por la clave de acceso a Blob Storage. Puede encontrar las credenciales de su cuenta de almacenamiento en [Azure Portal](https://portal.azure.com).

Ejecute el comando de AzCopy en PowerShell o desde un símbolo del sistema. Aquí puede ver algún ejemplo de uso del comando de AzCopy:

```powershell
# Copy *.sql from a local machine to an Azure blob
"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

# Copy back all files from an Azure blob container to a local machine

"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S
```

Después de ejecutar el comando de AzCopy para copiar en un blob de Azure, el archivo aparecerá en el Explorador de Azure Storage.

![Captura de pantalla de la cuenta de almacenamiento en la que se muestra el archivo .csv cargado](./media/vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

#### <a name="move-data-from-a-vm-to-an-azure-blob-azure-storage-explorer"></a>Traslado de datos de una máquina virtual a un blob de Azure: Explorador de Azure Storage

También puede cargar datos desde el archivo local en la máquina virtual mediante el Explorador de Azure Storage:

* Para cargar datos en un contenedor, elija el contenedor de destino y seleccione el botón **Cargar**.![Captura de pantalla del botón de carga en el Explorador de Azure Storage](./media/vm-do-ten-things/storage-accounts.png)
* Seleccione los puntos suspensivos ( **...** ) que se encuentran a la derecha del cuadro **Archivos**, elija uno o varios archivos que cargar del sistema de archivos y seleccione **Cargar** para empezar a cargarlos.![Captura de pantalla del cuadro de diálogo para cargar archivos](./media/vm-do-ten-things/upload-files-to-blob.png)

#### <a name="read-data-from-an-azure-blob-python-odbc"></a>Lectura de datos desde un blob de Azure: ODBC de Python

Puede usar la biblioteca BlobService para leer datos directamente de un blob en un cuaderno de Jupyter Notebook o en un programa de Python.

En primer lugar, importe los paquetes necesarios:

```python
import pandas as pd
from pandas import Series, DataFrame
import numpy as np
import matplotlib.pyplot as plt
from time import time
import pyodbc
import os
from azure.storage.blob import BlobService
import tables
import time
import zipfile
import random
```

Después, especifique las credenciales de su cuenta de Blob Storage y lea los datos del blob:

```python
CONTAINERNAME = 'xxx'
STORAGEACCOUNTNAME = 'xxxx'
STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
localfilename = 'trip_data_1.csv'
LOCALDIRECTORY = os.getcwd()
LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

#download from blob
t1 = time.time()
blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
t2 = time.time()
print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

#unzip downloaded files if needed
#with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
#    z.extractall(LOCALDIRECTORY)

df1 = pd.read_csv(LOCALFILE, header=0)
df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
print 'the size of the data is: %d rows and  %d columns' % df1.shape
```

Los datos se leen como una trama de datos:

![Captura de pantalla de las primeras 10 filas de datos](./media/vm-do-ten-things/IPNB_data_readin.png)


### <a name="azure-synapse-analytics-and-databases"></a>Azure Synapse Analytics y bases de datos
Azure Synapse Analytics es un almacén de datos elástico que funciona como un servicio con una experiencia de SQL Server de clase empresarial.

Puede aprovisionar Azure Synapse Analytics siguiendo las instrucciones de [este artículo](../../synapse-analytics/sql-data-warehouse/create-data-warehouse-portal.md). Después de aprovisionar Azure Synapse Analytics, puede usar [este tutorial](/azure/architecture/data-science-process/sqldw-walkthrough) para cargar, examinar y modelar los datos mediante datos de Azure Synapse Analytics.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos DB es una base de datos NoSQL en la nube. Puede usarlo para trabajar con documentos como JSON, así como para almacenar y consultar dichos documentos.

Siga estos pasos de requisitos previos para acceder a Azure Cosmos DB desde DSVM:

1. El SDK de Python de Azure Cosmos DB ya está instalado en DSVM. Para actualizarlo, ejecute ```pip install pydocumentdb --upgrade``` desde un símbolo del sistema.
2. Cree una base de datos y una cuenta de Azure Cosmos DB en [Azure Portal](https://portal.azure.com).
3. Descargue la herramienta de migración de datos de Azure Cosmos DB desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53595) y extráigala en el directorio que quiera.
4. Importe los datos JSON (datos de volcanes) almacenados en un [blob público](https://data.humdata.org/dataset/a60ac839-920d-435a-bf7d-25855602699d/resource/7234d067-2d74-449a-9c61-22ae6d98d928/download/volcano.json) en Azure Cosmos DB con los siguientes parámetros de comando en la herramienta de migración. (Use dtui.exe desde el directorio en el que ha instalado la herramienta de migración de datos de Azure Cosmos DB). Especifique las ubicaciones de origen y destino con estos parámetros:
   
    `/s:JsonFile /s.Files:https://data.humdata.org/dataset/a60ac839-920d-435a-bf7d-25855602699d/resource/7234d067-2d74-449a-9c61-22ae6d98d928/download/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1`

Después de importar los datos, puede ir a Jupyter y abrir el cuaderno titulado *DocumentDBSample*. Contiene el código de Python para acceder a Azure Cosmos DB y realizar algunas consultas básicas. Para obtener más información sobre Azure Cosmos DB, visite la [página de documentación](../../cosmos-db/index.yml) del servicio.

## <a name="use-power-bi-reports-and-dashboards"></a>Uso de informes y paneles de Power BI 
Puede visualizar el archivo JSON denominado Volcano del ejemplo de Azure Cosmos DB anterior en Power BI Desktop para obtener información visual sobre los datos. En este [artículo de Power BI](../../cosmos-db/powerbi-visualize.md)encontrará una explicación detallada de los pasos que se deben seguir. Los pasos generales son los siguientes:

1. Abra Power BI Desktop y seleccione **Obtener datos**. Especifique la URL como: `https://cahandson.blob.core.windows.net/samples/volcano.json`.
2. Debería ver los registros JSON importados como una lista. Convierta la lista en una tabla para que Power BI pueda trabajar con ella.
4. Seleccione el icono de expandir (flecha) para expandir las columnas.
5. Observe que la ubicación es un campo de **Registro**. Expanda el registro y seleccione solo las coordenadas. **Coordenada** es una columna de la lista.
6. Agregue una nueva columna para convertir la columna de coordenadas de la lista en una columna **LatLong** separada por comas. Concatene los dos elementos del campo de lista de coordenadas mediante la fórmula ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Convierta la columna **Elevación** en decimal y seleccione los botones **Cerrar** y **Aplicar**.

En lugar de los pasos anteriores, puede pegar el código siguiente. Incluye en el script los pasos usados en el editor avanzado de Power BI para escribir las transformaciones de datos en un lenguaje de consulta.

```pqfl
let
    Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
    #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
    #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
    #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
in
    #"Changed Type"
```

Ya tiene los datos en el modelo de datos de Power BI. La instancia de Power BI Desktop debe aparecer de la siguiente forma:

![Power BI Desktop](./media/vm-do-ten-things/PowerBIVolcanoData.png)

Puede empezar a crear informes y visualizaciones con el modelo de datos. Para generar un informe, puede seguir los pasos de [este artículo de Power BI](../../cosmos-db/powerbi-visualize.md#build-the-reports).

## <a name="scale-the-dsvm-dynamically"></a>Escalado dinámico de DSVM 
Puede escalar y reducir verticalmente la máquina DSVM para satisfacer las necesidades del proyecto. Si no necesita usar la máquina virtual por la noche o durante los fines de semana, puede apagarla desde [Azure Portal](https://portal.azure.com).

> [!NOTE]
> Incurrirá en gastos de proceso si solo usa el botón de apagado del sistema operativo de la máquina virtual. En su lugar, debe desasignar el DSVM mediante Azure Portal o Cloud Shell.
> 
> 

Es posible que deba controlar algún análisis a gran escala y que necesite más capacidad de CPU, memoria o disco. De ser así, puede encontrar una variedad de tamaños de máquina virtual en términos de núcleos de CPU, instancias basadas en GPU para aprendizaje profundo, capacidad de memoria y tipos de disco (incluidas las unidades de estado sólido) que satisfarán sus necesidades presupuestarias y de proceso. En la página [Precios de Azure Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/) encontrará la lista completa de máquinas virtuales, además de sus precios por hora de proceso.


## <a name="add-more-tools"></a>Adición de más herramientas
Las herramientas precompiladas en DSVM pueden abordar muchas necesidades comunes de análisis de datos. Esto le ahorra tiempo, ya que no tiene que instalar y configurar los entornos uno por uno. También le ahorra dinero, ya que solo paga por los recursos que usa.

Para mejorar su entorno de análisis, puede usar otros servicios de datos y análisis de Azure que se han descrito en este artículo. En algunos casos, podría necesitar herramientas adicionales, incluidas algunas herramientas propias de asociados. Tiene acceso administrativo completo en la máquina virtual para instalar las nuevas herramientas que necesite. También puede instalar paquetes adicionales de Python y R que no estén instalados previamente. En el caso de Python, puede usar ```conda``` o ```pip```. En cuanto a R, puede usar ```install.packages()``` en la consola de R o recurrir al IDE y seleccionar **Paquetes** > **Instalar paquetes**.

## <a name="deep-learning"></a>Aprendizaje profundo

Además de los ejemplos basados en marcos, también obtiene un conjunto de tutoriales completos que se han validado en DSVM, con los que puede iniciar el desarrollo de aplicaciones de aprendizaje profundo en ámbitos como la comprensión de lenguajes o de imágenes y texto.   


- [Ejecución de redes neuronales en distintos marcos](https://github.com/ilkarman/DeepLearningFrameworks): en este tutorial se muestra cómo migrar código de un marco a otro. También se muestra cómo comparar el rendimiento de los modelos y el entorno de ejecución en varios marcos. 

- [Una guía paso a paso para compilar una solución integral que detecte productos dentro de las imágenes](https://github.com/Azure/cortana-intelligence-product-detection-from-images): la detección de imágenes es una técnica que puede localizar y clasificar objetos dentro de las imágenes. Esta tecnología tiene el potencial de aportar grandes ventajas a muchos ámbitos comerciales de uso real. Por ejemplo, los minoristas pueden aplicar esta técnica para determinar qué producto ha seleccionado un cliente de una estantería. A su vez, esta información permite que las tiendas administren el inventario de productos. 

- [Aprendizaje profundo para audio](/archive/blogs/machinelearning/hearing-ai-getting-started-with-deep-learning-for-audio-on-azure): en este tutorial se muestra cómo entrenar un modelo de aprendizaje profundo para detectar eventos de audio en el [conjunto de datos de sonidos urbanos](https://urbansounddataset.weebly.com/). También se proporciona información general sobre cómo trabajar con datos de audio.

- [Clasificación de documentos de texto](https://github.com/anargyri/lstm_han): en este tutorial se muestra cómo compilar y entrenar dos arquitecturas de redes neuronales: red de atención jerárquica y red de memoria a corto y largo plazo (LSTM). Estas redes neurales usan la API de Keras de aprendizaje profundo para clasificar documentos de texto. 

## <a name="summary"></a>Resumen
En este artículo se han descrito algunas de las cosas que puede hacer en Microsoft Data Science Virtual Machine, pero puede hacer muchas más para convertir DSVM en un entorno de análisis eficaz.
