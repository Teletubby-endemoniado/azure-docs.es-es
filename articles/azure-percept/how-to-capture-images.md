---
title: Captura de imágenes en Azure Percept Studio
description: Aprenda a capturar imágenes con Azure Percept DK en Azure Percept Studio
author: NabilaBabar
ms.author: amiyouss
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/12/2021
ms.custom: template-how-to, ignite-fall-2021
ms.openlocfilehash: 0712c10df8f987237df74d2078519afbd70e72f1
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131006589"
---
# <a name="capture-images-in-azure-percept-studio"></a>Captura de imágenes en Azure Percept Studio

Siga esta guía para capturar imágenes mediante Azure Percept DK para un proyecto de visión existente. Si aún no ha creado un proyecto de visión, consulte el [tutorial de visión sin código](./tutorial-nocode-vision.md).

## <a name="prerequisites"></a>Requisitos previos

- Kit de desarrollo Azure Percept DK
- [Suscripción de Azure](https://azure.microsoft.com/free/)
- [Experiencia de instalación de Azure Percept DK](./quickstart-percept-dk-set-up.md): ya ha conectado el dispositivo DevKit a una red Wi-Fi, creado una instancia de IoT Hub y conectado el dispositivo DevKit a esa instancia.
- [Proyecto de visión sin código](./tutorial-nocode-vision.md)

## <a name="capture-images"></a>Captura de imágenes

1. Encienda el dispositivo DevKit.

1. Vaya a [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. En el lado izquierdo de la página de información general, seleccione **Dispositivos**.

    :::image type="content" source="./media/how-to-capture-images/overview-devices-inline.png" alt-text="Pantalla de información general de Azure Percept Studio." lightbox="./media/how-to-capture-images/overview-devices.png":::

1. Seleccione el dispositivo DevKit de la lista.

    :::image type="content" source="./media/how-to-capture-images/select-device.png" alt-text="Lista de dispositivos Percept.":::

1. En la página del dispositivo, seleccione **Capturar imágenes para un proyecto**.

    :::image type="content" source="./media/how-to-capture-images/capture-images.png" alt-text="Página de dispositivos Percept con las acciones disponibles enumeradas.":::

1. En la ventana **Captura de imágenes**, siga estos pasos:

    1. En el menú desplegable **Project** (Proyecto), seleccione el proyecto de visión para el que desea recopilar imágenes.

    1. Haga clic en **Ver flujo de dispositivo** para asegurarse de que la cámara del módulo de sistema de visión está correctamente colocada.

    1. Haga clic en **Hacer una foto** para capturar una imagen.

    1. Como alternativa, active la casilla junto a **Captura de imágenes automática** para configurar un temporizador para la captura de imágenes:

        1. Seleccione la velocidad de creación de imágenes preferida en **Capture rate** (Velocidad de captura).
        1. Seleccione el número total de imágenes que quiere recopilar en **Target** (Destino).

    :::image type="content" source="./media/how-to-capture-images/take-photo.png" alt-text="Pantalla Captura de imagen.":::

Todas las imágenes serán accesibles en [Custom Vision](https://www.customvision.ai/).

## <a name="next-steps"></a>Pasos siguientes

[Pruebe y vuelva a entrenar su modelo de visión sin código](../cognitive-services/custom-vision-service/test-your-model.md).
