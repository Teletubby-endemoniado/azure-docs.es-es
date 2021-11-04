---
title: Introducción a Azure IoT Central | Microsoft Docs
description: Azure IoT Central es una plataforma de aplicaciones de IoT que simplifica la creación de soluciones de IoT y ayuda a reducir la carga y el costo de las operaciones de administración de IoT y el desarrollo. En este artículo se proporciona información general sobre las características de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 04/19/2021
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc, contperf-fy21q2, contperf-fy22q1
ms.openlocfilehash: 98de0994e430ccf8bc3ab5026e9ef739c603368a
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131024284"
---
# <a name="what-is-azure-iot-central"></a>¿Qué es Azure IoT Central?

IoT Central es una plataforma de aplicaciones de IoT que reduce la carga y el costo del desarrollo, la administración y el mantenimiento de soluciones de IoT de nivel empresarial. La elección de compilar con IoT Central ofrece la oportunidad de centrar el tiempo, el dinero y la energía en transformar su negocio con datos de IoT, en lugar de simplemente mantener y actualizar una infraestructura de IoT compleja y continuamente en constante evolución.

La interfaz de usuario web le permite conectar rápidamente dispositivos, supervisar sus condiciones, crear reglas y administrar millones de dispositivos y sus datos a lo largo de su ciclo de vida. Además, le permite actuar sobre la información del dispositivo mediante la extensión de IoT Intelligence en aplicaciones de línea de negocio.

Para IoT Central, en este artículo se describe lo siguiente:

- Los roles típicos de usuario asociados a un proyecto.
- Cómo crear una aplicación.
- Conexión de los dispositivos a la aplicación.
- Integración de la aplicación con otros servicios.
- Administración de la aplicación.
- Opciones de precios.

## <a name="user-roles"></a>Roles de usuario

La documentación IoT Central hace referencia a cuatro roles de usuario que interactúan con una aplicación de IoT Central:

- Un _compilador de soluciones_ es responsable de [crear una aplicación](quick-deploy-iot-central.md), [configurar reglas y acciones](quick-configure-rules.md), [definir integraciones con otros servicios](quick-export-data.md) y personalizar aún más la aplicación para operadores y desarrolladores de dispositivos.
- Un _operador_, que [administra los dispositivos](howto-manage-devices-individually.md) conectados a la aplicación.
- Un _administrador_ es responsable de tareas administrativas como la administración de [roles de usuario y permisos](howto-administer.md) en la aplicación y de la [configuración de identidades administradas](howto-manage-iot-central-from-portal.md#configure-a-managed-identity) para proteger las conexiones con otros servicios.
- Un _desarrollador de dispositivos_ [crea el código que se ejecuta en un dispositivo](concepts-telemetry-properties-commands.md) o un [módulo de IoT Edge](concepts-iot-edge.md) conectado a la aplicación.

## <a name="create-your-iot-central-application"></a>Creación de la aplicación IoT Central

Puede implementar rápidamente una aplicación de IoT Central nueva y, luego, personalizarla según sus requisitos específicos. Comience con una _plantilla de aplicación_ genérica o con una de las enfocadas al sector:

- [Minoristas](../retail/overview-iot-central-retail.md)
- [Sector energético](../energy/overview-iot-central-energy.md)
- [Gobierno](../government/overview-iot-central-government.md)
- [Atención sanitaria](../healthcare/overview-iot-central-healthcare.md).

Consulte el inicio rápido [Creación de una aplicación](quick-deploy-iot-central.md) para ver un tutorial sobre cómo crear la primera aplicación.

## <a name="connect-devices"></a>Conexión de dispositivos

Después de crear la aplicación, el primer paso es crear y conectar los dispositivos. Cada dispositivo conectado a IoT Central usa una _plantilla de dispositivo_. Una plantilla de dispositivo es el plano técnico que define las características y el comportamiento de un tipo de dispositivo, por ejemplo:

- La telemetría que envía. Los ejemplos incluyen la temperatura y la humedad. La telemetría es la transmisión de datos.
- Las propiedades empresariales que un operador puede modificar. Los ejemplos incluyen una dirección de cliente y la fecha en que se prestó servicio por última vez.
- Las propiedades del dispositivo que se establecen mediante un dispositivo y que son de solo lectura en la aplicación. Por ejemplo, el estado de una válvula, abierta o cerrada.
- Propiedades, que un operador establece, que determinan el comportamiento del dispositivo. Por ejemplo, la temperatura de destino del dispositivo.
- Comandos, a los que un operador puede llamar, que se ejecutan en un dispositivo. Por ejemplo, un comando para reiniciar un dispositivo de forma remota.

Cada [plantilla de dispositivo](howto-set-up-template.md) incluye:

- Un _modelo de dispositivo_ que describe las funcionalidades que debe implementar el dispositivo. Entre estas funcionalidades, se incluyen:

  - La telemetría que transmite a IoT Central.
  - Las propiedades de solo lectura que utiliza para notificar el estado a IoT Central.
  - Las propiedades de escritura que recibe de IoT Central para establecer el estado del dispositivo.
  - Los comandos a los que se llama desde IoT Central.

- Propiedades de la nube que no están almacenadas en el dispositivo.
- Personalizaciones, formularios y vistas de dispositivo que forman parte de la aplicación IoT Central.

Tiene varias opciones para crear plantillas de dispositivo:

- Diseñe la plantilla de dispositivo en IoT Central e implemente el modelo de dispositivo en el código del dispositivo.
- Cree un modelo de dispositivo mediante Visual Studio Code y publíquelo en un repositorio. Implemente el código del dispositivo desde el modelo y conecte el dispositivo a la aplicación de IoT Central. IoT Central encuentra el modelo de dispositivo en el repositorio y crea automáticamente una plantilla de dispositivo simple.
- Cree un modelo de dispositivo mediante Visual Studio Code. Implemente el código del dispositivo a partir del modelo. Importe manualmente el modelo de dispositivo en la aplicación de IoT Central y agregue las propiedades, las personalizaciones y las vistas de la nube que la aplicación de IoT Central necesite.

### <a name="customize-the-ui"></a>Personalización de la interfaz de usuario

También puede personalizar la interfaz de usuario de la aplicación de IoT Central para los operadores responsables del uso cotidiano de la aplicación. Las personalizaciones que puede realizar incluyen:

- Configurar paneles personalizados para ayudar a los operadores a descubrir información y resolver los problemas con mayor rapidez.
- Configurar análisis personalizados para explorar los datos de series temporales de los dispositivos conectados.
- Definir el diseño de las propiedades y la configuración en una plantilla de dispositivo.

## <a name="manage-your-devices"></a>Administración de los dispositivos

Como operador, se usa la aplicación IoT Central para [administrar los dispositivos](howto-manage-devices-individually.md) de la solución de IoT Central. Los operadores realizan tareas como:

- Supervisar los dispositivos conectados a la aplicación.
- Solucionar problemas y errores de los dispositivos.
- Aprovisionar dispositivos nuevos.

Puede [definir reglas y acciones personalizadas](howto-configure-rules.md) que se aplican a la transmisión de datos desde los dispositivos conectados. Un operador puede habilitar o deshabilitar estas reglas en el nivel de dispositivo para controlar y automatizar las tareas dentro de la aplicación.

Al igual que con cualquier solución de IoT diseñada para funcionar a gran escala, es importante tener un enfoque estructurado en relación con la administración de dispositivos. No basta con conectar los dispositivos a la nube, sino que es necesario mantener los dispositivos conectados y en buen estado. Use las siguientes funcionalidades de IoT Central para administrar los dispositivos a lo largo del ciclo de vida de la aplicación:

### <a name="dashboards"></a>Paneles

Comience con un panel pregenerado en una plantilla de aplicación, o bien cree sus propios paneles adaptados a las necesidades de sus operadores. Puede compartir los paneles con todos los usuarios de la aplicación, o bien mantenerlos privados.

### <a name="rules-and-actions"></a>Reglas y acciones

Cree [reglas personalizadas](tutorial-create-telemetry-rules.md) basadas en el estado y la telemetría de los dispositivos, con el fin de identificar aquellos que necesitan atención. Configure acciones para notificar a las personas adecuadas y asegúrese de que se aplican medidas correctivas en el momento oportuno.

### <a name="jobs"></a>Trabajos

Los [trabajos](howto-manage-devices-in-bulk.md) permiten aplicar actualizaciones individuales o en masa a los dispositivos, para lo que se establecen propiedades o se realizan llamadas a comandos.

## <a name="integrate-with-other-services"></a>Integración con otros servicios

Como plataforma de aplicaciones, IoT Central permite transformar los datos de IoT en información empresarial que produce resultados útiles. Las [reglas](./tutorial-create-telemetry-rules.md), la [exportación de datos](./howto-export-data.md) y la [API REST pública](/learn/modules/manage-iot-central-apps-with-rest-api/) son ejemplos de integración de IoT Central con aplicaciones de línea de negocio:

![Transformación de datos de IoT mediante IoT Central](media/overview-iot-central/transform.png)

Puede generar información empresarial, como determinar las tendencias de eficiencia de la máquina o predecir el uso futuro de la energía en una fábrica, mediante la creación de canalizaciones de análisis personalizadas para procesar los datos de telemetría de los dispositivos y almacenar el resultado. Configure las exportaciones de datos en la aplicación de IoT Central para exportar datos de telemetría, cambios de propiedades de los dispositivos y cambios de las plantillas de dispositivos a otros servicios en los que puede analizar, almacenar y visualizar los datos con sus herramientas preferidas.

### <a name="build-custom-iot-solutions-and-integrations-with-the-rest-apis"></a>Creación de soluciones de IoT personalizadas e integraciones con las API REST

Cree soluciones de IoT como:

- Aplicaciones de Mobile Companion que pueden configurar y controlar dispositivos de forma remota.
- Integraciones personalizadas que permiten a las aplicaciones de línea de negocio existentes interactuar con los datos y dispositivos de IoT.
- Aplicaciones de administración de dispositivos para el modelado de dispositivos, la incorporación, la administración y el acceso a los datos.

## <a name="administer-your-application"></a>Administración de la aplicación

Las aplicaciones IoT Central están totalmente hospedadas por Microsoft, lo que reduce la sobrecarga de administración que suponen las aplicaciones. Los administradores administran el acceso a la aplicación con [permisos y roles de usuario](howto-administer.md).

## <a name="pricing"></a>Precios

Para crear aplicaciones de IoT Central se puede usar una evaluación gratuita de 7 días, o bien un plan de precios estándar.

- Las aplicaciones que se crean mediante el plan *gratuito* no tienen costo durante siete días y admiten un máximo de cinco dispositivos. En cualquier momento antes de que expiren puede convertirlas para que usen un plan de precios estándar.
- Las aplicaciones que se crean con un plan *estándar* se facturan por dispositivo, y se puede elegir entre un plan de precios **Estándar 0**, **Estándar 1** o **Estándar 2**, siendo gratis los dos primeros dispositivos. Más información sobre los [precios de IoT Central](https://aka.ms/iotcentral-pricing).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una visión general de IoT Central, estos son los siguientes pasos sugeridos:

- Si es un desarrollador de dispositivos y desea profundizar en algún código, el paso siguiente que se sugiere se indica en [Creación y conexión de un aplicación cliente a una aplicación de Azure IoT Central](./tutorial-connect-device.md).
- Familiarizarse con la [interfaz de usuario de Azure IoT Central](overview-iot-central-tour.md).
- [Crear una aplicación de Azure IoT Central](quick-deploy-iot-central.md) para empezar a trabajar.
