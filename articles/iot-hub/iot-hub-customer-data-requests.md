---
title: Solicitudes de datos de clientes para dispositivos de Azure IoT Hub
description: La mayoría de los dispositivos administrados en Azure IoT Hub no son personales, pero algunos lo son. En este artículo se habla sobre los administradores que pueden exportar o eliminar datos personales de un dispositivo.
author: eross-msft
ms.author: lizross
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 20a8d6c39b33a60277478605e3b4855aad04b55c
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132555155"
---
# <a name="customer-data-request-features-for-azure-iot-hub-devices"></a>Características de las solicitudes de datos de clientes para dispositivos de Azure IoT Hub

Azure IoT Hub es un servicio en la nube basado en la API de REST destinado a los clientes de empresas que permite una comunicación segura y bidireccional entre millones de dispositivos y un servicio de Azure con particiones.

[!INCLUDE [gdpr-related guidance](../../includes/gdpr-intro-sentence.md)]

Un administrador de inquilinos asigna un número de identificador (id. de dispositivo) a cada dispositivo. Los datos del dispositivo se basan en el id. de dispositivo asignado. Microsoft no retiene ninguna información y no tiene acceso a datos que permitan la correlación entre el id. de dispositivo y el usuario.

Muchos de los dispositivos administrados en Azure IoT Hub no son dispositivos personales; por ejemplo, el termostato de una oficina o el robot de una fábrica. Sin embargo, los clientes pueden considerar que algunos dispositivos se pueden identificar a nivel personal y, a su entera discreción, pueden mantener sus propios métodos de seguimiento de activos o de inventario que vinculan dispositivos a personas. Azure IoT Hub administra y almacena todos los datos asociados con dispositivos como si fueran datos personales.

Los administradores de inquilinos pueden usar Azure Portal o las API de REST del servicio para atender las solicitudes de información mediante la exportación o eliminación de los datos asociados con un id. de dispositivo.

Si usa la característica de enrutamiento del servicio Azure IoT Hub para reenviar mensajes de dispositivo a otros servicios, entonces, el administrador de inquilinos debe realizar solicitudes de datos para cada punto de conexión de enrutamiento con el fin de completar una solicitud completa para un determinado dispositivo. Para más información, consulte la documentación de referencia de cada punto de conexión. Si quiere obtener más información sobre los puntos de conexión compatibles, consulte [Referencia: puntos de conexión de IoT Hub](iot-hub-devguide-endpoints.md).

Si usa la característica de integración Azure Event Grid del servicio de Azure IoT Hub, entonces, el administrador de inquilinos debe realizar solicitudes de datos para cada suscriptor de estos eventos. Para obtener más información, consulte la sección sobre cómo [responder a eventos de IoT Hub mediante el uso de Event Grid](iot-hub-event-grid.md).

Si usa la característica de integración de Azure Monitor del servicio Azure IoT Hub para crear registros de recursos, entonces el administrador de inquilinos debe realizar las solicitudes de datos con los registros almacenados. Para más información, consulte [Supervisión de IoT Hub](monitor-iot-hub.md).

## <a name="deleting-customer-data"></a>Eliminación de datos del cliente

Los administradores de inquilinos pueden usar la hoja de dispositivos de IoT de la extensión de Azure IoT Hub de Azure Portal para eliminar un dispositivo. De ese modo, se eliminarán los datos asociados con ese dispositivo.

También se pueden realizar operaciones de eliminación para dispositivos mediante las API de REST. Para más información, consulte [Service - Delete Device](/azure/iot-hub/iot-c-sdk-ref/iothub-registrymanager-h/iothubregistrymanager-deletedevice) (Servicio - Eliminar servicio).

## <a name="exporting-customer-data"></a>Exportación de datos del cliente

Los administradores de inquilinos pueden copiar y pegar en la hoja de dispositivos de IoT de la extensión de Azure IoT Hub en Azure Portal para exportar los datos asociados a un dispositivo.

También se pueden realizar operaciones de exportación de dispositivos mediante las API de REST. Para más información, consulte [Service - Get Device](/azure/iot-hub/iot-c-sdk-ref/iothub-registrymanager-h/iothubregistrymanager-getdevice) (Servicio - Obtener dispositivo).

> [!NOTE]
> Cuando se usan servicios para empresas de Microsoft, Microsoft genera cierta información, conocida como registros generados por el sistema. Los administradores de inquilinos no pueden acceder a ciertos registros generados por el sistema de Azure IoT Hub ni exportarlos. Estos registros constituyen acciones objetivas que se realizan en el servicio y datos de diagnóstico relacionados con dispositivos individuales.

## <a name="links-to-additional-documentation"></a>Vínculos a documentación adicional

La documentación completa de las API del servicio Azure IoT Hub se encuentra en [API del servicio IoT Hub](/rest/api/iothub/service/configuration).