---
title: 'Tutorial: Conmutación por error manual de Azure IoT Hub | Microsoft Docs'
description: 'Tutorial: Aprenda a realizar una conmutación por error manual de Azure IoT Hub en otra región y a confirmar que funciona, y después a devolverlo a la región original y volver a comprobarlo.'
author: eross-msft
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 08/10/2021
ms.author: lizross
ms.custom:
- mvc
- mqtt
ms.openlocfilehash: 2687316551f7f1a7f11cd035130823c19d10b01a
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132550446"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub"></a>Tutorial: Realización de una conmutación por error manual de una instancia de IoT Hub

La conmutación por error manual es una característica del servicio IoT Hub que permite a los clientes [realizar la conmutación por error](https://en.wikipedia.org/wiki/Failover) de las operaciones de su centro desde una región primaria a la región de Azure emparejada geográficamente correspondiente. La conmutación por error manual se puede realizar si se produce un desastre regional o una interrupción prolongada del servicio. También se puede realizar una conmutación por error planeada para probar las funcionalidades de recuperación ante desastres, aunque se recomienda usar un centro de IoT de prueba, en lugar del centro que se ejecuta en producción. La característica de conmutación por error manual se ofrece a los clientes sin costo adicional para los centros de IoT creados después del 18 de mayo de 2017.

En este tutorial se realizan las siguientes tareas:

> [!div class="checklist"]
> * Crear un centro de IoT mediante Azure Portal. 
> * Realizar una conmutación por error. 
> * Ver el centro que se ejecutan en la ubicación secundaria.
> * Realizar una conmutación por recuperación para devolver las operaciones del centro de IoT a la ubicación principal. 
> * Confirmar que el concentrador se ejecuta adecuadamente en la ubicación correcta.

Para obtener más información sobre la conmutación por error manual y la conmutación por error iniciada por Microsoft con IoT Hub, consulte [Recuperación ante desastres entre regiones](iot-hub-ha-dr.md#cross-region-dr).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Asegúrese de que está abierto el puerto 8883 del firewall. En el dispositivos de ejemplo de este tutorial se usa el protocolo MQTT, que se comunica mediante el puerto 8883. Este puerto puede estar bloqueado en algunos entornos de red corporativos y educativos. Para más información y para saber cómo solucionar este problema, consulte el artículo sobre la [conexión a IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="perform-a-manual-failover"></a>Realización de una conmutación por error manual

> [!NOTE]
> Hay un límite de dos conmutaciones por error y dos conmutaciones por recuperación al día en un centro de IoT.

1. Haga clic en **Grupos de recursos** y, a continuación, seleccione el grupo de recursos. En la lista de recursos, haga clic en su centro.

1. En **Configuración del centro** en el panel de IoT Hub, haga clic en **Conmutación por error**.

:::image type="content" source="./media/tutorial-manual-failover/trigger-failover-01.png" alt-text="Captura de pantalla que muestra el panel de propiedades del centro de IoT":::

1. En el panel de conmutación por error manual, verá la **ubicación actual** y la **ubicación de la conmutación por error**. La ubicación actual siempre indica la ubicación en la que el centro está activo actualmente. La ubicación de conmutación por error es la [región emparejada geográficamente de Azure](../best-practices-availability-paired-regions.md) estándar que está emparejada con la ubicación actual. Los valores de las ubicaciones no se pueden cambiar. En este tutorial, la ubicación actual es `West US 2` y la ubicación de conmutación por error es `West Central US`.

   ![Captura de pantalla que muestra el panel Conmutación por error manual](./media/tutorial-manual-failover/trigger-failover-02.png)

1. En la parte superior del panel de conmutación por error manual, haga clic en **Iniciar conmutación por error**. 

1. En el panel de confirmación, escriba el nombre de la instancia de IoT Hub para confirmar que es el que quiere usar para la conmutación por error. Luego, para iniciar la conmutación por error, haga clic en **Conmutación por error**.

   La cantidad de tiempo que tarda en realizarse la conmutación por error manual es proporcional al número de dispositivos registrados en el centro. Por ejemplo, si tiene 100 000, podría tardar 15 minutos, pero si tiene 5 000 000, podría tardar una hora, o incluso más.

   ![Captura de pantalla que muestra el panel de confirmación de la conmutación por error manual](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Mientras se ejecuta el proceso de conmutación por error manual, aparece un banner que indica que hay una conmutación por error manual en curso. 

   ![Captura de pantalla que muestra una conmutación por error manual en curso](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   Si cierra el panel de IoT Hub y hace clic en él para abrirlo de nuevo en el panel Grupo de recursos, verá un banner que le indica que el centro se encuentra en medio de una conmutación por error manual. 

   ![Captura de pantalla que muestra la conmutación por error de IoT Hub en curso](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   Una vez finalizada, las regiones actual y de conmutación por error de la página Conmutación por error manual se invierten y el centro vuelve a estar activo. En este ejemplo, la ubicación actual es ahora `WestCentralUS` y la ubicación de conmutación por error es ahora `West US 2`. 

   ![Captura de pantalla que muestra la conmutación por error finalizada](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

   En la página de información general también se muestra un aviso que indica que la conmutación por error se ha completado y que IoT Hub se está ejecutando en `West Central US`.

   ![Captura de pantalla que muestra la conmutación por error completada en la página de información general](./media/tutorial-manual-failover/trigger-failover-06-finished-overview.png)


## <a name="perform-a-failback"></a>Realización de una conmutación por recuperación 

Después de haber realizado una conmutación por error manual, puede devolver las operaciones del centro a la región primaria original (lo que se conoce como conmutación por recuperación). Si acaba de realizar una conmutación por error, tendrá que esperar aproximadamente una hora para poder solicitar una conmutación por recuperación. Si intenta realizar la conmutación por recuperación en un período más corto, aparecerá un mensaje de error.

Las conmutaciones por recuperación se realizan de la misma forma que las conmutación por error manuales. Estos son los pasos necesarios: 

1. Para realizar una conmutación por recuperación, vuelva al panel IoT Hub de su centro de IoT.

2. En **Configuración** en el panel de IoT Hub, haga clic en **Conmutación por error**. 

3. En la parte superior del panel de conmutación por error manual, haga clic en **Iniciar conmutación por error**. 

4. En el panel de confirmación, escriba el nombre de la instancia de IoT Hub para confirmar que es el que quiere conmutar por recuperación. Para iniciar la conmutación por recuperación, haga clic en **Aceptar**. 

   ![Captura de pantalla de solicitud de conmutación por recuperación manual](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Los banners se muestran como se explica en la sección acerca de la realización de una conmutación por error. Cuando la conmutación por recuperación termina, vuelve a mostrar `West US 2` como la ubicación actual y `West Central US` como la ubicación de conmutación por recuperación, como se estableció originalmente.

## <a name="clean-up-resources"></a>Limpieza de recursos 

Para quitar los recursos que ha creado para este tutorial, elimine el grupo de recursos. Esta acción elimina también todos los recursos del grupo. En este caso, quita el centro de IoT y el propio grupo de recursos. 

1. Haga clic en **Grupos de recursos**. 

2. Busque y seleccione el grupo de recursos **ManlFailRG**. Haga clic en ella para abrirla. 

3. Haga clic en **Eliminar grupo de recursos**. Cuando se le pida, escriba el nombre del grupo de recursos y haga clic en **Eliminar** para confirmarlo. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a configurar y realizar una conmutación por error manual y a solicitar una conmutación por recuperación mediante la realización de las siguientes tareas:

> [!div class="checklist"]
> * Crear un centro de IoT mediante Azure Portal. 
> * Realizar una conmutación por error. 
> * Ver el centro que se ejecutan en la ubicación secundaria.
> * Realizar una conmutación por recuperación para devolver las operaciones del centro de IoT a la ubicación principal. 
> * Confirmar que el concentrador se ejecuta adecuadamente en la ubicación correcta.

Avance al siguiente tutorial para aprender a configurar el dispositivo desde un servicio de back-end. 

> [!div class="nextstepaction"]
> [Configuración de los dispositivos](tutorial-device-twins.md)
