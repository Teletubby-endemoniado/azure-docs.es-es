---
title: Devolución del dispositivo Azure Stack Edge
description: Obtenga información acerca de cómo borrar los datos y devolver el dispositivo Azure Stack Edge y, a continuación, eliminar el recurso asociado con el dispositivo.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 09/15/2021
ms.author: alkohli
ms.openlocfilehash: 3fa49fdab111c60c9f73dad5703c39dbc33e1472
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128554228"
---
# <a name="return-your-azure-stack-edge-device"></a>Devolución del dispositivo de Azure Stack Edge

[!INCLUDE [applies-to-pro-fpga](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-fpga-sku.md)]

En este artículo se explica cómo borrar los datos y luego devolver el dispositivo Azure Stack Edge. Una vez que haya devuelto el dispositivo, también puede eliminar el recurso asociado al dispositivo.

En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Borrar los datos de los discos de datos del dispositivo
> * Iniciar la devolución del dispositivo en Azure Portal
> * Empaquetar el dispositivo y programar una recogida
> * Eliminar el recurso en Azure Portal

## <a name="erase-data-from-the-device"></a>Borrar los datos del dispositivo

Para borrar los datos de los discos de datos del dispositivo, debe restablecer el dispositivo.

Antes de restablecerlo, cree una copia de los datos locales del dispositivo si es necesario. Puede copiar los datos del dispositivo en un contenedor de Azure Storage. 

Puede iniciar la devolución del dispositivo incluso antes de restablecer el dispositivo.

