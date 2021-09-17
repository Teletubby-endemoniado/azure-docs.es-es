---
title: Registro de la base de datos del metastore de Hive y exámenes de configuración en Azure Purview
description: En este artículo se describe cómo registrar una base de datos de metastore de Hive en Azure Purview y cómo configurar un examen.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: overview
ms.date: 5/17/2021
ms.openlocfilehash: 0a7d8a22cf8f9dcdaac9d3fe07bd6ab006e61818
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121751907"
---
# <a name="register-and-scan-hive-metastore-database"></a>Registro y examen de la base de datos de metastore de Hive

En este artículo se describe cómo registrar una base de datos de metastore de Hive en Purview y cómo configurar un examen.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

El origen de metastore de Hive admite un examen completo para extraer metadatos de una **base de datos de metastore de Hive** y captura el linaje entre recursos de datos. Las plataformas admitidas son Apache Hadoop, Cloudera, Hortonworks y Databricks.

## <a name="prerequisites"></a>Requisitos previos

1.  Configure la versión más reciente del [entorno de ejecución de integración autohospedado](https://www.microsoft.com/download/details.aspx?id=39717).
    Para obtener más información, consulte [Creación y configuración de un entorno de ejecución de integración autohospedado](../data-factory/create-self-hosted-integration-runtime.md).

2.  Asegúrese de que [JDK 11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) está instalado en la máquina virtual donde también lo está el entorno de ejecución de integración autohospedado.

3.  Asegúrese de que \"Visual C++ Redistributable 2012 Update 4\" está instalado en la máquina del entorno de ejecución de integración autohospedado. Si aún no está instalado, descárguelo [aquí](https://www.microsoft.com/download/details.aspx?id=30679).

4.  Tiene que descargar manualmente el controlador JDBC de la base de datos de metastore de Hive en la máquina virtual donde se ejecuta el entorno de ejecución de integración autohospedado. Por ejemplo, si la base de datos usada es mssql, asegúrese de descargar el [controlador JDBCde Microsoft para SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

    > [!Note]
    > Todas las cuentas de la máquina virtual deben poder acceder al controlador. No lo instale en una cuenta de usuario.

5.  Las versiones admitidas de Hive son de la 2.x a la 3.x. Las versiones admitidas de Databricks son 8.0 y posteriores. 

## <a name="setting-up-authentication-for-a-scan"></a>Configuración de la autenticación para un examen

La única autenticación admitida en una base de datos de metastore de Hive es la **autenticación básica**.

## <a name="register-a-hive-metastore-database"></a>Registro de una base de datos de metastore de Hive

Para registrar una base de datos de metastore de Hive en el catálogo de datos, haga lo siguiente:

1.  Vaya a la cuenta de Purview.

2.  Seleccione **Data Map** (Mapa de datos) en el panel de navegación izquierdo.

3.  Seleccione **Registrar**.

4.  En Registrar orígenes, seleccione **Metastore de Hive**. Seleccione **Continue** (Continuar).

    :::image type="content" source="media/register-scan-hive-metastore-source/register-sources.png" alt-text="registro del origen de Hive" border="true":::

En la pantalla Registrar orígenes (metastore de Hive), haga lo siguiente:

1.  Escriba un **Name** (Nombre) con el que se muestre el origen de datos en el catálogo.

2.  Escriba la dirección **URL del clúster de Hive**. La dirección URL del clúster se puede obtener de la dirección URL de Ambari o de la dirección URL del área de trabajo de Databricks. Por ejemplo, hive.azurehdinsight.net o adb-19255636414785.5.azuredatabricks.net

3.  Especifique la **dirección URL del servidor del metastore de Hive**. Por ejemplo, sqlserver://hive.database.windows.net o jdbc:spark://adb-19255636414785.5.azuredatabricks.net:443

4.  Seleccione una colección o cree una nueva (opcional).

5.  Seleccione Finish (Finalizar) para registrar el origen de datos.

       :::image type="content" source="media/register-scan-hive-metastore-source/configure-sources.png" alt-text="configuración del origen de Hive" border="true":::


## <a name="creating-and-running-a-scan"></a>Creación y ejecución de un examen

Para crear y ejecutar un nuevo examen, siga estos pasos:

1.  En el centro de administración, haga clic en Integration runtimes (Entornos de ejecución de integración). Asegúrese de que está configurado un entorno de ejecución de integración autohospedado. Si no lo está, use los pasos que se indican [aquí](./manage-integration-runtimes.md) para configurar un entorno de ejecución de integración autohospedado.

2.  Vaya a **Sources** (Orígenes).

3.  Seleccione la base de datos de **metastore de Hive** registrada.

4.  Seleccione **+ New scan** (+ Nuevo examen).

5.  Especifique los detalles siguientes:

    1. **Name** (Nombre): el nombre del examen.

    1. **Connect via integration runtime** (Conectar mediante el entorno de ejecución de integración): seleccione el entorno de ejecución de integración autohospedado configurado.

    1. **Credential** (Credencial): seleccione la credencial para conectarse al origen de datos. Asegúrese de que:

       - Selecciona la autenticación básica al crear una credencial.
       - Proporcione el nombre de usuario de Metastore en el campo de entrada Nombre de usuario.
       - Almacene la contraseña de Metastore en la clave secreta.

       Para obtener más información sobre credenciales, vea el vínculo que se indica [aquí](manage-credentials.md). 

       **Uso de Databricks**: vaya al clúster de Databricks -> Aplicaciones -> Iniciar terminal web. Ejecute el cmdlet **cat /databricks/hive/conf/hive-site.xml**

       Se puede acceder al nombre de usuario y la contraseña desde las dos propiedades, como se muestra a continuación.

       :::image type="content" source="media/register-scan-hive-metastore-source/databricks-credentials.png" alt-text="databricks-username-password-details" border="true":::

    1. **Ubicación del controlador JDBC de Metastore**: especifique la ruta de acceso a la ubicación del controlador JDBC en la VM donde se ejecuta el entorno de ejecución de integración autohospedado. Debe ser la ruta de acceso a la ubicación válida de la carpeta JAR.

       Si va a examinar Databricks, vea la sección siguiente sobre Databricks.

       > [!Note]
       > Todas las cuentas de la máquina virtual deben poder acceder al controlador. No lo instale en una cuenta de usuario.

    1. **Clase de controlador JDBC de Metastore**: proporcione el nombre de la clase del controlador de conexión. Por ejemplo, \com.microsoft.sqlserver.jdbc.SQLServerDriver.
    
       **Uso de Databricks**: vaya al clúster de Databricks -> Aplicaciones -> Iniciar terminal web. Ejecute el cmdlet **cat /databricks/hive/conf/hive-site.xml**
    
       Se puede acceder a la clase de controlador desde la propiedad, como se muestra a continuación.
    :::image type="content" source="media/register-scan-hive-metastore-source/databricks-driver-class-name.png" alt-text="databricks-driver-class-details" border="true":::

    1. **Dirección URL de JDBC de Metastore**: proporcione el valor de la dirección URL de conexión y defina la conexión a la dirección URL del servidor de base de datos de Metastore. Por ejemplo,     `jdbc:sqlserver://hive.database.windows.net;database=hive;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300`.

       **Uso de Databricks**: vaya al clúster de Databricks -> Aplicaciones -> Iniciar terminal web. Ejecute el cmdlet **cat /databricks/hive/conf/hive-site.xml**
    
       Se puede acceder a la dirección URL de JDBC desde la propiedad Dirección URL de conexión, como se muestra a continuación.
       
       :::image type="content" source="media/register-scan-hive-metastore-source/databricks-jdbc-connection.png" alt-text="databricks-jdbc-url-details" border="true":::
    
       > [!NOTE]
       > Al copiar la dirección URL de *hive-site.xml*, asegúrese de quitar `amp;` de la cadena para que no se produzca un error en el examen.

       A esta dirección URL anéxele la ruta de acceso a la ubicación donde está colocado el certificado SSL en la máquina virtual. El certificado SSL se puede descargar desde [aquí](../mysql/howto-configure-ssl.md).

       La dirección URL de JDBC del metastore es:
    
       `jdbc:mariadb://consolidated-westus2-prod-metastore-addl-1.mysql.database.azure.com:3306/organization1829255636414785?trustServerCertificate=true&amp;useSSL=true&sslCA=D:\Drivers\SSLCert\BaltimoreCyberTrustRoot.crt.pem`

    1. **Nombre de la base de datos del metastore**: proporcione el nombre de la base de datos del metastore de Hive.
    
       Si va a examinar Databricks, vea la sección siguiente sobre Databricks.

       **Uso de Databricks**: vaya al clúster de Databricks -> Aplicaciones -> Iniciar terminal web. Ejecute el cmdlet **cat /databricks/hive/conf/hive-site.xml**

       Se puede acceder al nombre de la base de datos desde la propiedad Dirección URL de JDBC, como se muestra a continuación. Por ejemplo: organization1829255636414785
       
       :::image type="content" source="media/register-scan-hive-metastore-source/databricks-data-base-name.png" alt-text="databricks-database-name-details" border="true":::

    1. **Esquema**: especifique la lista de esquemas de Hive que se importarán. Por ejemplo, schema1; schema2. 
    
        Si esa lista está vacía, se importan todos los esquemas del usuario. De forma predeterminada, se ignoran todos los esquemas del sistema (por ejemplo, SysAdmin) y los objetos. 

        Si la lista está vacía, se importan todos los esquemas disponibles. Los patrones de nombres de esquemas aceptables que usan la sintaxis de expresiones SQL LIKE incluyen el uso de %, por ejemplo, A%; %B; %C%; D

        - empieza por A o    
        - termina en B o    
        - contiene C o    
        - igual a D

        No se acepta el empleo de NOT ni de caracteres especiales.

     1. **Maximum memory available** (Memoria máxima disponible): memoria máxima (en GB) disponible en la máquina virtual del cliente que van a usar los procesos de examen. Esto depende del tamaño del origen de la base de datos de Metastore de Hive que se va a examinar.

        :::image type="content" source="media/register-scan-hive-metastore-source/scan.png" alt-text="examinar el origen de Hive" border="true":::

6.  Haga clic en **Continuar**.

7.  Elija el **desencadenador del examen**. Puede configurar una programación o ejecutar el examen una vez.

8.  Revise el examen y haga clic en **Save and run** (Guardar y ejecutar).

## <a name="viewing-your-scans-and-scan-runs"></a>Visualización de los exámenes y las ejecuciones de exámenes

1. Vaya al centro de administración. Seleccione **Data sources** (Orígenes de datos) en la sección **Sources and scanning** (Orígenes y examen).

2. Seleccione el origen de datos que desee. Verá una lista de los exámenes existentes en ese origen de datos.

3. Seleccione el examen cuyos resultados le interesa ver.

4. En esta página se muestran todas las ejecuciones de exámenes anteriores junto con las métricas y el estado de cada ejecución del examen. También mostrará si el análisis se ha programado o es manual, a cuántos recursos se han aplicado clasificaciones, cuántos recursos totales se han detectado, la hora de inicio y finalización del examen y la duración total del examen.

## <a name="manage-your-scans"></a>Administración de exámenes

Para administrar o eliminar un examen, haga lo siguiente:

1. Vaya al centro de administración. Seleccione **Data sources** (Orígenes de datos) en la sección **Sources and scanning** (Orígenes y examen) y, a continuación, seleccione el origen de datos deseado.

2. Seleccione el examen que desea administrar. Seleccione **Edit** (Editar) para editar el examen.

3. Seleccione **Delete** (Eliminar) para eliminar el examen.

## <a name="next-steps"></a>Pasos siguientes

- [Examen del catálogo de datos de Azure Purview](how-to-browse-catalog.md)
- [Búsqueda en el catálogo de datos de Azure Purview](how-to-search-catalog.md)
