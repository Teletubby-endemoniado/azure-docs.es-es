---
title: 'Tutorial: Preparación del entorno del centro de datos de Azure Portal para implementar Azure Stack Edge Pro con GPU | Microsoft Docs'
description: El primer tutorial para implementar Azure Stack Edge Pro con GPU trata sobre la preparación de Azure Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/06/2021
ms.author: alkohli
ms.openlocfilehash: b223d428daf1a7080478f4f80b6a997fb97cd7a0
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2021
ms.locfileid: "122322677"
---
# <a name="tutorial-prepare-to-deploy-azure-stack-edge-pro-with-gpu"></a>Tutorial: Preparación de la implementación de Azure Stack Edge Pro con GPU 

Este es el primero de una serie de tutoriales de implementación necesarios para implementar Azure Stack Edge Pro con GPU en su totalidad. En este tutorial se describe cómo preparar Azure Portal para implementar un recurso de Azure Stack Edge.

Para completar el proceso de instalación y configuración se necesitan privilegios de administrador. La preparación del portal dura menos de 10 minutos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Obtención de la clave de activación

### <a name="get-started"></a>Introducción

Para la implementación de Azure Stack Edge Pro, primero debe preparar el entorno. Una vez que el entorno esté listo, siga los pasos requeridos y, si es necesario, los pasos y procedimientos opcionales para implementar el dispositivo por completo. Las instrucciones detalladas de implementación indican cuándo debe realizar cada uno de estos pasos requeridos y opcionales.