Puede restablecer el dispositivo en la interfaz de usuario web local o en PowerShell. Para obtener instrucciones de PowerShell, vea [Restablecimiento del dispositivo](./azure-stack-edge-connect-powershell-interface.md#reset-your-device).

[!INCLUDE [Reset data from the device](../../includes/azure-stack-edge-device-reset.md)]

> [!NOTE]
> - Si va a intercambiar o actualizar a un dispositivo nuevo, se recomienda restablecer el dispositivo solo después de que haya recibido el dispositivo nuevo.
> - Al restablecer el dispositivo solo se eliminan todos los datos locales del dispositivo. Los datos que están en la nube no se eliminan, sino que generan [cargos](https://azure.microsoft.com/pricing/details/storage/). Estos datos deben eliminarse por separado mediante una herramienta de administración de almacenamiento en la nube como [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="initiate-device-return"></a>Inicio de la devolución del dispositivo

Para comenzar el proceso de devolución, siga los pasos siguientes.

1. Vaya al recurso de Azure Stack Edge en Azure Portal. En **Información general**, vaya a la barra de comandos en el panel derecho y seleccione **Devolver dispositivo**. 

    ![Devolver dispositivo 1](media/azure-stack-edge-return-device/return-device-1.png)  

2. En la hoja **Devolver dispositivo**, en **Detalles básicos**:

    1. Indique el número de serie del dispositivo. Para obtener el número de serie del dispositivo, vaya a la interfaz de usuario web local del dispositivo y, a continuación, vaya a **Información general**.  
    
       ![Número de serie del dispositivo 1](media/azure-stack-edge-return-device/device-serial-number-1.png) 

    2. Escriba el número de la etiqueta de servicio. El número de la etiqueta del servicio es un identificador con cinco o más caracteres, que es único para el dispositivo. La etiqueta de servicio se encuentra en la esquina inferior derecha del dispositivo (cuando se mira de frente). Extraiga la etiqueta de información (es un panel de etiqueta deslizante). Este panel contiene información del sistema, como la etiqueta de servicio, la NIC, la dirección MAC, etc. 
    
       ![Número de etiqueta de servicio 1](media/azure-stack-edge-return-device/service-tag-number-1.png)

    3. En la lista desplegable, elija un motivo para la devolución.

       ![Devolver dispositivo 2](media/azure-stack-edge-return-device/return-device-2.png) 

3. En **Detalles de envío**:

    1. Escriba su nombre, el nombre de la empresa y la dirección completa de la empresa. Escriba un teléfono del trabajo, incluido el código de área y un identificador de correo electrónico para notificaciones.
    2. Si necesita una caja de devolución, puede solicitarla. Responda **Sí** a la pregunta **Necesita una caja vacía para la devolución**.

    ![Devolver dispositivo 3](media/azure-stack-edge-return-device/return-device-3.png)

4. Revise los **Términos de privacidad** y active la casilla situada junto a la nota para indicar que los ha revisado y los acepta.

5. Seleccione **Initiate return** (Iniciar devolución).

    ![Devolver dispositivo 4](media/azure-stack-edge-return-device/return-device-4.png) 

6. Una vez que se capturan los detalles de la devolución del dispositivo, puede notificar al equipo de operaciones de Azure Stack Edge mediante un correo electrónico. Puede usar su aplicación de correo electrónico, suponiendo que la aplicación de correo electrónico esté instalada y configurada. También puede copiar los datos para crear y enviar un correo electrónico.

    ![Devolver dispositivo 5](media/azure-stack-edge-return-device/return-device-5.png) 

7. Una vez que el equipo de operaciones de Azure Stack Edge reciba el correo electrónico, le enviará una etiqueta de envío inverso. Cuando reciba esta etiqueta, puede programar la recogida del dispositivo con el transportista. 

## <a name="pack-the-device"></a>Empaquetado del dispositivo

Para empaquetar el dispositivo, realice los pasos siguientes.

1. Apague el dispositivo. En la interfaz de usuario web local, vaya a **Mantenimiento > Configuración de encendido**.
2. Seleccione **Apagar**. Cuando se le pida confirmación, haga clic en **Sí** para continuar. Para obtener más información, consulte [Administración de los cables de alimentación](../databox-online/azure-stack-edge-manage-access-power-connectivity-mode.md#manage-power).
3. Desconecte los cables de alimentación y quite todos los cables de red del dispositivo.
4. Prepare cuidadosamente el paquete de envío según las instrucciones siguientes:
    1. Use la caja de envío que solicitó de Azure o la caja de envío original con su empaquetado de espuma. 
    1. Coloque la pieza de espuma inferior en la caja.
    1. Ponga el dispositivo encima de la espuma, teniendo cuidado de que quede colocada perfectamente en ella.
    1. Coloque la pieza de espuma superior en el paquete.
    1. Coloque los cables de alimentación en la bandeja de accesorios y los raíles en la pieza de espuma superior.
    1. Selle la caja y coloque en el paquete la etiqueta de envío que ha recibido de Azure.
    
    > [!IMPORTANT]
    > Si no se cumplen las directrices adecuadas para preparar el envío de devolución, el dispositivo podría dañarse y es posible que se apliquen honorarios por el dispositivo dañado. Revise los [términos del servicio del producto](https://www.microsoft.com/licensing/product-licensing/products) y los [preguntas más frecuentes sobre dispositivos perdidos o dañados](https://azure.microsoft.com/pricing/details/databox/edge/).
 


## <a name="schedule-a-pickup"></a>Programar una recogida

Para programar una recogida, siga los pasos a continuación.

1. Programe una recogida con su transportista regional. Si devuelve el dispositivo en EE. UU., el transportista podría ser UPS o FedEx. Póngase en contacto con UPS para programar la recogida:

    1. Llame a la oficina local de UPS (número gratuito específico del país o región).
    2. En la llamada, indique el número de seguimiento del envío inverso que se muestra en la etiqueta impresa.
    3. Si no indica el número de seguimiento, UPS le exigirá que pague una cantidad adicional en la recogida.

    En lugar de programar la recogida, puede devolver Azure Stack Edge en el punto de recogida más cercano.

## <a name="delete-the-resource"></a>Eliminar el recurso

Una vez recibido en el centro de datos de Azure, el dispositivo se inspecciona para detectar daños o signos de manipulación.

- Si el dispositivo llega intacto y en buen estado, el medidor de facturación se detiene para ese recurso. El equipo de operaciones de Azure Stack Edge se pondrá en contacto con usted para confirmar que se ha devuelto el dispositivo. A continuación, puede eliminar el recurso asociado al dispositivo en Azure Portal.
- Si el dispositivo llega significativamente dañado, es posible que se apliquen cargos. Para obtener más información, consulte las [preguntas más frecuentes sobre dispositivos perdidos o dañados](https://azure.microsoft.com/pricing/details/databox/edge/) y los [términos del servicio del producto](https://www.microsoft.com/licensing/product-licensing/products).  


Puede eliminar el dispositivo en Azure Portal:

- Después de realizar un pedido y antes de que Microsoft prepare el dispositivo.
- Después de devolver un dispositivo a Microsoft y de que el equipo de operaciones de Azure Stack Edge haya llamado para confirmar que se ha devuelto. El equipo de operaciones no llama hasta que el dispositivo devuelto supere la inspección física en el centro de datos de Azure.

Si ha activado el dispositivo en otra suscripción o ubicación, Microsoft moverá el pedido a la nueva suscripción o ubicación en el plazo de un día laborable. Una vez movido el pedido, puede eliminar este recurso.


Siga estos pasos para eliminar el dispositivo y el recurso en Azure Portal.

1. En Azure Portal, vaya al recurso y luego a **Introducción**. En la barra de comandos, seleccione **Eliminar**.

    ![Seleccionar Eliminar](media/azure-stack-edge-return-device/delete-resource-1.png)

2. En la hoja **Eliminar dispositivo**, escriba el nombre del dispositivo que quiere eliminar y seleccione **Eliminar**.

    ![Confirmar eliminación](media/azure-stack-edge-return-device/delete-resource-2.png)

Se le notificará una vez el dispositivo y el recurso asociado se hayan eliminado correctamente.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda cómo [obtener el reemplazo de un dispositivo de Azure Stack Edge](azure-stack-edge-replace-device.md).