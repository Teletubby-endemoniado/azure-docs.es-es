---
title: Inicio rápido - Creación de flujos de trabajo de integración con Azure Logic Apps en Visual Studio Code
description: Cree y administre definiciones de flujo de trabajo con Azure Logic Apps multiinquilino en Visual Studio Code.
services: logic-apps
ms.suite: integration
ms.reviewer: azla
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/25/2021
ms.openlocfilehash: 15f3481e42c543bead8ccf5f01654912cce5085f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131078434"
---
# <a name="quickstart-create-and-manage-logic-app-workflow-definitions-with-multi-tenant-azure-logic-apps-and-visual-studio-code"></a>Inicio rápido - Cree y administre definiciones de flujo de trabajo de aplicación lógica con Azure Logic Apps multiinquilino y Visual Studio Code

En este inicio rápido se muestra cómo crear y administrar flujos de trabajo de aplicación lógica que le ayudan a automatizar tareas y procesos que integran aplicaciones, datos, sistemas y servicios en organizaciones y empresas mediante el uso de [Azure Logic Apps](../logic-apps/logic-apps-overview.md) multiinquilino y Visual Studio Code. Puede crear y editar las definiciones de los flujos de trabajo subyacentes, que usan la notación de objetos JavaScript (JSON), para aplicaciones lógicas mediante código. También puede trabajar en aplicaciones lógicas existentes ya implementadas en Azure. Para más información sobre el modelo multiinquilino frente al modelo de inquilino único, consulte [Inquilino único (versión preliminar) frente a multiinquilino y entorno del servicio de integración para Azure Logic Apps](single-tenant-overview-compare.md).

