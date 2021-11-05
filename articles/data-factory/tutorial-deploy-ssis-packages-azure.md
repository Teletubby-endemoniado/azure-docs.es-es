---
title: Aprovisionamiento de Azure-SSIS Integration Runtime
description: Aprenda a aprovisionar Integration Runtime de Azure SSIS en Azure Data Factory para que pueda implementar y ejecutar paquetes SSIS en Azure.
ms.service: data-factory
ms.subservice: integration-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 10/22/2021
author: swinarko
ms.author: sawinark
ms.openlocfilehash: b6718ddcd4090708dd77e192832a04b4f2591751
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131850325"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Aprovisionamiento de Azure-SSIS Integration Runtime en Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

En este tutorial se describen los pasos necesarios para usar Azure Portal para aprovisionar el entorno de ejecución de integración de Azure-SQL Server Integration Services (ISSIS) en Azure Data Factory (ADF). Azure-SSIS Integration Runtime admite:

- La ejecución de paquetes implementados en el catálogo de SSIS (SSISDB) hospedados por un servidor de Azure SQL Database o por Instancia administrada (modelo de implementación de proyectos)
- La ejecución de paquetes implementados en el sistema de archivos, en Azure Files o en una base de datos de SQL Server (MSDB) hospedados por Instancia administrada de Azure SQL (modelo de implementación de paquetes)

Después de aprovisionar una instancia de Azure-SSIS IR, puede usar herramientas conocidas para implementar y ejecutar los paquetes en Azure. Estas herramientas ya están habilitadas para Azure e incluyen SQL Server Data Tools (SSDT), SQL Server Management Studio (SSMS) y utilidades de la línea de comandos, como [dtutil](/sql/integration-services/dtutil-utility) y [AzureDTExec](./how-to-invoke-ssis-package-azure-enabled-dtexec.md).

Para obtener información conceptual acerca de Integration Runtime para la integración de SSIS en Azure, consulte [Introducción a Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime).

En este tutorial, va a completar los siguientes pasos:

> [!div class="checklist"]
> * Creación de una factoría de datos.
> * Aprovisionamiento de una instancia de Integration Runtime de SSIS en Azure.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

