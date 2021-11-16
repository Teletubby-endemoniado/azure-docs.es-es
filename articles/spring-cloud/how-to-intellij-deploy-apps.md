---
title: 'Tutorial: Implementación de aplicaciones Spring Boot mediante IntelliJ'
description: Use IntelliJ para implementar aplicaciones en Azure Spring Cloud.
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/03/2021
ms.custom: devx-track-java
ms.openlocfilehash: 0db8cb4393f07b4a7ab8f9f24d1c635aaffcd133
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132493730"
---
# <a name="deploy-spring-boot-applications-using-intellij"></a>Implementación de aplicaciones Spring Boot mediante IntelliJ


**Este artículo se aplica a:** ✔️ Java

El complemento IntelliJ para Azure Spring Cloud admite la implementación de aplicaciones desde IntelliJ IDEA.

Antes de ejecutar este ejemplo, puede probar la [guía de inicio rápido básica](./quickstart.md).

## <a name="prerequisites"></a>Requisitos previos

* [IntelliJ IDEA, Community/Ultimate Edition, versión 2020.1/2020.2](https://www.jetbrains.com/idea/download/#section=windows)

## <a name="install-the-plug-in"></a>Instalación del complemento

Puede agregar Azure Toolkit for IntelliJ IDEA 3.51.0 desde la opción **Plugins** (Complementos) de la interfaz de usuario de IntelliJ.

1. Inicie IntelliJ.  Si anteriormente ha abierto un proyecto, ciérrelo para mostrar el cuadro de diálogo de bienvenida. Seleccione **Configure** (Configurar) en el vínculo inferior derecho y, luego, seleccione **Plugins** (Complementos) para abrir el cuadro de diálogo de configuración de complementos. Elija **Install Plugins from disk** (Instalar complementos desde el disco).

    ![Seleccionar Configure (Configurar)](media/spring-cloud-intellij-howto/configure-plugin-1.png)

1. Búsqueda de Azure Toolkit for IntelliJ. Haga clic en **Instalar**.

    ![Instalación del complemento](media/spring-cloud-intellij-howto/install-plugin.png)

1. Seleccione **Restart IDE** (Reiniciar IDE).

## <a name="tutorial-procedures"></a>Procedimientos del tutorial

Los procedimientos siguientes implementan una aplicación Hola mundo con IntelliJ IDEA.

* Apertura del proyecto gs-spring-boot
* Implementación en Azure Spring Cloud
* Presentación de los registros de streaming

## <a name="open-gs-spring-boot-project"></a>Apertura del proyecto gs-spring-boot

1. Descargue y descomprima el repositorio de origen de este tutorial o clónelo con el siguiente comando de GIT: `git clone https://github.com/spring-guides/gs-spring-boot.git`.
1. Vaya a la carpeta *gs-spring-boot\complete*.
1. Abra el cuadro de diálogo de **bienvenida** de IntelliJ y seleccione **Import Project** (Importar proyecto) para abrir el asistente de importación.
1. Seleccione la carpeta *gs-spring-boot\complete*.

    ![Importar proyecto](media/spring-cloud-intellij-howto/import-project-1.png)

## <a name="deploy-to-azure-spring-cloud"></a>Implementación en Azure Spring Cloud

Para realizar la implementación en Azure, debe iniciar sesión con su cuenta de Azure y elegir su suscripción.  Puede encontrar información sobre el inicio de sesión en [Instalación e inicio de sesión](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in).

1. Vaya al explorador de proyectos de IntelliJ, haga clic con el botón derecho en el proyecto y seleccione **Azure** -> **Deploy to Azure Spring Cloud** (Implementar en Azure Spring Cloud).

    ![Implementación en Azure 1](media/spring-cloud-intellij-howto/deploy-to-azure-1.png)

1. Acepte el nombre de la aplicación en el campo **Name** (Nombre). El **nombre** hace referencia a la configuración, no al nombre de la aplicación. Normalmente, los usuarios no tienen que cambiarlo.
1. En **Artifact**(Artefacto), acepte el identificador del proyecto.
1. Seleccione **App:** (Aplicación:) y, luego, **Create app...** (Crear aplicación...).

    ![Implementación en Azure 2](media/spring-cloud-intellij-howto/deploy-to-azure-2.png)

1. Rellene el campo **App name** (Nombre de la aplicación) y, luego, seleccione **OK** (Aceptar).

    ![Implementación en Azure: OK (Aceptar)](media/spring-cloud-intellij-howto/deploy-to-azure-2a.png)

1. Para iniciar la implementación, seleccione el botón **Run** (Ejecutar).

    ![Implementación en Azure 3](media/spring-cloud-intellij-howto/deploy-to-azure-3.png)

1. El complemento ejecutará el comando `mvn package` en el proyecto y, luego, creará la aplicación e implementará el archivo JAR generado por el comando `package`.

1. Si la dirección URL de la aplicación no se muestra en la ventana de salida, puede obtenerla en Azure Portal. Desplácese desde el grupo de recursos hasta la instancia de Azure Spring Cloud.  Luego, seleccione **Aplicaciones**.  Se mostrará la aplicación en ejecución. Seleccione la aplicación y copie la **Dirección URL** o el **Punto de conexión de prueba**.

    ![Obtención de la dirección URL de prueba](media/spring-cloud-intellij-howto/get-test-url.png)

1. Vaya a la dirección URL o al punto de conexión de prueba en el explorador.

    ![Navegar en el explorador 2](media/spring-cloud-intellij-howto/navigate-in-browser-2.png)

## <a name="show-streaming-logs"></a>Presentación de los registros de streaming

Para obtener los registros:

1. Seleccione **Azure Explorer** (Explorador de Azure) y, luego, **Spring Cloud**.
1. Haga clic con el botón derecho en la aplicación en ejecución.
1. Seleccione **Streaming Logs** (Registros de streaming) en la lista desplegable.

    ![Selección de los registros de streaming](media/spring-cloud-intellij-howto/streaming-logs.png)

1. Seleccione la instancia.

    ![Selección de la instancia](media/spring-cloud-intellij-howto/select-instance.png)

1. El registro de streaming se verá en la ventana de salida.

    ![Salida del registro de streaming](media/spring-cloud-intellij-howto/streaming-log-output.png)

## <a name="next-steps"></a>Pasos siguientes

* [Preparación de la aplicación Spring para Azure Spring Cloud](how-to-prepare-app-deployment.md)
* [Más información sobre Azure Toolkit for IntelliJ](/azure/developer/java/toolkit-for-intellij/)
