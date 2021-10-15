---
title: Recepción de notificaciones del almacén de claves y respuesta con Azure Event Grid
description: Aprenda a integrar Key Vault con Azure Event Grid.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/25/2019
ms.author: mbaldwin
ms.openlocfilehash: ff1c5396ca2861250c5314ac785f8cbad42bf362
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129235485"
---
# <a name="receive-and-respond-to-key-vault-notifications-with-azure-event-grid"></a>Recepción de notificaciones del almacén de claves y respuesta con Azure Event Grid

La integración de Azure Key Vault en Azure Event Grid permite a los usuarios recibir una notificación cuando cambia el estado de un secreto almacenado en el almacén de claves. Para información general sobre esta característica, consulte [Supervisión de Key Vault con Event Grid](event-grid-overview.md).

En esta guía se describe cómo recibir notificaciones de Key Vault mediante Event Grid, y cómo responder a los cambios de estado con Azure Automation.

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
- Un almacén de claves en la suscripción de Azure. Puede crear rápidamente un almacén de claves si sigue los pasos descritos en [Establecimiento y recuperación de un secreto desde Azure Key Vault mediante la CLI de Azure](../secrets/quick-create-cli.md).

## <a name="concepts"></a>Conceptos

Event Grid es un servicio de eventos para la nube. Al seguir los pasos de esta guía, se suscribirá a los eventos de Key Vault y enrutará los eventos a Automation. Cuando uno de los secretos del almacén de claves está a punto de expirar (se define como 30 días antes de la fecha de expiración), Event Grid recibe una notificación del cambio de estado y realiza una solicitud HTTP POST al punto de conexión. Después, un webhook desencadena una ejecución de Automation de un script de PowerShell.

![Diagrama de flujo HTTP POST](../media/event-grid-tutorial-1.png)

## <a name="create-an-automation-account"></a>Creación de una cuenta de Automation

