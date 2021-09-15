---
title: 'Integración e implementación continuas en dispositivos Azure IoT Edge (editor clásico): Azure IoT Edge'
description: 'Configuración de la integración e implementación continuas mediante el editor clásico: Azure IoT Edge con Azure DevOps, Azure Pipelines'
author: kgremban
ms.author: kgremban
ms.date: 08/26/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5af2216423e625d2ba9545a37e58bb08529ebd55
ms.sourcegitcommit: 03f0db2e8d91219cf88852c1e500ae86552d8249
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2021
ms.locfileid: "123032030"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge-devices-classic-editor"></a>Integración e implementación continuas en dispositivos Azure IoT Edge (editor clásico)

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Azure Pipelines incluye una tarea integrada de Azure IoT Edge que le ayuda a adoptar un enfoque DevOps con sus aplicaciones de Azure IoT Edge. En este artículo se muestra cómo usar las características de integración e implementación continuas de Azure Pipelines para crear, probar e implementar aplicaciones de forma rápida y eficaz en Azure IoT Edge mediante el editor clásico. Como alternativa, puede usar [YAML](how-to-continuous-integration-continuous-deployment.md).

![Diagrama: ramas CI y CD para desarrollo y producción](./media/how-to-continuous-integration-continuous-deployment-classic/model.png)

En este artículo, aprenderá a usar las [tareas integradas de Azure IoT Edge](/azure/devops/pipelines/tasks/build/azure-iot-edge) para Azure Pipelines para crear canalizaciones de compilación y versión para la solución de IoT Edge. Cada tarea de Azure IoT Edge que se agrega a la canalización implementa una de las cuatro acciones siguientes:

 | Acción | Descripción |
 | --- | --- |
 | Generar imágenes de módulo | Usa el código de su solución de IoT Edge y compila las imágenes de contenedor.|
 | Insertar imágenes de módulo | Inserta las imágenes del módulo en el registro de contenedor especificado. |
 | Generar el manifiesto de implementación | Usa el archivo deployment.template.json y sus variables para generar el archivo final de manifiesto de implementación de IoT Edge. |
 | Implementación en dispositivos de IoT Edge | Crea implementaciones de IoT Edge en uno o varios dispositivos IoT Edge. |

A menos que se especifique lo contrario, los procedimientos descritos en este artículo no exploran toda la funcionalidad disponible mediante parámetros de tarea. Para obtener más información, vea los siguientes recursos:

* [Versión de tarea](/azure/devops/pipelines/process/tasks?tabs=classic#task-versions)
* **Avanzada**: si se aplica, especifique los módulos que no quiera compilar.
* [Opciones de control](/azure/devops/pipelines/process/tasks?tabs=classic#task-control-options)
* [Variables de entorno](/azure/devops/pipelines/process/variables?tabs=classic#environment-variables)
* [Variables de salida](/azure/devops/pipelines/process/variables?tabs=classic#use-output-variables-from-tasks)

## <a name="prerequisites"></a>Requisitos previos

* Un repositorio de Azure Repos. Si no tiene uno, puede [crear un nuevo repositorio de Git en el proyecto](/azure/devops/repos/git/create-new-repo). En este artículo, hemos creado un repositorio denominado **IoTEdgeRepo**.
* Una solución de IoT Edge confirmada e insertada en el repositorio. Si quiere crear una nueva solución de ejemplo para hacer pruebas según este artículo, siga los pasos para [desarrollar y depurar módulos en Visual Studio Code](how-to-vs-code-develop-module.md) o [desarrollar y depurar módulos de C# en Visual Studio](./how-to-visual-studio-develop-module.md). Para este artículo, hemos creado una solución en el repositorio denominada **IoTEdgeSolution**, que tiene el código de un módulo denominado **filtermodule**.

   En este artículo, todo lo que necesita es la carpeta de la solución creada mediante las plantillas de IoT Edge en Visual Studio Code o Visual Studio. No es necesario compilar, insertar, implementar ni depurar este código antes de continuar. Configurará esos procesos en Azure Pipelines.

  Conozca la ruta de acceso al archivo **deployment.template.js** de la solución, ya que se usa en varios pasos. Si no está familiarizado con el rol de la plantilla de implementación, consulte [Aprenda a implementar módulos y establecer rutas](module-composition.md).

  >[!TIP]
  >Si está creando una nueva solución, clone el repositorio localmente en primer lugar. A continuación, cuando cree la solución, puede elegir crearlo directamente en la carpeta del repositorio. Puede confirmar e insertar fácilmente los nuevos archivos desde allí.

* Un registro de contenedor donde pueda insertar imágenes del módulo. Puede usar [Azure Container Registry](../container-registry/index.yml) o un registro de terceros.
* Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) de Azure activo con al menos dos dispositivos de IoT Edge para hacer pruebas de las fases independientes de implementación de prueba y producción. Puede seguir los artículos de la guía de inicio rápido para crear un dispositivo IoT Edge en [Linux](quickstart-linux.md) o [Windows](quickstart.md)

## <a name="create-a-build-pipeline-for-continuous-integration"></a>Crear una canalización de compilación para la integración continua

En esta sección, creará una nueva canalización de compilación. Configurará la canalización para que se ejecute automáticamente y publique registros de compilación cuando registre cambios en la solución IoT Edge.

1. Inicie sesión en la organización de Azure DevOps (`https://dev.azure.com/{your organization}`) y abra el proyecto que contiene el repositorio de la solución de IoT Edge.

    ![Abrir el proyecto de DevOps](./media/how-to-continuous-integration-continuous-deployment-classic/initial-project.png)

2. En el menú del panel izquierdo del proyecto, seleccione **Canalizaciones**. Seleccione **Crear canalización** en el centro de la página. O bien, si ya tiene canalizaciones de compilación, seleccione el botón **Nueva canalización** en la parte superior derecha.

    ![Creación de una canalización de compilación](./media/how-to-continuous-integration-continuous-deployment-classic/add-new-pipeline.png)

3. En la parte inferior de la página **¿Dónde está el código?** , seleccione **Usar el editor clásico**. Si quiere usar YAML para crear las canalizaciones de compilación del proyecto, consulte la [guía de YAML](how-to-continuous-integration-continuous-deployment.md).

    ![Seleccionar Usar el editor clásico](./media/how-to-continuous-integration-continuous-deployment-classic/create-without-yaml.png)

4. Siga las indicaciones para crear la canalización.

   1. Proporcione la información de origen para la nueva canalización de compilación. Seleccione **GIT de Azure Repos** como origen y, luego, seleccione el proyecto, el repositorio y la rama donde está ubicado el código de la solución de IoT Edge. Después, seleccione **Continuar**.

      ![Seleccione el origen de la canalización](./media/how-to-continuous-integration-continuous-deployment-classic/pipeline-source.png)

   2. Seleccione **Fase vacía** en lugar de una plantilla.

      ![Comenzar con un trabajo vacío para la canalización de compilación](./media/how-to-continuous-integration-continuous-deployment-classic/start-with-empty-build-job.png)

5. Una vez creada la canalización, irá al editor de canalizaciones. Aquí puede cambiar el nombre de la canalización, el grupo de agentes y la especificación del agente.

   Puede seleccionar un grupo de hosts de Microsoft o un grupo autohospedado que administre.

   En la descripción de la canalización, elija la especificación del agente correcta a partir de la plataforma de destino:

   * Si quiere compilar los módulos en la plataforma amd64 para contenedores Linux, elija **ubuntu-18.04**.

   * Si quiere compilar los módulos en la plataforma amd64 para contenedores de Windows 1809, deberá [configurar un agente autohospedado en Windows](/azure/devops/pipelines/agents/v2-windows).

   * Si quiere compilar los módulos en la plataforma arm32v7 o arm64 para contenedores de Linux, deberá [configurar un agente autohospedado en Linux](https://devblogs.microsoft.com/iotdev/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent).

   :::image type="content" source="./media/how-to-continuous-integration-continuous-deployment-classic/configure-env.png" alt-text="Configuración de la especificación del agente de compilación":::

6. La canalización viene preconfigurada con un trabajo denominado **Trabajo del agente 1**. Seleccione el signo de más ( **+** ) para agregar cuatro tareas al trabajo: **Azure IoT Edge** dos veces, **Copiar archivos** una vez y **Publicar los artefactos de la compilación** una vez. Busque cada tarea y mantenga el mouse sobre el nombre de la tarea para ver el botón **Agregar**.

   :::image type="content" source="./media/how-to-continuous-integration-continuous-deployment-classic/add-iot-edge-task.png" alt-text="Incorporación de la tarea Azure IoT Edge":::

   Cuando se hayan agregado las cuatro tareas, el Trabajo del agente se verá similar al ejemplo siguiente:

   :::image type="content" source="./media/how-to-continuous-integration-continuous-deployment-classic/add-tasks.png" alt-text="Cuatro tareas en la canalización de compilación":::

7. Seleccione la primera tarea **Azure IoT Edge** para editarla. Esta tarea compila todos los módulos de la solución con la plataforma de destino que se especificó. Edite la tarea con los valores siguientes:

   | Parámetro | Descripción |
   | --- | --- |
   | Nombre para mostrar | El nombre para mostrar se actualiza automáticamente cuando cambia el campo Acción. |
   | Acción | Seleccione **Generar imágenes de módulo**. |
   | archivo .template.json | Seleccione los puntos suspensivos (**...**) y navegue hasta el archivo **deployment.template.json** en el repositorio que contiene la solución de IoT Edge. |
   | Plataforma predeterminada | Seleccione el sistema operativo adecuado para los módulos en función de su dispositivo IoT Edge de destino. |
   | Variables de salida | Proporcione un nombre de referencia para asociarlo a la ruta de acceso del archivo donde se genera el archivo deployment.json, como **edge**. |

   Para más información sobre esta tarea y sus parámetros, consulte [Tarea de Azure IoT Edge](/azure/devops/pipelines/tasks/build/azure-iot-edge).

   Estas configuraciones usan el repositorio de imágenes y la etiqueta que se definen en el archivo `module.json` para asignar un nombre a la imagen del módulo y etiquetarla. **Compilar imágenes del módulo** también ayuda a reemplazar las variables por el valor exacto que defina en el archivo `module.json`. En Visual Studio o Visual Studio Code, el valor real se especifica en un archivo `.env`. En Azure Pipelines, el valor se establece en la pestaña **Variables de canalización**. Seleccione la pestaña **Variables** en el menú del editor de la canalización y configure el nombre y el valor de la siguiente manera:

   * **ACR_ADDRESS**: Valor del **servidor de inicio de sesión** de Azure Container Registry. El valor del servidor de inicio de sesión está disponible en la página de información general del registro de contenedor en Azure Portal.

   Si tiene otras variables en el proyecto, puede especificar el nombre y el valor en esta pestaña. **Build module images** (Compilar imágenes del módulo) solo reconoce las variables que están en formato `${VARIABLE}`. Asegúrese de usar este formato en los archivos de `**/module.json`.

8. Seleccione la segunda tarea **Azure IoT Edge** para editarla. Esta tarea insertará todas las imágenes del módulo en el registro de contenedor que seleccionó.

   | Parámetro | Descripción |
   | --- | --- |
   | Nombre para mostrar | El nombre para mostrar se actualiza automáticamente cuando cambia el campo Acción. |
   | Acción | Seleccione **Insertar imágenes de módulo**. |
   | Tipo de registro de contenedor | Use el tipo predeterminado: `Azure Container Registry`. |
   | Suscripción de Azure | Elija su suscripción. |
   | Azure Container Registry | Seleccione el tipo de registro de contenedor que usa para almacenar las imágenes del módulo. Dependiendo del tipo de registro que elija, el formulario cambia. Si elige **Azure Container Registry**, use las listas desplegables para seleccionar la suscripción de Azure y el nombre del registro de contenedor. Si elige **Generic Container Registry** (Registro de contenedor genérico), seleccione **Nuevo** para crear una conexión del servicio del registro. |
   | archivo .template.json | Seleccione los puntos suspensivos (**...**) y navegue hasta el archivo **deployment.template.json** en el repositorio que contiene la solución de IoT Edge. |
   | Plataforma predeterminada | Seleccione el sistema operativo adecuado para los módulos en función de su dispositivo IoT Edge de destino. |
   | Agregar credenciales del registro al manifiesto de implementación | Especifique true para agregar las credenciales del registro para insertar imágenes de Docker en el manifiesto de implementación. |

   Para más información sobre esta tarea y sus parámetros, consulte [Tarea de Azure IoT Edge](/azure/devops/pipelines/tasks/build/azure-iot-edge).

   Si tiene varias instancias de Container Registry para hospedar las imágenes del módulo, tiene que duplicar esta tarea, seleccionar otra instancia de Container Registry y usar **Omitir módulos** en la configuración **Avanzada** para omitir las imágenes que no son de este registro específico.

9. Seleccione la tarea **Copiar archivos** para editarla. Use esta tarea para copiar archivos en el directorio de almacenamiento provisional de artefactos.

   | Parámetro | Descripción |
   | --- | --- |
   | Nombre para mostrar | Usar el nombre predeterminado o personalizar |
   | Carpeta de origen | Carpeta con los archivos que se van a copiar. |
   | Contenido | Agregue dos líneas: `deployment.template.json` y `**/module.json`. Estos dos archivos sirven de entrada para generar el manifiesto de implementación de IoT Edge. |
   | Carpeta de destino | Especifique la variable `$(Build.ArtifactStagingDirectory)`. Consulte las [variables de compilación](/azure/devops/pipelines/build/variables#build-variables) para obtener información acerca de sus descripciones. |

   Para más información sobre esta tarea y sus parámetros, consulte [Tarea de copia de archivos](/azure/devops/pipelines/tasks/utility/copy-files?tabs=classic).

10. Seleccione la tarea **Publicar los artefactos de la compilación** para editarla. Proporcione la ruta de acceso del directorio de almacenamiento provisional de artefactos a la tarea para que la ruta de acceso pueda publicarse en la canalización de versión.

    | Parámetro | Descripción |
    | --- | --- |
    | Nombre para mostrar | Use el nombre predeterminado o personalícelo. |
    | Ruta de acceso para publicar | Especifique la variable `$(Build.ArtifactStagingDirectory)`. Consulte [Variables de compilación](/azure/devops/pipelines/build/variables#build-variables) para obtener más información. |
    | Nombre del artefacto | Use el nombre predeterminado: **drop** |
    | Ubicación de publicación de artefactos | Use la ubicación predeterminada: **Azure Pipelines** |

    Para más información sobre esta tarea y sus parámetros, consulte [Publicar artefactos de compilación](/azure/devops/pipelines/tasks/utility/publish-build-artifacts).

11. Abra la pestaña **Desencadenadores** y marque la casilla para **Habilitar la integración continua**. Asegúrese de que se incluye la rama que contiene el código.

    ![Activación del desencadenador de integración continua](./media/how-to-continuous-integration-continuous-deployment-classic/configure-trigger.png)

12. Seleccione **Guardar** en el menú desplegable **Save & queue** (Guardar y poner en cola).

Esta canalización ahora está configurada para ejecutarse automáticamente al insertar código nuevo en el repositorio. La última tarea, la publicación de los artefactos de la canalización, desencadena una canalización de versión. Continúe con la sección siguiente para compilar la canalización de versión.

[!INCLUDE [iot-edge-create-release-pipeline-for-continuous-deployment](../../includes/iot-edge-create-release-pipeline-for-continuous-deployment.md)]

>[!NOTE]
>Las tareas de Azure IoT Edge en Azure DevOps aún no admiten las implementaciones superpuestas.
>
>Sin embargo, puede usar una [tarea de la CLI de Azure en Azure DevOps](/azure/devops/pipelines/tasks/deploy/azure-cli) para crear la implementación por capas. Para el valor **Script en línea**, puede usar el [comando az iot edge deployment create](/cli/azure/iot/edge/deployment):
>
>```azurecli-interactive
>az iot edge deployment create -d {deployment_name} -n {hub_name} --content modules_content.json --layered true
>```

[!INCLUDE [iot-edge-verify-iot-edge-continuous-integration-continuous-deployment](../../includes/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment.md)]

## <a name="next-steps"></a>Pasos siguientes

* Ejemplo de DevOps en IoT Edge en [Azure DevOps Starter para IoT Edge](how-to-devops-starter.md)
* Puede encontrar información sobre la implementación de IoT Edge en [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).
* Siga los pasos para crear, actualizar o eliminar una implementación en [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-at-scale.md).