- **Servidor de Azure SQL Database (opcional)** . Si aún no tiene un servidor de bases de datos, cree uno en Azure Portal antes de empezar. A su vez, Data Factory creará una instancia de SSISDB en este servidor de bases de datos. 

  Le recomendamos que cree el servidor de bases de datos en la misma región de Azure que el entorno de ejecución de integración. Esta configuración permite que el entorno de ejecución de integración escriba registros de ejecución en SSISDB sin traspasar regiones de Azure.

  Tenga en cuenta los siguientes puntos:

  - En función del servidor de bases de datos seleccionado, la instancia de SSISDB puede crearse en su nombre como una base de datos única, como parte de un grupo elástico o en una instancia administrada. Se puede acceder a ella en una red pública o mediante la unión a una red virtual. Para obtener asesoramiento acerca de cómo elegir el tipo de servidor de bases de datos que va a hospedar SSISDB, consulte [Comparación entre SQL Database e Instancia administrada de SQL](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-sql-database-and-sql-managed-instance). 
  
    Si usa un servidor de Azure SQL Database con reglas de firewall de IP/puntos de conexión de servicio de red virtual o una instancia administrada con punto de conexión privado para hospedar SSISDB, o si necesita acceder a datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para más información, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](./create-azure-ssis-integration-runtime.md).

  - Confirme que el valor **Permitir el acceso a servicios de Azure** está habilitado para el servidor de bases de datos. Esta configuración no es aplicable si para hospedar SSISDB se usa el servidor de Azure SQL Database con reglas de firewall de red/puntos de conexión de servicio de red virtual o una instancia administrada con punto de conexión privado. Para más información, consulte [Protección de Azure SQL Database](../azure-sql/database/secure-database-tutorial.md#create-firewall-rules). Para habilitar este valor mediante PowerShell, consulte [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).

  - Agregue la dirección IP de la máquina cliente, o un intervalo de direcciones IP que la incluya, a la lista de direcciones IP de cliente de la configuración del firewall del servidor de bases de datos. Para más información, consulte [Reglas de firewall de nivel de base de datos y de nivel de servidor de Azure SQL Database](../azure-sql/database/firewall-configure.md).

  - Puede conectarse al servidor de bases de datos mediante la autenticación de SQL con sus credenciales de administrador del servidor o por medio de la autenticación de Azure Active Directory (Azure AD) con la identidad administrada asignada por el usuario o especificada por el sistema para la factoría de datos. En el último de los casos, debe agregar la identidad administrada asignada por el usuario o especificada por el sistema para la factoría de datos a un grupo de Azure AD con permisos de acceso al servidor de bases de datos. Para más información, consulte [Creación de una instancia de Azure-SSIS IR con autenticación de Azure AD](./create-azure-ssis-integration-runtime.md).

  - Confirme que el servidor de bases de datos no tiene aún una instancia de SSISDB. El aprovisionamiento de Azure-SSIS IR no admite el uso de una instancia de SSISDB existente.

> [!NOTE]
> Para ver una lista de las regiones de Azure en las que Data Factory y Azure-SSIS IR están disponibles actualmente, consulte [Disponibilidad de Data Factory y SSIS IR por región](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Crear una factoría de datos

Para crear la factoría de datos mediante Azure Portal, siga las instrucciones paso a paso que se indican en [Creación de una factoría de datos mediante la interfaz de usuario](./quickstart-create-data-factory-portal.md#create-a-data-factory). Seleccione **Anclar al panel** mientras lo hace, para permitir el acceso rápido después de su creación. 

Después de crear la factoría de datos, abra su página de información general en Azure Portal. Seleccione el icono **Author & monitor**  (Creación y supervisión) para abrir su página de **inicio** en otra pestaña. Ahí puede continuar con la creación de la instancia de Azure-SSIS IR.

## <a name="create-an-azure-ssis-integration-runtime"></a>Creación de un entorno de ejecución de integración de SSIS para Azure

### <a name="from-the-data-factory-overview"></a>Desde la información general de Data Factory

1. En la página principal, seleccione el icono **Configure SSIS** (Configurar SSIS). 

   :::image type="content" source="./media/doc-common-process/get-started-page.png" alt-text="Captura de pantalla que muestra la página principal de Azure Data Factory.":::

1. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

### <a name="from-the-authoring-ui"></a>Desde la interfaz de usuario de creación

1. En la interfaz de usuario de Azure Data Factory, cambie a la pestaña **Manage** (Administrar) y, después, cambie a la pestaña **Integration runtimes** (Entornos de ejecución de integración) para ver los entornos de ejecución de integración existentes de la factoría de datos. 

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png" alt-text="Opciones para ver las instancias de Integration Runtime existentes":::

1. Seleccione **New** (Nuevo) para crear una instancia de Azure-SSIS IR y abrir el panel **Integration runtime setup** (Configuración del entorno de ejecución de integración). 

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png" alt-text="Integration Runtime a través del menú":::

1. En el panel **Integration runtime setup** (Configuración de Integration Runtime), seleccione el icono **Lift-and-shift existing SSIS packages to execute in Azure** (Migrar mediante lift-and-shift los paquetes de SSIS existentes para ejecutarlos en Azure) y haga clic en **Continue** (Continuar).

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png" alt-text="Especificación del tipo de instancia de Integration Runtime":::

1. Para el resto de pasos de configuración de Integration Runtime para la integración de SSIS en Azure, consulte la sección [Aprovisionamiento de Integration Runtime de SSIS de Azure](#provision-an-azure-ssis-integration-runtime). 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprovisionamiento de una instancia de Integration Runtime para la integración de SSIS en Azure

El panel **Integration runtime setup** (Configuración del entorno de ejecución de integración) tiene tres páginas en las que puede configurar opciones generales, de implementación y avanzadas respectivamente.

### <a name="general-settings-page"></a>Página General settings (Configuración general)

En la página **General settings** (Configuración general) del panel **Integration runtime setup** (Configuración de Integration Runtime), realice los pasos siguientes. 

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png" alt-text="Configuración general":::

   1. En **Name** (Nombre), escriba el nombre del entorno de ejecución de integración. 

   1. En **Description** (Descripción), escriba la descripción del entorno de ejecución de integración. 

   1. En **Location** (Ubicación), seleccione la ubicación del entorno de ejecución de integración. Se muestran solo las ubicaciones admitidas. Le recomendamos que seleccione la misma ubicación del servidor de bases de datos que va a hospedar SSISDB. 

   1. En **Node Size** (Tamaño de nodo), seleccione el tamaño de nodo del clúster del entorno de ejecución de integración. Se muestran solo los tamaños de nodo admitidos. Seleccione un tamaño de nodo grande (escalado vertical) si quiere ejecutar muchos paquetes de uso intensivo de proceso o memoria. 

   1. En **Node Number** (Número de nodos), seleccione el número de nodos del clúster del entorno de ejecución de integración. Se muestran solo los números de nodos admitidos. Seleccione un clúster de grande con muchos nodos (escalado horizontal), si quiere ejecutar muchos paquetes en paralelo. 

   1. En **Edition/License** (Edición o licencia), seleccione la edición de SQL Server para el entorno de ejecución de integración: Standard o Enterprise. Seleccione Enterprise si quiere usar características avanzadas en el entorno de ejecución de integración. 

   1. En **Save Money** (Ahorrar dinero), seleccione la opción Azure Hybrid Benefit (Ventaja híbrida de Azure) para el entorno de ejecución de integración: **Yes** (Sí) o **No**. Seleccione **Yes** (Sí) si quiere que su propia licencia de SQL Server con Software Assurance se beneficie de los ahorros con el uso híbrido. 

   1. Seleccione **Continuar**. 

### <a name="deployment-settings-page"></a>Página Deployment settings (Configuración de implementación)

En la página **Deployment settings** (Configuración de implementación) del panel **Integration runtime setup** (Configuración de Integration Runtime), tiene las opciones para crear los almacenes de paquetes de Azure-SSIS IR o SSISDB.

#### <a name="creating-ssisdb"></a>Creación de SSISDB

En la página **Deployment settings** (Configuración de implementación) del panel **Integration runtime setup** (Configuración de Integration Runtime), si desea implementar los paquetes en SSISDB (Modelo de implementación de proyectos), active la casilla **Create SSIS catalog (SSISDB) hosted by Azure SQL Database server/Managed Instance to store your projects/packages/environments/execution logs** (Crear catálogo de SSIS [SSISDB] hospedado en el servidor de Azure SQL Database/Instancia administrada para almacenar los proyectos/paquetes/entornos/registros de ejecución). De forma alternativa, no necesita crear una instancia de SSISDB ni activar la casilla, si quiere implementar los paquetes en el sistema de archivos, en Azure Files o en una base de datos de SQL Server (MSDB) hospedados por Azure SQL Managed Instance (modelo de implementación de paquetes).

Con independencia del modelo de implementación, active esta casilla de todos modos si quiere usar Agente SQL Server hospedado por Azure SQL Managed Instance para orquestar o programar las ejecuciones de paquetes, ya que está habilitado por SSISDB. Para obtener más información, consulte el artículo [Programación de ejecuciones de paquetes SSIS mediante el agente de Instancia administrada de Azure SQL](./how-to-invoke-ssis-package-managed-instance-agent.md).
   
Si activa la casilla, realice los pasos siguientes para traer su propio servidor de bases de datos y hospedar la instancia de SSISDB que se creará y administrará en su nombre.

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/deployment-settings.png" alt-text="Configuración de implementación de SSISDB":::
   
   1. En **Subscription** (Suscripción), seleccione la suscripción de Azure que tiene el servidor de bases de datos que va a hospedar SSISDB. 

   1. En **Location** (Ubicación), seleccione la ubicación del servidor de bases de datos que va a hospedar SSISDB. Es recomendable seleccionar la misma ubicación del entorno de ejecución de integración.

   1. En **Catalog Database Server Endpoint** (Punto de conexión del servidor de bases de datos de catálogo), seleccione el punto de conexión de su servidor de bases de datos que va a hospedar SSISDB. 
   
      En función del servidor de bases de datos seleccionado, la instancia de SSISDB puede crearse en su nombre como una base de datos única, como parte de un grupo elástico o en una instancia administrada. Se puede acceder a ella en una red pública o mediante la unión a una red virtual. Para obtener asesoramiento acerca de cómo elegir el tipo de servidor de bases de datos que va a hospedar SSISDB, consulte [Comparación entre SQL Database e Instancia administrada de SQL](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-sql-database-and-sql-managed-instance).   

      Si selecciona un servidor de Azure SQL Database con reglas de firewall de IP/puntos de conexión de servicio de red virtual o una instancia administrada con un punto de conexión privado para hospedar SSISDB, o si necesita acceder a los datos locales sin configurar IR autohospedado, debe unir la instancia de Azure-SSIS IR a una red virtual. Para más información, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](./create-azure-ssis-integration-runtime.md).

   1. Active la casilla **Use AAD authentication with the system managed identity for Data Factory** (Usar la autenticación de AAD con la identidad administrada por el sistema para Data Factory) o **Use AAD authentication with a user-assigned managed identity for Data Factory** (Usar la autenticación de AAD con una identidad administrada asignada por el usuario para Data Factory) para elegir el método de autenticación de Azure AD para que Azure-SSIS IR acceda al servidor de bases de datos que hospeda SSISDB. No seleccione ninguna de las casillas para elegir el método de autenticación de SQL en su lugar.

      Si activa cualquiera de las casillas, deberá agregar la identidad administrada asignada por el usuario o por el sistema especificada para la factoría de datos a un grupo de Azure AD con permisos de acceso al servidor de bases de datos. Si activa la casilla **Use AAD authentication with a user-assigned managed identity for Data Factory** (Usar la autenticación de AAD con una identidad administrada asignada por el usuario para Data Factory), puede seleccionar las credenciales existentes creadas con las identidades administradas asignadas por el usuario especificadas o crear otras. Para más información, consulte [Creación de una instancia de Azure-SSIS IR con autenticación de Azure AD](./create-azure-ssis-integration-runtime.md).

   1. En **Admin Username** (Nombre de usuario de administrador), escriba el nombre de usuario de autenticación de SQL del servidor de bases de datos que hospeda SSISDB. 

   1. En **Admin Password** (Contraseña de administrador), escriba la contraseña de la autenticación de SQL del servidor de bases de datos para hospedar SSISDB. 

   1. Active la casilla **Use dual standby Azure-SSIS Integration Runtime pair with SSISDB failover** (Usar par de Azure-SSIS Integration Runtime en espera dual) para configurar un par de Azure-SSIS IR en espera dual que funcione en sincronización con el grupo de conmutación por error de Azure SQL Database/Managed Instance para lograr continuidad empresarial y recuperación ante desastres.
   
      Si activa la casilla, escriba un nombre para identificar el par de Azure-SSIS IR principal y secundario en el cuadro de texto **Dual standby pair name** (Nombre de par en espera dual). Debe escribir el mismo nombre de par al crear sus instancias de Azure-SSIS IR principal y secundaria.

      Para más información, consulte [Configuración de Azure-SSIS Integration Runtime para continuidad empresarial y recuperación ante desastres (BCDR)](./configure-bcdr-azure-ssis-integration-runtime.md).

   1. En **Catalog Database Service Tier** (Nivel de servicio de bases de datos de catálogo), seleccione el nivel de servicio del servidor de bases de datos para hospedar SSISDB. Seleccione el nivel de servicio Basic, Standard o Premium, o seleccione un nombre de grupo elástico.

Seleccione **Probar conexión** cuando corresponda y, si es correcta, seleccione **Continuar**.

#### <a name="creating-azure-ssis-ir-package-stores"></a>Creación de almacenes de paquetes de Azure-SSIS IR

En la página **Deployment settings** (Configuración de implementación) del panel **Integration runtime setup** (Configuración de Integration Runtime), si desea administrar los paquetes implementados en MSDB, el sistema de archivos o Azure Files (Modelo de implementación de paquetes) con almacenes de paquetes de Azure-SSIS IR, active la casilla **Create package stores to manage your packages that are deployed into file system/Azure Files/SQL Server database (MSDB) hosted by Azure SQL Managed Instance** (Crear almacenes de paquetes para administrar los paquetes implementados en el sistema de archivos, en Azure Files o en una base de datos de SQL Server [MSDB] hospedados por Instancia administrada de Azure SQL).
   
Los almacenes de paquetes de Azure-SSIS IR permiten importar, exportar, eliminar y ejecutar paquetes, así como supervisar y detener los paquetes en ejecución mediante SSMS de forma similar al [almacén de paquetes de SSIS heredado](/sql/integration-services/service/package-management-ssis-service). Para más información, vea el artículo [Administración de paquetes SSIS mediante almacenes de paquetes de Azure-SSIS IR](./azure-ssis-integration-runtime-package-store.md).
   
Si activa esta casilla, puede seleccionar **New** (Nuevo) para agregar varios almacenes de paquetes a una instancia de Azure-SSIS IR. Por otro lado, un almacén de paquetes se puede compartir con varias instancias de Azure-SSIS IR.

:::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/deployment-settings2.png" alt-text="Configuración de implementación para MSDB, un sistema de archivos o Azure Files":::

En el panel **Add package store** (Adición de un almacén de paquetes), siga estos pasos:
   
   1. En **Package store name** (Nombre del almacén de paquetes), escriba el nombre del almacén de paquetes. 

   1. En **Package store linked service** (Servicio vinculado de almacén de paquetes), seleccione el servicio vinculado existente que almacena la información de acceso del sistema de archivos, de Azure Files o de Instancia administrada de Azure SQL, donde se implementan los paquetes, o cree un servicio vinculado nuevo; para ello, seleccione **New** (Nuevo). En el panel **New linked service** (Nuevo servicio vinculado), realice los pasos siguientes. 

      > [!NOTE]
      > Puede usar servicios vinculados de **Azure File Storage** o **Sistema de archivos** para acceder a Azure Files. Si usa el servicio vinculado **Azure File Storage**, por ahora el almacén de paquetes Azure-SSIS IR solo admite el método de autenticación de tipo **Básico** (no **Clave de cuenta** ni **URI de SAS**). 

      :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/deployment-settings-linked-service.png" alt-text="Configuración de implementación de servicios vinculados":::

      1. En **Name** (Nombre), escriba el nombre del servicio vinculado. 
         
      1. En **Description** (Descripción), escriba la descripción del servicio vinculado. 
         
      1. En **Type** (Tipo), seleccione **Azure File Storage**, **Azure SQL Managed Instance** (Instancia administrada de Azure SQL) o **File System** (Sistema de archivos).

      1. Puede omitir la opción **Connect via integration runtime** (Conectarse a través de IR), ya que siempre se usa su instancia de Azure-SSIS IR para obtener la información de acceso de los almacenes de paquetes.
      
      1. Si selecciona **Azure File Storage**, en **Authentication method** (Método de autenticación), seleccione **Basic** (Básico) y, después, siga estos pasos. 

         1. En **Account selection method** (Método de selección de cuenta), seleccione **From Azure subscription** (Desde la suscripción de Azure) o **Enter manually** (Especificar manualmente).
         
         1. Si selecciona **From Azure subscription** (Desde la suscripción de Azure), escoja la **suscripción de Azure**, el **nombre de la cuenta de almacenamiento** y el **recurso compartido de archivos** correspondientes.
            
         1. Si selecciona **Enter manually** (Especificar manualmente), escriba `\\<storage account name>.file.core.windows.net\<file share name>` en **Host**, `Azure\<storage account name>` en **Username** (Nombre de usuario), y `<storage account key>` en **Password** (Contraseña), o bien seleccione su instancia de **Azure Key Vault** donde se almacena la contraseña como secreto.

      1. Si selecciona **Azure SQL Managed Instance** (Instancia administrada de Azure SQL), siga estos pasos: 

         1. Seleccione **Connection string** (Cadena de conexión) o elija la instancia de **Azure Key Vault** donde se almacena como secreto.
         
         1. En caso de seleccionar **Connection string** (Cadena de conexión), realice los siguientes pasos: 
             1. En **Account selection method** (Método de selección de cuenta), si elige **From Azure subscription** (Desde la suscripción de Azure), seleccione la **suscripción de Azure**, el **nombre de servidor**, el **tipo de punto de conexión** y el **nombre de base de datos** pertinentes. Si elige **Enter manually** (Indicar manualmente), siga los pasos que se indican a continuación. 
                1.  En **Fully qualified domain name** (Nombre de dominio completo), escriba `<server name>.<dns prefix>.database.windows.net` o `<server name>.public.<dns prefix>.database.windows.net,3342` como el punto de conexión público o privado (respectivamente) de Instancia administrada de Azure SQL. Si escribe el punto de conexión privado, no se puede usar la **prueba de conexión**, ya que la interfaz de usuario de ADF no puede acceder a ella.

                1. En **Database name** (Nombre de la base de datos), escriba `msdb`.
               
            1. En **Authentication type** (Tipo de autenticación), seleccione **SQL Authentication** (Autenticación de SQL), **Managed Identity** (Identidad administrada), **Service Principal** (Entidad de servicio) o **User-Assigned Managed Identity** (Identidad administrada asignada por el usuario).

                - Si selecciona **SQL Authentication** (Autenticación de SQL), escriba el **nombre de usuario** y la **contraseña** correspondientes, o bien seleccione la instancia de **Azure Key Vault** donde se almacena la contraseña como secreto.

                -  Si selecciona **Managed Identity** (Identidad administrada), conceda a la identidad administrada por el sistema para ADF acceso a la instancia de Azure SQL Managed Instance.

                - Si selecciona **Service Principal** (Entidad de servicio), escriba el **identificador de entidad de servicio** y la **clave de entidad de servicio** correspondientes, o bien seleccione la instancia de **Azure Key Vault** donde se almacena la clave como secreto.
                
                -  Si selecciona **User-Assigned Managed Identity** (Identidad administrada asignada por el usuario), conceda a la identidad administrada asignada por el usuario especificada para ADF acceso a la instancia de Azure SQL Managed Instance. A continuación puede seleccionar cualesquiera credenciales existentes creadas con las identidades administradas asignadas por el usuario especificadas o crear otras.

      1. Si selecciona **File system** (Sistema de archivos), escriba en **Host** la ruta de acceso UNC de la carpeta en la que se implementan los paquetes y especifique el **nombre de usuario** y la **contraseña** correspondientes, o bien seleccione la instancia de **Azure Key Vault** donde se almacena la contraseña como secreto.

      1. Seleccione **Test connection** (Prueba de conexión) cuando proceda y, si la prueba se realiza correctamente, seleccione **Create** (Crear).

   1. Los almacenes de paquetes agregados aparecerán en la página **Deployment settings** (Configuración de implementación). Para quitarlas, active sus casillas y, luego, seleccione **Eliminar**.

Seleccione **Test connection** (Probar conexión) cuando corresponda y, si es correcta, seleccione **Continue** (Continuar).

### <a name="advanced-settings-page"></a>Página Advanced settings (Configuración avanzada)

En la página **Advanced settings** (Configuración avanzada) del panel **Integration runtime setup** (Configuración de Integration Runtime), realice los pasos siguientes. 

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png" alt-text="Configuración avanzada":::

   1. En **Maximum Parallel Executions Per Node** (Número máximo de ejecuciones en paralelo por nodo), seleccione el número máximo de paquetes que se van a ejecutar simultáneamente por nodo en el clúster del entorno de ejecución de integración. Se muestran solo los números de paquetes admitidos. Seleccione un número bajo si quiere usar más de un núcleo para ejecutar un único paquete grande o pesado con un uso intensivo de memoria o proceso. Seleccione un número alto si quiere ejecutar uno o varios paquetes pequeños en un único núcleo. 

   1. Active la casilla **Customize your Azure-SSIS Integration Runtime with additional system configurations/component installations** (Personalizar Azure-SSIS Integration Runtime con configuraciones de sistema/instalación de componentes adicionales) para elegir si quiere agregar instalaciones personalizadas estándar/rápidas a Azure-SSIS IR. Para más información, consulte [Instalación personalizada de una instancia de Azure-SSIS IR](./how-to-configure-azure-ssis-ir-custom-setup.md).
   
   1. Seleccione la casilla **Select a VNet for your Azure-SSIS Integration Runtime to join, allow ADF to create certain network resources, and optionally bring your own static public IP addresses** (Seleccionar una red virtual para Azure-SSIS Integration Runtime para unirse, permitir que ADF cree determinados recursos de red y, opcionalmente traer sus direcciones IP públicas propias) para elegir si desea unir la instancia de Azure-SSIS IR a una red virtual.

      Seleccione esta casilla si usa un servidor de Azure SQL Database con reglas de firewall de IP/puntos de conexión de servicio de red virtual o una instancia administrada con punto de conexión privado para hospedar SSISDB, o si necesita acceder a datos locales sin configurar un IR autohospedado. Para más información, consulte [Creación de una instancia de Azure-SSIS IR en una red virtual](./create-azure-ssis-integration-runtime.md). 
   
   1. Active la casilla **Set up Self-Hosted Integration Runtime as a proxy for your Azure-SSIS Integration Runtime** (Configurar el entorno de ejecución de integración autohospedado como proxy para su instancia de Azure-SSIS IR) para elegir si quiere configurar IR autohospedado como proxy para su instancia de Azure-SSIS IR. Para más información, consulte [Configuración de un entorno de ejecución de integración autohospedado como proxy](./self-hosted-integration-runtime-proxy-ssis.md).   

   1. Seleccione **Continuar**. 

En la página **Summary** (Resumen) del panel **Integration runtime setup** (Configuración de Integration Runtime), revise todos los valores de aprovisionamiento, marque los vínculos a la documentación recomendada y seleccione **Create** (Crear) para empezar la creación del entorno de ejecución de integración. 

   > [!NOTE]
   > Aparte del tiempo de configuración personalizada, este proceso debería realizarse en 5 minutos.
   >
   > Si usa SSISDB, el servicio Data Factory se conectará al servidor de bases de datos para prepararlo. 
   > 
   > Al aprovisionar Azure-SSIS IR, también se instalan el acceso redistribuible y el paquete de característica de Azure para SSIS. Estos componentes proporcionan conectividad a archivos de Excel y Access, y a diversos orígenes de datos de Azure, además de a aquellos que ya admiten los componentes integrados. Para obtener más información sobre los componentes integrados o preinstalados, vea el artículo [Componentes integrados y preinstalados en Azure-SSIS Integration Runtime](./built-in-preinstalled-components-ssis-integration-runtime.md). Para obtener más información sobre los componentes adicionales que puede instalar, consulte [Personalización de la instalación en una instancia de Azure-SSIS Integration Runtime](./how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="connections-pane"></a>Panel Connections (Conexiones)

En el panel **Connections** (Conexiones) del **centro de administración**, cambie a la página **Integration Runtimes** (Entornos de ejecución de integración) y seleccione **Refresh** (Actualizar). 

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/connections-pane.png" alt-text="Panel Connections (Conexiones)":::

   Puede editar y volver a configurar su instancia de Azure-SSIS IR; para ello, seleccione su nombre. También puede seleccionar los botones pertinentes para supervisar, iniciar, detener o eliminar su instancia de Azure-SSIS IR; puede generar automáticamente una canalización de ADF con la actividad Ejecutar paquete de SSIS para que se ejecute en su instancia de Azure-SSIS IR; y puede ver el código o la carga JSON de Azure-SSIS IR.  Solo puede editar o eliminar su instancia de Azure-SSIS IR si está detenida.

## <a name="deploy-ssis-packages"></a>Implementación de paquetes de SSIS

Si usa SSISDB, puede implementar los paquetes en esta base de datos y ejecutarlos en Azure-SSIS IR mediante las herramientas de SSDT o de SSMS habilitadas para Azure. Estas herramientas se conectan al servidor de bases de datos mediante su punto de conexión de servidor: 

- En el caso de un servidor de Azure SQL Database, el formato del punto de conexión de servidor es `<server name>.database.windows.net`.
- En el caso de una instancia administrada con punto de conexión privado, el formato del punto de conexión de servidor es `<server name>.<dns prefix>.database.windows.net`.
- En el caso de una instancia administrada con punto de conexión público, el formato del punto de conexión de servidor es `<server name>.public.<dns prefix>.database.windows.net,3342`. 

Si no usa SSISDB, puede implementar los paquetes en sistemas de archivos, en Azure Files o en MSDB hospedados por Azure SQL Managed Instance y ejecutarlos en Azure-SSIS IR mediante las utilidades de la línea de comandos [dtutil](/sql/integration-services/dtutil-utility) y [AzureDTExec](./how-to-invoke-ssis-package-azure-enabled-dtexec.md). 

Para obtener más información, vea [Implementación de proyectos o paquetes en SSIS](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages).

En ambos casos, también puede ejecutar los paquetes implementados en Azure-SSIS IR mediante la actividad de ejecución de paquetes SSIS de las canalizaciones de Data Factory. Para más información, consulte [Invocación de la ejecución de paquetes SSIS como una actividad de Data Factory de primera clase](./how-to-invoke-ssis-package-ssis-activity.md).

Consulte también la documentación de SSIS siguiente: 

- [Implementación, ejecución y supervisión de paquetes de SSIS en Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Conexión a SSISDB en Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Programación de la ejecución de paquetes en Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Conexión a orígenes de datos locales con autenticación de Windows) 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de la personalización del entorno de ejecución de integración de SSIS de Azure, vaya al siguiente artículo: 

> [!div class="nextstepaction"]
> [Personalización de Azure-SSIS IR](./how-to-configure-azure-ssis-ir-custom-setup.md)