Cree una cuenta de Automation mediante [Azure Portal](https://portal.azure.com):

1.  Vaya a portal.azure.com e inicie sesión en la suscripción.

1.  En el cuadro de búsqueda, escriba **Cuentas de Automation**.

1.  En la sección **Servicios** de la lista desplegable de la barra de búsqueda, seleccione **Cuentas de Automation**.

1.  Seleccione **Agregar**.

    ![Panel Cuentas de Automation](../media/event-grid-tutorial-2.png)

1.  Escriba la información necesaria en el panel **Agregar cuenta de Automation** y, a continuación, seleccione **Crear**.

## <a name="create-a-runbook"></a>Crear un runbook

Una vez que la cuenta de Automation esté lista, cree un runbook.

![Creación de la UI de un runbook](../media/event-grid-tutorial-3.png)

1.  Seleccione la cuenta de Automation que acaba de crear.

1.  Seleccione **Runbooks** en **Automatización de procesos**.

1.  Seleccione **Crear un runbook**.

1.  Asigne un nombre al runbook y seleccione **PowerShell** como tipo del runbook.

1.  Seleccione el runbook que ha creado y seleccione el botón **Editar**.

1.  Escriba el código siguiente (con fines de prueba) y seleccione el botón **Publicar**. Esta acción devuelve el resultado de la solicitud POST recibida.

```azurepowershell
param
(
[Parameter (Mandatory = $false)]
[object] $WebhookData
)

#If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

#rotate secret:
#generate new secret version in key vault
#update db/service with generated secret

#Write-Output "WebhookData <$WebhookData>"
Write-Output $WebhookData.RequestBody
}
else
{
# Error
write-Error "No input data found." 
}
```

![Publicación de la UI de un runbook](../media/event-grid-tutorial-4.png)

## <a name="create-a-webhook"></a>Creación de un webhook

Cree un webhook para desencadenar el runbook que acaba de crear.

1.  Seleccione **Webhooks** en la sección **Recursos** del runbook que acaba de publicar.

1.  Seleccione **Agregar Webhook**.

    ![Botón Agregar webhook](../media/event-grid-tutorial-5.png)

1.  Seleccione **Crear nuevo Webhook**.

1. Asígnele un nombre al webhook, establezca una fecha de expiración y copie la dirección URL.

    > [!IMPORTANT] 
    > Una vez que se cree, no podrá ver la dirección URL. Asegúrese de guardar una copia en una ubicación segura a la que pueda acceder durante el resto de esta guía.

1. Seleccione **Configuración de ejecución y parámetros** y seleccione **Aceptar**. No escriba ningún parámetro. Esto habilitará el botón **Crear**.

1. Seleccione **Aceptar** y después **Crear**.

    ![Creación de la UI de un nuevo webhook](../media/event-grid-tutorial-6.png)

## <a name="create-an-event-grid-subscription"></a>Creación de una suscripción de Event Grid

Cree una suscripción de Event Grid con [Azure Portal](https://portal.azure.com).

1.  Vaya al almacén de claves y seleccione la pestaña **Eventos**.

    ![Pestaña Eventos en Azure Portal](../media/event-grid-tutorial-7.png)

1.  Seleccione el botón **Suscripción a eventos**.

1.  Cree un nombre descriptivo para la suscripción.

1.  Elija **Esquema de Event Grid**.

1.  **Recurso de tema** debe ser el almacén de claves cuyos cambios de estado desea supervisar.

1.  En **Filtro para tipos de evento**, deje seleccionadas todas las opciones (**9 seleccionadas**).

1.  En **Tipo de punto de conexión**, seleccione **Webhook**.

1.  Elija **Seleccionar un punto de conexión**. En el panel del nuevo contexto, pegue la dirección URL del webhook del paso [Creación de un webhook](#create-a-webhook) en el campo **Punto de conexión de suscriptor**.

1.  Seleccione **Confirmar selección** en el panel de contexto.

1.  Seleccione **Crear**.

    ![Creación de la suscripción de eventos](../media/event-grid-tutorial-8.png)

## <a name="test-and-verify"></a>Prueba y comprobación

Verifique que la suscripción a Event Grid esté correctamente configurada. En esta prueba se supone que se ha suscrito a la notificación "Secret New Version Created" (Nueva versión del secreto creada) en [Creación de una suscripción de Event Grid](#create-an-event-grid-subscription) y que tiene los permisos necesarios para crear una versión de un secreto en un almacén de claves.

![Configuración de prueba de la suscripción a Event Grid](../media/event-grid-tutorial-9.png)

![Creación de un panel de secretos](../media/event-grid-tutorial-10.png)

1.  Vaya al almacén de claves en Azure Portal.

1.  Cree un secreto. Para fines de prueba, establezca la fecha de caducidad para el día siguiente.

1.  En la pestaña **Eventos** del almacén de claves, seleccione la suscripción a Event Grid que creó.

1.  En **Métricas**, consulte si se capturó un evento. Se esperan dos: SecretNewVersion y SecretNearExpiry. Estos eventos validan que Event Grid capturó correctamente el cambio de estado del secreto en el almacén de claves.

    ![Panel Métricas: comprobación de eventos capturados](../media/event-grid-tutorial-11.png)

1.  Vaya a su cuenta de Automation.

1.  Seleccione la pestaña **Runbooks** y seleccione el runbook que creó.

1.  Seleccione la pestaña **Webhooks** y confirme que la marca de tiempo "Última vez que se desencadenó" se encuentra en un plazo de 60 segundos a partir del momento de la creación del secreto. Este resultado confirma que Event Grid realizó una solicitud POST en el webhook con los detalles del evento de cambio de estado en el almacén de claves, y que el webhook se desencadenó.

    ![Pestaña Webhooks, marca de tiempo de Última vez que se desencadenó](../media/event-grid-tutorial-12.png)

1. Vuelva al runbook y seleccione la pestaña **Información general**.

1. Examine la lista **Trabajos recientes**. Debería ver que se ha creado un trabajo y que el estado es completado. Esto confirma que el webhook desencadenó el runbook para iniciar la ejecución de su script.

    ![Lista de trabajos recientes del webhook](../media/event-grid-tutorial-13.png)

1. Seleccione el trabajo reciente y examine la solicitud POST que se envió desde Event Grid al webhook. Examine el archivo JSON y asegúrese de que los parámetros para el almacén de claves y el tipo de evento son correctos. Si el parámetro "event type" del objeto JSON coincide con el evento que se produjo en el almacén de claves (en este ejemplo, Microsoft.KeyVault.SecretNearExpiry), la prueba se completó correctamente.

## <a name="troubleshooting"></a>Solución de problemas

### <a name="you-cant-create-an-event-subscription"></a>No puede crear la suscripción a un evento

Vuelva a registrar Event Grid y el proveedor del almacén de claves en los proveedores de recursos de la suscripción a Azure. Consulte [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Pasos siguientes

Felicidades. Si ha seguido correctamente todos estos pasos, ya está listo para responder mediante programación a los cambios de estado de los secretos almacenados en el almacén de claves.

Si ha usado un sistema basado en sondeo para buscar cambios de estado en los secretos de los almacenes de claves, puede empezar a usar esta característica de notificación. También puede reemplazar el script de prueba del runbook con código para renovar los secretos mediante programación cuando están a punto de expirar.

Más información:


- Introducción: [Supervisión de Key Vault con Azure Event Grid](event-grid-overview.md)
- Procedimientos: [Recibir un correo electrónico al cambiar un secreto del almacén de claves](event-grid-logicapps.md)
- [Esquema de eventos Azure Event Grid para Azure Key Vault](../../event-grid/event-schema-key-vault.md)
- [Introducción a Azure Key Vault](overview.md)
- [Introducción a Azure Event Grid](../../event-grid/overview.md)
- [Información general sobre Azure Automation](../../automation/index.yml)
