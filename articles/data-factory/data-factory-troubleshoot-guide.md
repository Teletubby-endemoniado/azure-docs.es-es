---
title: Solución general de problemas
titleSuffix: Azure Data Factory & Azure Synapse
description: Información sobre cómo solucionar problemas de actividades de control externo en canalizaciones de Azure Data Factory y Azure Synapse Analytics.
author: nabhishek
ms.service: data-factory
ms.subservice: troubleshooting
ms.custom: synapse
ms.topic: troubleshooting
ms.date: 09/09/2021
ms.author: abnarain
ms.openlocfilehash: c9e6c4c0475842d9eb8c674464ebcf997d98b548
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124749445"
---
# <a name="troubleshoot-azure-data-factory-and-synapse-pipelines"></a>Solución de problemas de canalizaciones de Azure Data Factory y Synapse

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se exploran métodos comunes de solución de problemas relativos a actividades de control externo en canalizaciones de Azure Data Factory y Synapse.

## <a name="connector-and-copy-activity"></a>Conector y actividad de copia

Para obtener más información sobre problemas de conector como, por ejemplo, errores relacionados con la actividad de copia, consulte el artículo [Solución de problemas relacionados con conectores](connector-troubleshoot-guide.md).

## <a name="azure-databricks"></a>Azure Databricks

### <a name="error-code-3200"></a>Código de error: 3200

- **Mensaje**: Error 403.

- **Causa**: `The Databricks access token has expired.`

- **Recomendación:** De forma predeterminada, el token de acceso de Azure Databricks es válido durante noventa días. Cree un nuevo token y actualice el servicio vinculado.

### <a name="error-code-3201"></a>Código de error: 3201

- **Mensaje**: `Missing required field: settings.task.notebook_task.notebook_path.`

- **Causa**: `Bad authoring: Notebook path not specified correctly.`

- **Recomendación:** Especifique la ruta de acceso de Notebook en la actividad de Databricks.

<br/> 

- **Mensaje**: `Cluster... does not exist.`

- **Causa**: `Authoring error: Databricks cluster does not exist or has been deleted.`

- **Recomendación:** Compruebe que existe el clúster de Databricks.

<br/> 

- **Mensaje**: `Invalid Python file URI... Please visit Databricks user guide for supported URI schemes.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Especifique las rutas de acceso absolutas para los esquemas de direccionamiento del área de trabajo, o bien `dbfs:/folder/subfolder/foo.py` para los archivos almacenados en el sistema de archivos de Databricks (DFS).

<br/> 

- **Mensaje**: `{0} LinkedService should have domain and accessToken as required properties.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service).

<br/> 

- **Mensaje**: `{0} LinkedService should specify either existing cluster ID or new cluster information for creation.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Compruebe la [definición del servicio vinculado](compute-linked-services.md#azure-databricks-linked-service).

<br/> 

- **Mensaje**: `Node type Standard_D16S_v3 is not supported. Supported node types: Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3, Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2, Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3, Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2, Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2, Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3, Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s, Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2, Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2, Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3, Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2, Standard_L64s_v2, Standard_L80s_v2.`

- **Causa**: `Bad authoring.`

- **Recomendación:** Consulte el mensaje de error.

<br/>

### <a name="error-code-3202"></a>Código de error: 3202

- **Mensaje**: `There were already 1000 jobs created in past 3600 seconds, exceeding rate limit: 1000 job creations per 3600 seconds.`

- **Causa**: `Too many Databricks runs in an hour.`

- **Recomendación:** Compruebe todas las canalizaciones que usen esta área de trabajo de Databricks para la velocidad de creación de trabajos. Si las canalizaciones inician demasiadas ejecuciones de Databricks en agregado, migre algunas canalizaciones a un área de trabajo nueva.

<br/> 

- **Mensaje**: `Could not parse request object: Expected 'key' and 'value' to be set for JSON map field base_parameters, got 'key: "..."' instead.`

- **Causa**: `Authoring error: No value provided for the parameter.`

- **Recomendación:** Inspeccione el json de canalización y asegúrese de que todos los parámetros del cuaderno baseParameters tienen especificado un valor no vacío.

<br/> 

- **Mensaje**: `User: `SimpleUserContext{userId=..., name=user@company.com, orgId=...}` is not authorized to access cluster.`

- **Causa**: El usuario que ha generado el token de acceso no tiene permiso para obtener acceso al clúster de Databricks especificado en el servicio vinculado.

- **Recomendación:** Asegúrese de que el usuario tiene los permisos necesarios en el área de trabajo.

<br/> 

- **Mensaje**: `Job is not fully initialized yet. Please retry later.`

- **Causa**: el trabajo no se ha inicializado.

- **Recomendación**: espere y vuelva a intentarlo más adelante.

### <a name="error-code-3203"></a>Código de error: 3203

- **Mensaje**: `The cluster is in Terminated state, not available to receive jobs. Please fix the cluster or retry later.`

- **Causa**: El clúster se ha finalizado. Para un clúster interactivo, este problema podría ser una condición de carrera.

- **Recomendación:** Para evitar este error, use clústeres de trabajo.

### <a name="error-code-3204"></a>Código de error: 3204

- **Mensaje**: `Job execution failed.`

- **Causa**: Los mensajes de error indican varias incidencias, como un estado de clúster inesperado o una actividad específica. Con frecuencia, no aparece ningún mensaje de error.

- **Recomendación:** N/D

### <a name="error-code-3208"></a>Código de error: 3208

- **Mensaje**: `An error occurred while sending the request.`

- **Causa**: Se interrumpió la conexión de red con el servicio de Databricks.

- **Recomendación:** Si usa un entorno de ejecución de integración autohospedado, asegúrese de que la conexión de red sea estable desde los nodos del entorno de ejecución de integración. Si usa Azure Integration Runtime, el reintento suele funcionar.
 
## <a name="azure-data-lake-analytics"></a>Análisis con Azure Data Lake

La tabla siguiente se aplica a U-SQL.
 
### <a name="error-code-2709"></a>Código de error: 2709

- **Mensaje**: `The access token is from the wrong tenant.`

- **Causa**: Inquilino de Azure Active Directory (Azure AD) incorrecto.

- **Recomendación:** Inquilino de Azure Active Directory (Azure AD) incorrecto.

<br/>

- **Mensaje**: `We cannot accept your job at this moment. The maximum number of queued jobs for your account is 200. `

- **Causa**: Este error se debe a una limitación en Data Lake Analytics.

- **Recomendación:** Reduzca el número de trabajos enviados a Data Lake Analytics. Cambie la configuración de simultaneidad y de los desencadenadores en las actividades, o bien aumente los límites en Data Lake Analytics.

<br/> 

- **Mensaje**: `This job was rejected because it requires 24 AUs. This account's administrator-defined policy prevents a job from using more than 5 AUs.`

- **Causa**: Este error se debe a una limitación en Data Lake Analytics.

- **Recomendación:** Reduzca el número de trabajos enviados a Data Lake Analytics. Cambie la configuración de simultaneidad y de los desencadenadores en las actividades, o bien aumente los límites en Data Lake Analytics.