Aunque puede realizar estas mismas tareas en [Azure Portal](https://portal.azure.com) y en Visual Studio, puede empezar a trabajar con mayor rapidez en Visual Studio Code si ya tiene familiaridad con la aplicación lógica y desea trabajar directamente en el código. Por ejemplo, puede deshabilitar, habilitar, eliminar y actualizar las aplicaciones lógicas ya creadas. Además, puede trabajar en Logic Apps y las cuentas de integración desde cualquier plataforma de desarrollo donde se ejecute Visual Studio Code, como Linux, Windows y Mac.

En este artículo, puede crear la misma aplicación lógica desde este [inicio rápido](../logic-apps/quickstart-create-first-logic-app-workflow.md), que se centra más en los conceptos básicos. También puede [aprender a crear la aplicación de ejemplo en Visual Studio](quickstart-create-logic-apps-with-visual-studio.md) y [aprender a crear y administrar aplicaciones mediante la interfaz de la línea de comandos de Azure (CLI de Azure)](quickstart-logic-apps-azure-cli.md). En Visual Studio Code, la aplicación lógica se parece a este ejemplo:

![Definición del flujo de trabajo de la aplicación lógica de ejemplo](./media/quickstart-create-logic-apps-visual-studio-code/visual-studio-code-overview.png)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que dispone de estos elementos:

* Si no tiene una cuenta de Azure y una suscripción, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Conocimientos básicos acerca de las [definiciones de flujos de trabajo de aplicaciones lógicas](../logic-apps/logic-apps-workflow-definition-language.md) y su estructura, que se describe con JSON

  Si nunca ha usado Logic Apps, pruebe este [inicio rápido](../logic-apps/quickstart-create-first-logic-app-workflow.md), que crea las primeras aplicaciones lógicas en Azure Portal y se centra más en los conceptos básicos.

* Acceso a la web para iniciar sesión en Azure y en su suscripción

* Descargue e instale estas herramientas si no las tiene aún:

  * [Visual Studio Code versión 1.25.1 o posterior](https://code.visualstudio.com/), que es gratis

  * La extensión de Visual Studio Code para Azure Logic Apps

    Puede descargar e instalar esta extensión desde [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) o directamente desde Visual Studio Code. Asegúrese de volver a cargar Visual Studio Code después de la instalación.

    ![Buscar "Extensión de Visual Studio Code para Azure Logic Apps"](./media/quickstart-create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Para comprobar que la extensión se ha instalado correctamente, seleccione el icono de Azure que aparecer en la barra de herramientas de Visual Studio Code.

    ![Confirmar que la extensión se ha instalado correctamente](./media/quickstart-create-logic-apps-visual-studio-code/confirm-installed-visual-studio-code-extension.png)

    Para más información, consulte [Extension Marketplace](https://code.visualstudio.com/docs/editor/extension-gallery) (Marketplace de extensiones). Para contribuir a la versión de código abierto de esta extensión; vaya a [Extensión de Azure Logic Apps para Visual Studio Code en GitHub](https://github.com/Microsoft/vscode-azurelogicapps).

* Si la aplicación lógica necesita comunicarse a través de un firewall que limite el tráfico a direcciones IP específicas, ese firewall debe permitir el acceso para *ambas* direcciones IP, la [de entrada](logic-apps-limits-and-config.md#inbound) y la [de salida](logic-apps-limits-and-config.md#outbound), utilizadas por el servicio Logic Apps o por el entorno de ejecución en la región de Azure donde se encuentra la aplicación lógica. Si la aplicación lógica también usa [conectores administrados](../connectors/managed.md), como el conector de Outlook de Office 365 o el conector de SQL, o emplea [conectores personalizados](/connectors/custom-connectors/), el firewall también debe permitir el acceso a *todas* las [direcciones IP de salida del conector administrado](logic-apps-limits-and-config.md#outbound) en la región de Azure de la aplicación lógica.

<a name="access-azure"></a>

## <a name="access-azure-from-visual-studio-code"></a>Acceso a Azure desde Visual Studio Code

1. Abra Visual Studio Code. En la barra de herramientas de Visual Studio Code, seleccione el icono de Azure.

   ![Seleccione el icono de Azure en la barra de herramientas de Visual Studio Code](./media/quickstart-create-logic-apps-visual-studio-code/open-extensions-visual-studio-code.png)

1. En la ventana de Azure, en **Logic Apps**, seleccione **Iniciar sesión en Azure**. Cuando la página de inicio de sesión de Microsoft se lo solicite, inicie sesión con su cuenta de Azure.

   ![Seleccionar "Iniciar sesión en Azure"](./media/quickstart-create-logic-apps-visual-studio-code/sign-in-azure-visual-studio-code.png)

   1. Si el inicio de sesión tarda más de lo habitual, Visual Studio Code le pedirá que inicie sesión a través de un sitio web de autenticación de Microsoft. Para ello, le proporcionará un código de dispositivo. Para iniciar sesión con el código en su lugar, seleccione **Use Device Code** (Usar el código de dispositivo).

      ![Continuar con el código de dispositivo](./media/quickstart-create-logic-apps-visual-studio-code/use-device-code-prompt.png)

   1. Para copiar el código, seleccione **Copy & Open** (Copiar y abrir).

      ![Copiar el código para el inicio de sesión en Azure](./media/quickstart-create-logic-apps-visual-studio-code/sign-in-prompt-authentication.png)

   1. Para abrir una nueva ventana del explorador y continuar con el sitio web de autenticación, seleccione **Abrir vínculo**.

      ![Confirmar la apertura de un explorador e ir al sitio web de autenticación.](./media/quickstart-create-logic-apps-visual-studio-code/confirm-open-link.png)

   1. En la página **Iniciar sesión en tu cuenta**, escriba el código de autenticación y seleccione **Siguiente**.

      ![Escribir el código de autenticación para el inicio de sesión en Azure](./media/quickstart-create-logic-apps-visual-studio-code/authentication-code-azure-sign-in.png)

1. Seleccione su cuenta de Azure. Después de iniciar sesión, puede cerrar el explorador y volver a Visual Studio Code.

   En el panel de Azure, las secciones **Logic Apps** y **Cuentas de integración** muestran las suscripciones a Azure asociadas con su cuenta. Sin embargo, si no ve las suscripciones que espera, o si las secciones muestran demasiadas suscripciones, siga estos pasos:

   1. Mueva el puntero sobre la etiqueta **Logic Apps**. Cuando aparezca la barra de herramientas, seleccione **Seleccionar suscripciones** (icono de filtro).

      ![Buscar o filtrar suscripciones a Azure](./media/quickstart-create-logic-apps-visual-studio-code/find-or-filter-subscriptions.png)

   1. En la lista que aparece, seleccione las suscripciones que quiere que aparezcan.

1. En **Logic Apps**, seleccione la suscripción que quiera. El nodo de la suscripción se expande y muestra las aplicaciones lógicas que existen en esa suscripción.

   ![Selección de su suscripción a Azure](./media/quickstart-create-logic-apps-visual-studio-code/select-azure-subscription.png)

   > [!TIP]
   > En **Cuentas de integración**, al seleccionar su suscripción, se muestran todas las cuentas de integración que existen en esa suscripción.

<a name="create-logic-app"></a>

## <a name="create-new-logic-app"></a>Creación de una nueva aplicación lógica

1. Si aún no ha iniciado sesión en su cuenta y suscripción de Azure desde dentro de Visual Studio Code, siga los [pasos anteriores para iniciar sesión ahora](#access-azure).

1. En Visual Studio Code, en **Logic Apps**, abra el menú contextual de la suscripción y seleccione **Crear aplicación lógica**.

   ![Desde el menú de la suscripción, seleccione "Crear aplicación lógica"](./media/quickstart-create-logic-apps-visual-studio-code/create-logic-app-visual-studio-code.png)

   Aparece una lista que muestra los grupos de recursos de Azure de su suscripción.

1. En la lista del grupo de recursos, seleccione **Crear un nuevo grupo de recursos** o un grupo de recursos existente. Para este ejemplo, cree un nuevo grupo de recursos:

   ![Crear un nuevo grupo de recursos de Azure](./media/quickstart-create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Proporcione un nombre para el grupo de recursos de Azure y presione ENTRAR.

   ![Especificar un nombre para el grupo de recursos de Azure](./media/quickstart-create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Seleccione la región de Azure en la que quiera guardar los metadatos de la aplicación lógica.

   ![Seleccionar la ubicación de Azure para guardar los metadatos de la aplicación lógica](./media/quickstart-create-logic-apps-visual-studio-code/select-azure-location-new-resources.png)

1. Proporcione un nombre para la aplicación lógica y presione Entrar.

   ![Especificar el nombre de la aplicación lógica](./media/quickstart-create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   En la ventana de Azure, en su suscripción, se mostrará la nueva aplicación lógica en blanco. Visual Studio Code también abre un archivo JSON (.logicapp.json), que incluye una definición de flujo de trabajo de esqueleto para la aplicación lógica. Ahora puede empezar a crear manualmente la definición de flujo de trabajo de la aplicación lógica en este archivo JSON. Para obtener información técnica sobre la estructura y la sintaxis de una definición de flujo de trabajo, consulte la [Guía de referencia del esquema del lenguaje de definición de flujo de trabajo en Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).

   ![Archivo JSON de definición del flujo de trabajo de la aplicación lógica vacía](./media/quickstart-create-logic-apps-visual-studio-code/empty-logic-app-workflow-definition.png)

   Por ejemplo, a continuación se muestra una definición de flujo de trabajo de aplicación lógica de muestra que comienza con un desencadenador RSS y una acción de Office 365 Outlook. Normalmente, los elementos JSON aparecen en orden alfabético en cada sección. Sin embargo, en este ejemplo se muestran aproximadamente en el orden en que aparecen los pasos de la aplicación lógica en el diseñador.

   > [!IMPORTANT]
   > Si quiere volver a usar esta definición de aplicación lógica de muestra, necesita una cuenta profesional, como por ejemplo, @fabrikam.com. Asegúrese de reemplazar la dirección de correo electrónico ficticia por su propia dirección de correo electrónico. Para usar un conector de correo electrónico diferente, como Outlook.com o Gmail, reemplace la acción `Send_an_email_action` por una acción similar disponible en un [conector de correo electrónico compatible con Azure Logic Apps](../connectors/apis-list.md).
   >
   > Si quiere usar el conector de Gmail, solo las cuentas empresariales de G-Suite pueden usar este conector sin restricciones en las aplicaciones lógicas. 
   > Si tiene una cuenta de consumidor de Gmail, puede usar este conector solo con servicios específicos aprobados por Google, o puede [crear una aplicación cliente de Google para usarla en la autenticación con el conector de Gmail](/connectors/gmail/#authentication-and-bring-your-own-application). 
   > Para más información, consulte [Directivas de privacidad y seguridad de datos de los conectores de Google en Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email_(V2)": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "<p>Title: @{triggerBody()?['title']}<br>\n<br>\nDate published: @{triggerBody()?['updatedOn']}<br>\n<br>\nLink: @{triggerBody()?['primaryLink']}</p>",
                  "Subject": "RSS item: @{triggerBody()?['title']}",
                  "To": "sophia-owen@fabrikam.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/v2/Mail"
            }
         }
      },
      "outputs": {}
   }
   ```

1. Cuando haya terminado, guarde la definición de flujo de trabajo de la aplicación lógica. (Menú Archivo > Guardar o presione Ctrl + S)

1. Cuando se le pida que cargue la aplicación lógica en su suscripción a Azure, seleccione **Cargar**.

   Este paso publica la aplicación lógica en [Azure Portal](https://portal.azure.com) y hace que la lógica se ejecute en Azure.

   ![Cargar la aplicación lógica nueva en la suscripción de Azure](./media/quickstart-create-logic-apps-visual-studio-code/upload-new-logic-app.png)

## <a name="view-logic-app-in-designer"></a>Ver la aplicación lógica en el diseñador

En Visual Studio Code, puede abrir la aplicación lógica en una vista de diseño de solo lectura. Aunque no puede editar la aplicación lógica en el diseñador, puede comprobar visualmente el flujo de trabajo de la aplicación lógica mediante la vista del diseñador.

En la ventana de Azure, en **Logic Apps**, abra el menú contextual de la aplicación lógica y seleccione **Abrir en el diseñador**.

El diseñador de solo lectura se abre en una ventana independiente y muestra el flujo de trabajo de la aplicación lógica, por ejemplo:

![Ver la aplicación lógica en el diseñador de solo lectura](./media/quickstart-create-logic-apps-visual-studio-code/logic-app-designer-view.png)

## <a name="view-in-azure-portal"></a>Ver en Azure Portal

Para revisar la aplicación lógica en Azure Portal, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con la misma cuenta de Azure y la misma suscripción asociadas con la aplicación lógica.

1. En el cuadro de búsqueda de Azure Portal, escriba el nombre de la aplicación lógica. En la lista de resultados, seleccione la aplicación lógica.

   ![La nueva aplicación lógica en Azure Portal](./media/quickstart-create-logic-apps-visual-studio-code/published-logic-app-in-azure.png)

<a name="edit-logic-app"></a>

## <a name="edit-deployed-logic-app"></a>Edición de la aplicación lógica implementada

En Visual Studio Code, puede abrir y editar la definición de flujo de trabajo de una aplicación lógica ya implementada en Azure.

> [!IMPORTANT] 
> Antes de editar una aplicación lógica que se ejecuta activamente en producción, evite el riesgo de interrumpir esa aplicación lógica y minimice la interrupción mediante la [deshabilitación de la aplicación lógica en primer lugar](#disable-enable-logic-apps).

1. Si aún no ha iniciado sesión en su cuenta y suscripción de Azure desde dentro de Visual Studio Code, siga los [pasos anteriores para iniciar sesión ahora](#access-azure).

1. En la ventana de Azure, en **Logic Apps**, expanda su suscripción de Azure y seleccione la aplicación lógica que quiera.

1. Abra el menú de la aplicación lógica y seleccione **Abrir en el** . O bien, junto al nombre de la aplicación lógica, seleccione el icono de edición.

   ![Abrir el editor para la aplicación lógica existente](./media/quickstart-create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code abre el archivo .logicapp.json en una carpeta temporal local para que pueda ver la definición del flujo de trabajo de la aplicación lógica.

   ![Ver la definición de flujo de trabajo para la aplicación lógica publicada](./media/quickstart-create-logic-apps-visual-studio-code/edit-published-logic-app-workflow-definition.png)

1. Aplique los cambios en la definición del flujo de trabajo de la aplicación lógica.

1. Cuando haya terminado, guarde los cambios. (Menú Archivo > Guardar o presione Ctrl + S)

1. Cuando se le pida que cargue los cambios y *sobrescriba* la aplicación lógica existente en Azure Portal, seleccione **Cargar**.

   Este paso publica las actualizaciones de la aplicación lógica en [Azure Portal](https://portal.azure.com).

   ![Cargar las ediciones de la definición de aplicación lógica en Azure](./media/quickstart-create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="view-or-promote-other-versions"></a>Ver o promover otras versiones

En Visual Studio Code, puede abrir y revisar las versiones anteriores de la aplicación lógica. También puede promover una versión anterior a la versión actual.

> [!IMPORTANT] 
> Antes de modificar una aplicación lógica que se ejecuta activamente en producción, evite el riesgo de interrumpir esa aplicación lógica y minimice la interrupción mediante la [deshabilitación de la aplicación lógica en primer lugar](#disable-enable-logic-apps).

1. En la ventana de Azure, en **Logic Apps**, expanda su suscripción a Azure para poder ver todas las aplicaciones lógicas de la suscripción.

1. En su suscripción, expanda la aplicación lógica y, a continuación, **Versiones**.

   La lista **Versiones** muestra las versiones anteriores de la aplicación lógica, si existe alguna.

   ![Versiones anteriores de la aplicación lógica](./media/quickstart-create-logic-apps-visual-studio-code/view-previous-versions.png)

1. Para ver una versión anterior, seleccione uno de estos pasos:

   * Para ver la definición de JSON, en **Versiones**, seleccione el número de versión de esa definición. O bien, abra el menú contextual de la versión y seleccione **Abrir en el editor**.

     Se abre un nuevo archivo en el equipo local y se muestra la definición de JSON de la versión.

   * Para ver la versión en la vista del diseñador de solo lectura, abra el menú contextual de esa versión y seleccione **Abrir en el diseñador**.

1. Para promover una versión anterior a la versión actual, siga estos pasos:

   1. En **Versiones**, abra el menú contextual de la versión anterior y seleccione **Promover**.

      ![Promover una versión anterior](./media/quickstart-create-logic-apps-visual-studio-code/promote-earlier-version.png)

   1. Para continuar después de que Visual Studio Code le pida confirmación, seleccione **Sí**.

      ![Confirmar la promoción de la versión anterior](./media/quickstart-create-logic-apps-visual-studio-code/confirm-promote-version.png)

      Visual Studio Code promueve la versión seleccionada a la versión actual y asigna un nuevo número a la versión promovida. La versión actual anterior aparece ahora bajo la versión promovida.

<a name="disable-enable-logic-apps"></a>

## <a name="disable-or-enable-logic-apps"></a>Deshabilitación o habilitación de la aplicaciones lógicas

En Visual Studio Code, si edita una aplicación lógica publicada y guarda los cambios, *sobrescribe* la aplicación ya implementada. Para evitar interrumpir la aplicación lógica en producción y minimizar la interrupción, deshabilite primero la aplicación lógica. Luego, puede reactivar la aplicación lógica, después de haber confirmado que aún funciona.

> [!NOTE]
> La deshabilitación de una aplicación lógica tiene los siguientes efectos en las instancias de flujo de trabajo:
>
> * El servicio Logic Apps continúa todas las ejecuciones en curso y pendientes hasta que finalizan. Según el volumen o el trabajo pendiente, este proceso puede tardar en completarse.
>
> * El servicio Logic Apps no crea ni ejecuta nuevas instancias de flujo de trabajo.
>
> * El desencadenador no se activará la próxima vez que se cumplan sus condiciones. Sin embargo, el estado del desencadenador recuerda el punto en el que se detuvo la aplicación lógica. Por lo tanto, si reactiva la aplicación lógica, el desencadenador se activará para todos los elementos no procesados desde la última ejecución.
>
>   Para evitar que el desencadenador se active con elementos no procesados desde la última ejecución, borre el estado del desencadenador antes de reactivar la aplicación lógica:
>
>   1. En la aplicación lógica, edite cualquier parte del desencadenador del flujo de trabajo.
>   1. Guarde los cambios. Este paso restablece el estado actual del desencadenador.
>   1. Reactive la aplicación lógica.

1. Si aún no ha iniciado sesión en su cuenta y suscripción de Azure desde dentro de Visual Studio Code, siga los [pasos anteriores para iniciar sesión ahora](#access-azure).

1. En la ventana de Azure, en **Logic Apps**, expanda su suscripción a Azure para poder ver todas las aplicaciones lógicas de la suscripción.

   1. Para deshabilitar la aplicación lógica que desee, abra el menú de la aplicación lógica y seleccione **Deshabilitar**.

      ![Deshabilitar la aplicación lógica](./media/quickstart-create-logic-apps-visual-studio-code/disable-published-logic-app.png)

   1. Cuando esté listo para volver a activar la aplicación lógica, abra el menú de la aplicación lógica y seleccione **Habilitar**.

      ![Habilitar la aplicación lógica](./media/quickstart-create-logic-apps-visual-studio-code/enable-published-logic-app.png)

<a name="delete-logic-apps"></a>

## <a name="delete-logic-apps"></a>Eliminación de aplicaciones lógicas

La eliminación de una aplicación lógica afecta a las instancias de flujo de trabajo de las maneras siguientes:

* El servicio Logic Apps hace todo lo posible por cancelar las ejecuciones en curso y pendientes.

  Incluso con un gran volumen o trabajo pendiente, la mayoría de las ejecuciones se cancelan antes de que finalicen o se inicien. Sin embargo, el proceso de cancelación puede tardar en completarse. Mientras tanto, pueden seleccionarse algunas ejecuciones para su ejecución mientras el servicio se encarga del proceso de cancelación.

* El servicio Logic Apps no crea ni ejecuta nuevas instancias de flujo de trabajo.

* Si elimina un flujo de trabajo y, luego, vuelve a crear el mismo flujo de trabajo, el flujo de trabajo recreado no tendrá los mismos metadatos que el flujo de trabajo eliminado. Tiene que volver a guardar todo flujo de trabajo que haya llamado al flujo de trabajo eliminado. De este modo, el autor de la llamada obtiene la información correcta para el flujo de trabajo recreado. De lo contrario, las llamadas al flujo de trabajo recreado producirán un error `Unauthorized`. Este comportamiento también se aplica a los flujos de trabajo que usan artefactos en cuentas de integración y a flujos de trabajo que llaman a funciones de Azure.

1. Si aún no ha iniciado sesión en su cuenta y suscripción de Azure desde dentro de Visual Studio Code, siga los [pasos anteriores para iniciar sesión ahora](#access-azure).

1. En la ventana de Azure, en **Logic Apps**, expanda su suscripción a Azure para poder ver todas las aplicaciones lógicas de la suscripción.

1. Busque la aplicación lógica que desee eliminar, abra el menú de la aplicación lógica y seleccione **Eliminar**.

   ![Eliminación de la aplicación lógica](./media/quickstart-create-logic-apps-visual-studio-code/delete-logic-app.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de flujos de trabajo de aplicación lógica basados en un solo inquilino en Visual Studio Code](../logic-apps/create-single-tenant-workflows-visual-studio-code.md)