| Paso | Descripción |
| --- | --- |
| **Preparación** |Estos pasos se deben completar como preparación para la próxima implementación. |
| **[Lista de comprobación de la configuración de implementación](#deployment-configuration-checklist)** |Use esta lista de comprobación para recopilar y registrar información antes y durante la implementación. |
| **[Requisitos previos de implementación](#prerequisites)** |Estos requisitos previos garantizan que el entorno esté preparado para la implementación. |
|  | |
|**Tutoriales de implementación** |Estos tutoriales son necesarios para implementar el dispositivo de Azure Stack Edge Pro en producción. |
|**[1. Preparación de Azure Portal para Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-prep.md)** |Cree y configure el recurso de Azure Stack Edge antes de instalar un dispositivo físico de Azure Stack Box Edge. |
|**[2. Instalación de Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-install.md)**|Desempaquete, monte en el bastidor y realice el cableado del dispositivo físico de Azure Stack Edge Pro.  |
|**[3. Conexión a Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-connect.md)** |Una vez instalado el dispositivo, conéctese a la interfaz de usuario web local del dispositivo.  |
|**[4. Configuración de los valores de red para Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md)** |Configure la red, incluida la red de proceso y los valores de proxy web para el dispositivo.   |
|**[5. Configuración de los valores del dispositivo para Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-set-up-device-update-time.md)** |Asigne un nombre de dispositivo y un dominio DNS, configure el servidor de actualización y la hora del dispositivo. |
|**[6. Configuración de los valores de seguridad para Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-configure-certificates.md)** |Configuración de certificados para el dispositivo. Use certificados generados por el dispositivo o aporte sus propios certificados.   |
|**[7. Activación de Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-activate.md)** |Use la clave de activación del servicio para activar el dispositivo. El dispositivo está listo para configurar recursos compartidos SMB o NFS o conectarse a través de REST. |
|**[8. Configuración de proceso](azure-stack-edge-gpu-deploy-configure-compute.md)** |Configure el rol de proceso en el dispositivo. Se crea también un clúster de Kubernetes. |
|**[9A. Transferencia de datos con recursos compartidos de Edge](./azure-stack-edge-gpu-deploy-add-shares.md)** |Agregue recursos compartidos y conéctese a ellos mediante SMB o NFS. |
|**[9B. Transferencia de datos con cuentas de almacenamiento de Edge](./azure-stack-edge-gpu-deploy-add-storage-accounts.md)** |Agregue cuentas de almacenamiento y conéctese al almacenamiento de blobs a través de las API de REST. |


Ahora puede empezar a recopilar información relacionada con la configuración de software del dispositivo de Azure Stack Edge Pro.

## <a name="deployment-configuration-checklist"></a>Lista de comprobación de la configuración de implementación

Antes de implementar el dispositivo, deberá recopilar información para configurar el software en el dispositivo de Azure Stack Edge Pro. La preparación de parte de esta información con antelación simplifica el proceso de implementación del dispositivo en el entorno. Use la [lista de comprobación de la configuración de implementación de Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-checklist.md) para anotar los detalles de la configuración cuando implemente el dispositivo.


## <a name="prerequisites"></a>Prerrequisitos

A continuación, encontrará los requisitos previos para configurar su recurso de Azure Stack Edge, el dispositivo de Azure Stack Edge Pro y la red de centros de datos.

### <a name="for-the-azure-stack-edge-resource"></a>Para el recurso de Azure Stack Edge

<!--Why isn't the include file used, as for the Pro R and Mini R SKUs? Check for differences. Also, the presentation of requirements is organized a bit differently; standard presentation would be more usable, even if the GPU requirements are different.-->

Antes de comenzar, asegúrese de que:

- Su suscripción de Microsoft Azure debe estar habilitada para un recurso de Azure Stack Edge. Asegúrese de que ha usado una suscripción admitida como [Contrato Enterprise (EA) de Microsoft](https://azure.microsoft.com/overview/sales-number/), [Proveedor de soluciones en la nube (CSP)](/partner-center/azure-plan-lp) o [Patrocinio de Microsoft Azure](https://azure.microsoft.com/offers/ms-azr-0036p/). No se admiten las suscripciones de pago por uso. Para identificar el tipo de suscripción de Azure que tiene, consulte [¿Qué es una oferta de Azure?](../cost-management-billing/manage/switch-azure-offer.md#what-is-an-azure-offer)
- Tiene acceso de propietario o colaborador en el nivel de grupo de recursos para los recursos de Azure Stack Edge Pro, IoT Hub y Azure Storage.

    - Para crear cualquier recurso de Azure Stack Edge, debe tener permisos de colaborador (o superiores) con ámbito en el nivel del grupo de recursos. 
    - También deberá asegurarse de que los proveedores de recursos `Microsoft.DataBoxEdge` y `MicrosoftKeyVault` estén registrados. Para crear cualquier recurso de IoT Hub, es necesario registrar el proveedor `Microsoft.Devices`. 
        - Para registrar un proveedor de recursos, en Azure Portal, vaya a **Inicio > Suscripciones > Su suscripción > Proveedores de recursos**. 
        - Busque el proveedor de recursos específico, por ejemplo, `Microsoft.DataBoxEdge`, y regístrelo. 
    - Para crear un recurso de una cuenta de Storage también se necesita acceso de colaborador, o superior, con ámbito en el nivel de grupo de recursos. De forma predeterminada Azure Storage es un proveedor de recursos registrado.
- Para crear un pedido en Azure Edge Hardware Center, debe asegurarse de que el proveedor `Microsoft.EdgeOrder` está registrado. Para obtener información acerca de cómo registrarse, vaya a [Registro de proveedores de recursos](azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#register-resource-providers).
- Tiene acceso de administrador o de usuario a Graph API de Azure Active Directory con fines de generación de claves de activación u operaciones de credenciales, como la creación de recursos compartidos que utilizan una cuenta de almacenamiento. Para más información, vea [Graph API de Azure Active Directory](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).

### <a name="for-the-azure-stack-edge-pro-device"></a>Para el dispositivo de Azure Stack Edge Pro

Antes de implementar un dispositivo físico, asegúrese de que:

- Ha revisado la información de seguridad que se incluyó en el paquete de envío.
- Tiene una ranura 1U disponible en un bastidor estándar de 19" en el centro de datos para montar el dispositivo.
- Tiene acceso a una superficie de trabajo plana, estable y nivelada en la que el dispositivo pueda apoyarse con seguridad.
- La ubicación en la que desea efectuar la instalación del dispositivo dispone de corriente alterna estándar de una fuente independiente o una unidad de distribución de energía (PDU) en bastidor con un sistema de alimentación ininterrumpida (UPS).
- Tiene acceso a un dispositivo físico.


### <a name="for-the-datacenter-network"></a>Para la red del centro de datos

Antes de comenzar, asegúrese de que:

- La red del centro de datos está configurada según los requisitos de red para el dispositivo de Azure Stack Edge Pro. Para más información, consulte [Requisitos del sistema de Azure Stack Edge Pro](azure-stack-edge-system-requirements.md).

- Para que Azure Stack Edge Pro funcione normalmente, necesita lo siguiente:

    - Un ancho de banda mínimo de 10 Mbps para garantizar que el dispositivo se mantiene actualizado.
    - Un ancho de banda mínimo de 20 Mbps dedicado a carga y descarga para transferir archivos.

## <a name="create-a-new-resource"></a>Crear un nuevo recurso

Si ya tiene un recurso de Azure Stack Edge para administrar el dispositivo físico, omita este paso y vaya a [Obtención de la clave de activación](#get-the-activation-key).

---

### <a name="azure-edge-hardware-center-preview"></a>[Azure Edge Hardware Center (versión preliminar)](#tab/azure-edge-hardware-center)

Azure Edge Hardware Center (versión preliminar) es un nuevo servicio que le permite explorar y pedir una variedad de hardware de la cartera híbrida de Azure, incluidos los dispositivos Azure Stack Edge Pro.

Cuando realiza un pedido mediante Azure Edge Hardware Center, puede solicitar varios dispositivos para que se envíen a más de una dirección y puede reutilizar el envío para direcciones de otros pedidos.

La realización de un pedido mediante Azure Edge Hardware Center creará un recurso de Azure que contendrá toda la información relacionada con el pedido. Se creará un recurso para cada una de las unidades solicitadas. Tendrá que crear un recurso de Azure Stack Edge después de recibir el dispositivo para activarlo y administrarlo.

[!INCLUDE [Create order in Azure Edge Hardware Center](../../includes/azure-edge-hardware-center-new-order.md)]

#### <a name="create-a-management-resource-for-each-device"></a>Creación de un recurso de administración para cada dispositivo

[!INCLUDE [Create management resource](../../includes/azure-edge-hardware-center-create-management-resource.md)]

### <a name="portal-classic"></a>[Portal (clásico)](#tab/azure-portal)

Siga estos pasos en Azure Portal para crear un recurso de Azure Stack Edge mediante el servicio Azure Stack Edge.

1. Use sus credenciales de Microsoft Azure para iniciar sesión en Azure Portal, en esta dirección URL: [https://portal.azure.com](https://portal.azure.com).

2. En **Servicios de Azure**, busque y seleccione **Azure Stack Edge**. A continuación, seleccione **+ Crear**. 

3. En **Manage Azure Stack Edge devices** (Administrar dispositivos Azure Stack Edge), seleccione el vínculo **Try Azure Edge Hardware Center** (Probar Azure Edge Hardware Center).

    ![Captura de la pantalla "Administrar dispositivos Azure Stack Edge", que se abre con el botón Más Crear. El vínculo "Probar Azure Edge Hardware Center" está resaltado.](media/azure-stack-edge-gpu-deploy-prep/classic-order-experience-1.png)

4. Si no desea realizar el pedido mediante Hardware Center, seleccione **Order using classic ordering experience** (Realizar el pedido mediante la experiencia de pedidos clásica) en la pantalla **Get started** (Comenzar).

   ![Captura de pantalla de la pantalla Comenzar en Azure Stack Edge. El vínculo "Realizar el pedido mediante la experiencia de pedidos clásica" está resaltado.](media/azure-stack-edge-gpu-deploy-prep/classic-order-experience-2.png)

5. Seleccione la suscripción que quiere usar para el dispositivo GPU de Azure Stack Edge Pro. Seleccione el país o región al que se enviará el dispositivo físico. Después, seleccione **Mostrar dispositivos**.

    ![Captura de pantalla de la pantalla "Seleccionar tipo de dispositivo" para seleccionar una suscripción y enviar a la región para un recurso Azure Stack Edge dispositivo. El botón Mostrar dispositivos está resaltado.](media/azure-stack-edge-gpu-deploy-prep/create-resource-1.png)

6. Seleccione el tipo de dispositivo. En **Azure Stack Edge Pro**, elija **Azure Stack Edge Pro con GPU** y, luego, elija **Seleccionar**. Si ve algún problema o no puede seleccionar el tipo de dispositivo, vaya a [Solución de problemas de pedidos](azure-stack-edge-troubleshoot-ordering.md).

    ![Captura de la pantalla "Seleccionar tipo de dispositivo" para seleccionar el tipo de dispositivo para un recurso de Azure Stack Edge. El botón Seleccionar de un tipo de dispositivo está resaltado.](media/azure-stack-edge-gpu-deploy-prep/create-resource-3.png)

7. En función de las necesidades de su empresa, puede seleccionar Azure Stack Edge Pro con 1 o 2 unidades de procesamiento gráfico (GPU) de NVIDIA. 

    ![Captura de la pantalla para seleccionar una configuración para un dispositivo GPU de Azure Stack Edge Pro para un recurso de Azure Stack Edge. La configuración de hardware y el botón Seleccionar aparecen resaltados.](media/azure-stack-edge-gpu-deploy-prep/create-resource-4.png)

8. En la pestaña **Datos básicos**, escriba o seleccione los siguientes **detalles del proyecto**.
    
    |Configuración  |Value  |
    |---------|---------|
    |Subscription    |La suscripción se rellena automáticamente según la selección anterior. La suscripción está vinculada a la cuenta de facturación. |
    |Resource group  |Cree un nuevo grupo o seleccione uno existente.<br>Más información sobre los [grupos de recursos de Azure](../azure-resource-manager/management/overview.md).     |

9. Escriba o seleccione los siguientes **detalles de la instancia**.

    |Configuración  |Value  |
    |---------|---------|
    |Nombre   | Nombre descriptivo que identifique el recurso.<br>El nombre tiene entre 2 y 50 caracteres, que incluyen letras, números y guiones.<br> El nombre comienza y termina con una letra o un número.        |
    |Region     |Para una lista de todas las regiones en las que está disponible el recurso de Azure Stack Edge, consulte [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Si usa Azure Government, todas las regiones de gobierno están disponibles como se muestra en las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).<br> Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo.|

    ![Captura de pantalla de la pestaña Información básica del Asistente para crear un recurso y solicitar un dispositivo para Azure Stack Edge. La pestaña Aspectos básicos y el botón Siguiente: Dirección de envío están resaltados.](media/azure-stack-edge-gpu-deploy-prep/create-resource-5.png)

10. Seleccione **Siguiente: Dirección de envío**.

    - Si ya tiene un dispositivo, seleccione el cuadro combinado **Ya tengo un dispositivo**.

        ![Captura de pantalla de la pestaña "Dirección de envío" con la opción "Ya tengo una dirección de envío para el dispositivo" seleccionada en el Asistente para crear un recurso para Azure Stack Edge.](media/azure-stack-edge-gpu-deploy-prep/create-resource-6.png)

    - Si este es el pedido del nuevo dispositivo, escriba el nombre de contacto, la empresa, la dirección de envío y la información de contacto.

        ![Captura de pantalla de la pestaña "Dirección de envío" del Asistente para crear un recurso al crear un nuevo recurso Azure Stack Edge.](media/azure-stack-edge-gpu-deploy-prep/create-resource-7.png)

11. Seleccione **Siguiente: Etiquetas**. También tiene la opción de proporcionar etiquetas para clasificar los recursos y consolidar la facturación. Seleccione **Siguiente: Review + create** (Revisar y crear).

12. En la pestaña **Revisar y crear**, revise la información de **Detalles de precios**, **Términos de uso** y los detalles de su recurso. Seleccione el cuadro combinado **I have reviewed the privacy terms** (He revisado los términos de privacidad).

    ![Captura de pantalla de la pestaña Revisar y crear para un pedido de Azure Stack Edge.](media/azure-stack-edge-gpu-deploy-prep/create-resource-8.png) 

    También se le notifica que, durante la creación de recursos, se habilita una identidad administrada que le permite autenticarse en los servicios en la nube. Esta identidad existe mientras exista el recurso.

13. Seleccione **Crear**.

    Se tarda unos minutos en crear el recurso. También se crea una identidad administrada que permite al dispositivo Azure Stack Edge comunicarse con el proveedor de recursos en Azure.

    Cuando el recurso se haya creado e implementado correctamente, recibirá una notificación. Haga clic en **Go to resource** (Ir al recurso).

    ![Captura de la pantalla que indica que se ha completado la implementación de un nuevo recurso de Azure Stack Edge. El botón Ir al recurso aparece resaltado.](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-1.png)

Una vez realizado el pedido, Microsoft lo revisa y se pone en contacto con usted (por correo electrónico) para indicarle los detalles del envío.

<!--![Notification for review of the Azure Stack Edge Pro order](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-2.png)-->

> [!NOTE]
> Si quiere crear varios pedidos al mismo tiempo o clonar un pedido existente, puede usar los [scripts de Azure Samples](https://github.com/Azure-Samples/azure-stack-edge-order). Para más información, consulte el archivo LÉAME.

Si surgen problemas durante el proceso de pedido, consulte [Solución de problemas de pedidos](azure-stack-edge-troubleshoot-ordering.md).

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Si es necesario, prepare el entorno de la CLI de Azure.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

Para crear un recurso de Azure Stack Edge, ejecute los comandos siguientes en la CLI de Azure.

1. Cree un grupo de recursos utilizando el comando [az group create](/cli/azure/group#az_group_create) o utilice un grupo de recursos existente:

   ```azurecli
   az group create --name myasepgpu1 --location eastus
   ```

1. Para crear un dispositivo, utilice el comando [az databoxedge device create](/cli/azure/databoxedge/device#az_databoxedge_device_create):

   ```azurecli
   az databoxedge device create --resource-group myasepgpu1 \
      --device-name myasegpu1 --location eastus --sku EdgeP_Base
   ```

   Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo. La región almacena solo los metadatos para la administración de dispositivos. Los datos reales se pueden almacenar en cualquier cuenta de almacenamiento.

   Para una lista de todas las regiones en las que está disponible el recurso de Azure Stack Edge, consulte [Productos de Azure disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Si usa Azure Government, todas las regiones de gobierno están disponibles como se muestra en las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).

1. Para crear un pedido, ejecute el comando [az databoxedge order create](/cli/azure/databoxedge/order#az_databoxedge_order_create):

   ```azurecli 
   az databoxedge order create --resource-group myasepgpu1 \
      --device-name myasegpu1 --company-name "Contoso" \
      --address-line1 "1020 Enterprise Way" --city "Sunnyvale" \
      --state "California" --country "United States" --postal-code 94089 \
      --contact-person "Gus Poland" --email-list gus@contoso.com --phone 4085555555
   ```

Se tarda unos minutos en crear el recurso. Ejecute el comando [az databoxedge order show](/cli/azure/databoxedge/order#az_databoxedge_order_show) para ver el pedido:

```azurecli
az databoxedge order show --resource-group myasepgpu1 --device-name myasegpu1 
```

Una vez realizado el pedido, Microsoft lo revisará y se pondrá en contacto con usted por correo electrónico para indicarle los detalles del envío.

---

## <a name="get-the-activation-key"></a>Obtención de la clave de activación

Cuando el recurso de Azure Stack Edge esté en funcionamiento, tendrá que obtener la clave de activación. Esta clave se usa para activar y conectar el dispositivo de Azure Stack Edge Pro con el recurso. Puede obtener esta clave ahora mientras está en Azure Portal.

1. Seleccione el recurso que ha creado y, a continuación, **Información general**.

2. En el panel derecho, escriba un nombre para Azure Key Vault o acepte el nombre predeterminado. El nombre puede tener entre 3 y 24 caracteres.

   Se crea un almacén de claves para cada recurso de Azure Stack Edge que se activa con el dispositivo. El almacén de claves permite almacenar los secretos y acceder a ellos; por ejemplo, la clave de integridad del canal (CIK) del servicio se almacena en el almacén de claves. 

   Una vez que haya especificado un nombre de almacén de claves, seleccione **Generar clave** para crear una clave de activación. 

   ![Captura de pantalla del panel Información general de un recurso de Azure Stack Edge creado recientemente. El botón Generar clave de activación aparece resaltado.](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-3.png)

   Espere unos minutos a que se creen el almacén de claves y la clave de activación. Seleccione el icono de copia para copiar la clave y guárdela para su uso posterior.<!--Verify that the new screen has a copy icon.-->


> [!IMPORTANT]
> - La clave de activación expira tres días después de generarla.
> - Si la clave ha expirado, genere una nueva. La clave antigua no es válida.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Stack Edge Pro, tales como:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Obtención de la clave de activación

En el siguiente tutorial aprenderá a instalar Azure Stack Edge Pro.

> [!div class="nextstepaction"]
> [Instalación de Azure Stack Edge Pro](./azure-stack-edge-gpu-deploy-install.md)