### <a name="error-code-2705"></a>Código de error: 2705

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/> <br/> User is not able to access Data Lake Store. <br/> <br/> User is not authorized to use Data Lake Analytics.`

- **Causa**: La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Compruebe que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.

### <a name="error-code-2711"></a>Código de error: 2711

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/> <br/> User is not able to access Data Lake Store. <br/> <br/> User is not authorized to use Data Lake Analytics.`

- **Causa**: La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Compruebe que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.

<br/> 

- **Mensaje**: `Cannot find the 'Azure Data Lake Store' file or folder.`

- **Causa**: La ruta de acceso al archivo U-SQL es incorrecta o las credenciales del servicio vinculado no tienen acceso.

- **Recomendación:** Compruebe la ruta de acceso y las credenciales proporcionadas en el servicio vinculado.

### <a name="error-code-2704"></a>Código de error: 2704

- **Mensaje**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/> <br/> User is not able to access Data Lake Store. <br/> <br/> User is not authorized to use Data Lake Analytics.`

- **Causa**: La entidad de servicio o el certificado no tiene acceso al archivo en el almacenamiento.

- **Recomendación:** Compruebe que la entidad de servicio o el certificado que el usuario proporciona para los trabajos de Data Lake Analytics tiene acceso a la cuenta de Data Lake Analytics y a la instancia de Data Lake Storage predeterminada de la carpeta raíz.

### <a name="error-code-2707"></a>Código de error: 2707

- **Mensaje**: `Cannot resolve the account of AzureDataLakeAnalytics. Please check 'AccountName' and 'DataLakeAnalyticsUri'.`

- **Causa**: La cuenta de Data Lake Analytics en el servicio vinculado es incorrecta.

- **Recomendación:** Compruebe que se proporciona la cuenta adecuada.

### <a name="error-code-2703"></a>Código de error: 2703

- **Mensaje**: `Error Id: E_CQO_SYSTEM_INTERNAL_ERROR (or any error that starts with "Error Id:").`

- **Causa**: El error es de Data Lake Analytics.

- **Recomendación:** El trabajo se envió a Data Lake Analytics y, junto con el script, produjeron un error. Investigue el error en Data Lake Analytics. En el portal, vaya a la cuenta de Data Lake Analytics y busque el trabajo mediante el identificador de ejecución de actividad de Data Factory (no use el identificador de ejecución de canalización). El trabajo proporcionará más información sobre el error y le ayudará a solucionar el problema.

   Si la resolución no está clara, póngase en contacto con el equipo de soporte técnico de Data Lake Analytics y proporcióneles el localizador de recursos universal (URL) del trabajo, que incluye el nombre de cuenta y el identificador del trabajo.
 
## <a name="azure-functions"></a>Azure Functions

### <a name="error-code-3602"></a>Código de error: 3602

- **Mensaje**: `Invalid HttpMethod: '%method;'.`

- **Causa**: La actividad de las funciones de Azure no admite el método HTTP especificado en la carga de la actividad.

- **Recomendación:** Los métodos HTTP admitidos son: PUT, POST, GET, DELETE, OPTIONS, HEAD y TRACE.

### <a name="error-code-3603"></a>Código de error: 3603

- **Mensaje**: `Response Content is not a valid JObject.`

- **Causa**: La función de Azure a la que se llamó no devolvió una carga JSON en la respuesta. La actividad de la función de Azure de la canalización de Azure Data Factory y Synapse admite únicamente contenido de respuesta JSON.

- **Recomendación:** Actualice la función de Azure para que devuelva una carga JSON válida, tal como una función C# puede devolver `(ActionResult)new OkObjectResult("{\"Id\":\"123\"}");`.

### <a name="error-code-3606"></a>Código de error: 3606

- **Mensaje**: falta la clave de función de la actividad de Azure Function.

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad de función de Azure de entrada tiene una propiedad denominada `functionKey`.

### <a name="error-code-3607"></a>Código de error: 3607

- **Mensaje**: `Azure function activity missing function name.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad de función de Azure de entrada tiene una propiedad denominada `functionName`.

### <a name="error-code-3608"></a>Código de error: 3608

- **Mensaje**: `Call to provided Azure function '%FunctionName;' failed with status-'%statusCode;' and message - '%message;'.`

- **Causa**: Los detalles de las funciones de Azure en la definición de la actividad pueden ser incorrectos.

- **Recomendación:** Corrija los detalles de las funciones de Azure y vuelva a intentarlo.

### <a name="error-code-3609"></a>Código de error: 3609

- **Mensaje**: `Azure function activity missing functionAppUrl.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad de función de Azure de entrada tiene una propiedad denominada `functionAppUrl`.

### <a name="error-code-3610"></a>Código de error: 3610

- **Mensaje**: `There was an error while calling endpoint.`

- **Causa**: La dirección URL de la función puede ser incorrecta.

- **Recomendación:** Asegúrese de que el valor de `functionAppUrl` en el JSON de actividad es correcto e inténtelo de nuevo.

### <a name="error-code-3611"></a>Código de error: 3611

- **Mensaje**: `Azure function activity missing Method in JSON.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad de función de Azure de entrada tiene una propiedad denominada `method`.

### <a name="error-code-3612"></a>Código de error: 3612

- **Mensaje**: `Azure function activity missing LinkedService definition in JSON.`

- **Causa**: La definición de actividad de funciones de Azure no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad de función de Azure de entrada tiene detalles del servicio vinculado.

## <a name="azure-machine-learning"></a>Azure Machine Learning

### <a name="error-code-4101"></a>Código de error: 4101

- **Mensaje**: `AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Causa**: Formato incorrecto o falta la definición de la propiedad `%propertyName;`.

- **Recomendación:** Compruebe si la actividad `%activityName;` tiene la propiedad `%propertyName;` definida con los datos correctos.

### <a name="error-code-4110"></a>Código de error: 4110

- **Mensaje**: `AzureMLExecutePipeline activity missing LinkedService definition in JSON.`

- **Causa**: La definición de la actividad AzureMLExecutePipeline no está completa.

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureMLExecutePipeline de entrada tiene detalles del servicio vinculado correctos.

### <a name="error-code-4111"></a>Código de error: 4111

- **Mensaje**: `AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Causa**: Definición de actividad incorrecta.

- **Recomendación:** Compruebe que la definición JSON de la actividad AzureMLExecutePipeline de entrada tiene detalles del servicio vinculado correctos.

### <a name="error-code-4112"></a>Código de error: 4112

- **Mensaje**: `AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Causa**: formato incorrecto o falta la definición de la propiedad "%propertyName;".

- **Recomendación:** Compruebe si el servicio vinculado tiene la propiedad `%propertyName;` definida con los datos correctos.

### <a name="error-code-4121"></a>Código de error: 4121

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: La credencial usada para acceder a Azure Machine Learning ha expirado.

- **Recomendación:** Compruebe que la credencial es válida y vuelva a intentarlo.

### <a name="error-code-4122"></a>Código de error: 4122

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: La credencial proporcionada en el servicio vinculado de Azure Machine Learning no es válida o no tiene permiso para la operación.

- **Recomendación:** Compruebe que la credencial del servicio vinculado es válida y tiene permiso para acceder a Azure Machine Learning.

### <a name="error-code-4123"></a>Código de error: 4123

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: Las propiedades de la actividad, como `pipelineParameters`, no son válidas para la canalización de Azure Machine Learning (ML).

- **Recomendación:** Compruebe el valor de las propiedades de la actividad para que coincida con la carga esperada de la canalización de Azure ML publicada especificada en el servicio vinculado.

### <a name="error-code-4124"></a>Código de error: 4124

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: El punto de conexión de canalización de Azure ML publicado no existe.

- **Recomendación:** Compruebe que el punto de conexión de la canalización de Azure Machine Learning publicado que se especifica en el servicio vinculado existe en Azure Machine Learning.

### <a name="error-code-4125"></a>Código de error: 4125

- **Mensaje**: `Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Causa**: Hay un error de servidor en Azure Machine Learning.

