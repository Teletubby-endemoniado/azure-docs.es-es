---
title: Compatibilidad de Azure IoT Hub Device Provisioning Service (DPS) con redes virtuales
description: Uso del patrón de conectividad de redes virtuales con Azure IoT Hub Device Provisioning Service (DPS)
services: iot-dps
author: anastasia-ms
ms.service: iot-dps
manager: lizross
ms.topic: conceptual
ms.date: 10/06/2021
ms.author: v-stharr
ms.openlocfilehash: 8d90a033f5af5afb55be9585756a7235dc6d89d7
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129659324"
---
# <a name="azure-iot-hub-device-provisioning-service-dps-support-for-virtual-networks"></a>Compatibilidad de Azure IoT Hub Device Provisioning Service (DPS) con redes virtuales

En este artículo se presenta el patrón de conectividad de red virtual (VNET) para el aprovisionamiento de dispositivos IoT con centros de IoT mediante DPS. Este patrón proporciona conectividad privada entre los dispositivos, DPS y el centro de IoT dentro de una red virtual de Azure propiedad del cliente. 

En la mayoría de los escenarios en los que DPS se ha configurado con una red virtual, la instancia de IoT Hub también se configurará en la misma red virtual. Para información más específica sobre la compatibilidad y la configuración de una red virtual para instancias de IoT Hub, consulte [Compatibilidad de IoT Hub con redes virtuales mediante Private Link e identidad administrada](../iot-hub/virtual-network-support.md).

## <a name="introduction"></a>Introducción

De forma predeterminada, los nombres de host de DPS se asignan a un punto de conexión público con una dirección IP enrutable públicamente a través de Internet. Este punto de conexión público es visible para todos los clientes. Los dispositivos IoT pueden intentar acceder al punto de conexión público a través de redes de área extensa y redes locales.

Por varios motivos, es posible que los clientes deseen restringir la conectividad a los recursos de Azure, como DPS. Estas razones incluyen las siguientes:

* Impedir la exposición de la conexión a través de Internet pública. La exposición puede reducirse introduciendo más capas de seguridad a través del aislamiento de nivel de red para los recursos de IoT Hub y DPS

* Habilitación de una experiencia de conectividad privada desde los recursos de la red local, lo que garantiza que sus datos y tráfico se transmiten directamente a la red troncal de Azure.

* Prevención de ataques de exfiltración de redes locales confidenciales. 

* Seguimiento de los patrones de conectividad generales de Azure establecidos mediante [puntos de conexión privados](../private-link/private-endpoint-overview.md).

Entre los enfoques comunes para restringir la conectividad se incluyen [las reglas de filtro de direcciones IP de DPS](./iot-dps-ip-filtering.md) y la red virtual (VNET) con [puntos de conexión privados](../private-link/private-endpoint-overview.md). El objetivo de este artículo es describir el enfoque de la red virtual para DPS con puntos de conexión privados. 