- **Recomendación:** Vuelva a intentarlo más tarde. Póngase en contacto con el equipo de Azure Machine Learning para obtener ayuda si el problema continúa.

### <a name="error-code-4126"></a>Código de error: 4126

- **Mensaje**: `Azure ML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in Azure Machine Learning for more error logs.`

- **Causa**: Error en la ejecución de la canalización de Azure ML.

- **Recomendación:** Compruebe si en Azure Machine Learning hay más registros de errores y después corrija la canalización de ML.

## <a name="common"></a>Comunes

### <a name="error-code-2103"></a>Código de error: 2103

- **Mensaje**: `Please provide value for the required property '%propertyName;'.`

- **Causa**: El valor necesario para la propiedad no se ha proporcionado.

- **Recomendación:** proporcione el valor del mensaje e inténtelo de nuevo.

### <a name="error-code-2104"></a>Código de error: 2104

- **Mensaje**: `The type of the property '%propertyName;' is incorrect.`

- **Causa**: El tipo de propiedad proporcionado no es correcto.

- **Recomendación:** Corrija el tipo de la propiedad e inténtelo de nuevo.

### <a name="error-code-2105"></a>Código de error: 2105

- **Mensaje**: `An invalid json is provided for property '%propertyName;'. Encountered an error while trying to parse: '%message;'.`

- **Causa**: El valor de la propiedad no es válido o no tiene el formato esperado.

- **Recomendación:** Consulte la documentación de la propiedad y compruebe que el valor proporcionado incluye el tipo y el formato correctos.

### <a name="error-code-2106"></a>Código de error: 2106

- **Mensaje**: `The storage connection string is invalid. %errorMessage;`

- **Causa**: la cadena de conexión del almacenamiento no es válida o tiene un formato incorrecto.

- **Recomendación:** Vaya a Azure Portal y busque el almacenamiento, después copie y pegue la cadena de conexión en el servicio vinculado y e inténtelo de nuevo.

### <a name="error-code-2110"></a>Código de error: 2110

- **Mensaje**: `The linked service type '%linkedServiceType;' is not supported for '%executorType;' activities.`

- **Causa**: El servicio vinculado especificado en la actividad es incorrecto.

- **Recomendación:** Compruebe que el tipo de servicio vinculado sea uno de los tipos admitidos para la actividad. Por ejemplo, el tipo de servicio vinculado para las actividades de HDI puede ser HDInsight o HDInsightOnDemand.

### <a name="error-code-2111"></a>Código de error: 2111

- **Mensaje**: `The type of the property '%propertyName;' is incorrect. The expected type is %expectedType;.`

- **Causa**: El tipo de la propiedad proporcionada no es correcto.

- **Recomendación:** Corrija el tipo de propiedad e inténtelo de nuevo.

### <a name="error-code-2112"></a>Código de error: 2112

- **Mensaje**: `The cloud type is unsupported or could not be determined for storage from the EndpointSuffix '%endpointSuffix;'.`

- **Causa**: El tipo de nube no es compatible o no se pudo determinar para el almacenamiento desde EndpointSuffix.

- **Recomendación:** Use el almacenamiento en otra nube e inténtelo de nuevo.

## <a name="custom"></a>Personalizado

La tabla siguiente se aplica a Azure Batch.
 
### <a name="error-code-2500"></a>Código de error: 2.500

- **Mensaje**: `Hit unexpected exception and execution failed.`

- **Causa**: `Can't launch command, or the program returned an error code.`

- **Recomendación:** Compruebe que existe el archivo ejecutable. Si se inicia el programa, compruebe si los archivos *stdout.txt* y *stderr.txt* se cargaron en la cuenta de almacenamiento. Es un procedimiento recomendado incluir registros en el código para la depuración.

### <a name="error-code-2501"></a>Código de error: 2501

- **Mensaje**: `Cannot access user batch account; please check batch account settings.`

- **Causa**: Clave de acceso o nombre de grupo de Batch incorrectos.

- **Recomendación:** Compruebe el nombre del grupo y la clave de acceso de Batch en el servicio vinculado.

### <a name="error-code-2502"></a>Código de error: 2502

- **Mensaje**: `Cannot access user storage account; please check storage account settings.`

- **Causa**: Nombre de cuenta de almacenamiento o clave de acceso incorrectos.

- **Recomendación:** Compruebe la clave de acceso y el nombre de cuenta de almacenamiento en el servicio vinculado.

### <a name="error-code-2504"></a>Código de error: 2504

- **Mensaje**: `Operation returned an invalid status code 'BadRequest'.`

- **Causa**: Hay demasiados archivos en `folderPath` de la actividad personalizada. El tamaño total de `resourceFiles` no puede superar los 32 768 caracteres.

- **Recomendación:** Elimine los archivos innecesarios o comprímalos y agregue un comando de descompresión para extraerlos.
   
   Por ejemplo, use `powershell.exe -nologo -noprofile -command "& { Add-Type -A 'System.IO.Compression.FileSystem'; [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ; $folder\yourProgram.exe`.

### <a name="error-code-2505"></a>Código de error: 2505

- **Mensaje**: `Cannot create Shared Access Signature unless Account Key credentials are used.`

- **Causa**: Las actividades personalizadas solo admiten cuentas de almacenamiento que usen una clave de acceso.

- **Recomendación:** Consulte la descripción del error.

### <a name="error-code-2507"></a>Código de error: 2507

- **Mensaje**: `The folder path does not exist or is empty: ...`

- **Causa**: No hay archivos en la cuenta de almacenamiento en la ruta de acceso especificada.

- **Recomendación:** La ruta de acceso a la carpeta debe contener los archivos ejecutables que desea ejecutar.

### <a name="error-code-2508"></a>Código de error: 2508

- **Mensaje**: `There are duplicate files in the resource folder.`

- **Causa**: Hay varios archivos con el mismo nombre en subcarpetas diferentes de folderPath.

- **Recomendación:** Estructura de carpetas de actividades personalizadas aplanada en folderPath. Si necesita conservar la estructura de carpetas, comprima los archivos y extráigalos en Azure Batch mediante un comando unzip.
   
   Por ejemplo, use `powershell.exe -nologo -noprofile -command "& { Add-Type -A 'System.IO.Compression.FileSystem'; [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ; $folder\yourProgram.exe`.

### <a name="error-code-2509"></a>Código de error: 2509

- **Mensaje**: `Batch url ... is invalid; it must be in Uri format.`

- **Causa**: Las direcciones URL del lote tienen que ser similares a `https://mybatchaccount.eastus.batch.azure.com`

- **Recomendación:** Consulte la descripción del error.

### <a name="error-code-2510"></a>Código de error: 2510

- **Mensaje**: `An error occurred while sending the request.`

- **Causa**: La dirección URL del lote no es válida.

- **Recomendación:** Compruebe la dirección URL del lote.
 
## <a name="hdinsight"></a>HDInsight

### <a name="error-code-206"></a>Código de error: 206

- **Mensaje**: `The batch ID for Spark job is invalid. Please retry your job.`

- **Causa**: Hubo un problema interno con el servicio que causó este error.

- **Recomendación:** Este problema puede ser transitorio. Vuelva a intentar el trabajo más tarde.

### <a name="error-code-207"></a>Código de error: 207

- **Mensaje**: `Could not determine the region from the provided storage account. Please try using another primary storage account for the on demand HDI.`

- **Causa**: se produjo un error interno al intentar determinar la región desde la cuenta de almacenamiento principal.

- **Recomendación:** pruebe con otro almacenamiento. 

### <a name="error-code-208"></a>Código de error: 208

- **Mensaje**: `Service Principal or the MSI authenticator are not instantiated. Please consider providing a Service Principal in the HDI on demand linked service which has permissions to create an HDInsight cluster in the provided subscription and try again.`

- **Causa**: se produjo un error interno al intentar leer la entidad de servicio o crear una instancia de la autenticación de MSI.

- **Recomendación:** Considere la posibilidad de proporcionar una entidad de servicio que tenga permisos para crear un clúster de HDInsight en la suscripción proporcionada e inténtelo de nuevo. Compruebe que las [identidades administradas están configuradas correctamente](../hdinsight/hdinsight-managed-identities.md).

### <a name="error-code-2300"></a>Código de error: 2300

- **Mensaje**: `Failed to submit the job '%jobId;' to the cluster '%cluster;'. Error: %errorMessage;.`

- **Causa**: El mensaje de error contiene un mensaje similar a `The remote name could not be resolved.`. Es posible que el URI del clúster proporcionado no sea válido.