Los dispositivos que operan en redes locales pueden usar el emparejamiento privado de [red privada virtual (VPN)](../vpn-gateway/vpn-gateway-about-vpngateways.md) o [ExpressRoute](https://azure.microsoft.com/services/expressroute/) para conectarse a una red virtual en Azure y acceder a los recursos de DPS a través de puntos de conexión privados. 

Un punto de conexión privado es una dirección IP privada asignada en una red virtual propiedad de un cliente a través de la cual se puede acceder a un recurso de Azure. Al tener un punto de conexión privado para el recurso de DPS, podrá permitir que los dispositivos que funcionan dentro de la red virtual soliciten el aprovisionamiento por el recurso de DPS sin permitir el tráfico al punto de conexión público.

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, asegúrese de que se cumplen los requisitos previos siguientes:

* El recurso de DPS ya está creado y vinculado a los centros de IoT. Para obtener instrucciones sobre cómo configurar un nuevo recurso de DPS, consulte [Configuración de Azure IoT Hub Device Provisioning Service con Azure Portal](./quick-setup-auto-provision.md).

* Ha aprovisionado una red virtual de Azure con una subred en la que se creará el punto de conexión privado. Para más información, consulte [Creación de una red virtual mediante la CLI de Azure](../virtual-network/quick-create-cli.md).

* En el caso de los dispositivos que operan en redes locales, configure la [red privada virtual (VPN)](../vpn-gateway/vpn-gateway-about-vpngateways.md) o el emparejamiento privado de [ExpressRoute](https://azure.microsoft.com/services/expressroute/) en su red virtual de Azure.

## <a name="private-endpoint-limitations"></a>Limitaciones del punto de conexión privado

Tenga en cuenta las siguientes limitaciones actuales de DPS cuando use puntos de conexión privados:

* Los puntos de conexión privados no funcionarán con DPS cuando el recurso de DPS y el concentrador vinculado se encuentran en nubes diferentes. Por ejemplo, [Azure Government y Azure global](../azure-government/documentation-government-welcome.md).

* Actualmente, [las directivas de asignación personalizadas con Azure Functions](how-to-use-custom-allocation-policies.md) para DPS no funcionarán cuando la función de Azure esté bloqueada en una red virtual y en los puntos de conexión privados. 

* La compatibilidad actual de la red virtual de DPS es solo para la entrada de datos en DPS. La salida de datos, que es el tráfico desde DPS hasta IoT Hub, usa un mecanismo de servicio a servicio interno en lugar de una red virtual dedicada. La compatibilidad con el bloqueo de salida basado en red virtual completo entre DPS y IoT Hub no está disponible actualmente.

* La directiva de asignación de latencia más baja se usa para asignar un dispositivo al centro de IoT con la latencia más baja. Esta directiva de asignación no es fiable en un entorno de red virtual. 

>[!NOTE]
>**Consideración sobre la residencia de datos:**
>
>DPS proporciona un **punto de conexión global del dispositivo** (`global.azure-devices-provisioning.net`). Sin embargo, cuando se usa el punto de conexión global, los datos se pueden redirigir fuera de la región en la que se creó inicialmente la instancia de DPS. Para garantizar la residencia de datos dentro de la región de DPS inicial, use puntos de conexión privados.

## <a name="set-up-a-private-endpoint"></a>Configuración de un punto de conexión privado

Para configurar un punto de conexión privado, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), abra el recurso de DPS y haga clic en la pestaña **Redes**. Haga clic en **Conexiones de punto de conexión privado** y **+ Punto de conexión privado**.

    ![Incorporación de un nuevo punto de conexión privado para DPS](./media/virtual-network-support/networking-tab-add-private-endpoint.png)

2. En la página _Crear un punto de conexión privado_ de la sección Datos básicos, escriba la información que se menciona en la tabla siguiente.

    ![Aspectos básicos de Crear un punto de conexión privado](./media/virtual-network-support/create-private-endpoint-basics.png)

    | Campo | Valor |
    | :---- | :-----|
    | **Suscripción** | Elija la suscripción de Azure que desea que contenga el punto de conexión privado.  |
    | **Grupos de recursos** | Elija o cree un grupo de recursos que contenga el punto de conexión privado. |
    | **Nombre**       | Escriba un nombre para el punto de conexión privado. |
    | **Región**     | La región elegida debe ser la misma que la región que contiene la red virtual, pero no tiene que ser la misma que la del recurso de DPS. |

    Haga clic en **Siguiente: Recurso** para configurar el recurso al que apuntará el punto de conexión privado.

3. En la página _Crear un punto de conexión privado_ de la sección Recurso, escriba la información que se menciona en la tabla siguiente.

    ![Recurso de Crear un punto de conexión privado](./media/virtual-network-support/create-private-endpoint-resource.png)

    | Campo | Valor |
    | :---- | :-----|
    | **Suscripción**        | Elija la suscripción de Azure que contiene el recurso de DPS al que apuntará el punto de conexión privado.  |
    | **Tipo de recurso**       | Elija **Microsoft.Devices/ProvisioningServices**. |
    | **Recurso**            | Seleccione el recurso de DPS al que se asignará el punto de conexión privado. |
    | **Recurso secundario de destino** | Seleccione **iotDps**. |

    > [!TIP]
    > En la sección [Solicitud de un punto de conexión privado](#request-a-private-endpoint) de este artículo se proporciona información sobre la opción de configuración **Conéctese a un recurso de Azure por identificador de recurso o alias**.


    Haga clic en **Siguiente: Configuración** para configurar la red virtual para el punto de conexión privado.

4. En la página _Crear un punto de conexión privado_ de la sección Configuración, elija la red virtual y la subred donde se creará el punto de conexión privado.

    Haga clic en **Siguiente: Etiquetas** y, de manera opcional, proporcione las etiquetas del recurso.

    ![Configuración de un punto de conexión privado](./media/virtual-network-support/create-private-endpoint-configuration.png)

5. Haga clic en **Revisar y crear** y luego en **Crear** para crear el recurso del punto de conexión privado.

## <a name="use-private-endpoints-with-devices"></a>Uso de puntos de conexión privados con dispositivos

Para usar puntos de conexión privados con código de aprovisionamiento de dispositivos, el código de aprovisionamiento debe usar el **punto de conexión de servicio** específico para el recurso de DPS, como se muestra en la página de información general del recurso de DPS en [Azure Portal](https://portal.azure.com). El punto de conexión de servicio tiene el siguiente formulario.

`<Your DPS Tenant Name>.azure-devices-provisioning.net`

La mayor parte del código de ejemplo que se muestra en nuestra documentación y SDK, usa el **punto de conexión de dispositivo global** (`global.azure-devices-provisioning.net`) y el **ámbito de identificador** para resolver un recurso de DPS determinado. Use el punto de conexión de servicio en lugar del punto de conexión global del dispositivo al conectarse a un recurso de DPS mediante puntos de conexión privados para aprovisionar los dispositivos.

Por ejemplo, la muestra de cliente del dispositivo de aprovisionamiento ([pro_dev_client_sample](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client/samples/prov_dev_client_sample)) en [SDK de Azure IoT](https://github.com/Azure/azure-iot-sdk-c) está diseñado para usar el **punto de conexión de dispositivo global** como URI de aprovisionamiento global (`global_prov_uri`) en [prov_dev_client_sample.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c)

:::code language="c" source="~/iot-samples-c/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c" range="60-64" highlight="4":::

:::code language="c" source="~/iot-samples-c/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c" range="138-144" highlight="3":::

Para usar la muestra con un punto de conexión privado, el código resaltado anteriormente se cambiaría para usar el punto de conexión de servicio para el recurso de DPS. Por ejemplo, si el punto de conexión de servicio fuera `mydps.azure-devices-provisioning.net`, el código tendría el siguiente aspecto.

```C
static const char* global_prov_uri = "global.azure-devices-provisioning.net";
static const char* service_uri = "mydps.azure-devices-provisioning.net";
static const char* id_scope = "[ID Scope]";
```

```C
    PROV_DEVICE_RESULT prov_device_result = PROV_DEVICE_RESULT_ERROR;
    PROV_DEVICE_HANDLE prov_device_handle;
    if ((prov_device_handle = Prov_Device_Create(service_uri, id_scope, prov_transport)) == NULL)
    {
        (void)printf("failed calling Prov_Device_Create\r\n");
    }
```


## <a name="request-a-private-endpoint"></a>Solicitud de un punto de conexión privado

Puede solicitar un punto de conexión privado a un recurso de DPS por identificador de recurso. Para hacer esta solicitud, necesita que el propietario del recurso le proporcione el identificador de este. 

1. El identificador de recurso se proporciona en la pestaña Propiedades del recurso de DPS, como se muestra a continuación.

    ![Pestaña Propiedades de DPS](./media/virtual-network-support/dps-properties.png)

    > [!CAUTION]
    > Tenga en cuenta que el identificador de recurso contiene el identificador de suscripción. 

2. Una vez que tenga el identificador de recurso, siga los pasos descritos anteriormente en [Configuración de un punto de conexión privado](#set-up-a-private-endpoint) hasta el paso 3 en la página _Crear un recurso de punto de conexión_ de la sección Recurso. Haga clic en **Conéctese a un recurso de Azure por identificador de recurso o alias** y escriba la información en la tabla siguiente. 

    | Campo | Value |
    | :---- | :-----|
    | **Identificador de recurso o alias** | Escriba el identificador de recurso para el recurso de DPS. |
    | **Recurso secundario de destino** | Escriba **iotDps**. |
    | **Mensaje de solicitud** | Escriba un mensaje de solicitud para el propietario del recurso de DPS.<br>Por ejemplo, <br>`Please approve this new private endpoint`<br>`for IoT devices in site 23 to access this DPS instance`  |

    Haga clic en **Siguiente: Configuración** para configurar la red virtual para el punto de conexión privado.

3. En la página _Crear un punto de conexión privado_ de la sección Configuración, elija la red virtual y la subred donde se creará el punto de conexión privado.
 
    Haga clic en **Siguiente: Etiquetas** y, de manera opcional, proporcione las etiquetas del recurso.

4. Haga clic en **Revisar y crear** y luego en **Crear** para crear la solicitud del punto de conexión privado.

5. El propietario de DPS verá la solicitud del punto de conexión privado en la lista **Conexiones de punto de conexión privado** en la pestaña Redes de DPS. En esa página, el propietario tiene las opciones **Aprobar** o **Rechazar** para llevar a cabo la acción correspondiente en el punto de conexión privado, como se muestra a continuación.

    ![Aprobación de DPS](./media/virtual-network-support/approve-dps-private-endpoint.png)


## <a name="pricing-private-endpoints"></a>Precios de los puntos de conexión privados

Para más información sobre los precios, consulte [Precios de Azure Private Link](https://azure.microsoft.com/pricing/details/private-link).



## <a name="next-steps"></a>Pasos siguientes

Use los vínculos siguientes para obtener más información sobre las características de seguridad de DPS:

* [Seguridad](./concepts-service.md#attestation-mechanism)
* [Compatibilidad con TLS 1.2](tls-support.md)