- **Recomendación:** Compruebe que el clúster no se ha eliminado y que el URI proporcionado es correcto. Al abrir el identificador URI en cualquier explorador, debe ver la interfaz de usuario de Ambari. Si el clúster está en una red virtual, el URI debe ser el URI privado. Para abrirlo, use una máquina virtual (VM) que forme parte de la misma red virtual.

   Para más información, consulte [Conexión directa a los servicios de Apache Hadoop](../hdinsight/hdinsight-plan-virtual-network-deployment.md#directly-connect-to-apache-hadoop-services).
 
 </br>

- **Causa**: Si el mensaje de error contiene un mensaje similar a `A task was canceled.`, quiere decir que el envío del trabajo agotó el tiempo de espera.

- **Recomendación:** podría tratarse de un problema de conectividad general de HDInsight o de un problema de conectividad de red. En primer lugar, confirme que la interfaz de usuario de HDInsight Ambari está disponible desde cualquier explorador. A continuación, compruebe que las credenciales siguen siendo válidas.
   
   Si usa Integrated Runtime (IR) autohospedado, realice este paso desde la VM o la máquina donde está instalada la instancia de IR autohospedado. Luego, intente volver a enviar el trabajo.

   Para más información, lea [Interfaz de usuario web de Ambari](../hdinsight/hdinsight-hadoop-manage-ambari.md#ambari-web-ui).

 </br>

- **Causa**: Cuando el mensaje de error contiene un mensaje similar a `User admin is locked out in Ambari` o `Unauthorized: Ambari user name or password is incorrect`, quiere decir que las credenciales de HDInsight son incorrectas o han expirado.

- **Recomendación:** Corrija las credenciales y vuelva a implementar el servicio vinculado. En primer lugar, compruebe que las credenciales funcionan en HDInsight; para ello, abra el URI del clúster en cualquier explorador e intente iniciar sesión. Si las credenciales no funcionan, puede restablecerlas desde Azure Portal.

   En el caso del clúster de ESP, restablezca la contraseña mediante el [autoservicio de restablecimiento de contraseña](https://support.microsoft.com/account-billing/reset-your-work-or-school-password-using-security-info-23dde81f-08bb-4776-ba72-e6b72b9dda9e).

 </br>

- **Causa**: Cuando el mensaje de error contiene un mensaje similar a `502 - Web server received an invalid response while acting as a gateway or proxy server`, el servicio HDInsight devuelve este error.

- **Recomendación:** A menudo se produce un error 502 cuando el proceso del servidor de Ambari se apaga. Para reiniciar los servicios de Ambari puede reiniciar el nodo principal.

    1. Conéctese a uno de los nodos de HDInsight con SSH.
    1. Identifique el host del nodo principal activo mediante la ejecución de `ping headnodehost`.
    1. Conéctese al nodo principal activo, ya que el servidor de Ambari se coloca en el nodo principal activo mediante SSH. 
    1. Reinicie el nodo principal activo.

       Para obtener más información, consulte la documentación de solución de problemas de Azure HDInsight. Por ejemplo:

       * [Error 502 de la interfaz de usuario de Ambari](https://hdinsight.github.io/ambari/ambari-ui-502-error.html)
       * [RpcTimeoutException para el servidor Thrift de Apache Spark](../hdinsight/spark/apache-spark-troubleshoot-rpctimeoutexception.md)
       * [Solución de errores de puerta de enlace incorrecta en el servicio Application Gateway](../application-gateway/application-gateway-troubleshooting-502.md).

 </br>

- **Causa**: Cuando el mensaje de error contiene un mensaje similar a `Unable to service the submit job request as templeton service is busy with too many submit job requests` o `Queue root.joblauncher already has 500 applications, cannot accept submission of application`, quiere decir que se están enviando demasiados trabajos a HDInsight al mismo tiempo.

- **Recomendación:** Limite el número de trabajos simultáneos que se envían a HDInsight. Consulte la simultaneidad de actividad si los trabajos los envía la misma actividad. Cambie los desencadenadores para que las ejecuciones de canalización simultánea se repartan en el tiempo.

   Consulte la [documentación de HDInsight](../hdinsight/hdinsight-hadoop-templeton-webhcat-debug-errors.md) para ajustar `templeton.parallellism.job.submit` tal y como sugiere el error.

### <a name="error-code-2301"></a>Código de error: 2301

- **Mensaje**: `Could not get the status of the application '%physicalJobId;' from the HDInsight service. Received the following error: %message;. Please refer to HDInsight troubleshooting documentation or contact their support for further assistance.`

- **Causa**: el servicio o el clúster de HDInsight tiene problemas.

- **Recomendación**: este error se produce cuando el servicio no recibe respuesta del clúster de HDInsight al intentar solicitar el estado del trabajo en ejecución. Este problema podría deberse al propio clúster o a que el servicio HDInsight haya sufrido una interrupción.

   Consulte la [documentación de solución de problemas de HDInsight](../hdinsight/hdinsight-troubleshoot-guide.md) o póngase en contacto con el soporte técnico de Microsoft para obtener más ayuda.

### <a name="error-code-2302"></a>Código de error: 2302

- **Mensaje**: `Hadoop job failed with exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Contact HDInsight team for further support.`

- **Causa**: El trabajo se envió al clúster de HDI y generó un error.

- **Recomendación:** 

 1. Compruebe la interfaz de usuario de Ambari:
    1. Asegúrese de que aún se están ejecutando todos los servicios.
    1. En la interfaz de usuario de Ambari, compruebe la sección de alerta en el panel.
       1. Para obtener más información sobre las alertas y las resoluciones para las alertas, consulte [Administración y supervisión de un clúster](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.5.0/managing-and-monitoring-ambari/content/amb_predefined_alerts.html).
    1. Revise la memoria de YARN. Si la memoria de YARN es alta, el procesamiento de los trabajos se puede retrasar. Si no tiene recursos suficientes para acomodar la aplicación o el trabajo de Spark, escale verticalmente el clúster para asegurarse de que el clúster tiene suficiente memoria y núcleos. 
 1. Ejecute un trabajo de prueba de ejemplo.
    1. Si ejecuta el mismo trabajo en el back-end de HDInsight, compruebe que se ha realizado correctamente. Para ver ejemplos de ejecuciones de ejemplo, consulte [Ejecución de los ejemplos de MapReduce incluidos en HDInsight](../hdinsight/hadoop/apache-hadoop-run-samples-linux.md). 
 1. Si el trabajo todavía no se ha realizado correctamente en HDInsight, compruebe los registros de la aplicación y la información que debe proporcionar para el soporte técnico:
    1. Compruebe si el trabajo se envió a YARN. Si el trabajo no se ha enviado a YARN, use `--master yarn`.
    1. Si la aplicación ha finalizado la ejecución, recopile la hora de inicio y la hora de finalización de la aplicación YARN. Si la aplicación no completó la ejecución, recopile la hora de inicio.
    1. Compruebe y recopile el registro de aplicaciones con `yarn logs -applicationId <Insert_Your_Application_ID>`.
    1. Compruebe y recopile los registros de Resource Manager en el directorio `/var/log/hadoop-yarn/yarn`.
    1. Si estos pasos no son suficientes para resolver el problema, póngase en contacto con el equipo de Azure HDInsight para obtener soporte técnico y proporcione los registros y las marcas de tiempo anteriores.

### <a name="error-code-2303"></a>Código de error: 2303

- **Mensaje**: `Hadoop job failed with transient exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Try again or contact HDInsight team for further support.`

- **Causa**: El trabajo se envió al clúster de HDI y generó un error.

- **Recomendación:** 

 1. Compruebe la interfaz de usuario de Ambari:
    1. Asegúrese de que aún se están ejecutando todos los servicios.
    1. En la interfaz de usuario de Ambari, compruebe la sección de alerta en el panel.
       1. Para obtener más información sobre las alertas y las resoluciones para las alertas, consulte [Administración y supervisión de un clúster](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.5.0/managing-and-monitoring-ambari/content/amb_predefined_alerts.html).
    1. Revise la memoria de YARN. Si la memoria de YARN es alta, el procesamiento de los trabajos se puede retrasar. Si no tiene recursos suficientes para acomodar la aplicación o el trabajo de Spark, escale verticalmente el clúster para asegurarse de que el clúster tiene suficiente memoria y núcleos. 
 1. Ejecute un trabajo de prueba de ejemplo.
    1. Si ejecuta el mismo trabajo en el back-end de HDInsight, compruebe que se ha realizado correctamente. Para ver ejemplos de ejecuciones de ejemplo, consulte [Ejecución de los ejemplos de MapReduce incluidos en HDInsight](../hdinsight/hadoop/apache-hadoop-run-samples-linux.md). 
 1. Si el trabajo todavía no se ha realizado correctamente en HDInsight, compruebe los registros de la aplicación y la información que debe proporcionar para el soporte técnico:
    1. Compruebe si el trabajo se envió a YARN. Si el trabajo no se ha enviado a YARN, use `--master yarn`.
    1. Si la aplicación ha finalizado la ejecución, recopile la hora de inicio y la hora de finalización de la aplicación YARN. Si la aplicación no completó la ejecución, recopile la hora de inicio.
    1. Compruebe y recopile el registro de aplicaciones con `yarn logs -applicationId <Insert_Your_Application_ID>`.
    1. Compruebe y recopile los registros de Resource Manager en el directorio `/var/log/hadoop-yarn/yarn`.
    1. Si estos pasos no son suficientes para resolver el problema, póngase en contacto con el equipo de Azure HDInsight para obtener soporte técnico y proporcione los registros y las marcas de tiempo anteriores.

### <a name="error-code-2304"></a>Código de error: 2304

- **Mensaje**: `MSI authentication is not supported on storages for HDI activities.`

- **Causa**: Los servicios vinculados de almacenamiento usados en el servicio vinculado de HDInsight (HDI) o la actividad de HDI están configurados con la autenticación MSI, la cual no se admite.

- **Recomendación:** Proporcione cadenas de conexión completas para las cuentas de almacenamiento usadas en el servicio vinculado o la actividad de HDI.

### <a name="error-code-2305"></a>Código de error: 2305

- **Mensaje**: `Failed to initialize the HDInsight client for the cluster '%cluster;'. Error: '%message;'`

- **Causa**: la información de conexión del clúster de HDI es incorrecta, el usuario proporcionado no tiene los permisos para realizar la acción necesaria o el servicio HDInsight tiene problemas para responder a las solicitudes del servicio.

- **Recomendación:** Compruebe que la información del usuario sea correcta y que la interfaz de usuario de Ambari del clúster de HDI se pueda abrir en un explorador desde la VM en la que está instalado IR (en el caso de IR autohospedado) o desde cualquier máquina (en el caso de Azure IR).

### <a name="error-code-2306"></a>Código de error: 2306

- **Mensaje**: `An invalid json is provided for script action '%scriptActionName;'. Error: '%message;'`

- **Causa**: El código JSON proporcionado para la acción de script no es válido.

- **Recomendación:** el mensaje de error debe ayudar a identificar el problema. Corrija la configuración de JSON e inténtelo de nuevo.

   Consulte [Servicio vinculado a petición de Azure HDInsight](./compute-linked-services.md#azure-hdinsight-on-demand-linked-service) para obtener más información.

### <a name="error-code-2310"></a>Código de error: 2310

- **Mensaje**: `Failed to submit Spark job. Error: '%message;'`

- **Causa**: el servicio intentó crear un lote en un clúster de Spark mediante Livy API (Livy/batch), pero recibió un error.

- **Recomendación:** Siga el mensaje de error para solucionar el problema. Si no hay suficiente información para resolverlo, póngase en contacto con el equipo de HDI y proporcione los identificadores de trabajo y lote, que encontrará en la salida de ejecución de actividad de la página de supervisión del servicio. Para seguir solucionando el problema, recopile el registro completo del trabajo por lotes.

   Para obtener más información sobre cómo recopilar el registro completo, consulte [Obtener el registro completo de un trabajo por lotes](/rest/api/hdinsightspark/hdinsight-spark-batch-job#get-the-full-log-of-a-batch-job).

### <a name="error-code-2312"></a>Código de error: 2312

- **Mensaje**: `Spark job failed, batch id:%batchId;. Please follow the links in the activity run Output from the service Monitoring page to troubleshoot the run on HDInsight Spark cluster. Please contact HDInsight support team for further assistance.`

- **Causa**: Error en el trabajo en el clúster de HDInsight Spark.

- **Recomendación**: siga los vínculos de la salida de ejecución de actividad de la página de supervisión del servicio para solucionar los problemas de ejecución en el clúster de HDInsight Spark. Póngase en contacto con el equipo de soporte técnico de HDInsight para obtener más ayuda.

   Para obtener más información sobre cómo recopilar el registro completo, consulte [Obtener el registro completo de un trabajo por lotes](/rest/api/hdinsightspark/hdinsight-spark-batch-job#get-the-full-log-of-a-batch-job).

### <a name="error-code-2313"></a>Código de error: 2313

- **Mensaje**: `The batch with ID '%batchId;' was not found on Spark cluster. Open the Spark History UI and try to find it there. Contact HDInsight support for further assistance.`

- **Causa**: el lote se eliminó en el clúster de HDInsight Spark.

- **Recomendación:** solucione los problemas de lotes en el clúster de HDInsight Spark. Póngase en contacto con el soporte técnico de HDInsight para obtener más ayuda. 

   Para obtener más información sobre cómo recopilar el registro completo, consulte [Obtener el registro completo de un trabajo por lotes](/rest/api/hdinsightspark/hdinsight-spark-batch-job#get-the-full-log-of-a-batch-job) y comparta el registro completo con el soporte técnico de HDInsight para obtener más ayuda.

### <a name="error-code-2328"></a>Código de error: 2328

- **Mensaje**: `Failed to create the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Causa**: el mensaje de error debe mostrar los detalles de lo que ha ido mal.

- **Recomendación:** el mensaje de error debe ayudarle a solucionar el problema.

### <a name="error-code-2329"></a>Código de error: 2329

- **Mensaje**: `Failed to delete the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Causa**: el mensaje de error debe mostrar los detalles de lo que ha ido mal.

- **Recomendación:** el mensaje de error debe ayudarle a solucionar el problema.

### <a name="error-code-2331"></a>Código de error: 2331

- **Mensaje**: `The file path should not be null or empty.`

- **Causa**: la ruta de acceso del archivo proporcionada está vacía.

- **Recomendación:** Proporcione una ruta de acceso para un archivo que exista.

### <a name="error-code-2340"></a>Código de error: 2340

- **Mensaje**: `HDInsightOnDemand linked service does not support execution via SelfHosted IR. Your IR name is '%IRName;'. Please select an Azure IR instead.`

- **Causa**: El servicio vinculado HDInsightOnDemand no admite la ejecución a través de IR autohospedado.

- **Recomendación:** Seleccione una instancia de Azure IR e inténtelo de nuevo.

### <a name="error-code-2341"></a>Código de error: 2341

- **Mensaje**: `HDInsight cluster URL '%clusterUrl;' is incorrect, it must be in URI format and the scheme must be 'https'.`

- **Causa**: La dirección URL proporcionada no tiene el formato correcto.

- **Recomendación:** Corrija la dirección URL del clúster e inténtelo de nuevo.

### <a name="error-code-2342"></a>Código de error: 2342

- **Mensaje**: `Failed to connect to HDInsight cluster: '%errorMessage;'.`

- **Causa**: Las credenciales proporcionadas para el clúster son incorrectas o se produjo un problema de conexión o configuración de la red; también, puede que IR esté teniendo problemas para conectarse al clúster.

- **Recomendación:** 
    1. compruebe que las credenciales sean correctas; para ello, abra la interfaz de usuario de Ambari del clúster de HDInsight en un explorador.
    1. Si el clúster se encuentra en Virtual Network (VNet) y se está usando un IR autohospedado, la dirección URL de HDI debe ser la dirección URL privada de las redes virtuales y debe tener `-int` después del nombre del clúster.
    
       Por ejemplo, cambie `https://mycluster.azurehdinsight.net/` a `https://mycluster-int.azurehdinsight.net/`. Tenga en cuenta que `-int` debe ir después de `mycluster`, pero antes de `.azurehdinsight.net`.
    1. Si el clúster está en la red virtual, se usa IR autohospedado, se ha utilizado la dirección URL privada y se sigue produciendo un error en la conexión, la VM en la que se instaló IR tenía problemas para conectarse a HDI. 
    
       Conéctese a la VM en la que está instalado IR y abra la interfaz de usuario de Ambari en un explorador. Use la dirección URL privada del clúster. Esta conexión debería funcionar desde el explorador. Si no es así, póngase en contacto con el equipo de soporte técnico de HDInsight para obtener más ayuda.
    1. Si no se usa IR autohospedado, el clúster de HDI debe ser accesible públicamente. Abra la interfaz de usuario de Ambari en un explorador y compruebe que se abre. Si hay algún problema con el clúster o con los servicios que contiene, póngase en contacto con el equipo de soporte técnico de HDInsight para obtener ayuda.

       La dirección URL del clúster de HDI usada en el servicio vinculado debe ser accesible para IR (autohospedado o Azure) para que la conexión de prueba se supere y para que las ejecuciones funcionen. Una manera fácil de comprobar este estado es abrir esa dirección URL en un explorador desde una VM o desde cualquier máquina pública.

### <a name="error-code-2343"></a>Código de error: 2343

- **Mensaje**: `User name and password cannot be null or empty to connect to the HDInsight cluster.`

- **Causa**: El nombre de usuario o la contraseña están vacíos.

- **Recomendación:** Proporcione las credenciales correctas para conectarse a HDI e inténtelo de nuevo.

### <a name="error-code-2345"></a>Código de error: 2345

- **Mensaje**: `Failed to read the content of the hive script. Error: '%message;'`

- **Causa**: el archivo de script no existe o el servicio no se pudo conectar a la ubicación del script.

- **Recomendación:** Compruebe que el script existe y que el servicio vinculado asociado tenga las credenciales adecuadas para una conexión.

### <a name="error-code-2346"></a>Código de error: 2346

- **Mensaje**: `Failed to create ODBC connection to the HDI cluster with error message '%message;'.`

- **Causa**: el servicio intentó establecer una conexión de conectividad abierta de bases de datos (ODBC) con el clúster de HDI y se produjo un error.

- **Recomendación:** 

   1. Confirme que ha configurado correctamente la conexión de ODBC o Java Database Connectivity (JDBC).
      1. En el caso de JDBC, si está usando la misma red virtual, puede obtener esta conexión desde:<br>
        `Hive -> Summary -> HIVESERVER2 JDBC URL`
      1. Para asegurarse de que tiene la configuración de JDBC correcta, consulte [Consulta de Apache Hive mediante el controlador JDBC en HDInsight](../hdinsight/hadoop/apache-hadoop-connect-hive-jdbc-driver.md).
      1. En el caso de la conectividad abierta de bases de datos (ODB), consulte [Tutorial: Consulta de Apache Hive con ODBC y PowerShell](../hdinsight/interactive-query/apache-hive-query-odbc-driver-powershell.md) para asegurarse de que tiene la configuración correcta. 
   1. Compruebe que Hiveserver2, Hive MetaStore e Hiveserver2 Interactive están activos y en funcionamiento. 
   1. Compruebe la interfaz de usuario de Ambari (UI):
      1. Asegúrese de que aún se están ejecutando todos los servicios.
      1. En la interfaz de usuario de Ambari, compruebe la sección de alerta en el panel.
         1. Para obtener más información sobre las alertas y las resoluciones para las alertas, consulte [Administración y supervisión de un clúster](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.5.0/managing-and-monitoring-ambari/content/amb_predefined_alerts.html).
   1. Si estos pasos no son suficientes para resolver el problema, póngase en contacto con el equipo de Azure HDInsight.

### <a name="error-code-2347"></a>Código de error: 2347

- **Mensaje**: `Hive execution through ODBC failed with error message '%message;'.`

- **Causa**: el servicio envió el script de Hive para ejecutar al clúster de HDI a través de la conexión ODBC y el script ha generado un error en HDI.

- **Recomendación:** 

   1. Confirme que ha configurado correctamente la conexión de ODBC o Java Database Connectivity (JDBC).
      1. En el caso de JDBC, si está usando la misma red virtual, puede obtener esta conexión desde:<br>
        `Hive -> Summary -> HIVESERVER2 JDBC URL`
      1. Para asegurarse de que tiene la configuración de JDBC correcta, consulte [Consulta de Apache Hive mediante el controlador JDBC en HDInsight](../hdinsight/hadoop/apache-hadoop-connect-hive-jdbc-driver.md).
      1. En el caso de la conectividad abierta de bases de datos (ODB), consulte [Tutorial: Consulta de Apache Hive con ODBC y PowerShell](../hdinsight/interactive-query/apache-hive-query-odbc-driver-powershell.md) para asegurarse de que tiene la configuración correcta. 
   1. Compruebe que Hiveserver2, Hive MetaStore e Hiveserver2 Interactive están activos y en funcionamiento. 
   1. Compruebe la interfaz de usuario de Ambari (UI):
      1. Asegúrese de que aún se están ejecutando todos los servicios.
      1. En la interfaz de usuario de Ambari, compruebe la sección de alerta en el panel.
         1. Para obtener más información sobre las alertas y las resoluciones para las alertas, consulte [Administración y supervisión de un clúster](https://docs.cloudera.com/HDPDocuments/Ambari-2.7.5.0/managing-and-monitoring-ambari/content/amb_predefined_alerts.html).
   1. Si estos pasos no son suficientes para resolver el problema, póngase en contacto con el equipo de Azure HDInsight.

### <a name="error-code-2348"></a>Código de error: 2348

- **Mensaje**: `The main storage has not been initialized. Please check the properties of the storage linked service in the HDI linked service.`

- **Causa**: las propiedades del servicio vinculado de almacenamiento no se han establecido correctamente.

- **Recomendación:** solo se admiten cadenas de conexión completas en el servicio vinculado de almacenamiento principal para las actividades de HDI. Compruebe que no está utilizando aplicaciones o autorizaciones de MSI.

### <a name="error-code-2350"></a>Código de error: 2350

- **Mensaje**: `Failed to prepare the files for the run '%jobId;'. HDI cluster: '%cluster;', Error: '%errorMessage;'`

- **Causa**: las credenciales proporcionadas para conectarse al almacenamiento donde se deben ubicar los archivos son incorrectas o los archivos no existen ahí.

- **Recomendación**: este error se produce cuando el servicio se prepara para actividades de HDI e intenta copiar archivos en el almacenamiento principal antes de enviar el trabajo a HDI. Compruebe que los archivos se encuentren en la ubicación proporcionada y que la conexión de almacenamiento sea correcta. Las actividades de HDI no admiten la autenticación de MSI en cuentas de almacenamiento relativas a actividades de HDI, por lo que debe comprobar que esos servicios vinculados tengan claves completas o usen Azure Key Vault.

### <a name="error-code-2351"></a>Código de error: 2351

- **Mensaje**: `Could not open the file '%filePath;' in container/fileSystem '%container;'.`

- **Causa**: El archivo no existe en la ruta de acceso especificada.

- **Recomendación:** Compruebe si el archivo existe realmente y si el servicio vinculado con información de conexión que apunta a este archivo tiene las credenciales correctas.

### <a name="error-code-2352"></a>Código de error: 2352

- **Mensaje**: `The file storage has not been initialized. Please check the properties of the file storage linked service in the HDI activity.`

- **Causa**: las propiedades del servicio vinculado de almacenamiento de archivos no se han establecido correctamente.

- **Recomendación:** Compruebe que las propiedades del servicio vinculado de almacenamiento de archivos estén configuradas correctamente.

### <a name="error-code-2353"></a>Código de error: 2353

- **Mensaje**: `The script storage has not been initialized. Please check the properties of the script storage linked service in the HDI activity.`

- **Causa**: las propiedades del servicio vinculado de almacenamiento de scripts no se han establecido correctamente.

- **Recomendación:** Compruebe que las propiedades del servicio vinculado de almacenamiento de scripts estén configuradas correctamente.

### <a name="error-code-2354"></a>Código de error: 2354

- **Mensaje**: `The storage linked service type '%linkedServiceType;' is not supported for '%executorType;' activities for property '%linkedServicePropertyName;'.`

- **Causa**: El tipo de servicio vinculado de almacenamiento no es compatible con la actividad.

- **Recomendación:** Compruebe que el servicio vinculado seleccionado tenga uno de los tipos admitidos de la actividad. Las actividades de HDI admiten los servicios vinculados AzureBlobStorage y AzureBlobFSStorage.

   Para más información, consulte [Comparación de opciones de almacenamiento para los clústeres de Azure HDInsight](../hdinsight/hdinsight-hadoop-compare-storage-options.md).

### <a name="error-code-2355"></a>Código de error: 2355

- **Mensaje**: `The '%value' provided for commandEnvironment is incorrect. The expected value should be an array of strings where each string has the format CmdEnvVarName=CmdEnvVarValue.`

- **Causa**: El valor proporcionado para `commandEnvironment` es incorrecto.

- **Recomendación:** Compruebe que el valor proporcionado sea similar al siguiente:
 
    ``` \"commandEnvironment\": [
    \"variableName=variableValue\"
    ]
    ```

    Compruebe también que cada variable aparece en la lista solo una vez.

### <a name="error-code-2356"></a>Código de error: 2356

- **Mensaje**: `The commandEnvironment already contains a variable named '%variableName;'.`

- **Causa**: El valor proporcionado para `commandEnvironment` es incorrecto.

- **Recomendación:** Compruebe que el valor proporcionado sea similar al siguiente:
 
    ``` \"commandEnvironment\": [
    \"variableName=variableValue\"
    ]
    ```

    Compruebe también que cada variable aparece en la lista solo una vez.

### <a name="error-code-2357"></a>Código de error: 2357

- **Mensaje**: `The certificate or password is wrong for ADLS Gen 1 storage.`

- **Causa**: las credenciales proporcionadas son incorrectas.

- **Recomendación:** Compruebe la información de conexión en el servicio vinculado ADLS Gen 1 y compruebe que la conexión de prueba se realiza correctamente.

### <a name="error-code-2358"></a>Código de error: 2358

- **Mensaje**: `The value '%value;' for the required property 'TimeToLive' in the on demand HDInsight linked service '%linkedServiceName;' has invalid format. It should be a timespan between '00:05:00' and '24:00:00'.`

- **Causa**: El valor proporcionado para la propiedad necesaria `TimeToLive` tiene un formato no válido. 

- **Recomendación:** Actualice el valor para que esté en el intervalo sugerido e inténtelo de nuevo.

### <a name="error-code-2359"></a>Código de error: 2359

- **Mensaje**: `The value '%value;' for the property 'roles' is invalid. Expected types are 'zookeeper', 'headnode', and 'workernode'.`

- **Causa**: El valor proporcionado para la propiedad `roles` no es válido.

- **Recomendación:** Actualice el valor para que sea una de las sugerencias e inténtelo de nuevo.

### <a name="error-code-2360"></a>Código de error: 2360

- **Mensaje**: `The connection string in HCatalogLinkedService is invalid. Encountered an error while trying to parse: '%message;'.`

- **Causa**: La cadena de conexión proporcionada para `HCatalogLinkedService` no es válida.

- **Recomendación:** Actualice el valor a una cadena de conexión de Azure SQL correcta e inténtelo de nuevo.

### <a name="error-code-2361"></a>Código de error: 2361

- **Mensaje**: `Failed to create on demand HDI cluster. Cluster name is '%clusterName;'.`

- **Causa**: no se pudo crear el clúster y el servicio no ha recibido un error del servicio HDInsight.

- **Recomendación:** Abra Azure Portal, intente encontrar el recurso de HDI con el nombre proporcionado y compruebe el estado de aprovisionamiento. Póngase en contacto con el equipo de soporte técnico de HDInsight para obtener más ayuda.

### <a name="error-code-2362"></a>Código de error: 2362

- **Mensaje**: `Only Azure Blob storage accounts are supported as additional storages for HDInsight on demand linked service.`

- **Causa**: el almacenamiento adicional proporcionado no era almacenamiento de blobs de Azure.

- **Recomendación:** Proporcione una cuenta de Azure Blob Storage como almacenamiento adicional para el servicio vinculado de HDInsight a petición.

### <a name="ssl-error-when-linked-service-using-hdinsight-esp-cluster"></a>Error de SSL cuando el servicio vinculado usa un clúster de ESP de HDInsight

- **Mensaje**: `Failed to connect to HDInsight cluster: 'ERROR [HY000] [Microsoft][DriverSupport] (1100) SSL certificate verification failed because the certificate is missing or incorrect.`

- **Causa**: lo más probable es que el problema esté relacionado con el almacén de confianza del sistema.

- **Solución:** puede ir a la ruta de acceso **Microsoft Integration Runtime\4.0\Shared\ODBC Drivers\Microsoft Hive ODBC Driver\lib** y abrir DriverConfiguration64.exe para cambiar la configuración.

    :::image type="content" source="./media/connector-troubleshoot-guide/system-trust-store-setting.png" alt-text="Desactive la casilla Use System Trust Store (Usar almacén de confianza del sistema)":::

## <a name="web-activity"></a>Actividad web

### <a name="error-code-2128"></a>Código de error: 2128

- **Mensaje**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Causa**: Este problema puede deberse a la conectividad de red, un error de DNS, una validación de certificado de servidor o un tiempo de espera.

- **Recomendación:** compruebe que el punto de conexión con el que intenta comunicarse responde a las solicitudes. Puede usar herramientas como **Fiddler/Postman/Netmon/Wireshark**.

### <a name="error-code-2108"></a>Código de error: 2108

- **Mensaje**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Causa**: No se pudo realizar la solicitud debido a un problema subyacente (conectividad de red, error de DNS, validación del certificado de servidor o tiempo de espera).

- **Recomendación**: use Fiddler/Postman/Netmon/Wireshark para validar la solicitud.

#### <a name="more-details"></a>Más detalles
Para usar **Fiddler** para crear una sesión HTTP de la aplicación web supervisada:

1. Descargue, instale y abra [Fiddler](https://www.telerik.com/download/fiddler).

1. Si su aplicación web usa HTTPS, vaya a **Tools** > **Fiddler Options** > **HTTPS** (Herramientas>Opciones de Fiddler>HTTPS).

   1. Seleccione **Capturar CONEXIONES HTTPS** y **Descifrar tráfico HTTPS**.

      :::image type="content" source="media/data-factory-troubleshoot-guide/fiddler-options.png" alt-text="Opciones de Fiddler":::

1. Si la aplicación usa certificados TLS/SSL, agregue el certificado de Fiddler al dispositivo.

   Vaya a: **Herramientas** > **Opciones de Fiddler** > **HTTPS** > **Acciones** > **Exportar el certificado raíz al escritorio**.

1. Desactive la captura en **File** > **Capture Traffic** (Archivo > Capturar tráfico). O bien, presione **F12**.

1. Borre la caché del explorador para quitar todos los elementos almacenados en caché, que tendrá que volver a descargar.

1. Creación de una solicitud:

1. Seleccione la pestaña **Composer** (Compositor).

   1. Establezca el método HTTP y la dirección URL.
 
   1. Si es necesario, agregue encabezados y un cuerpo de la solicitud.

   1. Seleccione **Execute**(Ejecutar).

1. Active la captura de tráfico de nuevo y complete la transacción problemática en su página.

1. Vaya a: **Archivo** > **Guardar** > **Todas las sesiones**.

Para más información, consulte [Introducción a Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="general"></a>General

### <a name="activity-stuck-issue"></a>Problema de bloqueo de actividad

Si observa que la actividad tarda mucho más tiempo en ejecutarse que sus ejecuciones normales sin apenas progreso, es posible que se haya bloqueado. Puede intentar cancelarla y volver a intentarlo para ver si sirve. Si se trata de una actividad de copia, puede encontrar información sobre la solución de problemas y la supervisión de rendimiento en [Solución de problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md); si se trata de un flujo de datos, puede encontrar información en [Rendimiento de los flujos de datos de asignación](concepts-data-flow-performance.md) y la guía de ajuste.

### <a name="payload-is-too-large"></a>La carga es demasiado grande

**Mensaje de error:** `The payload including configurations on activity/dataSet/linked service is too large. Please check if you have settings with very large value and try to reduce its size.`

**Causa:** La carga de cada ejecución de actividad incluye la configuración de la actividad, las configuraciones de los conjuntos de datos asociados y los servicios vinculados (si los hay), y una pequeña parte de las propiedades del sistema que se generan por tipo de actividad. El límite de este tamaño de carga es 896 KB, tal y como se menciona en la documentación sobre los límites de Azure para [Data Factory](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits) y [Azure Synapse Analytics](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-synapse-analytics-limits).

**Recomendación:** Probablemente se alcanza este límite porque se pasan uno o más valores grandes de parámetro desde una salida de actividad ascendente o de forma externa, especialmente si se pasan datos reales entre actividades en el flujo de control. Compruebe si puede reducir el tamaño de los valores de parámetros grandes, o bien ajuste la lógica de canalización para evitar pasar estos valores entre las actividades y administrarlos dentro de la actividad en su lugar.

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda para solucionar problemas, pruebe estos recursos:

* [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
* [Solicitud de características de Data Factory](/answers/topics/azure-data-factory.html)
* [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
* [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)
* [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/)
* [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-data-factory.html)